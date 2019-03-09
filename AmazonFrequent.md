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