# Amazon Frequent Coding Problem
## 88 [Merge Sorted Array](https://leetcode.com/problems/longest-palindromic-substring/)

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int pointer = m + n - 1;
        if(n == 0){
            return;
        }
        
        int pointer1 = m - 1;
        int pointer2 = n - 1;
        while(pointer2 >= 0 && pointer1 >= 0){
            if(nums2[pointer2] >= nums1[pointer1]){
                nums1[pointer--] = nums2[pointer2];
                pointer2--;
            }else{
                nums1[pointer--] = nums1[pointer1];
                pointer1--;
            }            
        }
        if(pointer1 >= 0){
            return;
        }
        
        while(pointer2 >= 0){
            nums1[pointer--] = nums2[pointer2];
            pointer2--;
        }
        return;
        
        
    }
}
```

## [Longest Substring with At Most Two Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/)
```java
class Solution {
    public int lengthOfLongestSubstringTwoDistinct(String s) {
        if(s == null || s.length() == 0){
            return 0;
        }
        if(s.length() <= 2){
            return s.length();
        }
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        int i = 0;
        map.put(s.charAt(i), 1);
        int j = 1;
        if(map.containsKey(s.charAt(j))){
            map.put(s.charAt(j),map.get(s.charAt(j)) + 1);
        }else{
            map.put(s.charAt(j), 1);
        }
        int max_len = j - i + 1;
        j++;
        while(j < s.length()){
            if(map.size() < 2){
                if(map.containsKey(s.charAt(j))){
                    map.put(s.charAt(j),map.get(s.charAt(j)) + 1);
                }else{
                    map.put(s.charAt(j), 1);
                }
                max_len = Math.max(j - i + 1, max_len);
            }else if(map.size() == 2){
                if(map.containsKey(s.charAt(j))){
                    map.put(s.charAt(j),map.get(s.charAt(j)) + 1);
                    max_len = Math.max(j - i + 1, max_len);
                }else{
                    map.put(s.charAt(j), 1);
                    map.put(s.charAt(i),map.get(s.charAt(i)) - 1);
                    if(map.get(s.charAt(i)) == 0){
                        map.remove(s.charAt(i));
                    }
                    i++;
                }
            }else{
                map.put(s.charAt(i),map.get(s.charAt(i)) - 1);
                if(map.get(s.charAt(i)) == 0){
                    map.remove(s.charAt(i));
                }
                i++;
                
                if(map.containsKey(s.charAt(j))){
                    map.put(s.charAt(j),map.get(s.charAt(j)) + 1);
                }else{
                    map.put(s.charAt(j), 1);
                }
            }
            j++;         
        }
        return max_len;
        
    }
}
```

## [Combination, Subsets, Permutation summary](https://leetcode.com/problems/permutations/discuss/18239/A-general-approach-to-backtracking-questions-in-Java-(Subsets-Permutations-Combination-Sum-Palindrome-Partioning))

Subsets:
Results in subsets need to store every possible combination.
```java
public List<List<Integer>> subsets(int[] nums){
	List<List<Integer>> result = new ArrayList<>();
	//Actually if there are no duplicated elements, this step could me skipped
	Array.sort(nums);
	ArrayList<Integer> list = new ArrayList<>();
	backtrack(0, nums, list, result);
	return reuslt;

}

private void backtrack(int start, int[] nums, ArrayList<Integer> list, ArrayList<ArrayList<Integer>> res){
	// For subset, we need to store every combination.
	res.add(new Arraylist(list));

	for(int i = start; i < nums.length; i++){
		list.add(nums[i]);
		backtrack(i + 1, nums, list, res);
		list.remove(list.size() - 1);
	}

}

```

Subsets with duplicated elements
```java
public List<List<Integer>> subsets(int[] nums){
	List<List<Integer>> result = new ArrayList<>();

	Array.sort(nums);
	ArrayList<Integer> list = new ArrayList<>();
	backtrack(0, nums, list, result);
	return reuslt;

}

private void backtrack(int start, int[] nums, ArrayList<Integer> list, ArrayList<ArrayList<Integer>> res){
	// For subset, we need to store every combination.
	res.add(new Arraylist(list));

	for(int i = start; i < nums.length; i++){
		// Key statement for removing duplicated element
		if(i > start && nums[i] == nums[i - 1]) continue;
		list.add(nums[i]);
		backtrack(i + 1, nums, list, res);
		list.remove(list.size() - 1);
	}

}

```

Permutation Problem
```java
// Version of no duplicated elements
public List<List<Integer>> permute(int[] nums){
	List<List<Integer>> result = new ArrayList<>();
	ArrayList<Integer> list = new ArrayList<>();
	backtrack(0, nums, list, result);
	return result;
}

private void backtrack(int index, int[] nums, ArrayList<Integer> list, ArrayLlist<ArrayList<Integer>> res){
	if(index == nums.length){
		res.add(new ArrayList<Integer>(list));
		return;
	}
	// For permutation, every list contains all elements, so every recursion should begin from 0.
	for(int i = 0; i < nums.length; i++){
		if(list.contains(nums[i])) continue;
		list.add(nums[i]);
		backtrack(i + 1, nums, list, res);
		list.remove(list.size() - 1);
	}


}
```

