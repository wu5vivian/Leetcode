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


3. Most Frequent Used Words   [819. Most Common Word](https://leetcode.com/problems/most-common-word/)

找出句子中出现频率最高的单词。这题首先向句子中的单独的单词Parse出来，然后用hashmap存储出现的频率，得到最高的。
第一个注意点： 用正则表达式Parse出单个的单词。
第二个注意点： 每次更新hashmap的count的时候就与max比较，同时更新最大值。
```java
class Solution {
    public String mostCommonWord(String paragraph, String[] banned) {
        if(paragraph == null || paragraph.length() == 0){
            return "";
        }
        String[] words = paragraph.replaceAll("[^a-zA-Z]"," ").toLowerCase().split("\\s+");  
        Set<String> ban = new HashSet<>();
        HashMap<String, Integer> count = new HashMap<>();
        int max = 0;
        String max_string = "";
        
        for(String b : banned){
            ban.add(b.toLowerCase());
        }
        
        for(String w : words){
            if(ban.contains(w)){
                continue;
            }            
            if(count.containsKey(w)){
                int cur = count.get(w);
                count.put(w, cur + 1);
                if(cur + 1 > max){
                    max = cur + 1;
                    max_string = w;
                }
                
            }else{
                count.put(w, 1);
                if(max <= 1){
                    max = 1;
                    max_string = w;
                }
            }
                        
        }
        return max_string;        
    }
}
```
[正则表达式资料链接]（http://www.vogella.com/tutorials/JavaRegularExpressions/article.html）

**Tips** from this questions:
1. ```java  str.toLowerCase() / str.toUpperCase() ```
2. regrex: "[^a-zA-z]" / "\\W+": all non-word character. Attention: "+" used for representing "more"
3. regrex: "\\s+"： all white-space. Also need attention: use "+" here.