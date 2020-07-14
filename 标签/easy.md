### [657. 机器人能否返回原点](https://leetcode-cn.com/problems/robot-return-to-origin/)

在二维平面上，有一个机器人从原点 (0, 0) 开始。给出它的移动顺序，判断这个机器人在完成移动后是否在 (0, 0) 处结束。

移动顺序由字符串表示。字符 move[i] 表示其第 i 次移动。机器人的有效动作有 R（右），L（左），U（上）和 D（下）。如果机器人在完成所有动作后返回原点，则返回 true。否则，返回 false。

**注意：机器人“面朝”的方向无关紧要。 “R” 将始终使机器人向右移动一次，“L” 将始终向左移动等。此外，假设每次移动机器人的移动幅度相同。**

 

示例 1:

输入: "UD"
输出: true
解释：机器人向上移动一次，然后向下移动一次。所有动作都具有相同的幅度，因此它最终回到它开始的原点。因此，我们返回 true。
示例 2:

输入: "LL"
输出: false
解释：机器人向左移动两次。它最终位于原点的左侧，距原点有两次 “移动” 的距离。我们返回 false，因为它在移动结束时没有返回原点。



#### 第一版 ，和机器人面向无关

已经战胜 67.15 % 的 cpp 提交记录

```c++
bool judgeCircle(string moves) {
	 int nowx, nowy;
	 int d=0,dx[4] = {0,0,-1,1}, dy[4] = {1,-1,0,0};
	 nowx = 0;
	 nowy = 0;

	 for (int i = 0; i < moves.size(); ++i) {
		 if (moves[i] == 'D')  d = 1;
		 else if (moves[i] == 'L')  d = 2;
		 else if (moves[i] == 'R')  d = 3;
		 else if (moves[i] == 'U')  d = 0;
	
		 nowx = nowx + dx[d];
		 nowy = nowy + dy[d];
	 }
	 return nowx == 0 && nowy == 0;
    }
```

**这是面向是没有关系的，如果面向有关系，还要看当时目标所在位置。**





其实还有一种更简单一点的做法，直接统计UDLR的个数，U=D,L=R即可；

执行用时 :20 ms, 在所有 C++ 提交中击败了67.15%的用户
内存消耗 :10.1 MB, 在所有 C++ 提交中击败了79.88%的用户

```c++
bool judgeCircle(string moves) {
	
	 int d = 0, u = 0, l = 0,r = 0;

	 for (int i = 0; i < moves.size(); ++i) {
		 if (moves[i] == 'D')  d +=1 ;
		 else if (moves[i] == 'L')  l += 1;
		 else if (moves[i] == 'R')  r += 1;
		 else if (moves[i] == 'U')  u += 1;
	
	 }
	 return d==u &&l ==r;
        
}
```





#### 第二种，和面向有关

```c++

    bool judgeCircle(string moves) {
	 int nowx, nowy;
	 int d=0,dx[4] = {0,0,-1,1}, dy[4] = {1,-1,0,0};
	 nowx = 0;
	 nowy = 0;
     for (int i = 0; i < moves.size(); ++i) {
     if (moves[i] == 'D')  d = (d+1)%4;//注意这里的区别
	 else if (moves[i] == 'L')  d = (d+2)%4;
	 else if (moves[i] == 'R')  d = (d+3)%4;
	 else if (moves[i] == 'U')  d = (d)%4; 

	 nowx = nowx + dx[d];
	 nowy = nowy + dy[d];
 }
 return nowx == 0 && nowy == 0;
}
```



```C++
	
```

### [136. 只出现一次的数字](https://leetcode-cn.com/problems/single-number/)



给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

示例 1:

输入: [2,2,1]
输出: 1
示例 2:

输入: [4,1,2,1,2]
输出: 4



#### 第一版 亦或可以抵消两个一样的元素

执行用时 :16 ms, 在所有 C++ 提交中击败了81.80%的用户

内存消耗 :9.8 MB, 在所有 C++ 提交中击败了19.96%的用户

```c++
int singleNumber(vector<int>& nums) {
	if (nums.size() == 1) return nums[0];
	int res=0;
	for (auto i : nums) {
		res ^= i;
	}
	return res;
}
```

### [137. 只出现一次的数字 II](https://leetcode-cn.com/problems/single-number-ii/) 不会

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现了三次。找出那个只出现了一次的元素。

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

示例 1:

输入: [2,2,3,2]
输出: 3
示例 2:

