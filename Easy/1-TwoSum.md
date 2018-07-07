# 1.两数之和
-------------------
给定一个整数数组和一个目标值，找出数组中和为目标值的两个数。

你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用。

## 示例： 

``` 	
给定 nums = [2, 7, 11, 15], target = 9	
``` 

```
因为 nums[0] + nums[1] = 2 + 7 = 9 	
所以返回 [0, 1]	
```
## 思路：
遇到此类相似问题：在数组找相加为特定值的两个数时，主要使用`unordered_map<int,int>`即哈希表。 
 
`for`循环遍历整个数组，对于hash中没有找到对应的另一半元素的，将它val和key添加到hash中去。继续遍历，如果在hash中发现了所需要的另一半，就返回结果。

## 代码： 


    
    vector<int> twoSum(vector<int>& numbers, int target)
    {
	unordered_map<int, int> hash;
	vector<int> result;
	for (int i = 0; i < numbers.size(); i++) {
		int numberToFind = target - numbers[i];

		if (hash.find(numberToFind) != hash.end()) {
                    //+1 because indices are NOT zero based
			result.push_back(hash[numberToFind]);
			result.push_back(i);			
			return result;
		}

		hash[numbers[i]] = i;
	}
	return result;
    } 

## 时间代价分析：
O(n)
