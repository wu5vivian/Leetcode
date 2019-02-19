
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

## 4. MaximumMinimumPath (need knowing DP version)
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

		int[] dx = new int[]{0, 1};
		int[]dy = new int[]{-1, 0};
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
__There is a DP version for this question__

## 5. Substree: Maximum average node (need verify)

Given a binary tree, find the subtree with maximum average, return the root of the subtree.

Variations: not binary tree or the average doesn't include the root of the subtree.
```java
/**
	Definition of Treenode:
	public class TreeNode{
		public int val;
		public TreeNode left, right;
		public TreeNode(int val){
			this.val = val;
			this.left = this.right = null;
		}
	}
**/ 
public class Solution{
	public class ResultType{
		public int sum;
		public int node_num;
		public ResultType(int s, int n){
			this.sum = s;
			this.node_num = n;
		}

	}
	public TreeNode res = new TreeNode(0);
	public double max = Double.MIN_VALUE;
	public TreeNode findMaximum(TreeNode root){
		helper(root);
		return res;
	}

	public ResultType helper(TreeNode root){
		if(root == null){
			return new ResultType(0,0);
		}

		ResultType leftresult = helper(root.left);
		ResultType rightresult = helper(root.right);

		int totalsum = leftresult.sum + rightresult.sum + root.val;
		int totalnode = leftresult.node_num + rightresult.node_num + 1;

		if(max < totalsum / totalnode){
			max = totalsum / totalnode;
			res = root;
		}
		return new ResultType(totalsum, totalnode);

	}

}
```

## 6. K Minimum Distance

In a two dimensional lot, there is trenchs and obstacle. Robots needs to begin from top-left corner of the lot, could go left, right, up and down. The goal is to get the obstacle in minimum distance and at the same time the robit needs to avoid trenchs.

Assumptions:
* flat areas are represented as 1, areas with trenches are represented by 0 and obstacle is represented by 9.


BFS
```java
public class Solution{
	public int MinimumDistance(int[][] lot){
		if(lot[0][0] == 0){
			return -1;
		}

		if(lot[0][0] == 9){
			return 0;
		}
		int r = lot.length;
		int c = lot[0].length;
		int[][] visited = new int[r][c];

		int[] dx = {0, 1, 0, -1};
		int[] dy = {1, 0, -1, 0};
		int level = 0;
		Queue<Integer> q_x = new LinkedList<>();
		Queue<Integer> q_y = new LinkedList<>();
		q_x.offer(0);
		q_y.offer(0);

		while(!q_x.isEmpty()){
			int size = q_x.size();
			level++;

			for(int i = 0; i < size; i++){
				int cur_x = q_x.poll();
				int cur_y = q_y.poll();

				visited[cur_x][cur_y] = 1;

				for(int k = 0; k < 4; k++){
					int new_x = cur_x + dx[k];
					int new_y = cur_y + dy[k];
					if(new_x >= 0 && new_y >= 0 && new_x < r && new_y < c && lot[new_x][new_y] != 0 && visited[new_x][new_y] != 1){
						if(lot[new_x][new_y] == 9){
							return level;
						}
						q_x.offer(new_x);
						q_y.offer(new_y);

					}
				}

			}
		}

		return -1;
	}
}
```

