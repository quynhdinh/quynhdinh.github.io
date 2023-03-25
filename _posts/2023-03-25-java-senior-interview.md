Today, I watch this mock interview (link)[https://www.youtube.com/watch?v=sGg-mhUMbyA]

Here are some learning notes for further research:

1. Why String length() function is not accurate?(Answer copy from Quora)
It isn't accurate because it will only account for the number of characters within the String. In other words, it will fail to account for code points outside of what is called the BMP (Basic Multilingual Plane), that is, code points with a value of U+10000 or greater.

The reason is historical: when Java was first defined, one of its goal was to treat all text as Unicode; but at this time, Unicode did not define code points outside of the BMP. By the time Unicode defined such code points, it was too late for char to be changed.

2. What is dgc?

3. What is the difference between finally and finalize in Java?

4. Copy contructor in Java, clones and GC?

5. How to handle Exceptions in Java?

6. Life cycle of an applet?
