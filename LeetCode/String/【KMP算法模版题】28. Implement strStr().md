28.Implement strStr() 字符串匹配
=====

https://leetcode.com/problems/implement-strstr/
----

![image](https://user-images.githubusercontent.com/91653378/141719726-fb8f8d30-23ab-4182-ac9c-8d6c925546e3.png)

代码：
----
````Java
// KMP算法模版题
// 该算法讲解：https://www.programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC
// 1. 创建前缀值数组 -- prefix array
// 2. 利用前缀值数组来匹配haystack和needle

class Solution {
    public int strStr(String haystack, String needle) {
        // base case
        if (needle.length() == 0) return 0;
        
        // 创建prefix前缀值数组，调用前缀值函数
        int[] prefix = new int[needle.length()];
        helper(prefix, needle);
        
        /* 比较haystack和needle的每个元素，看匹配
           haystack   "a a b a a b a a f"
                       s         s2
           needle     "a a b a a f"
                       t   t1    t2 (当t=t2时，由于s2!=t2，t2便回退到t1处)
           prefix.    [0,1,0,1,2,0] (needle的前缀值数组)
        */
        
        int t = 0;
        for (int s = 0; s < haystack.length(); s++) {
            while (t > 0 && haystack.charAt(s) != needle.charAt(t)) {
                t = prefix[t - 1]; //t要回退到t - 1在prefix数组中的数值在needle数组对应的元素上
            }
            if (haystack.charAt(s) == needle.charAt(t)) {
                t++;
            }
            if (t == needle.length()) {
                return s - needle.length() + 1;
            }
        }
        return -1;
    }
    
    // 创建前缀值函数并完善prefix前缀数组
    //    needle = "a  a  b  a  a  f"
    // prefix       [0, 1，0，1，2，0]
    // 指针i         i
    // 指针j            j
    
    private void helper(int[] prefix, String needle) {
        int i = 0; //i = 0; j = 1;
        prefix[0] = 0; // 初始化第一个前缀值为0
        for (int j = 1; j < needle.length(); j++) {
            while (i > 0 && needle.charAt(i) != needle.charAt(j)) { // 说明两指针指向的元素不相同
                i = prefix[i - 1]; // 当不相同时，指针i回退到i - 1在前缀值数组中指向的位置
            }
            if (needle.charAt(i) == needle.charAt(j)) {
                i++;
            }
            prefix[j] = i; // 利用指针j来收集前缀值，等于以j为末尾（包括j）的这段子串中的相同前后缀数量，其实也就是i值
        }
    }
    
}




/*
class Solution {
    public int strStr(String haystack, String needle) {
        // base case
        if (haystack == null || needle == null || needle.length() > haystack.length()) {
            return -1;
        }
        if (needle.length() == 0) return 0;
        
        int j = 0;
        for (int i = 0; i < haystack.length(); i++) {
            if (j < needle.length() && haystack.charAt(i) == needle.charAt(j)) {
                j++;
            }
            if (j < needle.length() && j != 0 && haystack.charAt(i) != needle.charAt(j - 1)) return -1;
            if (j >= needle.length()) {
                return i - needle.length() + 1;
            }
        }
        return -1;
    }
}

以上为错误解法，过不了这个例子：
"mississippi"
"issipi"
输出：5
期望输出值：-1
*/
````
