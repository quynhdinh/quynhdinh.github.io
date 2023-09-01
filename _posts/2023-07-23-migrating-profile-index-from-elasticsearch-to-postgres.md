This is one of the project I did in one of my job. In short, there is an Elasticsearch index called `Profile` and we want to move the persistent storage to Postgres. This is my writing-up of the process.

Build script
```json
./script/build master
```

Run script

```json
// starting postgres locally first
docker run --rm -p 5432:5432 --name bench-postgres -e POSTGRES_PASSWORD=postgres -d postgres
// point to some non-empty ES, because we want some profiles.
SERVICE_PROFILE_PERSISTENCE_TYPE=eav ELASTICSEARCH_PORT=19200 ./script/run master
```

Schemas

```json
postgres=# \dt
                List of relations
 Schema |        Name         | Type  |  Owner
--------+---------------------+-------+----------
 public | profile             | table | postgres
 public | profile_kv_bool     | table | postgres
 public | profile_kv_date     | table | postgres
 public | profile_kv_num      | table | postgres
 public | profile_kv_str      | table | postgres
 public | profile_segment     | table | postgres
 public | profile_sys_kv_bool | table | postgres
 public | profile_sys_kv_date | table | postgres
 public | profile_sys_kv_num  | table | postgres
 public | profile_sys_kv_str  | table | postgres
(10 rows)

postgres=# \d profile;
                         Table "public.profile"
   Column    |           Type           | Collation | Nullable | Default
-------------+--------------------------+-----------+----------+---------
 tenant_id   | integer                  |           | not null |
 profile_id  | character varying(27)    |           | not null |
 is_persona  | boolean                  |           |          |
 merged_with | character varying(27)    |           |          |
 first_visit | timestamp with time zone |           |          |
 last_visit  | timestamp with time zone |           |          |

postgres=# \d profile_segment;
                    Table "public.profile_segment"
   Column   |          Type          | Collation | Nullable | Default
------------+------------------------+-----------+----------+---------
 tenant_id  | integer                |           |          |
 profile_id | character varying(27)  |           |          |
 segment_id | character varying(100) |           |          |

**postgres=# \d profile_kv_str;**
                    Table "public.profile_kv_str"
   Column   |         Type          | Collation | Nullable | Default
------------+-----------------------+-----------+----------+---------
 tenant_id  | integer               |           |          |
 profile_id | character varying(27) |           |          |
 key        | character varying     |           |          |
 value      | character varying     |           |          |
Indexes:
    "profile_kv_str_tenant_id_profile_id_idx" btree (tenant_id, profile_id)
// All other table are simliar to above.
```

## How records are stored in a kv table

```json
postgres=# select * from profile_kv_str where profile_id='2JDLf0p1N7SfT9wLQqSweyVxVJm';
 tenant_id |profile_id|                     key                      | value
-----------+-----------------------------+----------------------------------------------+--------------------------------------
         1 | 2JDLf0p1N| eventTag[]                                   | New Visit
         1 | 2JDLf0p1N| channels_reachable$channels[]                | web_popup
         1 | 2JDLf0p1N| channels_reachable$onsite$notification_token | 96ac16f2-a90f-456b-a18d-6218a91c757e
         1 | 2JDLf0p1N| lastVisit                                    | 2022-12-21T07:30:29Z
         1 | 2JDLf0p1N| firstVisit                                   | 2022-12-21T07:30:06Z
         1 | 2JDLf0p1N| previousVisit                                | 2022-12-21T07:30:07Z
         1 | 2JDLf0p1N| create formula 6h                            | 2023-06-16 02:06:24 +0700
```

```json
"properties" : {
            **"eventTag" : [**
              "New Visit"
            **],**
            "nbOfVisits" : 2,
            **"channels_reachable" : {
              "onsite" : {
                "notification_token" :** "96ac16f2-a90f-456b-a18d-6218a91c757e"
              **},
              "channels" : [**
                "web_popup"
              **]
            },**
            **"lastVisit" :** "2022-12-21T07:30:29Z",
            **"firstVisit" :** "2022-12-21T07:30:06Z",
            **"previousVisit" :** "2022-12-21T07:30:07Z",
            "Còn mấy ngày nữa tới tết" : 0,
            **"create formula 6h" :** "2023-06-16 02:06:24 +0700"
}
```

