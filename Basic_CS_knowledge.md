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