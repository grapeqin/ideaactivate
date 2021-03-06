  
# LeetCode 1 : 两数之和  
  
## [题目描述](https://leetcode-cn.com/problems/two-sum/)  
  
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标
  
**示例1：**  
  
 nums = [2, 7, 11, 15], target = 9  
  
返回： [0,1]   

**示例2：**  
  
 nums = [2, 11, 7, 2], target = 4
  
返回： [0,3] 

  
## 题目分析  
  
### 1. 思路  
  
*  我们假设目标数组为a, 存在 a[i]+a[j] = 目标值t， 对于任意的a[i]，只要从a中找到存在值为a[j]=t-a[i]的元素并返回[i,j]即为结果。

* 源码  
```java  
/**  
 * 给定数组s和目标值t，返回数组中两数之和等于t的下标 
 * @param s  
 * @param t  
 * @return  
 */  
public int[] twoSum(int[] s,int t){  
	Map<Integer,Integer> tMap = new HashMap<>();  
	
	for(int i=0;i<s.length;i++){  
		if(tMap.containsKey(t-s[i])){  
			return new int[]{tMap.get(t - s[i]),i};  
		}  
		tMap.put(s[i],i);  
	}  
	
	throw new RuntimeException("数组s中不存在两数之和等于t的元素!");  
} 
```