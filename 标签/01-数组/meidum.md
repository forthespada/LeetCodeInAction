#### [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器，且 n 的值至少为 2。



图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

 

示例:

输入: [1,8,6,2,5,4,8,3,7]
输出: 49

##### 第一版，速度太慢

```c++
    int maxArea(vector<int>& height) {
    int len=height.size();
	int mostWater = 0;
	for (int i = 0,row,col; i < len; ++i)
	{
		for (int j = i+1; j < len; ++j) 
		{
			row = j - i;
			col = min(height[i], height[j]);
			mostWater = row * col > mostWater ? row * col : mostWater;
		}
	}
	return mostWater;  
    }
```



执行用时 :1640 ms, 在所有 C++ 提交中击败了9.51%的用户

内存消耗 :9.9 MB, 在所有 C++ 提交中击败了65.57%的用户



##### 第二版 双指针，很快

```c++
    int maxArea(vector<int>& height) {
	int high=height.size()-1,low=0;
	int mostWater = 0,temp;
	while (low < high)
	{
		temp = (high - low) * min(height[low], height[high]);
		mostWater = mostWater > temp ? mostWater : temp;
		if (height[low] <= height[high]) low++;
		else high--;
	}
	return mostWater;
    }
```

执行用时 :16 ms, 在所有 C++ 提交中击败了97.32%的用户

内存消耗 :9.8 MB, 在所有 C++ 提交中击败了74.86%的用户



##### 官方题解：

https://leetcode-cn.com/problems/container-with-most-water/solution/sheng-zui-duo-shui-de-rong-qi-by-leetcode/

**最初我们考虑由最外围两条线段构成的区域。现在，为了使面积最大化，我们需要考虑更长的两条线段之间的区域。如果我们试图将指向较长线段的指针向内侧移动，矩形区域的面积将受限于较短的线段而不会获得任何增加。但是，在同样的条件下，移动指向较短线段的指针尽管造成了矩形宽度的减小，但却可能会有助于面积的增大。因为移动较短线段的指针会得到一条相对较长的线段，这可以克服由宽度减小而引起的面积减小。**





##### 比较经典的介绍

思路：
算法流程： 设置双指针 ii,jj 分别位于容器壁两端，根据规则移动指针（后续说明），并且更新面积最大值 res，直到 i == j 时返回 res。

指针移动规则与证明： 每次选定围成水槽两板高度 h[i]h[i],h[j]h[j] 中的短板，向中间收窄 11 格。以下证明：

设每一状态下水槽面积为 S(i, j)S(i,j),(0 <= i < j < n)(0<=i<j<n)，由于水槽的实际高度由两板中的短板决定，则可得面积公式 S(i, j) = min(h[i], h[j]) × (j - i)S(i,j)=min(h[i],h[j])×(j−i)。
在每一个状态下，无论长板或短板收窄 11 格，都会导致水槽 底边宽度 -1−1：
若向内移动短板，水槽的短板 min(h[i], h[j])min(h[i],h[j]) 可能变大，因此水槽面积 S(i, j)S(i,j) 可能增大。
若向内移动长板，水槽的短板 min(h[i], h[j])min(h[i],h[j]) 不变或变小，下个水槽的面积一定小于当前水槽面积。
因此，向内收窄短板可以获取面积最大值。换个角度理解：
若不指定移动规则，所有移动出现的 S(i, j)S(i,j) 的状态数为 C(n, 2)C(n,2)，即暴力枚举出所有状态。
在状态 S(i, j)S(i,j) 下向内移动短板至 S(i + 1, j)S(i+1,j)（假设 h[i] < h[j]h[i]<h[j] ），则相当于消去了 {S(i, j - 1), S(i, j - 2), ... , S(i, i + 1)}S(i,j−1),S(i,j−2),...,S(i,i+1) 状态集合。而所有消去状态的面积一定 <= S(i, j)<=S(i,j)：
短板高度：相比 S(i, j)S(i,j) 相同或更短（<= h[i]<=h[i]）；
底边宽度：相比 S(i, j)S(i,j) 更短。
因此所有消去的状态的面积都 < S(i, j)<S(i,j)。通俗的讲，我们每次向内移动短板，所有的消去状态都不会导致丢失面积最大值 。
复杂度分析：

时间复杂度 O(N)O(N)，双指针遍历一次底边宽度 NN 。
空间复杂度 O(1)O(1)，指针使用常数额外空间。

![1569632554576](C:\Users\forth\AppData\Roaming\Typora\typora-user-images\1569632554576.png)







- 

#### [1497. 检查数组对是否可以被 k 整除](https://leetcode-cn.com/problems/check-if-array-pairs-are-divisible-by-k/)



给你一个整数数组 `arr` 和一个整数 `k` ，其中数组长度是偶数，值为 `n` 。

现在需要把数组恰好分成 `n / 2` 对，以使每对数字的和都能够被 `k` 整除。

如果存在这样的分法，请返回 *True* ；否则，返回 *False* 。

 

**示例 1：**

```
输入：arr = [1,2,3,4,5,10,6,7,8,9], k = 5
输出：true
解释：划分后的数字对为 (1,9),(2,8),(3,7),(4,6) 以及 (5,10) 。
```

**示例 2：**

```
输入：arr = [1,2,3,4,5,6], k = 7
输出：true
解释：划分后的数字对为 (1,6),(2,5) 以及 (3,4) 。
```

**示例 3：**

```
输入：arr = [1,2,3,4,5,6], k = 10
输出：false
解释：无法在将数组中的数字分为三对的同时满足每对数字和能够被 10 整除的条件。
```

**示例 4：**

```
输入：arr = [-10,10], k = 2
输出：true
```

**示例 5：**

```
输入：arr = [-1,1,-2,2,-3,3,-4,4], k = 3
输出：true
```

 

**提示：**

- `arr.length == n`
- `1 <= n <= 10^5`
- `n` 为偶数
- `-10^9 <= arr[i] <= 10^9`
- `1 <= k <= 10^5`



##### 1，贪心 + 取余做法

思路:错误想法,找到两个之和整除,这样可能找不全
正确思路:对数组每个数求余,负数就得+k,然后统计每个余数的数量
从0到k-1, 1的数量要和k-1的数量相同才能,2和k-2等等
0的数量要是2的倍数



执行用时：264 ms, 在所有 C++ 提交中击败了84.21%的用户

内存消耗：61.5 MB, 在所有 C++ 提交中击败了100.00%的用户

~~~cpp
    bool canArrange(vector<int>& arr, int k) {

        vector<int> res(k,0);
        for(auto a:arr){
            res[(a%k + k)%k]++;//可能会有负数，因此要加上 k
        }
        if(res[0]%2!=0) return false;//为0的必须是 2的倍数，比如 0 + 7 或者 7 + 14 这样的
        for(int i=1;i<=k/2;++i){
            if(res[i]!=res[k-i]) return false;
        }
        return true;

    }
~~~



