# Amazon OA

**Pakages**
```java
?? import java.util.*
import java.util.Comparator;
import java.util.Map;
import java.util.Set;
import java.util.TreeMap;
```

## 1. Order Dependency. (Similar to leetcode 210)
 [Course Schedule](https://leetcode.com/problems/course-schedule-ii/)

## 2. Longest Palindromic Substring
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


## 3. Most Frequent Used Words  
[819. Most Common Word](https://leetcode.com/problems/most-common-word/)

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
2. regrex: "[^a-zA-Z]" / "\\W+": all non-word character. Attention: "+" used for representing "more"
3. regrex: "\\s+"： all white-space. Also need attention: need "+" here.


## 4. Count number of substrings with exactly K characters
similar to [340. Longest Substring with At Most K Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)
and could refer to (https://www.geeksforgeeks.org/count-number-of-substrings-with-exactly-k-distinct-characters/)

**Brute Force**  Get all possible subtrings from the string and them count the distinct characters in the substring, count all valid substrings. Most brute force ways will need O(n * n * n), which for every substring it will go through all the elements. This will cause duplicates. When the start of substring is fixed, using a hashmap / set to store current distinct characters when the substring's end is moving. This way will get a O(n * n) solution.
```java
import java.utils.Arrays;
import java.utils.Set;
public class CountKSubstr{
	int countKDist(String str, int k){
		if(str == null || str.length() < k){
			return 0;
		}
		int count = 0;
		for(int i = 0; i <= str.length() - k; i++){
			Set<Character> set = new HashSet<>();
	
			for(int j = i; j < str.length(); j++){
				set.add(str.charAt(j));
				if(set.size() == k){
					count++;
					break;
				}
			}
		}

	}
	return count;

}
```
* Need to consider lower time complexity.

## 4. MaximumMinimumPath
[174. Dungeon Game](https://leetcode.com/problems/dungeon-game/)

You are gonna climb mountains represented by a matrix. Each integer in the matrix represent altitude at that point. You are supposed to climb from the top-left corner to the bottom-right corner and only move rightward or downward in each step.

You can have many paths to do so. Each path has a minimum altitude. Find the maximum among all the minimum altitude of all paths.

example:
```txt
[8, 4, 7]
[6, 5, 9]
3 paths：
8-4-7-9 min: 4
8-4-5-9 min: 4
8-6-5-9 min: 5
return: 5
```
```java
public class MaximumMinimumPath{
	private int min, max;
	public int MaxMinPath(int[][] mountain){
		if(mountain == null || mountain.length == 0){
			return 0;
		}
		min = Integer.MAX_VALUE;
		max = Integer.MIN_VALUE;
		DFS(mountain, min, 0, 0);
		return max; 

	}
	public void DSF(int[][] mountain,int cur_min, int i, int j){
		if(i == mountain.length - 1 && j == mountain[0].length - 1){
			cur_min = Math.min(mountain[i][j], cur_min);
			max = Math.max(cur_min, max);
			return;
		}

		dx = {0, 1};
		dy = {-1, 0};
		cur_min = Math.min(mountain[i][j], cur_min);

		for(int k = 0; k < 2; k++){
			int newi = i + dx[k];
			int newj = j + dy[k];
			if(newi >= 0 && newi < mountain.length && newj >= 0 && newj < mountain[0].length){
				DFS（mountain, newi, newj);
			}
		}

	}
}
```
Another Version of DFS:
```java
public class MaximumMinimumPath {
	private int min, max, row, col;
	public int maxMinPath(int[][] matrix) {
		row = matrix.length;
		col = matrix[0].length;
		min = Integer.MAX_VALUE;
		max = Integer.MIN_VALUE;
		dfsHelper(matrix, min, 0, 0);
		return max;
	}

public void dfsHelper(int[][] matrix, int min, int i, int j ){
		if (i >= row || j >= col) return;
		if (i == row - 1 && j == col - 1) {
			min = Math.min(min, matrix[i][j]);
			max = Math.max(max, min);
			return;
		}
		min = Math.min(min, matrix[i][j]);
		dfsHelper(matrix, min, i, j + 1);
		dfsHelper(matrix, min, i + 1, j);
	}
}
```
