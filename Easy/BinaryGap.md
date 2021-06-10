## Link:
https://app.codility.com/programmers/lessons/1-iterations/binary_gap/
## Question:
A binary gap within a positive integer N is any maximal sequence of consecutive zeros that is surrounded by ones at both ends in the binary representation of N.

For example, number 9 has binary representation 1001 and contains a binary gap of length 2. The number 529 has binary representation 1000010001 and contains two binary gaps: one of length 4 and one of length 3. The number 20 has binary representation 10100 and contains one binary gap of length 1. The number 15 has binary representation 1111 and has no binary gaps. The number 32 has binary representation 100000 and has no binary gaps.

Write a function:

`class Solution { public int solution(int N); }`

that, given a positive integer N, returns the length of its longest binary gap. The function should return 0 if N doesn't contain a binary gap.

For example, given N = 1041 the function should return 5, because N has binary representation 10000010001 and so its longest binary gap is of length 5. Given N = 32 the function should return 0, because N has binary representation '100000' and thus no binary gaps.

Write an efficient algorithm for the following assumptions:

* N is an integer within the range [1..2,147,483,647].
* Time Complexity: O(n)

## Solution:
*  trans int to binary-->`Integer.toBinaryString`, which will omit the blank/0 that has no meaning 
*  when 0--> count++; when 1--> 1) count>currMax--> currMax=count; 2) count<=currMax continue; count==0;

```java
public class BinaryGap {
	public static int solution(int N) {
		String binString=Integer.toBinaryString(N);
		int maxGap=0, count=0;
		for(int i=0; i<binString.length(); i++) {
			if(binString.charAt(i)=='0') {
				count++;
				continue;
			}else if(binString.charAt(i)=='1') {
				if(count>maxGap) {
					maxGap=count;	
				}
				count=0;
			}
		}
		return maxGap;
	}
	public static void main(String[] args) {
		System.out.println(solution(561892)); //10001001001011100100
		System.out.println(solution(74901729)); //100011101101110100011100001
		System.out.println(solution(1376796946)); //1010010000100000100000100010010
	}
}
```
