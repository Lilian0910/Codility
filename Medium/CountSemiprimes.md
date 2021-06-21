## Link:
https://app.codility.com/programmers/lessons/11-sieve_of_eratosthenes/count_semiprimes/
## Question:
A prime is a positive integer X that has exactly two distinct divisors: 1 and X. The first few prime integers are 2, 3, 5, 7, 11 and 13.

A semiprime is a natural number that is the product of two (not necessarily distinct) prime numbers. The first few semiprimes are 4, 6, 9, 10, 14, 15, 21, 22, 25, 26.

You are given two non-empty arrays P and Q, each consisting of M integers. These arrays represent queries about the number of semiprimes within specified ranges.

Query K requires you to find the number of semiprimes within the range (P[K], Q[K]), where 1 ≤ P[K] ≤ Q[K] ≤ N.

For example, consider an integer N = 26 and arrays P, Q such that:

    P[0] = 1    Q[0] = 26
    P[1] = 4    Q[1] = 10
    P[2] = 16   Q[2] = 20
The number of semiprimes within each of these ranges is as follows:

(1, 26) is 10,
(4, 10) is 4,
(16, 20) is 0.
Write a function:

class Solution { public int[] solution(int N, int[] P, int[] Q); }

that, given an integer N and two non-empty arrays P and Q consisting of M integers, returns an array consisting of M elements specifying the consecutive answers to all the queries.

For example, given an integer N = 26 and arrays P, Q such that:

    P[0] = 1    Q[0] = 26
    P[1] = 4    Q[1] = 10
    P[2] = 16   Q[2] = 20
the function should return the values [10, 4, 0], as explained above.

Write an efficient algorithm for the following assumptions:
* N is an integer within the range [1..50,000];
* M is an integer within the range [1..30,000];
* each element of arrays P, Q is an integer within the range [1..N]; 
* P[i] ≤ Q
* expected worst-case time complexity is O(N*log(log(N))+M);
## Solution:
1. **brute force**    
    1) list all the primes and semiprimes within N
    2) count the semiprimes
  ```java
public int solution(int N, int[] P, int[] Q) {
        int[] prime=seive(N);
        boolean[] semiprime = semiprime(prime);
        for(int i; i<N.len; i++){ 
              int semiNum =countSemiprimes(P[i], Q[i], semiprime, N);
              int[] res[i]=semiNum;
        }
        return res;
}

public int[] sieve(int N) {
        boolean[] primeArray = new boolean[N+1]; // note: plus one for "0"       
        // initial settting (sieve of Eratosthenes)
        Arrays.fill(primeArray, true); // initial setting: all primes
        primeArray[0] = false;         // not prime 
        primeArray[1] = false;         // not prime
        // sieve of Eratosthenes
        for(int i =1; i < (int)Math.sqrt(N); i++){   
            if(primeArray[i] == true) {// prime
                int j = i + i;
                for(j=j; j<=N; j=j+i){primeArray[j] = false;} // all the multiples of prime are not prime       
            }   
        }  
        
        // store all primes in "List"
        List<Integer> primeList = new ArrayList<>();
        for(int i=2; i<= N; i++){
            if(primeArray[i] == true){primeList.add(i);}    // "i" is prime            
        }
        return primeList;
}

public boolean[] semiprime(int[] prime) {
        boolean[] semiprimeArray = new boolean[N+1];
        Arrays.fill(semiprimeArray, false); // initial setting: all "not" semiprimes
        long semiprimeTemp; // using "long" (be careful)
        // for "prime.size()"
        for(int i=0; i< prime.size(); i++){
            for(int j=i; j< prime.size(); j++){
                semiprimeTemp = (long) prime.get(i) * (long) prime.get(j); // semiprimes
                if(semiprimeTemp > N) break;
                else{ semiprimeArray[(int)semiprimeTemp] = true;} // semiprimes                
            }
        }
        return semiprimeArray;
}

 public int countSemiprimes(int P, int Q, boolean[] semiprime, int N) {
        int count = 0;
        if(P > Q || P > N || Q > N) return 0;
        for(int i=P==1?2:P;i<=Q;i++) {
            if(semiprime[i]) count++;
        }        
        return count;    
    }
 }
  ```
  Time Complexity=O(n^2)
  
 2. **Optimize**     
    the process need to optimize is the second step, we could use `cumulatCount` to find the number of semiprimes of P[i] and Q[i]
    and get the result by subtract `cumulateCount(P[i])` from `cumulateCount(Q[i])` 
 ```java
public class CountSemiprimes {
	public static int[] solution(int N, int[] P, int[] Q) {
		boolean[] primeArray = new boolean[N + 1]; // note: plus one for "0"
		// initial settting (sieve of Eratosthenes)
		Arrays.fill(primeArray, true); // initial setting: all primes
		primeArray[0] = false; // not prime
		primeArray[1] = false; // not prime
		int sqrtN = (int) Math.sqrt(N);
		// sieve of Eratosthenes
		for (int i = 1; i < sqrtN; i++) {
			if (primeArray[i] == true) { // prime
				int j = i + i;
				for (j = j; j <= N; j = j + i) {primeArray[j] = false; }// not prime		
			}
		}
		// store all primes in "List"
		List<Integer> primeList = new ArrayList<>();
		for (int i = 2; i <= N; i++) {
			if (primeArray[i] == true) primeList.add(i); // "i" is prime		
		}

		// find "semiprimes"
		boolean[] semiprimeArray = new boolean[N + 1]; 
		Arrays.fill(semiprimeArray, false); 
		long semiprimeTemp; // using "long" (be careful)
		// for "primeList.size()"
		for (int i = 0; i < primeList.size(); i++) {
			for (int j = i; j < primeList.size(); j++) {
				semiprimeTemp = (long) primeList.get(i) * (long) primeList.get(j); // semiprimes
				if (semiprimeTemp > N) break;
				else semiprimeArray[(int) semiprimeTemp] = true; // semiprimes			
			}
		}

		// compute "cumulative Count of semiprimes"
		int[] cumuCount = new int[N + 1]; // note: plus one for "0"
		for (int i = 1; i <= N; i++) {
			cumuCount[i] = cumuCount[i - 1]; // cumulative
			if (semiprimeArray[i] == true) cumuCount[i]++; // semiprimes	
		}

		// compute "results" (for each query)
		int numQuery = Q.length;
		int[] result = new int[numQuery];
		for (int i = 0; i < numQuery; i++) {
			result[i] = cumuCount[Q[i]] - cumuCount[P[i] - 1]; // note: "P[i]-1" (not included)
		}
		return result;
	}

	public static void main(String[] args) {
		int[] res = solution(26, new int[] { 1, 4, 16 }, new int[] { 26, 10, 20 });
		for (int e : res) System.out.println(e);
	}
}

  
 ```
