## Link:
https://app.codility.com/demo/results/trainingA6G8WP-FSM/
## Question:
A non-empty array A consisting of N integers is given. A pair of integers (P, Q), such that 0 ≤ P ≤ Q < N, is called a slice of array A. The sum of a slice (P, Q) is the total of A[P] + A[P+1] + ... + A[Q].

Write a function:

`class Solution { public int solution(int[] A); }`

that, given an array A consisting of N integers, returns the maximum sum of any slice of A.

For example, given array A such that:

A[0] = 3  A[1] = 2  A[2] = -6
A[3] = 4  A[4] = 0
the function should return 5 because:

(3, 4) is a slice of A that has sum 4,
(2, 2) is a slice of A that has sum −6,
(0, 1) is a slice of A that has sum 5,
no other slice of A has sum greater than (0, 1).
Write an efficient algorithm for the following assumptions:

N is an integer within the range [1..1,000,000];
each element of array A is an integer within the range [−1,000,000..1,000,000];
the result will be an integer within the range [−2,147,483,648..2,147,483,647].

## Solution:
* **Frist Thought 69%**: using accumulate sum array to find max sum . But it has the following questions:
  * cannot compare accumulate with current num. For example: [ -2, 1], the max slide should be 1, but the accumulate will return -1;
  * cannot solve consecutive negatives case. For example: [-1, -2, -3] should return -1, but accumulate will return 0;    
* **Second Thought 100%**: using localMax and absoluteMax to compare twice

```java
public class MaxSliceSum {
	public static int solution(int[] A) {
		int local=A[0];
		int absolute=A[0];
		int nextSum=0, curr=0;
		for(int i=1; i<A.length; i++) {
			curr=A[i];
			nextSum=local+curr;
			local=Math.max(A[i], nextSum);
			absolute=Math.max(absolute, local);
		}
		return absolute;
	}
	
	//for the input [-2, 1] the solution returned a wrong answer (got 0 expected 1)
	//对于input都是负数的不能return正确结果
	public static int solution1(int[] A) {
		int[] accum=new int[A.length];
		accum[0]=A[0];
		for(int i=1; i<A.length; i++) {
			accum[i]=Math.max(accum[i-1]+A[i], 0);
		}
		Arrays.sort(accum);
		int max=accum[accum.length-1];
		return max;
	}
	public static void main(String[] args) {
		int[] a={ 5, -7, 3, 5, -2, 4, -1 }; //10
		int[] b={-2, 1, -3, 4, -1, 2, 1, -5, 4 }; //6
		int[] c={ 3, 2, -6, 4, 0 }; //7
		System.out.println(solution(a));
		System.out.println(solution(b));
		System.out.println(solution(c));
	}
}
```
