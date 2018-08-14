# 278 First Bad Version
--------------------------------------------
你是产品经理，目前正在带领一个团队开发新的产品。不幸的是，你的产品的最新版本没有通过质量检测。由于每个版本都是基于之前的版本开发的，所以错误的版本之后的所有版本都是错的。

假设你有 ```n``` 个版本 ```[1, 2, ..., n]```，你想找出导致之后所有版本出错的第一个错误的版本。

你可以通过调用 ```bool isBadVersion(version)``` 接口来判断版本号 ```version``` 是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。

## 示例：

	给定 n = 5，并且 version = 4 是第一个错误的版本。

	调用 isBadVersion(3) -> false
	调用 isBadVersion(5) -> true
	调用 isBadVersion(4) -> true
	
	所以，4 是第一个错误的版本。  


## 思路：
简单的查找问题，比如√√√√××××，那么FirstBadVersion就是5.   
很容易想到二分查找，所以```最朴素```的算法为：

	int firstBadVersion(int n) {
        int left=1,right=n;
        while(left<right){
            int mid=(right+left)/2;
            if(!isBadVersion(mid)){
                if(isBadVersion(mid+1))
                    return mid+1;
                left=mid+1;
                continue;
            }
            else{
                if(!isBadVersion(mid-1))
                    return mid;
                right=mid-1;
                continue;
            }
        }
          return 1;
    }

## 分析：
- 对于上述朴素代码，会出现```Limited time exceed```，超时。  
原因在于如果```left```和```right```都是很大的数，相加超出```int```范围，再做除法，这样就会导致超时。  
**所以用一个技巧**```mid=left+(right-left)/2```
- 每一次循环都会调用两次```isBadVersion()```，想办法减少使得每次循环只调用一次。故改进后的代码：
  
		int firstBadVersion(int n) {
	        int lower = 1, upper = n, mid;
	        while(lower < upper) {
	            mid = lower + (upper - lower) / 2;
	            if(!isBadVersion(mid)) lower = mid + 1;   /* Only one call to API */
	            else upper = mid;
	        }
	        return lower;   /* Because there will alway be a bad version, return lower here */
	    }


