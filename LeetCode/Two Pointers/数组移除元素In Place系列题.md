数组移除元素In Place系列题：
-----
思路：
-----
都是利用双指针，一个指针A起到“预留位置”的作用，另一个指针B遍历数组，找到符合题目要求的元素，然后放到指针A指向的位置上，进行原位替换。<br>

题目:
----
27.Remove Element https://leetcode.com/problems/remove-element/<br>
283.Move Zeroes https://leetcode.com/problems/move-zeroes/<br>
26.Remove Duplicates from Sorted Array https://leetcode.com/problems/remove-duplicates-from-sorted-array/<br>
80.Remove Duplicates from Sorted Array II https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/<br>




27.Remove Element 移除元素
===
https://leetcode.com/problems/remove-element/
----

![image](https://user-images.githubusercontent.com/91653378/139013168-83f71e31-1f7b-4dbd-bef1-46608f38047f.png)

代码：
-----
````Java
// Solution: Two Pointers
// 相同解题模版的系列题：27，283，26，80 (https://www.youtube.com/watch?v=_cVu4AYoeTw)
class Solution {
    public int removeElement(int[] nums, int val) {
        // base case
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        // init the two pointers:
        // start pointer: 预留位置，给满足条件的元素；(题目要求in place，所以start指针起到了原位替换的作用)
        // i pointer: 遍历旧数组，寻找满足条件的元素，给start指针指向的位置，从而替换掉不符合要求的元素；
        int start = 0;
        for (int i = 0; i < nums.length; i++) { // 也可以写成 int i : nums
            if (nums[i] != val) { // 也可以写成 i != val
                nums[start] = nums[i];
                start++;
            }
        }
        return start;
    }
}
````

283.Move Zeroes 移除0
====
https://leetcode.com/problems/move-zeroes/
----

![image](https://user-images.githubusercontent.com/91653378/139014212-2c2be98e-e58f-402f-960b-c894e4565cdf.png)

代码：
----
````Java
// Solution: Two Pointers
// 相同解题模版的系列题：27，283，26，80 (https://www.youtube.com/watch?v=_cVu4AYoeTw)
class Solution {
    public void moveZeroes(int[] nums) {
        int start = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) {
                nums[start] = nums[i];
                start++;
            }
        }
        for (int j = start; j < nums.length; j++) {
            nums[j] = 0;
        }
    }
}
````

26.Remove Duplicates from Sorted Array 从数组中移除重复元素
=====
https://leetcode.com/problems/remove-duplicates-from-sorted-array/
----

![image](https://user-images.githubusercontent.com/91653378/139014877-ce39bf18-90ba-48cf-a46e-cec31a257073.png)

代码：
-----
````Java
// Solution: Two Pointers
// 相同解题模版的系列题：27，283，26，80 (https://www.youtube.com/watch?v=_cVu4AYoeTw)
class Solution {
    public int removeDuplicates(int[] nums) {
        // base case
        if (nums == null && nums.length == 0) {
            return 0;
        }
        int start = 1;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] != nums[start - 1]) {
                nums[start] = nums[i];
                start++;
            }
        }
        return start;
    }
}
````

80.Remove Duplicates from Sorted Array II 从数组中移除重复元素II
=====
https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/
-----

![image](https://user-images.githubusercontent.com/91653378/139015073-ebded3a5-e4bf-41c7-a96a-b527052ca878.png)

````Java
// Solution: Two Pointers
// 相同解题模版的系列题：27，283，26，80 (https://www.youtube.com/watch?v=_cVu4AYoeTw)
class Solution {
    public int removeDuplicates(int[] nums) {
        // base case
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        // init two pointers
        int start = 2;
        for (int i = 2; i < nums.length; i++) {
            if (nums[i] != nums[start - 2]) {
                nums[start] = nums[i];
                start++; 
            }
        }
        return start;
    }
}
````
