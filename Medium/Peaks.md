## Link:
https://app.codility.com/programmers/lessons/10-prime_and_composite_numbers/peaks/
## Question:
A non-empty array A consisting of N integers is given.

A peak is an array element which is larger than its neighbors. More precisely, it is an index P such that 0 < P < N − 1,  A[P − 1] < A[P] and A[P] > A[P + 1].

For example, the following array A:
* A[0] = 1 A[1] = 2 A[2] = 3 A[3] = 4 A[4] = 3 A[5] = 4 A[6] = 1 A[7] = 2 A[8] = 3 A[9] = 4 A[10] = 6 A[11] = 2

has exactly three peaks: 3, 5, 10.

We want to divide this array into blocks containing the same number of elements. More precisely, we want to choose a number K that will yield the following blocks:

A[0], A[1], ..., A[K − 1],
A[K], A[K + 1], ..., A[2K − 1],
...
A[N − K], A[N − K + 1], ..., A[N − 1].
What's more, every block should contain at least one peak. Notice that extreme elements of the blocks (for example A[K − 1] or A[K]) can also be peaks, but only if they have both neighbors (including one in an adjacent blocks).

The goal is to find the maximum number of blocks into which the array A can be divided.

Array A can be divided into blocks as follows:

one block (1, 2, 3, 4, 3, 4, 1, 2, 3, 4, 6, 2). This block contains three peaks.
two blocks (1, 2, 3, 4, 3, 4) and (1, 2, 3, 4, 6, 2). Every block has a peak.
three blocks (1, 2, 3, 4), (3, 4, 1, 2), (3, 4, 6, 2). Every block has a peak. Notice in particular that the first block (1, 2, 3, 4) has a peak at A[3], because A[2] < A[3] > A[4], even though A[4] is in the adjacent block.
However, array A cannot be divided into four blocks, (1, 2, 3), (4, 3, 4), (1, 2, 3) and (4, 6, 2), because the (1, 2, 3) blocks do not contain a peak. Notice in particular that the (4, 3, 4) block contains two peaks: A[3] and A[5].

The maximum number of blocks that array A can be divided into is three.

Write a function:

`class Solution { public int solution(int[] A); }`

that, given a non-empty array A consisting of N integers, returns the maximum number of blocks into which A can be divided.

If A cannot be divided into some number of blocks, the function should return 0.

For example, given:

    A[0] = 1
    A[1] = 2
    A[2] = 3
    A[3] = 4
    A[4] = 3
    A[5] = 4
    A[6] = 1
    A[7] = 2
    A[8] = 3
    A[9] = 4
    A[10] = 6
    A[11] = 2
the function should return 3, as explained above.

Write an efficient algorithm for the following assumptions:
* Complexity: expected worst-case time complexity is O(N);
## Solution:
* find all peaks and put them in a list
* try to divide the array into number of groups and check if all the groups have at least one peak
* if the group has a peak then the peakIdx//group should be 0, 1, 2, 3, ....
![peaks01](https://user-images.githubusercontent.com/59671980/122317618-d91b1800-ceeb-11eb-8964-975cea4a29fc.jpg)
![peaks02](https://user-images.githubusercontent.com/59671980/122317953-52b30600-ceec-11eb-8565-cba797dd838a.PNG)

```java
public class Peaks {
	public static int solution(int[] A) {
		int N = A.length;
	    // Find all the peaks
	    ArrayList<Integer> peaks = new ArrayList<Integer>();
	    for(int i = 1; i < N-1; i++){
	      if(A[i] > A[i-1] && A[i] > A[i+1]) peaks.add(i);
	    }

	    for(int size = 1; size <= N; size++){
	      if(N % size != 0) continue; // skip if non-divisible
	      int find = 0;
	      int groups = N/size;
	      boolean ok = true;
	      // Find whether every group has a peak
	      for(int peakIdx : peaks){
	        if(peakIdx/size > find){
	          ok = false;
	          break;
	        }
	        if(peakIdx/size == find) find++;
	      }
	      if(find != groups) ok = false;
	      if(ok) return groups;
	    }
	    return 0;
	}
	public static void main(String[] args) {
		int[] a= {1,2,3,4,3,4,1,2,3,4,6,2};
		System.out.println(solution(a));
	}
}
```
