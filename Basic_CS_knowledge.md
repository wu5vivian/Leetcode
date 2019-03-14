# Binary Search Tree

**Search in BST**
```java
// A utility function to search a given key in BST 
public Node search(Node root, int key) 
{ 
	// Base Cases: root is null or key is present at root 
	if (root==null || root.key==key) 
		return root; 

	// val is greater than root's key 
	if (root.key > key) 
		return search(root.left, key); 

	// val is less than root's key 
	return search(root.right, key); 
} 

```

**Insertion of a key**

A new key is always inserted at leaf. We start searching a key from root till we hit a leaf node. Once a leaf node is found, the new node is added as a child of the leaf node.

This method uses Recursion.

```java
// Java program to demonstrate insert operation in binary search tree 
class BinarySearchTree { 

	/* Class containing left and right child of current node and key value*/
	class Node { 
		int key; 
		Node left, right; 

		public Node(int item) { 
			key = item; 
			left = right = null; 
		} 
	} 

	// Root of BST 
	Node root; 

	// Constructor 
	BinarySearchTree() { 
		root = null; 
	} 

	// This method mainly calls insertRec() 
	void insert(int key) { 
		root = insertRec(root, key); 
	} 
	
    Node insertRec(Node root, int key){
    	if(root == null){
    		root = new Node(key);
    		return root;
    	}

    	if(key < root.key){
    		root.left = insertRec(root.left, key);

    	}else if(key > root.key){
    		root.right = insertRec(root.right, key);
    	}

    	// return the (unchanged) node pointer;
    	return root;
    }
```


Time Complexity: The worst case time complexity of search and insert operations is O(h) where h is height of Binary Search Tree. In worst case, we may have to travel from root to the deepest leaf node. The height of a skewed tree may become n and the time complexity of search and insert operation may become O(n).

**Delete a key**

```java
void deleteKey(int key){
	root = deleteKey(root, key);
}

Node deleteRec(Node root, int key){
	if(root == null){
		return root;
	}

	if(key < root.key){
		root.left = deleteKey(root, key);
	}else if(key > root.key){
		root.right = deleteKey(root, key);
	}else{
		if(root.left == null){
			return root.right;
		}else if(root.right == null){
			return root.left;
		}

		root.key = minValue(root.right);

		root.right = deleteRec(root.right, root.key);
	}

	int minValue(Node root){
		int minv = root.key;
		while(root.left != null){
			minv = root.left,key;
			root = root.left;
		}

		return minv;
	}

}
```

# Heap

Heap is a special Tree-based data structure in which the tree is a complete tree.

```java
public class HeapMin(){
	private int[] heap;
	private int maxsize;
	private int size;

	public HeapMin(int max){
		maxsize = max;
		heap = new int[maxsize];
		size = 0;
		Heap[0] = Integer.MIN_VALUE;
	}

	private int leftchild(int pos){
		return 2 * pos;
	}

	private int rightchild(int pos){
		return 2 * pos + 1;
	}

	private int partent(int pos){
		return pos / 2;
	}

	private boolean isleaf(int pos){
		return (pos > size / 2) && (pos <= size);
	}

	private void swap(int pos1, int pos2){
		int tmp;
		tmp = Heap[pos1];
		Heap[pos1] = Heap[pos2];
		Heap[pos2] = tmp; 
	}

	public void insert(int ele){
		size++;
		Heap[size] = ele;
		int current = size;
		while(Heap[current] < Heap[parent(current)]){
			swap(current, parent(current));
			current = parent(current);
		}
	}

	public int extractMin(){
		int min = Heap[1];
		swap(1, size);
		size--;
		if(size != 0){
			pushdown(1);
		}
		return min;
	}

	private void pushdown(int position){
		int smallestchild;
		while(!isleaf(position)){
			smallestchild = leftchild(position);
			if(smallestchild < size && Heap[smallestchild] > Heap[smallestchild + 1]){
				smallestchild = smallestchild + 1;
			}

			if(Heap[position] <= Heap[smallestchild]){
				return;
			}

			swap(position, smallestchild);
			position = samllestchild;
		}
	}
}
```