## 7 K Nearest Points
[973. K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

First Version: Array Sorting
```java
class Solution {
    public int[][] kClosest(int[][] points, int K) {
        int N = points.length;
        int[][] ans = new int[K][2];
        int[] dist = new int[N];
        for(int i = 0; i < N; i++){
            dist[i] = dist(points[i]);
        }
        Arrays.sort(dist);
        int distK = dist[K - 1];
        int k = 0;
        for(int i = 0; i < N; i++){
            int d = dist(points[i]);
            if(d <= distK){
                ans[k++] = points[i];
            }
        }
        return ans;
        
    }
    public int dist(int[] point){
        return point[0]*point[0] + point[1]*point[1];
    }
}
```

Second Version Using priority Queue
1. Use max heap priority queue, first add all elements to the priority queue, then if the priority queue size larger than K, poll the element.
2. Could also use min heap, first add all elements to minimum priority queue, and poll first K elements.
```java
// Max Heap
class Solution {
    public int[][] kClosest(int[][] points, int K) {
        if(points == null || points.length <= K){
            return points;
        }
        
        int[][] result = new int[K][2];
        PriorityQueue<int[]> q = new PriorityQueue<int[]>(K, new Comparator<int[]>(){
            @Override
            public int compare(int[] a, int[] b){
                int dist_a = a[0] * a[0] + a[1] * a[1];
                int dist_b = b[0] * b[0] + b[1] * b[1];
                return dist_b - dist_a;
            }
        });
        
        for(int i= 0; i < points.length; i++){
            q.offer(points[i]);
            if(q.size() > K){
                q.poll();
            }
        }
        
        for(int i = 0; i < K; i++){
            result[i] = q.poll();
        }
        
        return result;
        
    }
}

// Min heap:
class Solution {
    public int[][] kClosest(int[][] points, int K) {
        if(points == null || points.length <= K){
            return points;
        }
        
        int[][] result = new int[K][2];
        PriorityQueue<int[]> q = new PriorityQueue<int[]>(K, new Comparator<int[]>(){
            @Override
            public int compare(int[] a, int[] b){
                int dist_a = a[0] * a[0] + a[1] * a[1];
                int dist_b = b[0] * b[0] + b[1] * b[1];
                return dist_a - dist_b;
            }
        });
        
        for(int i= 0; i < points.length; i++){
            q.offer(points[i]);
        }
        
        for(int i = 0; i < K; i++){
            result[i] = q.poll();
        }
        
        return result;
        
    }
}

```

**Tips** 

Write your own Comparator for priority queue.
```Java
PriorityQueue<Object> q = new PriorityQueue<Object>(int capacity, new Comparator<Object>(){
	@Override
	public int compare(Object o1, Object o1){
		if(o1 < o2)
			return 1;
		else if(o1 == o2){
			return 0;
		}else
			return -1;
	}

	});

```
Write with separate comparator.
```java
// Write Separate Comparator
class myCmp implements Comparator<Object>{
	public int compare(Object o1, Object o2){
		return o1 - o2;
	}
}

PriorityQueue<Object> q = new PriorityQueue<Object>(int capacity, new myCmp());
```

## 8 Flight  (Need verify)

Amazon Prime Air is developing  a system that divides shipping routes using flight optimization routing systems to a cluster of aircraft that can fullfill these routes. Each shipping route is identified by a unique integer identifier, requires a fixed non-zero amount of travel distance between airports, and is defined to be either a forward shipping route or a return shipping route. Identifiers are guaranteed to be unique within their own route type, but not across route types.......
题目太长不想打了。大概意思就是一个飞机有一个maximum operating distance， 要从去程和回程中找到一个最优的路径组合，似的这个路径组合最接近飞机的maximum operating distance，但是又不超过这个值。
Similiar Question to __"Two cloest sum"__

```java
class Solution{
	class Distance{
		int id;
		int distance;
		Distance(int i, int d){
			this.id = i;
			this.distance = d;
		}
	}

	class Mycmp implements Comparator<Distance>{
		@Override
		public int compare(Distance d1, Distance d2){
			return d1.distance - d2.distance;
		}
	}

	public List<List<Integer>> optmialPair(int maximumOD, List<List<Integer>> forward, List<List<Integer>> backward){
			List<List<Integer>> result = new ArrayList<List<Integer>>();
			if(forward == null || backward == null || forward.size() == 0 || backward.size() == 0){
				return result;
			}

			Distance[] forwardDistance = new Distance[forward.size()];
			Distance[] backwardDistance = new Distance[backward.size()];

			for(int i = 0; i < forward.size(); i++){
				forwardDistance[i] = new Distance(forward.get(i).get(0), forward.get(i).get(1));
			}

			for(int i = 0; i < backward.size(); i++){
				backwardDistance[i] = new Distance(backward.get(i).get(0), backward.get(i).get(1));
			}

			Arrays.sort(forwardDistance, new Mycmp());
			Arrays.sort(backwardDistance, new Mycmp());

			int left = 0;
			int right = backwardDistance.length - 1;
			int cur_max = Integer.MIN_VALUE;
			int left_min = 0;
			int right_min = 0;

			while(left < forwardDistance.length || right >= 0){
				if(forwardDistance[left].distance + backwardDistance[right].distance <= maximumOD){
					ArrayList<Integer> res = new ArrayList<Integer>();
					res.add(forwardDistance[left].id);
					res.add(backwardDistance[right].id);
					if(forwardDistance[left].distance + backwardDistance[right].distance > cur_max){
						cur_max = forwardDistance[left].distance + backwardDistance[right].distance;
						left_min = left;
						right_min = right;
						result.clear();
						result.add(res);
					}else if(forwardDistance[left].distance + backwardDistance[right].distance == cur_max){
						result.add(res);
					}else{

					}

					if(left < forwardDistance.length - 1)
						left++;
					if(forwardDistance[left].distance + backwardDistance[right].distance == maximumOD){
						if(right > 0) right--;
					}
				}else{
					if(right > 0) right--;
				}

				if(left == forwardDistance.length && right == 0){
					break;
				}
				
			}
			return result;
	}

}
```


## 9 Maze II
[505. The Maze II](https://leetcode.com/problems/the-maze-ii/)
Improved DFS: 一个方向走到头再转向， 加一个while循环keep一个方向往前走。
最短路线：持续保持更新，保证每个点的路径是最短的。



```java
class Solution {
    public int shortestDistance(int[][] maze, int[] start, int[] destination) {
        int[][] distance = new int[maze.length][maze[0].length];
        
        for(int[] row : distance){
            Arrays.fill(row, Integer.MAX_VALUE);
        }
        
        distance[start[0]][start[1]] = 0;
        DFS(maze, start[0], start[1], distance);
        return distance[destination[0]][destination[1]] == Integer.MAX_VALUE? -1 : distance[destination[0]][destination[1]];
        
    }
    
    public void DFS(int[][] maze, int i, int j, int[][] distance){
         int[] dx = {-1, 0, 1, 0};
         int[] dy = {0, -1, 0, 1};
        
         for(int k = 0; k < 4; k++){
             int count = 0;
             int new_i = i + dx[k];
             int new_j = j + dy[k];
             
             while(new_i >= 0 && new_i < maze.length && new_j >= 0 && new_j < maze[0].length && maze[new_i][new_j] == 0){
                 count++;
                 new_i += dx[k];
                 new_j += dy[k];
             }
             
             if(distance[i][j] + count < distance[new_i - dx[k]][new_j - dy[k]]){
                 distance[new_i - dx[k]][new_j - dy[k]] = distance[i][j] + count;
                 DFS(maze, new_i - dx[k], new_j - dy[k], distance);
             }
             
         }
        
    }
}
```

## 10 High Five
```java
class Record{
	int id;
	int score;
	Record(int i, int s){
		id = i;
		score = s;
	}
}

class My_cmp implements Comparator<Record>{
	@Override
	public int compare(Object o1, Object o2){
		return o1.score - o2.score;
	}
}


public Map<Integer, Double> highFive(Record[] records){
	Map<Integer, Double> result = new HashMap<Integer,Double>();
	if(results == null || results.length == 0){
		return result;
	}

	Map<Integer, PriorityQueue<Record>> map = new HashMap<>();

	for(Record r : records){
		if(!map.containsKey(r.id)){
			PriorityQueue<Record> pq = new PriorityQueue<Record>(10,new My_cmp());
			map.put(r.id, pq);

		}

		map.get(r.id).offer(r.score);
		if(map.get(r.id).size() > 5){
			map.get(r.id).poll();
		}
	}

	for(int Id : map.keySet()){
		PriorityQueue<Record> getpq = map.get(Id);
		double sum = 0;
		int size = getpq.size();
		while(!getpq.isEmpty()){
			Record rd = getpq.poll();
			sum += rd.score;
		}
		result.put(Id, sum / size);
	}

	return result;


}
```

## 11.MST
```java
import java.util.*;

class Connection{
	int node1;
	int node2;
	int cost;
	public Connection(String a, String b, int c){
		node1 = a;
		node2 = b;
		cost = c;
	}

public class CityConnection{
	priviate static int numOfGroup;

	public static ArrayList<Connection> getLowCost(ArrayList<Connection> connections){
		if(connections == null)}{
			return new ArrayList<Connection>();
		}

		ArrayList<Connection> result = new ArrayList<>();
		Map<String, Integer> map = new HashMap<>();

		Collections.sort(connections, new Comparator<Connection>(){
			@Override
			public int compare(Object o1, Object o2){
				return o1.cost - o2.cost;
			}
			});
		numOfGroup = 0;

		for(Connection c : connections){
			String a = c.node1;
			String b = c.node2;

			if(union(map, a, b)){
				result.add(c);
			}
		}

		String start = connections.get(0).node1;
		int groupId = map.get(start);

		for(int k : map.keySet()){
			if(map.get(k) != groupId){
				return new ArrayList<>();
			}
		}

		Collections.sort(result, new Comparator<Connection>(){
			@Override
			public int compare(Object o1, Object o2){
				if(o1.node1.equals(o2.node1)){
					return o1.node2.compareTo(o2.node2);
				}else{
					return o1.node1.compareTo(o2.node1);
				}
			}
			});

		return result;

	}

	private staitc boolean int(Map<String, Integer> map, String a, String b){
		if(!map.containsKey(a) && !map.containsKey(b)){
			map.put(a, numOfGroup);
			map.put(b, numOfGroup);
			numOfGroup++;
			return true;
		}

		if(map.containsKey(a) && !map.containsKey(b)){
			int group_a = map.get(a);
			map.put(b, group_a);
			return true;
		}

		if(!map.containsKey(a) && map.containsKey(b)){
			int group_b = map.get(b);
			map.put(a, group_b);
			return true;
		}

		int group_a = map.get(a);
		int group_b = map.get(b);

		if(group_a == group_b) return false;
		for(String s : map.keySet()){
			if(map.get(s) == group_b) map.put(s, group_a);
		}

		return true;

	}

}
}
```