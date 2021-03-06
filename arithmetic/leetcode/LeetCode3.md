
# LeetCode 3：无重复字符的最长子串

## 问题描述

**给你一个字符串，找出其中不含有重复字符的 最长子串 的长度**

**示例1：**

**输入：** "au123"

**输出：** 5

**分析：** 因为无重复字符的最长子串为"au123"，所以其长度为5

**示例2：**

**输入：** ""

**输出：** 0

**分析：** 因为无重复字符的最长子串为""，所以其长度为0

**示例3：**

**输入：** " "

**输出：** 1

**分析：** 因为无重复字符的最长子串为" "，所以其长度为1

**示例4：**

**输入：** "bbbbb"

**输出：** 1

**分析：** 因为无重复字符的最长子串为"b"，所以其长度为1

**示例4：** 

**输入：** "abcabcbb"

**输出：** 3

**分析：** 因为无重复字符的最长子串为"abc"，所以其长度为3

**示例5：** 

**输入：** "pwwkew"

**输出：** 3

**分析：** 因为无重复字符的最长子串为"wke"，所以其长度为3

**示例6：** 

**输入：** "123145"

**输出：** 5

**分析：** 因为无重复字符的最长子串为"23145"，所以其长度为5

## 分析过程及实现

### 1. 常规思路

*  我们假设给定字符串s,对任意字符串下标i(i从0开始计),依次遍历j=i+1后的字符,若发现j位置所在的字符已经存在s[i,j)范围的子串中,则当前以i开头的子串s[i,j)即为符合条件的最大子串,对其长度进行记录；i+1 重复前述步骤。

*  源码演示
```java
public int lengthOfLongestSubstring(String s) {
	if (null == s || s.length() == 0) {
		return 0;
	}
	int max = 1, j;
	String str;
	for (int i = 0; i < s.length(); i++) {
		for (j = i + 1; j < s.length(); j++) {
			if (s.substring(i, j).indexOf(s.charAt(j) + "") >= 0)
				break;
		}
		str = s.substring(i, j);
		if (max < str.length()) {
			max = str.length();
		}
	}
	return max;
}
```

### 2. 滑动窗口思路

* 常规思路虽然能获得正确的结果，但从时间复杂度上还有优化的空间，常规思路时间复杂度为O(n2)。那么有没有一种方案只需要在外层循环一遍就能得到结果呢？使其时间复杂度降到O(n)，为此我们引入滑动窗口的思路。
假设字符串s,字符串双下标分别为i,j（i,j都从0开始计)，引入一个set（用于存放s[i,j)范围内的字符），如果s[j]位置上的字符存在于set中，则左窗口向右滑动并从set中移除s[i]，否则右窗口滑动并在set中放入s[j]，同时更新ans。

* 源码演示
```java
public int lengthOfLongestSubstringWithSlideWindow(String s) {  
	int ans = 0,n = s.length();  
	int i=0,j=0;  
	Set<Character> set = new HashSet<>();  
	while(i < n && j < n){  
		if(set.contains(s.charAt(j))){  
			set.remove(s.charAt(i++));  
		}else{  
			set.add(s.charAt(j++));  
			ans = Math.max(ans,j-i);  
		}  
	}  
	return ans;  
}
```

### 3. 优化后的滑动窗口思路	

* 滑动窗口思路中我们可以看到，左窗口依次向右滑动，但理论表明，对于任意的s[i,j)，下标j上的字符s[j]与字符s[j']重复，则左窗口可以直接移到j'+1，这样最多能减少左窗口上j'-i次无效滑动，进一步提升算法的时间复杂度。
* 源码演示
```java
public int lengthOfLongestSubstringWithOptimizeSlideWindow(String s) {  
	int ans = 0,n = s.length();  
	int i=0,j=0;  
	HashMap<Character,Integer> map = new HashMap<>();  
	while(j<n){  
		if(map.containsKey(s.charAt(j))){  
			i = Math.max(map.get(s.charAt(j))+1,i);  
		}  
		ans = Math.max(ans,j-i+1);  
		map.put(s.charAt(j),j++);  
	}  
	return ans;  
}
```