Recently, I had an interview with a tier one company in Vietnam and finally had an offer to work at the company. Today I want to share my interview experience with you.

I left my last company in the middle of June I finally got an offer in the middle of August 2023. So I had 2 months of relax, preparation, sharpening my programming skills and applying for jobs.

I heard of the company because one of my old colleague had worked at the company for a long time. I actively did research on Linkedin and found a job advertisement. What suprised me that the job title is something like "Java Developer(Algorithm)". I'm interested in algorithms so that grabbed me right away. I contacted the HR and she said I was a good match for the job maybe because I specify my algorithms skills in my CV.

There are in total of 3 interview rounds. The last two happened in the same day. All conducted online. All technical and including live programming.

### First interview
The round is supposed to last for an hour. There were two interviewers. One asked about my background, English proficiency and Java trivia. Here are some of the questions he asked
- What are enum and constants (I told him I recently learned the answer from my favourite Java book(Effective Java))
- Describe interfaces and abstract class
- Given a class having declaring a character and an int, what is the size of the object (the only one I couldn't answer)

Then the second interviewer gave me an algorithmic [problem](https://practice.geeksforgeeks.org/problems/roll-the-characters-of-a-string2127/1). I read the problem and knew I could fully solve it right away. I shared the naive solution and code it as I explained it, and, of course, the solution isn't good enough and passed 7/10 test cases. Then, I explained that this problem needs a trick called prefix sum. I wrote the solution and after some debugging and some typos fixes. I passed all 10/10 test cases. The interviewer congratulated on me and we finished the interview in less than an hour. I think this is good problem to access candidate's knowledge of basic algorithms.

The HR told me that I passed in that same Friday afternoon and asked for my available time slots scheduling for the next two interviews. 

### Second interview
The second interview is with a Chinese. He is located in the China office and, yes, the interview was conducted in English. He shared me a Hackerrank notepad, and paste the Java questions on it. It is really hard to understand him because he was in a strange room settings and doesn't really speak to microphone. After pasting the questions, I type fast the answers and also read out lout the answer :D. He seems satisfied with my answers. Here are some Java core quetions that he asked.
- How many types of JVM memory area? Where is String types located?
- How does GC work? How to find objects which need to be free?
- How to  create a thread in Java(Omg, I answered this exact same question 3 times in that week!)
- Explain the lifecycle of Java thread, thread states?
- What kinds of design patterns do you used?
- IO, the process when we read a file from disk to memory?
- Why do we need index for dbms?
- Data structure of an index?
- Some status code? (networking)

Then, he proceeded to the pair programming problem. The problem statement is really long (I can't find problem's link though). The solution needs a modified BFS. I didn't know the detailed solution at first. I construct the graph and write code as I thought about the problem. The code is ugly and messy but I stilled managed to pass 11/12 test cases. The fail test case is hidden. The interviewer seemed satisfied with my performance and he said something like "This is very good already" and concluded the interview soon after that.
### Third interview
The third interview is with a Chinese woman. She seems to sit in the same office as my second interviewer. She gave me a hard-Leetcode-level algorithmic [problem](https://leetcode.com/problems/max-chunks-to-make-sorted-ii/). The problem seems really scary at first, I had never solved it. I thought about that for a moment I realized this is some kind of greedy. I knew I could solve this if the input array is a permutation(which is the easier version of the problem!). I came up with an easy way to compress the input array to a permutation. Plugged everything in, and passed all the test cases! Later I knew I had invented a trick called `coordinate compression` in the interview!. The interviewer said very good and we still have 15 minutes left so she gave me another problem.

The second problem is about Java OOP. There is not much thinking into the problem, to be honest. I just need to know how Java syntax works and read the statement carefully. But there are this annoying bug of Java that neither do I and she understand why.
```java
public class Car {
    private int mass;
    public Car(int mass){
        this.mass = mass // (*)
    }
}
```
The Hackerrank Java compiler see `this.mass` same as `mass` in the brackets, which is not what I learned in school. The interview came and debug with me for some time and finally I was the one fix the problem and passed all the test cases(replace the paramter by `m` instead of `mass`). At the end, I said "We did it!" and she even said "I don't know why the keyword `this` doesn't work here :D". The interview ended 15 minutes overdue which is very not common in interviews. But I think she gave me more time because I solved the first problem so quick.

### Overall
This is by far the most interesting interview I ever had. The interview is well-conducted and the problems are not trivial. The hiring bar is not low at all(at least in this country). Never earlier in my life did I write a BFS in an interview. The interviewers don't really need me explaining the solution or communicating often as I was writing code. The number of test case passed is  clearly the passing grade of the candidate. I had the result the next day and an offer letter came a day after that.
