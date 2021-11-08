并查集的概念：
=====

![image](https://user-images.githubusercontent.com/91653378/140663381-8686ec15-0b3f-463e-b9d2-348cc0d50db9.png)

![image](https://user-images.githubusercontent.com/91653378/140663388-90417ba8-af8d-4aa3-869c-aa71e644e6ad.png)

并查集解题基础模版：
-----
![image](https://user-images.githubusercontent.com/91653378/140663519-1b3897ac-0b35-4d35-bf10-ae981eedd619.png)

并查集基础模版的优化：
----
1.Improved with size（weighted quick union）<br>
-----
背景：union操作的时候，将两个子集合为一个，基础模版是盲目的进行合并，不看两个子集树的高度，这个优化比较两个子集树中node的数目(size)，将小数目的tree合并到大数目的tree上<br>
优化方法：引入一个size数组，size数组保留每一个node所在的set里面有多少个elements，elements数量小的set合并到elements数量大的set里面去<br>
结果：通过比较set中元素个数的方式来进行union，大部分情况下会working，但也有corner case，如果题目中明确了“有多少个”“size个数”这样的字眼，则选择此种方法比较合适<br>
![image](https://user-images.githubusercontent.com/91653378/140664356-7d7d99d1-f6c9-4b99-8a20-feaaa6e1ec6a.png)

2.Improved with rank <br>
------
优化方法：引入一个rank数组，rank数组代表每个node所在的set的最大高度是多少，union的过程是高度低的set向高度高的set进行union<br>
结果：大部分情况下都会选择rank来优化，相对于节点个数，set的深度/高度是更加重要<br>
![image](https://user-images.githubusercontent.com/91653378/140664684-cfdd6d82-e670-4a11-b4e9-7ab3f263f665.png)