## Correctness

### Test condition

In dev environment, fetch all profiles from Elasticsearch, save all to Postgres first.

```java
List<Profile> profiles = getAllProfilesFromES()
saveProfilesToPG(profiles)
```

For each Profile object fetch from ES `p1`, load from Postgres as up a Profile object `p2`. Compare `p1.getProperties()` with `p2.getProperties()`, and `p1.getSystemProperties()` and `p2.getSystemProperties()`

```java
for p1: profiles
	p2 = loadFromPG(p1.getItemId())
	assert(p1.getProperties().getKeys() == p2.getProperties().getKeys())
	assert(p1.getSystemProperties().getKeys() == p2.getSystemProperties.getKeys())
}
```

### Testing results (what are mismatch?)

- Values that are `null`, `empty map` or `empty list` are **not** stored to PostgreSQL tables.
- And for keys that have defined separator(which is now dollar-sign **$**) in name, it is wrongly parsed when convert back to a Java object. See the below Jira ticket for more detail.

[https://primehq.atlassian.net/browse/PDT-6741](https://primehq.atlassian.net/browse/PDT-6741)

- The current implementation doesn’t support multi-level searching.

[https://primehq.atlassian.net/browse/PDT-6729](https://primehq.atlassian.net/browse/PDT-6729)

```json
{
	"properties" : {
							"before" : { },
	            "bi_frequency_value_last_year" : null,
							"zalo_user_notes" : [ ],
							"address$city": "Ho Chi Minh"
	 },
	"systemProperties" : {
			"friends": [
			      {
			        "id": 1, "name": "John"
			      },
			      {
			        "id2": 2, "name2": "Andy"
			      }
			 ]
	 },
}
```

**Features:** 

- bulk update
- `sortBy`and `offset - limit`
    
    → Because of the nature of the EAV model. `sortBy` and `offset-limit` profiles are post-processed when done querying all profiles up from Postgres.
    

## ConditionType supported

- [x]  `matchAllCondition`
- [x]  `booleanCondition`
- [x]  `notCondition`
- [x]  `profilePropertyCondition`(partly)
    
    `comparisonOperator` are not supported yet( `inContains`, `hasSomeOf`, `hasNoneOf`, `all`, `isDay` `isNotDay`)
    

## Migration data from ES to PG (WIP)

For ~ **347734 profiles** on Dev, the current naive migrating strategy takes **90 mins** to finish

Here are a few ways to try to improve performance

- [x]  Batch the profiles-to-migrate as multiple batches. Run them concurrently.
    
    Run surprisingly fast (<10 mins)
    
    Tool for migrating data.
    
    Run with flag `MODE=1` for testing copy data
    
    ```jsx
    ELASTICSEARCH_PORT=19200 ~/workspace/unomix/script/run_pg_migration master
    ```
    
    Takes **16 mins (**starting from the moment run the above until all **347734** are saved to PG)
    
- [ ]  Build a `.csv` script. Feed it to PostgreSQL (seems faster)

```java
COPY profile_kv_bool FROM '/path/to/csv/profile_kv_bool.txt' WITH (FORMAT csv);
// same with all other tables.
```

## Benchmark

### Write profiles to PG

I initially want to save 1 million profiles. But the CPU usage at one point is too high, so i dies midway.

<!-- ![Untitled](Migrating%20Profile%20index%20from%20Elasticsearch%20to%20Post%20f933503c419a403ba8f240ecf6a6a196/Untitled.png) -->

<!-- ![Untitled](Migrating%20Profile%20index%20from%20Elasticsearch%20to%20Post%20f933503c419a403ba8f240ecf6a6a196/Untitled%201.png) -->

### Read profile by `profile_id`

Collected all the `profile_id` from the above write test. Use api `cxs/profiles/{profie_id}` to fetch profile. The average time is ~ `3ms` The RPS is average around 200.

| Total request | Median(ms) | 90% percentile | 99% percentile | Average(ms) | Min(ms) | Max(ms) |
| --- | --- | --- | --- | --- | --- | --- |
| 554164 | 2 | 3 | 4 | 3 | 1 | 3 |
|  |  |  |  |  |  |  |
