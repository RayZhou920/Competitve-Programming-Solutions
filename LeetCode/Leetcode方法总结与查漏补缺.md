1.解决“找第K大的元素”的问题，用Heap/Priority Queue优先队列；<br>
=====
[215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

2.Interview Tip: In-place Algorithms
====
In-place algorithms overwrite the input to save space, but sometimes this can cause problems. Here are a couple of situations where an in-place algorithm might not be suitable.<br>

The algorithm needs to run in a multi-threaded environment, without exclusive access to the array. Other threads might need to read the array too, and might not expect it to be modified.<br>

Even if there is only a single thread, or the algorithm has exclusive access to the array while running, the array might need to be reused later or by another thread once the lock has been released.<br>

In an interview, you should always check whether or not the interviewer minds you overwriting the input. Be ready to explain the pros and cons of doing so if asked!

3.循环里面的break和continue
======
````Java
if语句里面continue和break的区别
break:结束整个循环体
continue:结束本次循环

代码说明：
public static void main(String[] args) {
        int x=0;
        while(x++ < 10)
        {
            if(x == 3)
            {
                break;
            }
            System.out.println("x="+x);
        }
         
        int y=0;
         
        while(y++ < 10)
        {
            if(y == 3)
            {
                continue;
            }
            System.out.println("y="+y);
        }
}
输出：
x=1
x=2
y=1
y=2
y=4
y=5
y=6
y=7
y=8
y=9
y=10
````
