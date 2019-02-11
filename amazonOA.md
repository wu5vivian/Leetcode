# Amazon OA

1. Order Dependency. (Similar to leetcode 210)
 [Course Schedule](https://leetcode.com/problems/course-schedule-ii/)

2. Longest Palindromic Substring
[5. longest Palindromic substring](https://leetcode.com/problems/longest-palindromic-substring/)

**思路**：找到中心点从两边延伸来判断是不是palindromic。对于原string,每一个字符可以作为一个可能的palindromic的中心（substring长度为奇数的情况），或者它与他邻接的字符一起作为中心（substring长度为偶数的情况）。遍历所有可能的中心，找出最长的palindromic。


**Tips** : 
1. 注意遍历所有substring中心的时候，Index < s.lenght() - 1， 因为你会考虑以 （index, index + 1）为中心的情况。不减1会出现OutofBound 的情况。