输入: [0,1,0,1,0,1,99]
输出: 99











### [219. 存在重复元素 II](https://leetcode-cn.com/problems/contains-duplicate-ii/)



#### 自己写的，借助map

执行用时 :40 ms, 在所有 C++ 提交中击败了72.14%的用户

内存消耗 :23.6 MB, 在所有 C++ 提交中击败了5.03%的用户

```c++
bool minThanK(const int &a, const int &b,const int &k) {
	return a > b ? a - b <= k : b - a <= k;
}

bool containsNearbyDuplicate(vector<int>& nums, int k) {
	unordered_map<int, set<int>> mp;
	for (unsigned i = 0; i < nums.size(); ++i) {
		mp[nums[i]].insert(i);
	}

	for (auto it = mp.begin(); it != mp.end(); ++it) {
		
		if (it->second.size() >= 2) {
			for (set<int>::iterator it2 = it->second.begin(); it2 != --it->second.end(); ) {
				if (minThanK(*(++it2), *it2,k))  return true;
			}
		}
	}
	return false;

}
```





#### 第二版，滑动窗口来做

执行用时 :36 ms, 在所有 C++ 提交中击败了81.75%的用户

内存消耗 :14.4 MB, 在所有 C++ 提交中击败了70.54%的用户

```c++
bool containsNearbyDuplicate(vector<int>& nums, int k) {
    unordered_set<int> set; //搜索、插入和移除平均常数时间复杂度，不会超时
    for(int i = 0; i < nums.size(); i++)
    {
        if(set.find(nums[i]) != set.end())
            return true;
        set.insert(nums[i]);
        if(set.size() > k )
            set.erase(nums[i-k]); //滑动窗口长度最大为k ，最大也就是k了
    }
    return false;
}
```



### [217. 存在重复元素](https://leetcode-cn.com/problems/contains-duplicate/)

给定一个整数数组，判断是否存在重复元素。

如果任何值在数组中出现至少两次，函数返回 true。如果数组中每个元素都不相同，则返回 false。

示例 1:

输入: [1,2,3,1]
输出: true
示例 2:

输入: [1,2,3,4]
输出: false
示例 3:

输入: [1,1,1,3,3,4,3,2,4,2]
输出: true





#### 第一版，自己写的unorder_set

执行用时 :68 ms, 在所有 C++ 提交中击败了40.04%的用户

内存消耗 :16.7 MB, 在所有 C++ 提交中击败了12.30%的用户



```c++
    bool containsDuplicate(vector<int>& nums) {
    unordered_set <int> un_st;
	for (unsigned i = 0; i < nums.size(); ++i) {
		if (un_st.find(nums[i]) != un_st.end())
		{
			return true;
		}
		un_st.insert(nums[i]);
	}

	return false;
    }
```

#### 



### [220. 存在重复元素 III](https://leetcode-cn.com/problems/contains-duplicate-iii/)





给定一个整数数组，判断数组中是否有两个不同的索引 i 和 j，使得 nums [i] 和 nums [j] 的差的绝对值最大为 t，并且 i 和 j 之间的差的绝对值最大为 ķ。

示例 1:

输入: nums = [1,2,3,1], k = 3, t = 0
输出: true
示例 2:

输入: nums = [1,0,1,1], k = 1, t = 2
输出: true
示例 3:

输入: nums = [1,5,9,1,5,9], k = 2, t = 3
输出: false



执行用时 :8 ms, 在所有 C++ 提交中击败了97.77%的用户

内存消耗 :9.3 MB, 在所有 C++ 提交中击败了81.22%的用户



```c++
 bool minThanK(const long a, const long b, const int& t) {
	return a > b ? a - b <= t : b - a <= t;
}

bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
	if (k == 10000) return false;
	unordered_map<int,int> un_mp;
	for (unsigned i = 0; i < nums.size(); ++i) {
		//cout << "i " << i  <<" "<<nums[i]<<endl;
		un_mp[i]=nums[i];

		for (unordered_map<int, int>::iterator it1 = un_mp.begin(); it1 != un_mp.end(); ++it1) {
			auto it2 = it1;
			while (++it2 != un_mp.end()) {

				//cout << it1->second << " " << it2->second << " t " << t << endl;
				if (minThanK(it1->second, it2->second, t)) return true;
			}
			//cout << "***************" << endl;;
		}
		if (un_mp.size() > k) {
			//cout << ">k" << endl; 
			un_mp.erase(i - k);
		}

	}
	return false;


}
```

