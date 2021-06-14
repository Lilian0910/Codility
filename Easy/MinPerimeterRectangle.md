## Link:
https://app.codility.com/programmers/lessons/10-prime_and_composite_numbers/min_perimeter_rectangle/
## Question:
An integer N is given, representing the area of some rectangle.

The area of a rectangle whose sides are of length A and B is A * B, and the perimeter is 2 * (A + B).

The goal is to find the minimal perimeter of any rectangle whose area equals N. The sides of this rectangle should be only integers.

For example, given integer N = 30, rectangles of area 30 are:

(1, 30), with a perimeter of 62,
(2, 15), with a perimeter of 34,
(3, 10), with a perimeter of 26,
(5, 6), with a perimeter of 22.
Write a function:

`class Solution { public int solution(int N); }`

that, given an integer N, returns the minimal perimeter of any rectangle whose area is exactly equal to N.

For example, given an integer N = 30, the function should return 22, as explained above.

Write an efficient algorithm for the following assumptions:

* N is an integer within the range [1..1,000,000,000].

## Solution:
*  check from 1 to "sqrt_of_N"
*  if i is the dividor, get its pair and perimeter and compare with min perimeter
*  if smaller than minP, let minP=currP

```java
public class MinPerimeterRectangle {
	public static int solution(int N) {
		Integer minP=Integer.MAX_VALUE;
		int  div=0,currP=0;
		for(int i=1; i<=(double)Math.sqrt(N); i++) {
			if(N%i==0) {
				div=N/i;
				currP=2*(i+div);
				if(currP<minP) minP=currP;
			}
		}
		return minP;
	}
	public static void main(String[] args) {
		System.out.println(solution(30));//22
		System.out.println(solution(1));//2
		System.out.println(solution(3));//8
		System.out.println(solution(36));//24
		System.out.println(solution(100));//40	
	}
}
```