# Rewrite Iterator
```java
// Java Iterator interface reference:
// https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html
class PeekingIterator implements Iterator<Integer> {
    Integer next = null;
    Iterator<Integer> iter;
	public PeekingIterator(Iterator<Integer> iterator) {
	    // initialize any member here.
        iter = iterator;
        if(iter.hasNext()){
            next = iter.next();
        }
	    
	}

    // Returns the next element in the iteration without advancing the iterator.
	public Integer peek() {
        return next;
	}

	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	@Override
	public Integer next() {
        Integer res = next;
	    next = iter.hasNext()? iter.next : null;
        return res;
	}

	@Override
	public boolean hasNext() {
        if(iter.next() == null){
            return false;
        }else{
            return true;
        }
	    
	}
}
```

# 3 SUM
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        if(nums == null || nums.length == 0){
            return result;
        }
        
        Arrays.sort(nums);
        for(int i = 0; i < nums.length - 2; i++){
            if(i > 0 && nums[i] == nums[i - 1]){
                continue;
            }
            int target = 0 - nums[i];
            int left = i + 1;
            int right = nums.length - 1;
                while(left < right){
                    if(nums[left] + nums[right] == target){
                        ArrayList<Integer> res = new ArrayList<Integer>();
                        res.add(nums[i]);
                        res.add(nums[left]);
                        res.add(nums[right]);
                        result.add(res);
                        while(left < right && nums[left] == nums[left + 1])
                            left++;
                        while(left < right && nums[right] == nums[right - 1])
                            right--;
                        left++;
                        right--;
                    }else if(nums[left] + nums[right] < target){
                        left++;
                    }else{
                        right--;
                    }            
            }
        }
        return result;        
    }
}
```

# 322 Coin Change
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        if(amount == 0){
            return 0;
        }
        int[] counts = new int[amount + 1];
        Arrays.fill(counts, Integer.MAX_VALUE - 1);
        for(int coin : coins){
            if(amount >= coin){
                counts[coin] = 1;
            }
        }
        counts[0] = 1;
        
        for(int i = 1; i <= amount; i++){            
            for(int coin : coins){
                if(i >= coin){
                    counts[i] = Math.min(counts[i - coin] + 1, counts[i]);
                }
            }
        }
        
        return counts[amount] == Integer.MAX_VALUE - 1? -1 : counts[amount];
        
    }
}
```

# 79 Word Search
```java

```

# 110 Balanced Binary TREE
```JAVA
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isBalanced(TreeNode root) {
            return height(root) != -1;
        
    }
    
    public int height(TreeNode root){
        if(root == null){
            return 0;
        }
        int left = height(root.left);
        if(left == -1) return -1;
        
        int right = height(root.right);
        if(right == -1) return -1;
        
        if(Math.abs(left - right) > 1){
            return -1;
        }
        
        return Math.max(left, right) + 1;
        
    }
}
```

# 139 Word Break
```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        if(s == null || s.length() == 0 || wordDict == null || wordDict.size() == 0){
            return false;
        }
        
        HashSet<String> set = new HashSet<>();
        for(String str : wordDict){
            set.add(str);
        }
        HashMap<String, Boolean> memo = new HashMap<>();
        
        return helper(s, 0, set, memo);
        
        
        
    }
    private boolean helper(String s, int start, HashSet<String> word, HashMap<String, Boolean> memo){
        if(start == s.length()){
            return true;
        }
        if(memo.containsKey(s.substring(start))){
            return memo.get(s.substring(start));
        }
        
        for(int i = start; i < s.length(); i++){
            String substring = s.substring(start, i + 1);
            if(word.contains(substring)){
                if(helper(s, i + 1, word, memo)){
                    memo.put(s.substring(i + 1), true);
                    return true;
                }
            }
            
        }
        memo.put(s.substring(start), false);
        return false;
    }
}
```