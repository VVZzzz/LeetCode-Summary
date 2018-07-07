# 7.Reverse Integer
-------------------
Given a 32-bit signed integer, reverse digits of an integer.

## 示例： 

``` 	
	Input: 123 
	Output: 321
``` 

```
	Input: -123
	Output: -321
``` 

``` 
	Input: 120
	Output: 21
```
## 思路：
对于处理int的每一位是（十进制），常用的是%和/操作。 

## 代码：    
 
	class Solution {
	public:
	    int reverse(int x) {
	        long long res = 0;
	        while(x) {
	            res = res*10 + x%10;
	            x /= 10;
	        }
	        return (res<INT_MIN || res>INT_MAX) ? 0 : res;
	    }
	}; 



## 时间代价分析：
O(n)
## 注意：
对于C++中的取余`%`运算符，比如`m%n`.最后结果的符号一定是和`m`一致的。   
因为有`(m/n)*n+(m%n)==m`，并且C++中的取整永远是向0取整。	    
故`(-m)%n==-(m%n)`              
`m%(-n)==m%n`

在最后一行判断时候，也可以用`return (int(res)==res)*res`
此处注意，若res超过了int的范围，int(res)不是返回的int的最大值或者最小值。