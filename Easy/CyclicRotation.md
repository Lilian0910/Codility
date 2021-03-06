## Link:
https://app.codility.com/programmers/lessons/2-arrays/cyclic_rotation/
## Question:
An array A consisting of N integers is given. Rotation of the array means that each element is shifted right by one index, and the last element of the array is moved to the first place. For example, the rotation of array A = [3, 8, 9, 7, 6] is [6, 3, 8, 9, 7] (elements are shifted right by one index and 6 is moved to the first place).

The goal is to rotate array A K times; that is, each element of A will be shifted to the right K times.

Write a function:

class Solution { public int[] solution(int[] A, int K); }

that, given an array A consisting of N integers and an integer K, returns the array A rotated K times.

For example, given

    A = [3, 8, 9, 7, 6]
    K = 3
the function should return [9, 7, 6, 3, 8]. Three rotations were made:

    [3, 8, 9, 7, 6] -> [6, 3, 8, 9, 7]
    [6, 3, 8, 9, 7] -> [7, 6, 3, 8, 9]
    [7, 6, 3, 8, 9] -> [9, 7, 6, 3, 8]
For another example, given

    A = [0, 0, 0]
    K = 1
the function should return [0, 0, 0]

Given

    A = [1, 2, 3, 4]
    K = 4
the function should return [1, 2, 3, 4]

Assume that:
* N and K are integers within the range [0..100];

## Solution:
*  temp=A[A.len-1], use iteration let A[i]=A[i-1]

```java
public class CyclicRotation {
	public static int[] solution(int[] A, int K) {
		if(A.length==0) return A;
		int count=0;
		while(count<K) {	
			int temp=A[A.length-1];
			for(int i=A.length-1; i>0; i--) { 
				A[i]=A[i-1];
			}
			A[0]=temp;
			count++;
		}
		return A;
	}

	public static void main(String[] args) {
		int[] a = {3,8,9,7,6};
		int[] res1=solution(a, 3);
		for(int e: res1) System.out.println(e);
		System.out.println();
		int[] b = {1,2,3,4};
		int[] res2=solution(b, 4);
		for(int e: res2) System.out.println(e);
	}
```
