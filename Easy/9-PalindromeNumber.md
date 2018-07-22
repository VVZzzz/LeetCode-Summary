# 9.Palindrome Number  
# 回文数
----------------------------------------
判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。
例子：  

	输入: 121   
	输出: true

	输入: -121
	输出: false
	解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

	输入: 10
	输出: false
	解释: 从右向左读, 为 01 。因此它不是一个回文数。

### 思路：  
此题和```第7题 翻转数```很相似，同样主要用 ```对于处理int的每一位是（十进制），常用的是%和/操作。```   
### 常规解法： 
	bool isPalindrome(int x) {
	        int res=0,temp=x;
	        while(x>0){
	            res=res*10+x%10;
	            x/=10;
	        }
	        return res==temp;
	} 


### 进阶思路：
对于判断一个数翻转后还是不是原来的，我们不必将整个数翻转再和原来的数比较。可以只比较一半。比如：   
	origin:12321
	res:0
	
	origin:1232
	res:1
	
	origin:123
	res:12
	
	origin:12
	res:123
	此时比较res/10==origin即可判断是否为回文数。

若原数字为偶数位，就比较res==origin即可。

### 进阶代码：
	bool isPalindrome(int x) {
	    if(x<0|| (x!=0 &&x%10==0)) return false;
	    int sum=0;
	    while(x>sum)
	    {
	        sum = sum*10+x%10;
	        x = x/10;
	    }
	    return (x==sum)||(x==sum/10);
	}
