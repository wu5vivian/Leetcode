# Amazon OA

1. Order Dependency. (Similar to leetcode 210)
 [Course Schedule](https://leetcode.com/problems/course-schedule-ii/)

2. Longest Palindromic Substring
[5. longest Palindromic substring](https://leetcode.com/problems/longest-palindromic-substring/)

**思路**：找到中心点从两边延伸来判断是不是palindromic。对于原string,每一个字符可以作为一个可能的palindromic的中心（substring长度为奇数的情况），或者它与他邻接的字符一起作为中心（substring长度为偶数的情况）。遍历所有可能的中心，找出最长的palindromic。


**Tips** : 
1. 注意遍历所有substring中心的时候，Index < s.lenght() - 1， 因为你会考虑以 （index, index + 1）为中心的情况。不减1会出现OutofBound 的情况。

```java
class Solution {
    public String longestPalindrome(String s) {
        if(s == null){
            return "";
        }
        if(s.length() == 0 || s.length() == 1){
            return s;
        }
        String max_substring = "";
        for(int i = 0; i < s.length() - 1; i++){
            String cur1 = getLongest(s, i, i);
            String cur2 = getLongest(s, i, i + 1);
            if(cur1.length() > max_substring.length()){
                max_substring = cur1;
            }
            
            if(cur2.length() > max_substring.length()){
                max_substring = cur2;
            }
        }
        return max_substring;        
    }
    
    public String getLongest(String s, int i, int j){        
        while(i >= 0 && j < s.length()){
            if(s.charAt(i) != s.charAt(j)){
                break;
            }
            i--;
            j++;
        }
//        String result = j == s.length()? s.substring(i+1, j+1): s.substring(i + 1, j);
        return s.substring(i + 1, j);
    }
    
}
```