前缀表 / next数组
====
1. 前缀表：记录下标i之前（包括i）的字符串中，有多大长度的相同前缀后缀。
2. 前缀表的任务是当前位置匹配失败，找到之前已经匹配上的位置，在重新匹配，此也意味着在某个字符失配时，前缀表会告诉你下一步匹配中，模式串应该跳到哪个位置。
3. 文本串是指原来的字符串，模式串是指目标匹配串

最长公共前后缀
=====
字符串的前缀是指不包含最后一个字符的所有以第一个字符开头的连续子串；后缀是指不包含第一个字符的所有以最后一个字符结尾的连续子串。<br>
![image](https://user-images.githubusercontent.com/91653378/141693955-78317c63-9448-42b2-b4df-7b72e5bcb446.png)<br>
![image](https://user-images.githubusercontent.com/91653378/141693969-420f94fb-ae91-4418-9598-7bc6f2877e77.png)<br>
以下这句话，对于理解为什么使用前缀表可以告诉我们匹配失败之后跳到哪里重新匹配 非常重要！<br>
下标5之前这部分的字符串（也就是字符串aabaa）的最长相等的前缀 和 后缀字符串是：子字符串aa ，因为找到了最长相等的前缀和后缀，匹配失败的位置是后缀子串的后面，那么我们找到与其相同的前缀的后面从新匹配就可以了。<br>
所以前缀表具有告诉我们当前位置匹配失败，跳到之前已经匹配过的地方的能力。

如何计算前缀表？
====
![image](https://user-images.githubusercontent.com/91653378/141694171-649ae90e-0ffa-46a8-93b6-c780d9f1300d.png)<br>
模式串与前缀表对应位置的数字表示的就是：下标i之前（包括i）的字符串中，有多大长度的相同前缀后缀。

如何根据前缀表找到当字符不匹配的时候指针应该移动到的位置？
====
![image](https://user-images.githubusercontent.com/91653378/141694247-2e98dbc5-835e-4e88-8566-03ebbbd0e6be.png)<br>
如上图，当指针移动到模式串'f‘的位置时，我们去看它前一个字符的前缀表，发现前缀表数值是2，我们把指针移动到模式串下标2的位置上，然后再继续移动指针，直到找到我们要匹配的相同字符串：<br>
![image](https://user-images.githubusercontent.com/91653378/141694303-62520485-ae4b-457d-a7cf-d3c8ad62f019.png)<br>

前缀表和Next数组（prefix数组）
=====
1. 前缀表既可以是next数组本身，也可以是前缀表中的数值统一减1之后的next数组，采用前缀表数值统一减1之后的next数组，是为了防止不断算法死循环。<br>
2. 使用next数组来匹配文本串s和模式串t了（这里的next数组是前缀表数字统一减一之后的）：<br>
找到模式串t中不匹配的字符，找到该字符前一个字符在next数组中对应的数值，然后指针移动到模式串中该数值+1的地方，从这里开始遍历，重复该操作，直至模式串遍历结束。

复杂度分析
====
其中n为文本串长度，m为模式串长度，因为在匹配的过程中，根据前缀表不断调整匹配的位置，可以看出匹配的过程是O(n)<br>
之前还要单独生成next数组，时间复杂度是O(m)。所以整个KMP算法的时间复杂度是O(n+m)的。<br>

构造next数组
=====
构造next数组其实就是计算模式串s，前缀表的过程。 主要有如下三步：<br>
1.初始化<br>
2.处理前后缀不相同的情况<br>
3.处理前后缀相同的情况<br>

1.初始化next数组和getNext函数<br>
-----
getNext(String t, int[] next)<br>
定义两指针：j指向模式串t的前缀末尾位置，i指向模式串t的后缀末尾位置，初始化j为-1，即前缀表数组next的第一位的前缀数值为-1。<br>
next[i] 表示 i（包括i）之前最长相等的前后缀长度（其实就是j），所以初始化next[0] = j = -1。<br>
````Java
void getNext(int* next, const string& s)
int j = -1;
next[0] = j;
````
2.处理前后缀不相同和前后缀相同的情况
-----
````Java
因为j初始化为-1，那么i就从1开始，进行s[i] 与 s[j+1]的比较。(即比较s[1]和s[0]开始，s[2]和s[1]）

所以遍历模式串s的循环下标i 要从 1开始，代码如下：
for(int i = 1; i < s.size(); i++) {
如果 s[i] 与 s[j+1]不相同，也就是遇到 前后缀末尾不相同的情况，就要向前回退。
next[j]就是记录着j（包括j）之前的子串的相同前后缀的长度。
那么 s[i] 与 s[j+1] 不相同，就要找 j+1前一个元素在next数组里的值（就是next[j]）。

所以，处理前后缀不相同的情况代码如下：
while (j >= 0 && s[i] != s[j + 1]) { // 前后缀不相同了
    j = next[j]; // 向前回退
}

所以，处理前后缀相同的情况：
如果s[i] 与 s[j + 1] 相同，那么就同时向后移动i 和j 说明找到了相同的前后缀，同时还要将j（前缀的长度）赋给next[i], 因为next[i]要记录相同前后缀的长度。

代码如下：
if (s[i] == s[j + 1]) { // 找到相同的前后缀
    j++;
}
next[i] = j; 
// next[i]指的是i包括i之前的这个子串的最长相等前后缀的长度
// 也就是j的值，也就是j指代的前缀末尾位置，也就是前缀值，也就是next数组中的前缀的数值

最后整体构建next数组的函数代码如下：
void getNext(int* next, const string& s){
    int j = -1;
    next[0] = j;
    for(int i = 1; i < s.size(); i++) { // 注意i从1开始
        while (j >= 0 && s[i] != s[j + 1]) { // 前后缀不相同了
            j = next[j]; // 向前回退
        }
        if (s[i] == s[j + 1]) { // 找到相同的前后缀
            j++;
        }
        next[i] = j; // 将j（前缀的长度）赋给next[i]
    }
}

````
![image](https://user-images.githubusercontent.com/91653378/141695770-b02af812-946a-4ca8-b2dc-65cebeecd0ae.png)<br>
![image](https://user-images.githubusercontent.com/91653378/141695788-b274d627-96dd-4716-8fae-ce8746d3d9f3.png)<br>
![image](https://user-images.githubusercontent.com/91653378/141695810-34f3fe3b-900c-4241-a599-b083645082ef.png)<br>
![image](https://user-images.githubusercontent.com/91653378/141695917-49146ea6-8482-4627-97a1-58fa3fba2433.png)<br>
![image](https://user-images.githubusercontent.com/91653378/141696014-75457dce-6f81-4b18-b20c-f66ba279dc34.png)<br>
![image](https://user-images.githubusercontent.com/91653378/141696020-a74cf0ee-41e1-4635-8e00-2c55a7543a74.png)<br>

使用next数组来做匹配
====
````Java

int j = -1; // 因为next数组里记录的起始位置为-1
for (int i = 0; i < s.size(); i++) { // 注意i就从0开始
    while(j >= 0 && s[i] != t[j + 1]) { // 不匹配 s中i对应的值始终与t中j+1对应的值进行比较
        j = next[j]; // j 寻找之前匹配的位置 如果不匹配，j就要回到next[j]的位置，然后s[i]再又与t[j+1]进行比较
                     // 注意j是回到next[j]的位置，而非next[j]+1的位置，因为比较的对象是j+1，所以j直接回到next[j]的位置即可
    }
    if (s[i] == t[j + 1]) { // 匹配，j和i同时向后移动
        j++; // i的增加在for循环里
    }
    if (j == (t.size() - 1) ) { // 文本串s里出现了模式串t
        return (i - t.size() + 1); // 返回在文本串字符串中找出模式串出现的第一个位置 (从0开始)
    }
}
````

KMP算法代表例题：
=====
28. Implement strStr() <br>
https://leetcode.com/problems/implement-strstr/
