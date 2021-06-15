## Link:
https://app.codility.com/programmers/lessons/10-prime_and_composite_numbers/flags/
## Question:
A non-empty array A consisting of N integers is given.
A peak is an array element which is larger than its neighbours. More precisely, it is an index P such that 0 < P < N − 1 and A[P − 1] < A[P] > A[P + 1].
For example, the following array A:
   * A[0] = 1 &nbsp; &nbsp; 
    **A[1] = 5** &nbsp; &nbsp; 
    A[2] = 3 &nbsp; &nbsp; 
    **A[3] = 4** &nbsp; &nbsp; 
    A[4] = 3 &nbsp; &nbsp; 
    **A[5] = 4** &nbsp; &nbsp; 
    A[6] = 1 &nbsp; &nbsp; 
    A[7] = 2 &nbsp; &nbsp; 
    A[8] = 3 &nbsp; &nbsp; 
    A[9] = 4 &nbsp; &nbsp; 
    **A[10] = 6** &nbsp; &nbsp; 
    A[11] = 2    
has exactly four peaks: elements 1, 3, 5 and 10.

You are going on a trip to a range of mountains whose relative heights are represented by array A, as shown in a figure below. You have to choose how many flags you should take with you. The goal is to set the maximum number of flags on the peaks, according to certain rules.

![image](https://codility-frontend-prod.s3.amazonaws.com/media/task_static/flags/static/images/auto/6f5e8faa3000c0a74157e6e0bc759b8a.png)

Flags can only be set on peaks. What's more, if you take K flags, then the distance between any two flags should be greater than or equal to K. The distance between indices P and Q is the absolute value |P − Q|.

For example, given the mountain range represented by array A, above, with N = 12, if you take:

two flags, you can set them on peaks 1 and 5;
three flags, you can set them on peaks 1, 5 and 10;
four flags, you can set only three flags, on peaks 1, 5 and 10.
You can therefore set a maximum of three flags in this case.

Write a function:

`class Solution { public int solution(int[] A); }`

that, given a non-empty array A of N integers, returns the maximum number of flags that can be set on the peaks of the array.

For example, the following array A:

    A[0] = 1
    A[1] = 5
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
1. **brute force**    
    1) save all peaks in `peak_list`      
    2) try `1` to `size(peal_list)` to put flags
  ```java
  public int solution(int[] A) {
	List<Integer> peaks = getAllPeaks(A);
	int ans = 0;
	for (int i = 1; i < peak.size(); i ++) {
		if (canPutFlags(peaks, i)) ans = i;
		else break;
	}
	return ans;
}
  ```
  Time Complexity=O(n^2)
  
 2. **Optimize**     
    the process need to optimize is the second step, we could use binary search to reduce time complexity
    find mid=peak_size/2:
      1) `peak[mid]` satistified `canputflags`, then all peak[0]-peak[mid] could satisfied
      2) `peak[mid]` not satisfied, then all peak[mid]-peak[peak_size] could not satisfied 
 ```java
 public int solution(int[] A) {
	     List<Integer> peaks = getAllPeaks(A);
	     return binarySearch(peaks, 1, peaks.size());;
  }
  private int binarySearch(peaks) {
        if(canputflags(peaks, mid)) left=mid;
        else right=mid;
  }
  public boolean canPutFlags (peaks, mid) {
        if(peaks[i+1]-peaks[i]>=mid) return true;
        else return false;        
  }
 ```
 3. **O(n) solution**
    1) trans time comlexity to space complexity, use two arrays to store peaks and their next peaks     
    2) since distance should >= flagNum, the array A at least contains (flagNum - 1) * flagNum numbers
    
```java
public class Flags {
	public static int solution(int[] A) {
		boolean[] peaks = buildPeaks(A);
		int[] nextPeaks = buildNextPeaks(peaks);

		int maxFlagNum = 0;
		for (int flagNum = 1; (flagNum - 1) * flagNum < A.length; flagNum++) {
			if (canSetFlags(nextPeaks, flagNum)) {
				maxFlagNum = Math.max(maxFlagNum, flagNum);
			}
		}
		return maxFlagNum;
	}

	static boolean[] buildPeaks(int[] A) {
		boolean[] peaks = new boolean[A.length];
		for (int i = 1; i < A.length - 1; i++) {
			if (A[i] > A[i - 1] && A[i] > A[i + 1]) {
				peaks[i] = true;
			}
		}
		return peaks;
	}

	static int[] buildNextPeaks(boolean[] peaks) {
		int[] nextPeaks = new int[peaks.length];
		int nextPeak = -1;
		for (int i = nextPeaks.length - 1; i >= 0; i--) {
			if (peaks[i]) {
				nextPeak = i;
			}
			nextPeaks[i] = nextPeak;
		}
		return nextPeaks;
	}

	static boolean canSetFlags(int[] nextPeaks, int flagNum) {
		int index = 0;
		for (int i = 0; i < flagNum; i++) {
			if (index >= nextPeaks.length || nextPeaks[index] < 0) {
				return false;
			}
			index = nextPeaks[index] + flagNum;
		}
		return true;
	}

	public static void main(String[] args) {
		int[] a= {1,5,3,4,3,4,1,2,3,4,6,2};
		System.out.println(solution(a));
	}
}
```
