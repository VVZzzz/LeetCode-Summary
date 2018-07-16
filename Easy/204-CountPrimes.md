# 204 Count Primes-质数计数
--------------------------------------------
Count the number of prime numbers less than a non-negative number, ```n```.

## 示例：

	Input: 10 
	output: 4 
	Primes: 2,3,5,7 


## 思路：
该题目是计算小于n的所有数中的所有质数的数目。要记住的是，0，1不算质数。 
```质数是一个只能被1和自己整除的数```     
所以朴素思路为：遍历2-n的每一个数字，检查它是否为一个质数。检查它是否为质数时，遍历1到sqrt(k)而不是1到k，原因很简单是因为k=sqrt(k)*sqrt(k)。即只用遍历一侧的因数即可。     
故： 
## 朴素思路代码:
	int countPrimes(int n) {
        int res=0;bool isPrime=true;
        for(int i=2;i<n;i++){
            isPrime=true;
            int temp=sqrt(i);  
            for(int j=2;j<=temp;j++){  //此处注意不要写j<=sqrt(i)，否则会每次循环都会计算sqrt。耗时
                if(i%j==0){
                    isPrime=false;
                    break;
                }
            }
            if (isPrime)
                res++;
        }
        return res;
    } 


而我们换一个角度思考，我们不去找那些**质数**，而是将**合数除去**也可达到同样效果。      
对于合数，无非是有除1和他本身的其他因数。所以假设找到1到n的合数，每个合数的因数（除1和他本身）都是另外几个质数。所以假设k是1到n的一个质数，那么k * 2，k * 3......k * m 只要小于n都是合数，对吧。    
故： 
## 筛选出质数代码(Java)：
     public int countPrimes(int n) {
        boolean[] notPrime = new boolean[n];
        int count = 0;
        for (int i = 2; i < n; i++) {
            if (notPrime[i] == false) {
                count++;
                for (int j = 2; i*j < n; j++) {
                    notPrime[i*j] = true;
                }
            }
        }
        
        return count;
    }

实际上这就是著名的Eratosthenes筛选法。[维基百科](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)    
时间效率为：O(n log logn)     


  
而对于该筛选法，在内层for循环中，我们会发现i*j这个会导致重复的标记。比如i=2的时候j为 2,3,4....而到了i=4的时候 又有2,3,4....这样的话2×4 4×2都会重复标记。    
故：
## 改进的Eratosthenes筛选算法:
[Click Here!](leetcode.com/problems/count-primes/discuss/57593/12-ms-Java-solution-modified-from-the-hint-method-beats-99.95/59090)

	public class Solution {
	    
	    /**
	     * Count the number of prime numbers less than a non-negative number, n
	     * @param n a non-negative integer
	     * @return the number of primes less than n
	     */
	    public int countPrimes(int n) {
	        
	        /**
	         * if n = 2, the prime 2 is not less than n,
	         * so there are no primes less than n
	         */
	        if (n < 3) return 0;
	        
	        /** 
	         * Start with the assumption that half the numbers below n are
	         * prime candidates, since we know that half of them are even,
	         * and so _in general_ aren't prime.
	         * 
	         * An exception to this is 2, which is the only even prime.
	         * But also 1 is an odd which isn't prime.
	         * These two exceptions (a prime even and a for-sure not-prime odd)
	         * cancel each other out for n > 2, so our assumption holds.
	         * 
	         * We'll decrement count when we find an odd which isn't prime.
	         *
	         * If n = 3,  c = 1.
	         * If n = 5,  c = 2.
	         * If n = 10, c = 5.
	         */
	        int c = n / 2;
	        
	        /**
	         * Java initializes boolean arrays to {false}.
	         * In this method, we'll use truth to mark _composite_ numbers.
	         * 
	         * This is the opposite of most Sieve of Eratosthenes methods,
	         * which use truth to mark _prime_ numbers.
	         * 
	         * We will _NOT_ mark evens as composite, even though they are.
	         * This is because `c` is current after each `i` iteration below.
	         */
	        boolean[] s = new boolean[n];
	        
	        /**
	         * Starting with an odd prime-candidate above 2, increment by two
	         * to skip evens (which we know are not prime candidates).
	         */
	        for (int i = 3; i * i < n; i += 2) {
	            if (s[i]) {
	                // c has already been decremented for this composite odd
	                continue;
	            }
	            
	            /** 
	             * For each prime i, iterate through the odd composites
	             * we know we can form from i, and mark them as composite
	             * if not already marked.
	             * 
	             * We know that i * i is composite.
	             * We also know that i * i + i is composite, since they share
	             * a common factor of i.
	             * Thus, we also know that i * i + a*i is composite for all real a,
	             * since they share a common factor of i.
	             * 
	             * Note, though, that i * i + i _must_ be composite for an
	             * independent reason: it must be even.
	             * (all i are odd, thus all i*i are odd,
	             * thus all (odd + odd) are even).
	             * 
	             * Recall that, by initializing c to n/2, we already accounted for
	             * all of the evens less than n being composite, and so marking
	             * i * i + (odd)*i as composite is needless bookkeeping.
	             * 
	             * So, we can skip checking i * i + a*i for all odd a,
	             * and just increment j by even multiples of i,
	             * since all (odd + even) are odd.
	             */
	            for (int j = i * i; j < n; j += 2 * i) {
	                if (!s[j]) {
	                    c--;
	                    s[j] = true;
	                    }
	                }
	            }
	        return c;
	    }
	}  

## O(n)时间复杂度的Eratosthenes筛选法
除此之外，还有时间复杂度为O(n)的对于Eratosthenes筛选法的改进。[Click Here!](https://www.geeksforgeeks.org/sieve-eratosthenes-0n-time-complexity/)等有时间再继续分析！       

## 欧拉筛选法
[Click Here!](https://blog.csdn.net/u012313335/article/details/47663801)

