

#### 17、整值的整数次方

要分正负的

~~~C++
    double Power(double base, int exponent) {
    
        if(exponent==0)  return 1;
        double dtemp = 0;
        if(dtemp==base) return dtemp;
        dtemp =1;
        for(int i=0;i<abs(exponent);++i){
        dtemp *=base;
        }
        if(exponent>0)
          return dtemp;
        else
          return 1.0/dtemp;
    }
~~~





#### 

#### 21、反转单词序列

##### 1、别想太多，能做出来就好

~~~C++
    string ReverseSentence(string str) {
	string res = "", tmp = "";
	for (unsigned int i = 0; i < str.size(); ++i) {
		if (str[i] == ' ')
		{
			res = " " + tmp + res;
			tmp = "";
		}
		else tmp += str[i];
	}
	if (tmp.size()) 
		res = tmp + res;
	return res;
    }
~~~

##### 2、借助栈 反而会出错，直接第一种方法就可以



#### 22、扑克牌顺子

##### 1、比较容易想到的一种方法

1、排序 

2、计算所有相邻数字间隔总数 

3、计算0的个数 

4、如果2、3相等，就是顺子 

5、如果出现对子，则不是顺子

~~~C++
    bool IsContinuous( vector<int> numbers ) {
        int len = numbers.size();
        if(len<5) return false;
        sort(numbers.begin(),numbers.end());
        int numOfZreo = 0,numOfInner=0;
        for(int i=0;i<len-1;++i){
            if(numbers[i]==0)  ++numOfZreo;
            else if(numbers[i]==numbers[i+1]){
                return false;
            }
            else{
                numOfInner += numbers[i+1] - numbers[i] -1;//这里千万注意要减去1
            }
            //cout<<numOfZreo<<" "<<numOfInner<<endl;
        }
        if(numOfZreo>=numOfInner) return true;
        return false;
    }
~~~

##### 2、第二种方法

max 记录 最大值
 min 记录  最小值
 min ,max 都不记0
 满足条件 1 max - min   <5
                2 除0外没有重复的数字(牌)
                3 数组长度 为5  

~~~C++
 bool IsContinuous( vector<int> numbers ) {
	int maxNum = -1, minNum = 14;
	if (numbers.size() < 5)//小于5则为false
		return false;
	vector<int> result(14, 0);
	result[0] = -5;
	for (int i = 0; i < numbers.size(); ++i)
	{  
		result[numbers[i]]++;
		if (numbers[i] == 0)//出现0则跳过
			continue;
		if (result[numbers[i]] > 1) return false;
		if (numbers[i] > maxNum)
			maxNum = numbers[i];//取最大数
		if (numbers[i] < minNum)
			minNum = numbers[i];//取最小数
	}
	if (maxNum - minNum < 5)
		return true;//判断是否小于5
	return false;
    }
~~~





下面的代码有问题，无法判断是否有重复的数字，比如1,2,4,5,4就无法判断

~~~C++
    bool IsContinuous( vector<int> numbers ) {
	int maxNum = -1, minNum = 14;
	if (numbers.size() < 5)//小于5则为false
		return false;
	for (int i = 0; i < numbers.size(); i++)
	{   //判断是是否小于0和大于13以及有没有重复数字
		if (numbers[i] < 0 || numbers[i]>13 || numbers[i] == maxNum || numbers[i] == minNum)
			return false;
		if (numbers[i] == 0)//出现0则跳过
			continue;
		if (numbers[i] > maxNum)
			maxNum = numbers[i];//取最大数
		if (numbers[i] < minNum)
			minNum = numbers[i];//取最小数
	}
	if (maxNum - minNum < 5)
		return true;//判断是否小于5
	return false;
    }
~~~

#### 23、求1+2+3+...+N

##### 1、他妈的，我服了

~~~C++
   int Sum_Solution(int n) {
     bool a[n][n+1];
     return sizeof(a)>>1;
    }
~~~

因为bool类型的为1个字节，或者换为char的也行，他们都是一个字节，如果是short(2),int(4)就不行了

##### 2、这个方法真的很妙

解题思路：
1.需利用逻辑与的短路特性实现递归终止。 
2.当n == 0时，(n > 0) && ((sum += Sum_Solution(n - 1)) > 0)只执行前面的判断，为false，然后直接返回0；
3.当n > 0时，执行sum += Sum_Solution(n - 1)，实现递归计算Sum_Solution(n)。

~~~C++
    int Sum_Solution(int n) {
	int sumNum = n;
	bool ans = (n > 0) && ((sumNum += Sum_Solution(n - 1)) > 0);
	return sumNum;
    }
~~~

#### 24、求两个数相加

##### 1、这种解法真的太厉害了..

~~~C++
    int Add(int num1, int num2)
    {
        while( num2!=0 ){
        int sum = num1 ^ num2;
        int carray = (num1 & num2) << 1;
        num1 = sum;
        num2 = carray;
         } 
    return num1;
    }
~~~

1. **两个数异或**：相当于每一位相加，而不考虑进位；
2. **两个数相与**，并左移一位：相当于求得进位；
3. 将上述两步的结果相加



首先看十进制是如何做的： 5+7=12，三步走 

第一步：相加各位的值，不算进位，得到2。

 第二步：计算进位值，得到10. 如果这一步的进位值为0，那么第一步得到的值就是最终结果。  

第三步：重复上述两步，只是相加的值变成上述两步的得到的结果2和10，得到12。 



 同样我们可以用三步走的方式计算二进制值相加：

5-101，7-111

 第一步：相加各位的值，不算进位，得到010，二进制每位相加就相当于各位做异或操作，101^111。 

 第二步：计算进位值，得到1010，相当于各位做与操作得到101，再向左移一位得到1010，(101&111)<<1。  

第三步重复上述两步， 各位相加 010^1010=1000，进位值为100=(010&1010)<<1。      

继续重复上述两步：1000^100 = 1100，进位值为0，跳出循环，1100为最终结果。   





什么时候进位制为0也就说明两个数相加到了最终点，也就计算结束了





#### 25、字符串转化为整数



将一个字符串转换成一个整数，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0

输入描述:

```
输入一个字符串,包括数字字母符号,可以为空
```

输出描述:

```
如果是合法的数值表达则返回该数字，否则返回0
```

示例1

输入

```
+2147483647
1a33
```

输出

```
2147483647
0
```

##### 1、自己思考的一种笨方法,这题用C++   AC 不了

负数 -1234，正数 +2563的情形 第一个为正负号 要考虑到

第一位为0的也是不是合法的

出现0~9之外的字符也是不合法的

~~~C++
    int StrToInt(string str) {
	long long num = 0;
	if (str.size() == 0) return 0;
	int len = str.size();
	bool isNegative = false,isPositive = false;
	if (str[0] == '-') isNegative=true;
	else if (str[0] == '+') isPositive = true;
	else
		if (str[0]<='0' || str[0]>'9')  return 0;

	int i = 0;
	if (isPositive || isNegative) i = 1;
	for (    ; i <len ; ++i) {
		if (str[i]<'0' || str[i]>'9') return 0;
		else {
			num = num * 10 + str[i] - '0';
		}

	}
	if (isNegative) num = -1 * num;
	if (num <= INT_MAX && num >= INT_MIN) return num;
    return 0;
    }
~~~

只通过85.71%的案例。



##### 2、第二种精简一点的方法

~~~C++
    int StrToInt(string str) {
    int len = str.size();
	if (len == 0) return 0;//为空，直接返回即可
	int i = 0, flag = 1,isSingal = 0;// 索引 正负号标志位  正负号出现次数
	long res = 0; //默认flag = 1，正数
	while (i<len && str[i] == ' ') i++; //若str全为空格，str[i] = '\0'(最后一个i)
	if (i >= len) return 0;//全部都是空格，直接返回吧
	if (i < len && str[i] == '-') { flag = -1; ++i; isSingal++; }
	if (i < len && str[i] == '+') { ++i; ++isSingal; }
	if (isSingal > 1) return 0;
	for (  ; i < len ; ++i) {
		if(str[i]<'0' || str[i] > '9') return 0;
        res = res * 10 + (str[i] - '0');
		if (res >= INT_MAX && flag == 1) return  INT_MAX;
		if (res > INT_MAX && flag == -1) return  INT_MIN;
	}
	return flag * res;
        
    }
~~~



##### 3、有很多要注意的地方

~~~C++
int StrToInt(string str) {
	
	int len = str.size();
	if (len == 0) return 0;
	int  flag=1,singal=0, i = 0;
	long long num = 0;
	while (i < len && str[i] == ' ') i++;//可能一直为空或者前面若干都是 空格，处理空格
	if (i >= len) return 0;//一直为空则返回0
	if (str[i] == '-') { flag = -1; singal++; ++i; }//符号判断，同时千万记得 ++i
	if (str[i] == '+') { singal++; ++i; }//正号判断 ,++ i
	if (singal > 1) return 0;//如果出现两个符号，则是不合法的数字表达了 -+45这样的数字


	for (; i < len; ++i) {
		if (str[i]<'0' || str[i]>'9') return 0;// 是否有不合法数字出现 比如12a454
		else {
			num = num * 10 + str[i] - '0';
			if (num >= INT_MAX && flag==1) return INT_MAX;//注意这里的表达 输入如果是 INT_MAX也就是 2147483647 ，就还好
			if (num > INT_MAX && flag==-1) return INT_MIN;//但是如果输入是 INY_MIN 也就是 -2147483647-1 = -2147483648的话，
															// num因为是正数表达，所以必须num>INT_MAX才能正确判断了
		}

	}
	
	return num*flag;
}
~~~



#### 26、数组中的重复数字

##### 1、用unordered_map保存即可

~~~C++
bool duplicate(int numbers[], int length, int* duplication) {
	unordered_map<int, int> unmp;
	unmp.reserve(length);
	for (int i = 0; i < length; ++i) {
		if (unmp.find(numbers[i]) == unmp.end()) {
			unmp.insert({ numbers[i],1 });
		}
		else
		{
			*duplication = numbers[i];
			return true;
		}
	}
	return false;
}
~~~

##### 2、减少内存，降低内存复杂度

用 vector<char>来存 

~~~C++
    bool duplicate(int numbers[], int length, int* duplication) {
	vector<bool> result(length,false);
	for (int i = 0; i < length; ++i) {
		if (result[numbers[i]] == false) {
			result[numbers[i]] = true;
		}
		else
		{
			duplication[0] = numbers[i];
			return true;
		}
	}
	return false;
    }
~~~

##### 3、不占用任何空间的一种做法

题目里写了数组里数字的范围保证在0 ~ n-1   之间，所以可以利用现有数组设置标志，当一个数字被访问过后，可以设置对应位上的数 +   n，之后再遇到相同的数时，会发现对应位上的数已经大于等于n了，那么直接返回这个数即可。  

~~~C++
bool duplicate(int numbers[], int length, int* duplication) {
	for (int i = 0; i < length; ++i) {
		int index = numbers[i];
		if (index >= length) index -= length;
		if (numbers[index] >= length) {
			duplication[0] = index;
			return true;
		}
		numbers[index] += length;
	}
	return false;
}
~~~

#### 27、表示数值的字符串 很好的题目

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。
例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

~~~C++
    bool isNumeric(char* string)
    {
             // 正反标记符号、小数点、e是否出现过
	bool sign = false, decimal = false, hasE = false;
	for (int i = 0; i < strlen(string); ++i) {
		if (string[i] == 'e' || string[i] == 'E') {
			if (i == strlen(string) - 1) return false; // e后面一定要接数字
			if (hasE) return false;  // 不能同时存在两个e
			hasE = true;
		}
		else if (string[i] == '+' || string[i] == '-') {
			// 第二次出现+-符号，则必须紧接在e之后
			if (sign && string[i - 1] != 'e' && string[i - 1] != 'E') return false;
			// 第一次出现+-符号，且不是在字符串开头，则也必须紧接在e之后
			if (!sign && i > 0 && string[i - 1] != 'e' && string[i - 1] != 'E') return false;
			sign = true;
		}
		else if (string[i] == '.') {
			// e后面不能接小数点，小数点不能出现两次
			if (hasE || decimal) return false;
			decimal = true;
		}
		else if (string[i] < '0' || string[i] > '9') // 不合法字符
			return false;
	}
	return true;
        
    }
~~~



#### 28、字符流中第一个不重复的字符

##### 1、自己想的一种方法

~~~C++
class Solution
{
public:
	//Insert one char from stringstream
	void Insert(char ch)
	{
		v.push_back(ch);
	}
	//return the first appearence once char in current stringstream
	char FirstAppearingOnce()
	{
        if(v.empty())  return '#';
		/*int len = v.size();*/
		for (auto &ch:v) {
			if (count(v.begin(), v.end(), ch) == 1) return ch;
		}
		return '#';
	}

	vector<char> v;
};
~~~

##### 2、借助一个unordered_map

这个方法要慢一些

~~~C++
class Solution
{
public:
	//Insert one char from stringstream
	void Insert(char ch)
	{
		v.push_back(ch);
		unmp[ch]++;
	}
	//return the first appearence once char in current stringstream
	char FirstAppearingOnce()
	{
		for (auto &ch:v) {
			if (unmp[ch] == 1) return ch;
		}
		return '#';
	}

	vector<char> v;
	unordered_map<char, int> unmp;
};
~~~



#### 29、链表中环的入口

##### 1、老办法，借助unordered_map

~~~
ListNode* EntryNodeOfLoop(ListNode* pHead)
{
	if (pHead == nullptr) return NULL;
	unordered_map<ListNode*, int> unmp;//注意是ListNode*，不是ListNode
	while (pHead != NULL) {

		unmp[pHead]++;
		if (unmp[pHead] == 2) return pHead;
		pHead = pHead->next;
	}
	return NULL;
}
~~~

借助se't其实也可以，但是set和map底层其实差不多，而且set里的两个元素类型相同，sizeof（listnode）肯定比 sizeof要大

~~~C++
    ListNode* EntryNodeOfLoop(ListNode* pHead)
    {
     set<ListNode*> s;
        ListNode* node = pHead;
        while(node!=NULL){
            if(s.insert(node).second)
                node = node->next;
            else
                return node;
        }
        return node;
        
    }
~~~





##### 2、有个快慢指针的做法



```
//先说个定理：两个指针一个fast、一个slow同时从一个链表的头部出发
//fast一次走2步，slow一次走一步，如果该链表有环，两个指针必然在环内相遇
//此时只需要把其中的一个指针重新指向链表头部，另一个不变（还在环内），
//这次两个指针一次走一步，相遇的地方就是入口节点。
//这个定理可以自己去网上看看证明。
```

~~~C++
    ListNode* EntryNodeOfLoop(ListNode* pHead)
    {
    ListNode*fast=pHead,*low=pHead;
        while(fast&&fast->next){
            fast=fast->next->next;
            low=low->next;
            if(fast==low)
                break;
        }
        if(!fast||!fast->next)return NULL;
        low=pHead;//low从链表头出发
        while(fast!=low){//fast从相遇点出发
            fast=fast->next;
            low=low->next;
        }
        return low;
        
    }
~~~

#### 30、删除链表中的重复结点，保留一个重复点

在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->3->4->5

~~~C++
    ListNode* deleteDuplication(ListNode* pHead)
    {
	if (pHead == nullptr) return nullptr;
	ListNode* node = (ListNode*)malloc(sizeof(struct ListNode));
	node = pHead;
	while (node != nullptr) {

		if (node->next!=nullptr && node->val == node->next->val) {//这里千万要判断node->next也不为空才可以
			while (node->next != nullptr && node->val == node->next->val) {
				node->next = node->next->next;
			}
		}
		node = node->next;
	}
	return pHead;
    }
~~~

#### 31、删除链表中的重复结点，不保留重复点 很好的题目

在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

##### 1、真的是超级笨，我服了，调试了很多遍才通过的

大概思想：采用vector保存链表中的不重复元素，然后将链表从表头开始挨个对比，一样就将当前结点保存下来，然后index++，不一样就继续向下遍历，注意边界条件。

~~~C++
  ListNode* deleteDuplication(ListNode* pHead)
    {
	if (pHead == nullptr || pHead->next == nullptr) return pHead;
	ListNode* node = (ListNode*)malloc(sizeof(struct ListNode));
	node = pHead;
	vector<int> result;
	result.push_back(node->val);
	node = node->next;
	while (node != nullptr) {
		if (result.size()!=0 && result.back() == node->val) {
			while (node!=nullptr && result.back() == node->val) {
				node = node->next;
			}
			result.pop_back();
		}
		else if (result.size() == 0 || (result.size()!=0 && result.back()!=node->val))
		{
			result.push_back(node->val);
			node = node->next;
		}	
		else
			node = node->next;
	}

	if (result.size() == 0) {
		return nullptr;
	}
	node = pHead;
	int index = 0;
	int len = result.size();
	ListNode* resultNode = (ListNode*)malloc(sizeof(struct ListNode));
	while (node != nullptr) {
		if (index<len && node->val == result[index]) {
			index++;
			resultNode = node;
			break;
			
		} node = node->next;
	}
	pHead = resultNode;
	while (node != nullptr) {
		if (index < len && node->val == result[index]) {
			index++;
			pHead->next = node;
			pHead = pHead->next;

		} node = node->next;
	}
    pHead->next = nullptr;//最后要设置尾点结束
	return resultNode;
    }
~~~



##### 2、别人的思路和方法，三指针法，取到原来指针的前一个指针

1. 首先添加一个头节点，以方便碰到第一个，第二个节点就相同的情况

​    2.设置 pre ，last 指针， pre指针指向当前确定不重复的那个节点，而last指针相当于工作指针，一直往后面搜索。

~~~C++
if (pHead == nullptr || pHead->next == nullptr) { return pHead; }
	ListNode *Head = (ListNode*)malloc(sizeof(struct ListNode));
	ListNode* pre = (ListNode*)malloc(sizeof(struct ListNode));
	ListNode* last = (ListNode*)malloc(sizeof(struct ListNode));
	Head->next = pHead;
	pre = Head; //pre相当于原来节点的前一个节点
	cur = Head->next; //cur相当于 原来的节点
	while (cur != nullptr) {
		if (cur->next != nullptr && cur->val == cur->next->val) {
			// 找到最后的一个相同节点
			while (cur->next != nullptr && cur->val == cur->next->val) {
				cur = cur->next;
			}
			pre->next = cur->next; //这里等于cur->next真的很棒
			cur = cur->next;
		}
		else {
			pre = pre->next;
			cur = cur->next;
		}
	}
	return Head->next;
~~~



#### 32、数据流中的中位数

##### 1、自己的想法与做法

~~~C++
class Solution {
public:
	void Insert(int num)
	{
		result.push_back(num);
	}

	double GetMedian()
	{
		sort(result.begin(), result.end());
		int len = result.size();
		if (len % 2 == 0) return (result[len / 2] + result[-1 + len / 2]) / 2.0//注意这里是2.0 这样才能返回值为double
		else
			return result[len / 2];
	}

	vector<int> result;
};
~~~

##### 2、借助两个堆，非常精妙的方法

~~~
这里讨论两种方法：
一：代码复杂：减少不必要插入，提高效率
二：代码大大简化：可能有不必要插入，效率有所降低
==============思路解析=================================
思考：如何得到一个数据流中的中位数？
如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。
如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。
一：代码复杂：
* 分析：对于海量数据和流的数据，用最大堆和最小堆来管理
* 我们希望 数据分为[小]|[大]两个部分，细化一点
[最大堆 |   左边最大 leftMax]
右边最小rightMin | 最小堆]


* 定义一个规则：保证左边和右边个数相差不大于1，且左边小于右边
* 1.数据是偶数的时候 insert的数据进入 [右边，最小堆]中
*  1.1当插入的数字cur > leftMax时，直接插入到[右边，最小堆]中
*  1.2当插入的数字cur < leftMax时，为了保证左边小于右边，
*      先把cur插入[最大堆|左边最大leftMax]中，
*      然后把leftMax放入[右边最小rightMin|最小堆]中
*      就可以保证： 左边 < 右边
* 2.数据是奇数的时候 insert的数据进入 [左边，最大堆]中
*      2.1当插入的数字cur < rightMin时，直接插入到[左边，最小堆]中
*      2.2当插入的数字cur > rightMin时，为了保证左边小于右边，
*      先把cur插入[右边最小rightMin|最小堆]中，
*      然后把rightMin放入[最大堆|左边最大leftMax]中
*      就可以保证： 左边 < 右边
* 最后：
* 如果是偶数：中位数mid= (leftMax+right)/2
* 如果是奇数：中位数mid= leftMax 因为先插入到左边，再插入到右边，为奇数时，中位数就是mid
~~~



~~~C++
class Solution {

public:
void Insert(int num)
	{
	count += 1; //数据是奇数的时候 insert的数据进入 [左边，最大堆]中
	if (count % 2 == 1)//奇数
	{
		if (big_heap.empty())  big_heap.push(num); //直接插入到[左边，最小堆]中
		else {
			int rightMin = small_heap.top();
			if (num <= rightMin)  big_heap.push(num);
			else {
				small_heap.push(num);  //先把cur插入[右边最小rightMin|最小堆]中
				big_heap.push(rightMin);  //然后把rightMin放入[最大堆|左边最大leftMax]中
				small_heap.pop();
			}
		}
	}
	else {

		if (small_heap.empty()) { //当第一个元素 比 第二个元素大的时候，会造成左边比右边大的情形，因此要加上判断
//当第一个数据比第二个大的时候，比如[5,2,3,4,1,6,7,0,8]的情况，会造成最大堆的唯一数据，比最小堆的唯一数据大的情况，这跟思想就不同了，因此需要加上一层判断。
			if (num > big_heap.top())
			{
				small_heap.push(num);
			}
			else
			{
				small_heap.push(big_heap.top());
				big_heap.pop();
				big_heap.push(num);
			}
		}
		else {
			int leftMax = big_heap.top();
			if (num >= leftMax)  small_heap.push(num);//直接插入到[右边，最小堆]中
			else {
				big_heap.push(num);//先把cur插入[右边最小rightMin|最小堆]中，
				small_heap.push(big_heap.top()); //然后把rightMin放入[最大堆|左边最大leftMax]中
				big_heap.pop();
			}
		}
	}		
}

double GetMedian()
{
	if (count & 0x1) {//看见这个0x你肯定知道这就是16进制表示了，而0x1就是最后一位肯定是1。偶数的二进制表示中最后一位肯定是0，
		//如果是奇数那肯定是1，所以一个整数与0x1做按位与运算得到的结果是0或者1就可以判断出这个整数是偶数还是奇数。
		return big_heap.top();
	}
	else {
		return double((small_heap.top() + big_heap.top()) / 2.0);
	}
}
private:
	int count = 0;
	priority_queue<int, vector<int>, less<int>> big_heap;        // 左边一个大顶堆
	priority_queue<int, vector<int>, greater<int>> small_heap;   // 右边一个小顶堆
};
~~~





##### 3、将上述代码简化版

~~~C++
取消了判断过程，直接插入到对面的堆中，然后再转移到自己的堆中
* 但是！！！时间复杂度提高，每次都插入时间复杂度O(log n)这是很可怕的 
* 定义一个规则：不要判断了
* 1.数据是偶数的时候 insert的数据进入 [右边，最小堆]中
*      先把cur插入[最大堆|左边最大leftMax]中，
*      然后把leftMax放入[右边最小rightMin|最小堆]中
*      就可以保证： 左边 < 右边
* 2.数据是奇数的时候 insert的数据进入 [左边，最大堆]中
*      先把cur插入[右边最小rightMin|最小堆]中，
*      然后把rightMin放入[最大堆|左边最大leftMax]中
*      就可以保证： 左边 < 右边
* 最后：
* 如果是偶数：中位数mid= (leftMax+right)/2 
* 如果是奇数：中位数mid= leftMax
~~~



然后把leftMax放入[右边最小rightMin|最小堆]中 \*  就可以保证： 左边 < 右边 \* 2.数据是奇数的时候 insert的数据进入 [左边，最大堆]中 \*      先把cur插入[右边最小rightMin|最小堆]中， \*      然后把rightMin放入[最大堆|左边最大leftMax]中 \*      就可以保证： 左边 < 右边 \* 最后： \* 如果是偶数：中位数mid= (leftMax+right)/2       \* 如果是奇数：中位数mid= leftMax**

~~~C++
class Solution {
public:
void Insert(int num)
	{
		count += 1;
		// 元素个数是偶数时,将大顶堆堆顶放入小顶堆
        if (count % 2 == 0) {
			big_heap.push(num);
			small_heap.push(big_heap.top());
			big_heap.pop();
		}
		else {
			small_heap.push(num);
			big_heap.push(small_heap.top());
			small_heap.pop();
		}
	}

double GetMedian()
{
	if (count & 0x1) {//看见这个0x你肯定知道这就是16进制表示了，而0x1就是最后一位肯定是1。偶数的二进制表示中最后一位肯定是0，
		//如果是奇数那肯定是1，所以一个整数与0x1做按位与运算得到的结果是0或者1就可以判断出这个整数是偶数还是奇数。
		return big_heap.top();
	}
	else {
		return double((small_heap.top() + big_heap.top()) / 2.0);
	}
}
private:
	int count = 0;
	priority_queue<int, vector<int>, less<int>> big_heap;        // 左边一个大顶堆
	priority_queue<int, vector<int>, greater<int>> small_heap;   // 右边一个小顶堆
	// 大顶堆所有元素均小于等于小顶堆的所有元素.
};
~~~

#### 33、滑动窗口的最大值

##### 1、自己想的，边界条件很多

总的来说，利用 low high maxIndex三个指针维护整个数组的情况

1、滑动窗口大小为0，num数组为空，滑动窗口大于 num.size 也不符合规矩，直接返回空

2、先考虑第一个滑动窗口的情况，走一遍，找出最大值的index

~~~C++
 vector<int> maxInWindows(const vector<int>& num, unsigned int size)
    {
	vector<int> result;
	if (num.size() == 0 || size == 0 || size > num.size()) return result;
	if (size == num.size()) {
        result.push_back(*max_element(num.begin(), num.end())); 
         return result;
      }

	int low = 0, high = size - 1, maxIndex = 0;
	int len = num.size();
	for (int i = 0; i <= high; ++i) {
		if (num[i] > num[maxIndex])  maxIndex = i;
	}
	//result.push_back(num[maxIndex]); //这里不能直接先push，要不然第一个滑动窗口的最大值会push两次
	while (high <= len - 1) {
		if (maxIndex == low - 1) {//如果maxIndex还是上个窗口的最低索引，需要更新
			maxIndex = low;
			for (int i = low; i <= high; ++i)
				if (num[i] > num[maxIndex])  maxIndex = i;

		}
		else if (num[maxIndex] < num[high]) //如果最新添加进来的high索引比原窗口中的所有值都要大，也要更新
		{
			maxIndex = high;
		}
		high++;
		low++;

		result.push_back(num[maxIndex]);

	}
	return result;
    }
~~~

##### 2、第二种做法，比较水，借助优先队列来做，小顶堆

~~~C++
vector<int> maxInWindows(const vector<int>& num, unsigned int size)
{
	vector<int> result;
	if (num.size() == 0 || size == 0 || size > num.size()) return result;
	priority_queue<int> pri_que;
	int count = 0;
	for (int i = 0; i < num.size()-size+1; ++i) {
		while (count < size) {
			pri_que.push(num[count + i]);
			count++;
		}
		count = 0;
		result.push_back(pri_que.top());
		while (!pri_que.empty()) {
			pri_que.pop();
		}
	}
	return result;
}

~~~

##### 3、借助双端队列来做，最为高效的一种方法

~~~C++
	vector<int>res;
	int len = num.size();
	if (len == 0 || size == 0 || size > len)	return res;
	deque<int>s;  //deque s中存储的是num的下标
	for (int i = 0; i < len; ++i)
	{
		while (!s.empty() && num[s.back()] <num[i])//当前值比队列从后往前的大，成为下一个待选值
			s.pop_back();
		while (!s.empty() && i - s.front() + 1 > size)//最大值已不在窗口中
			s.pop_front();
		s.push_back(i);

		if (i + 1 >= size)//当滑动窗口首地址i大于等于size时才开始写入窗口最大值
			res.push_back(num[s.front()]);
	}
	return res;
~~~

#### 34、机器人的运动范围 十分经典的题目



地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，
每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 
例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。
但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？



##### 1、借助标记法，看的解释，其实很好理解和明白

~~~C++
bool canReach(int threshold, int x, int y) {
	int sum = 0;
	while (x > 0) {
		sum += x % 10;
		x /= 10;
	}
	while (y > 0) {
		sum += y % 10;
		y /= 10;
	}
	return sum <= threshold;
}

int movingCountCore(int threshold, int i, int rows,int j ,int cols, vector<vector<bool>>&visit) {
	if (i < 0 || i >= rows || j < 0 || j >= cols || !canReach(threshold, i, j) || visit[i][j] == true) return 0;
	//边界值不满足，不能到达或者已经走过了，也到达不了，返回0
	visit[i][j] = true; // 当前已经走过了，并且满足要求，所有后续return 要加上1

	return movingCountCore(threshold, i - 1, rows, j, cols, visit) + //分别是上下左右各个方向判断一下
		movingCountCore(threshold, i + 1, rows, j, cols, visit) +
		movingCountCore(threshold, i , rows, j-1, cols, visit) +
		movingCountCore(threshold, i, rows, j+1, cols, visit) + 1;

}
int movingCount(int threshold, int rows, int cols)
{
	vector<vector<bool>> visit(rows,vector<bool>(cols,false));
	return movingCountCore(threshold, 0,  rows, 0, cols, visit);
	
}

~~~

##### 2、标注借助法的简化版



递归只要俩行就够了，helper(threshold, rows, cols, flags, i + 1, j) +  helper(threshold, rows, cols, flags, i, j + 1) + 1，不需要往回走，然后前面的判断i，j也不会小于零了  

因为是从（0 0 ），开始走的，所以只需要判断向上和向右的情况即可

~~~C++
bool canReach(int threshold, int x, int y) {
	int sum = 0;
	while (x > 0) {
		sum += x % 10;
		x /= 10;
	}
	while (y > 0) {
		sum += y % 10;
		y /= 10;
	}
	return sum <= threshold;
}

int movingCountCore(int threshold, int i, int rows,int j ,int cols, vector<vector<bool>>&visit) {
	if (i >= rows || j >= cols || !canReach(threshold, i, j) || visit[i][j] == true) return 0;
	//边界值不满足，不能到达或者已经走过了，也到达不了，返回0
	visit[i][j] = true; // 当前已经走过了，并且满足要求，所有后续return 要加上1

	return  movingCountCore(threshold, i + 1, rows, j, cols, visit) +
		movingCountCore(threshold, i, rows, j+1, cols, visit) + 1;

}
int movingCount(int threshold, int rows, int cols)
{
	vector<vector<bool>> visit(rows,vector<bool>(cols,false));
	return movingCountCore(threshold, 0,  rows, 0, cols, visit);
	
}
~~~





##### 3、BFS

~~~C++
bool canReach(int threshold, int x, int y) {
	int sum = 0;
	while (x > 0) {
		sum += x % 10;
		x /= 10;
	}
	while (y > 0) {
		sum += y % 10;
		y /= 10;
	}
	return sum <= threshold;
}

int movingCount(int threshold, int rows, int cols)
{
	vector<vector<bool>> grid(rows,vector<bool>(cols,false));
	queue<pair<int, int>> que;
	if (canReach(threshold, 0, 0)) {
		que.push(make_pair(0, 0));
		grid[0][0] = true;
	}
	int cnt = 0;
	while (!que.empty()) {
		++cnt;
		int x, y;
		tie(x, y) = que.front();
		que.pop();
		if (x + 1 < rows && !grid[x + 1][y] && canReach(threshold, x + 1, y)) {
			grid[x + 1][y] = true;
			que.push(make_pair(x + 1, y));
		}
		if (y + 1 < cols && !grid[x][y + 1] && canReach(threshold, x, y + 1)) {
			grid[x][y + 1] = true;
			que.push(make_pair(x, y + 1));
		}
	}
	return cnt;
	
}
~~~

#### 35、矩阵中的路径 超级经典的题目

##### 1、DFS



~~~C++
//这道题是典型的深度优先遍历DFS的应用，原二维数组就像是一个迷宫，可以  //上下左右四个方向行走
//我们的二维数组board中每个数都作为起点和给定的字符串做匹配，我们需要
//一个和原二维数组board等大小的visited数组，是bool型的，用来记录当前位置
//是否被访问过。因为题目要求一个cell只能被访问一次。
//如果二维数组的当前字符和目标字符串str对应的字符相等，则对其上下左右四个邻字
//符串分别调用dfs的递归函数，只要有一个返回true，那么就表示找到对应的字符串 

bool dfs(vector<vector<char>> &board, char* str, int index, int x, int y,
	vector<vector<bool>>& visited) 

{
	if (index == strlen(str)) return true;//搜寻超过路径长度，符合条件，返回true，//此时已经超过最大程度了 strlen返回不带 ‘\0’的长度，此时str[k]已经越界了，所以这个判断一定要放在函数开头，如果放在if之后，str[k]会越界
	if ((x < 0) || (y < 0) || (x >= board.size()) || (y >= board[0].size()))
		return false;//访问越界，终止，返回false
	if (visited[x][y]) return false;//之前访问过，剪枝
	if (board[x][y] != str[index]) return false;//不相等，剪枝
	visited[x][y] = true;
	if (dfs(board, str, index + 1, x, y - 1, visited) || //上
		dfs(board, str, index + 1, x, y + 1, visited) ||     //下
		dfs(board, str, index + 1, x - 1, y, visited) ||     //左
		dfs(board, str, index + 1, x + 1, y, visited))      //右
		return true; //有符合要求的

	visited[x][y] = false;//记得此处改回false，以方便下一次遍历搜索。
	return false;
}

bool hasPath(char* matrix, int rows, int cols, char* str)
{
	if (str == NULL || rows <= 0 || cols <= 0)
		return false;
	vector<vector<char>> board(rows, vector<char>(cols));
	for (int i = 0; i < rows; ++i) {//将matrix装入二维数组board中
		for (int j = 0; j < cols; ++j) {
			board[i][j] = matrix[i * cols + j];
		}
	}
	vector<vector<bool>> visited(rows, vector<bool>(cols, false));
	for (int i = 0; i < rows; ++i) {
		for (int j = 0; j < cols; ++j) {
			if (dfs(board, str, 0, i, j, visited) == true)
				return true;//以矩阵board中的每个字符为起点进行广度优先搜索
			//找到一个符合条件的即返回true.
		}
	}
	return false;//遍历完都没找到匹配的路径，返回false
}

~~~



##### 2、回溯法  写法非常的好啊

~~~C++
/*参数说明  k 字符串索引初始为0即先判断字符串的第一位*/
bool judge(char* matrix, int rows, int cols, int i, int j, char* str, int k, bool* flag)
{
	//因为是一维数组存放二维的值，index值就是相当于二维数组的（i，j）在一维数组的下标
	int index = i * cols + j;
	//flag[index]==true,说明被访问过了，那么也返回true;
	if (i < 0 || i >= rows || j < 0 || j >= cols || matrix[index] != str[k] || flag[index] == true)
		return false;
	//字符串已经查找结束，说明找到该路径了
	if (str[k + 1] == '\0') return true;
	//向四个方向进行递归查找,向左，向右，向上，向下查找
	flag[index] = true;//标记访问过 //要走的第一个位置置为true，表示已经走过了0

	  //回溯，递归寻找，每次找到了就给k加一，找不到，还原
	if (judge(matrix, rows, cols, i - 1, j, str, k + 1, flag)
		|| judge(matrix, rows, cols, i + 1, j, str, k + 1, flag)
		|| judge(matrix, rows, cols, i, j - 1, str, k + 1, flag)
		|| judge(matrix, rows, cols, i, j + 1, str, k + 1, flag))
	{
		return true;
	}

	//走到这，说明这一条路不通，还原，再试其他的路径
	flag[index] = false;
	return false;
}

bool hasPath(char* matrix, int rows, int cols, char* str)
{
	if (matrix == NULL || rows < 1 || cols < 1 || str == NULL) return false;
	bool* flag = new bool[rows * cols];
	memset(flag, false, rows * cols);
	for (int i = 0; i < rows; i++)
	{
		for (int j = 0; j < cols; j++)
		{
			if (judge(matrix, rows, cols, i, j, str, 0, flag))
			{
				return true;
			}
		}
	}
	delete[] flag;
	return false;
}
~~~



#### 38、把二叉树打印成多行 ，跟二叉树的层次遍历差不多

从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。

##### 1、队列做法，保存每层的节点个数

~~~C++
vector<vector<int> > Print(TreeNode* pRoot) {
	vector<vector<int>> result;
	if (pRoot == nullptr) return result;
	queue<TreeNode*> q;
	q.push(pRoot);
	while (!q.empty()) {
		int len = q.size();//利用len保存每层的个数
		vector<int> temp;
		while (len--) {
			TreeNode* node = q.front();
			q.pop();
			temp.push_back(node->val);
			if (node->left)	  q.push(node->left);//为空才push进去,而不是if(node) 然后直接push左右两个节点
			if (node->right)  q.push(node->right);;
		}
		result.push_back(temp);
	}
	return result;
}
~~~





#### 40、二叉树的下一个结点  很经典的题目

给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

##### 1、没有思路，自己瞎写的，错误

~~~C++
TreeLinkNode* GetNext(TreeLinkNode* pNode)
{
	if (pNode == nullptr) return nullptr;
	if (pNode->next == nullptr) {
		if (pNode->right == nullptr) return nullptr;
		else
			return pNode->right;
	} 
	if (pNode->left == nullptr && pNode->right == nullptr) return pNode->next;
	if (pNode->left == nullptr) return pNode->right;
	if (pNode->right == nullptr) return pNode->next;
}
~~~

画了图来分析，没有父亲节点再分情况讨论

如果无左右孩子，则返回父亲节点

无左孩子返回右孩子，无右孩子则返回父亲节点



##### 2、牛客网上做法

分析可知：

 1.二叉树为空，则返回空； 

  2.节点右孩子存在，则设置一个指针从该节点的右孩子出发，一直沿着指向左子结点的指针找到的叶子节点即为下一个节点； 

   3.右孩子不存在，如果节点不是根节点，如果该节点是其父节点的左孩子，则返回父节点；否则继续向上遍历其父节点的父节点，重复之前的判断，返回结果。

~~~C++
TreeLinkNode* GetNext(TreeLinkNode* pNode)
{
	if (pNode == nullptr)
			return nullptr;
	if (pNode->right != nullptr)
	{
		pNode = pNode->right;
		while (pNode->left != nullptr)
			pNode = pNode->left;
		return pNode;
	}
	while (pNode->next != nullptr)
	{
		TreeLinkNode* proot = pNode->next;
		if (proot->left == pNode)
			return proot;
		pNode = pNode->next;
	}
	return nullptr;
}
~~~

##### 3、第二种写法的变种

~~~C++
TreeLinkNode* GetNext(TreeLinkNode* pNode)
    {
	if (pNode == nullptr)
		return nullptr;
	TreeLinkNode* node = nullptr;
	if (pNode->right != nullptr) {//如果当前节点有右子树,则右子树最左边的那个节点就是
		node = pNode->right;
		while (node->left != nullptr)
			node = node->left;
		return node;
	}
	node = pNode;
	while (node->next != nullptr && node == node->next->right) {//找到当前节点是其父亲节点的左孩子的那个节点，然后返回其父亲节点，如果当前节点没有右子树
		node = node->next;
	}
	return node->next;
    }
~~~





#### 44、按照之字形打印二叉树 终于自己写出来了，很难啊





##### 1、左右栈和右左栈协助来做的

注意左右子树在两个栈中的入栈顺序

~~~C++
vector<vector<int> > Print(TreeNode* pRoot) {
	vector<vector<int>> result;
	if (pRoot == nullptr) return result;
	stack<TreeNode*> left_right_st;
	stack<TreeNode*> right_left_st;
	left_right_st.push(pRoot);
	while (left_right_st.size() ||  right_left_st.size()) {
		if (!left_right_st.empty()) {
			vector<int> temp;
			TreeNode* node;
			while (!left_right_st.empty()) {
				node = left_right_st.top();
				temp.push_back(node->val);
				if (node->left)//这里先左再右
					right_left_st.push(node->left);
				if (node->right)
					right_left_st.push(node->right);
				left_right_st.pop();
			}
			result.push_back(temp);
		}

		if (!right_left_st.empty()) {
			vector<int> temp;
			TreeNode* node;
			while (!right_left_st.empty()) {
				node = right_left_st.top();
				temp.push_back(node->val);
				if (node->right)//这里需要是先右再左
					left_right_st.push(node->right);
				if (node->left)
					left_right_st.push(node->left);
				right_left_st.pop();
			}
			result.push_back(temp);
		}

	}
	return result;
}
~~~

##### 2、稍微优化一下代码

~~~C++
vector<vector<int> > Print(TreeNode* pRoot) {
	vector<vector<int>> result;
	if (pRoot == nullptr) return result;
	stack<TreeNode*> left_right_st;
	stack<TreeNode*> right_left_st;
	left_right_st.push(pRoot);
	while (left_right_st.size() ||  right_left_st.size()) {
		vector<int> temp;
		TreeNode* node;
		if (!left_right_st.empty()) {
			while (!left_right_st.empty()) {
				node = left_right_st.top();
				temp.push_back(node->val);
				if (node->left)
					right_left_st.push(node->left);
				if (node->right)
					right_left_st.push(node->right);
				left_right_st.pop();
			}
			result.push_back(temp);
			
		}
		vector<int>().swap(temp);
		if (!right_left_st.empty()) {
			while (!right_left_st.empty()) {
				node = right_left_st.top();
				temp.push_back(node->val);
				if (node->right)
					left_right_st.push(node->right);
				if (node->left)
					left_right_st.push(node->left);
				right_left_st.pop();
			}
			result.push_back(temp);
		}

	}
	return result;
}
~~~



##### 3、只用一个栈来做，很不错的想法

~~~C++
vector<vector<int> > Print(TreeNode* pRoot) {
	vector<vector<int>> result;
	if (pRoot == nullptr) {
		return result;
	}
	queue<TreeNode*> q;
	q.push(pRoot);
	bool isLeft = false;
	while (!q.empty()) {
		int rowLen = q.size();
		vector<int> temp;
		while(rowLen--) {
			TreeNode* curNode = q.front();
			q.pop();
			if (curNode != nullptr) {
				temp.push_back(curNode->val);
				if (curNode->left)q.push(curNode->left);
				if (curNode->right)q.push(curNode->right);
			}
		}
		isLeft = !isLeft;
		if (!isLeft) {
			result.push_back(vector<int>(temp.rbegin(), temp.rend()));//注意这里是翻转一下的
		}
		else {
			result.push_back(temp);
		}
	}
	return result;
}
~~~





#### 48、对称的二叉树

请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。

##### 1、递归法比较好做，也很方便

~~~C++
bool isEqual(TreeNode*node1,TreeNode*node2){

        if(node1==nullptr && node2 ==nullptr)  return true;
        if(node1 ==nullptr || node2==nullptr) return false;//减少逻辑判断
        if(node1->val == node2->val) {
            return isEqual(node1->left,node2->right) && isEqual(node1->right,node2->left);//注意这里是右左，左右来进行判断

        }else
            return false;
    }
    bool isSymmetrical(TreeNode* pRoot) {
        if(pRoot==nullptr) return true;//这里是返回true的
        return isEqual(pRoot->left,pRoot->right);
    }
~~~







#### 49、二叉搜索树的第K个节点

给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）    中，按结点数值大小顺序第三小结点的值为4。

##### 1、笨方法，全部保存下来

会超时，这个方法不行



##### 2、中序遍历其实就是从小到大的排列顺序

~~~C++
class situation {
public:
	int count=0;

	TreeNode* KthNode(TreeNode* pRoot, int k)
	{
		if (pRoot == nullptr) return nullptr;
		TreeNode* left_node = KthNode(pRoot->left, k);
		if (left_node) return left_node;
		count++;
		if (k == count) {
			return pRoot;
		}
		TreeNode* right_node = KthNode(pRoot->right, k);
		if (right_node) return right_node;
		return nullptr;
	}
}
~~~



##### 3、中序遍历模板解法

~~~C++
TreeNode* KthNode(TreeNode* pRoot, int k)
	{
		if (pRoot == nullptr) return nullptr;
		stack<TreeNode*> s;
		s.push(pRoot);
		while (!s.empty() || pRoot != nullptr) {
			if (pRoot != nullptr) {
				s.push(pRoot);
				pRoot = pRoot->left;
			}
			else {
				pRoot = s.top();
				s.pop();
				k--;
				if (k == 0) return pRoot;
				pRoot = pRoot->right;
			}
		}
		return nullptr;
	}
~~~

#### 50、序列化二叉树  有点难，挺经典的

请实现两个函数，分别用来序列化和反序列化二叉树



二叉树的序列化是指：把一棵二叉树按照某种遍历方式的结果以某种格式保存为字符串，从而使得内存中建立起来的二叉树可以持久保存。序列化可以基于先序、中序、后序、层序的二叉树遍历方式来进行修改，序列化的结果是一个字符串，序列化时通过 某种符号表示空节点（#），以 ！ 表示一个结点值的结束（value!）。

二叉树的反序列化是指：根据某种遍历顺序得到的序列化字符串结果str，重构二叉树。



例如，我们可以把一个只有根节点为1的二叉树序列化为"1,"，然后通过自己的函数来解析回这个二叉树。

##### 1、看的大神的写法，这种写法超级棒，代码逻辑非常好，

##### 但是牛客上并没有考虑到负数的情况，力扣上的题目有负数的限制

~~~C++
class Solution {
private:
	string SerializeCore(TreeNode* root) {
		if (root == nullptr) {
			return "#!";
		}
		string str;
		str = to_string(root->val) + "!";
		str += SerializeCore(root->left);
		str += SerializeCore(root->right);
		return str;
	}

	TreeNode* DeserializeCore(char*& str) {
		if (*str == '#') {
			str++;
			return nullptr;
		}
		int num = 0;
		while (*str != '!') {
			num = num * 10 + *str - '0';
			str++;
		}
		TreeNode* node = new TreeNode(num);
		node->left = DeserializeCore(++str);
		node->right = DeserializeCore(++str);
		return node;
	}
public:
	char* Serialize(TreeNode* root) {
		string str = SerializeCore(root);
		char* res = new char[str.size()];
		for (int i = 0; i < str.size(); i++) {
			res[i] = str[i];
		}
		return res;
	}

	TreeNode* Deserialize(char* str) {
		return DeserializeCore(str);
	}
};
~~~



#### 51、构建乘积数组

给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。（注意：规定B[0] = A[1] * A[2] * ... * A[n-1]，B[n-1] = A[0] * A[1] * ... * A[n-2];）

##### 1、暴力法

~~~C++
vector<int> multiply(const vector<int>& A) {
	vector<int> B;
	for (int i = 0; i < A.size(); ++i) {

		int temp = 1;
		for (int j = 0; j < A.size(); ++j) {
			if (j != i) temp *= A[j];
		}
		B.push_back(temp);
	}
	return B;
}
~~~

##### 2、一种超级精妙的解法，吹爆了

~~~C++
vector<int> multiply(const vector<int>& A) {
	int len = A.size();
	vector<int> B(len,0);
	int temp = 1;
	for (int i = 0; i <len; temp*=A[i],++i) {

		B[i] = temp;
	}

	temp = 1;
	for (int i = len-1; i >= 0; temp *= A[i], --i) {

		B[i] = B[i]*temp;
	}
	return B;
}
~~~

#### 52.正则表达式匹配  很经典的题目

请实现一个函数用来匹配包括'.'和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但是与"aa.a"和"ab*a"均不匹配

##### 1、太他吗难了，不会不会

~~~C++
//字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但是与"aa.a"和"ab*a"均不匹配
bool match(char* str, char* pattern)
{
	int len1 = strlen(str), len2 = strlen(pattern);
	int low1 = 0, low2 = 0;
	while (low1 < len1 && low2 < len2) {
		if (str[low1] == pattern[low2]) {

			if (low2 + 1 < len2 && pattern[low2 + 1] == '*') {
				while (str[low1] == pattern[low2]) { low1++; }
				low2++;//跳过 * 
			}
			else {
				low1++;
				low2++;
			}
			
		}
		else if (str[low1] != pattern[low2]) {
			cout << "111" << endl;
			if (pattern[low2] == '.' && low2 + 1 < len2 && pattern[low2 + 1] != '*') { low1++; low2++; }
			else if (pattern[low2] == '.' && low2 + 1 < len2 && pattern[low2 + 1] == '*')  return true;
			else if (low2 + 1 < len2 && pattern[low2 + 1] == '*') {//出现 aa 与 ab*a这样的情况
				low2+=2;
			}
				return false;
		}
	}
	return low1 == len1 && low2 == len2;
}
~~~



##### 2、看的思路



```C++
/*
    解这题需要把题意仔细研究清楚，反正我试了好多次才明白的。
    首先，考虑特殊情况：
         1>两个字符串都为空，返回true
         2>当第一个字符串不空，而第二个字符串空了，返回false（因为这样，就无法
            匹配成功了,而如果第一个字符串空了，第二个字符串非空，还是可能匹配成
            功的，比如第二个字符串是“a*a*a*a*”,由于‘*’之前的元素可以出现0次，
            所以有可能匹配成功）
    之后就开始匹配第一个字符，这里有两种可能：匹配成功或匹配失败。但考虑到pattern
    下一个字符可能是‘*’， 这里我们分两种情况讨论：pattern下一个字符为‘*’或
    不为‘*’：
          1>pattern下一个字符不为‘*’：这种情况比较简单，直接匹配当前字符。如果
            匹配成功，继续匹配下一个；如果匹配失败，直接返回false。注意这里的
            “匹配成功”，除了两个字符相同的情况外，还有一种情况，就是pattern的
            当前字符为‘.’,同时str的当前字符不为‘\0’。
          2>pattern下一个字符为‘*’时，稍微复杂一些，因为‘*’可以代表0个或多个。
            这里把这些情况都考虑到：
               a>当‘*’匹配0个字符时，str当前字符不变，pattern当前字符后移两位，
                跳过这个‘*’符号；
               b>当‘*’匹配1个或多个时，str当前字符移向下一个，pattern当前字符
                不变。（这里匹配1个或多个可以看成一种情况，因为：当匹配一个时，
                由于str移到了下一个字符，而pattern字符不变，就回到了上边的情况a；
                当匹配多于一个字符时，相当于从str的下一个字符继续开始匹配）
    之后再写代码就很简单了。
*/
```

~~~C++
 bool match(char* str, char* pattern)
    {
        if (*str == '\0' && *pattern == '\0')
            return true;
        if (*str != '\0' && *pattern == '\0')
            return false;
        //if the next character in pattern is not '*'
        if (*(pattern+1) != '*')
        {
            if (*str == *pattern || (*str != '\0' && *pattern == '.'))
                return match(str+1, pattern+1);
            else
                return false;
        }
        //if the next character is '*'
        else
        {
            if (*str == *pattern || (*str != '\0' && *pattern == '.'))
                return match(str, pattern+2) || match(str+1, pattern);
            else
                return match(str, pattern+2);
        }
    }
~~~

**讲解:**

首先能够匹配的情况就是两种：1、两者相等，2、s!='\0' && p=='.'

只有这两种情况

1、p【1】!=* ,那么可能是 ba与.a 或者 ba ba这两种形式，那就直接s+1 p+1，递归到下一层，否则就是false了

2、p[1]==  *,有 * 过来捣乱，如果匹配的话也就是两者相等或者**s!=‘\0’并且p==‘.’的话，前者比如 abc 与 aabc直接 s p+2 ，后者就是aaabc 与 .**</u>*bc这样，直接s+1,p，s向前走一步逐渐递归到下次

如果不匹配的话那就是 abc  b*abc这样的，p向前走两步，走到后面的p再去匹配





##### 3、另一种写法

~~~C++
/*
要分为几种情况：（状态机）
（1）当第二个字符不为‘*’时：匹配就是将字符串和模式的指针都下移一个，不匹配就直接返回false
（2）当第二个字符为'*'时：
        匹配：
                a.字符串下移一个，模式不动
                c.字符串不动，模式下移两个
        不匹配：字符串下移不动，模式下移两个
搞清楚这几种状态后，用递归实现即可：
*/
class Solution {
public:
    bool match(char* str, char* pattern){
        if(str[0]=='\0'&&pattern[0]=='\0')
            return true;
        else if(str[0]!='\0'&&pattern[0]=='\0')
            return false;
        if(pattern[1]!='*'){
            if(str[0]==pattern[0]||(pattern[0]=='.'&&str[0]!='\0'))
                return match(str+1,pattern+1);
            else
                return false;
        }
        else{
            if(str[0]==pattern[0]||(pattern[0]=='.'&&str[0]!='\0'))
                return match(str,pattern+2)||match(str+1,pattern);
            else
                return match(str,pattern+2);
        }
    }
};
~~~



#### 53、孩子们的游戏

每年六一儿童节,牛客都会准备一些小礼物去看望孤儿院的小朋友,今年亦是如此。HF作为牛客的资深元老,自然也准备了一些小游戏。其中,有个游戏是这样的:首先,让小朋友们围成一个大圈。然后,他随机指定一个数m,让编号为0的小朋友开始报数。每次喊到m-1的那个小朋友要出列唱首歌,然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,从他的下一个小朋友开始,继续0...m-1报数....这样下去....直到剩下最后一个小朋友,可以不用表演,并且拿到牛客名贵的“名侦探柯南”典藏版(名额有限哦!!^_^)。请你试着想下,哪个小朋友会得到这份礼品呢？(注：小朋友的编号是从0到n-1)



如果没有小朋友，请返回-1



#### 54、剪绳子

给你一根长度为n的绳子，请把绳子剪成整数长的m段（m、n都是整数，n>1并且m>1），每段绳子的长度记为k[1],...,k[m]。请问k[1]x...xk[m]可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

输入描述:

```
输入一个数n，意义见题面。（2 <= n <= 60）
```

输出描述:

```
输出答案。
```

示例1

输入

```
8
```

输出

```
18
```





##### 1、很厉害的一种思路

~~~C++
链接：https://www.nowcoder.com/questionTerminal/57d85990ba5b440ab888fc72b0751bf8?f=discussion
来源：牛客网

/**
 * 题目分析：
 * 先举几个例子，可以看出规律来。
 * 4 ： 2*2
 * 5 ： 2*3
 * 6 ： 3*3
 * 7 ： 2*2*3 或者4*3
 * 8 ： 2*3*3
 * 9 ： 3*3*3
 * 10：2*2*3*3 或者4*3*3
 * 11：2*3*3*3
 * 12：3*3*3*3
 * 13：2*2*3*3*3 或者4*3*3*3
 *
 * 下面是分析：
 * 首先判断k[0]到k[m]可能有哪些数字，实际上只可能是2或者3。
 * 当然也可能有4，但是4=2*2，我们就简单些不考虑了。
 * 5<2*3,6<3*3,比6更大的数字我们就更不用考虑了，肯定要继续分。
 * 其次看2和3的数量，2的数量肯定小于3个，为什么呢？因为2*2*2<3*3，那么题目就简单了。
 * 直接用n除以3，根据得到的余数判断是一个2还是两个2还是没有2就行了。
 * 由于题目规定m>1，所以2只能是1*1，3只能是2*1，这两个特殊情况直接返回就行了。
 *
 * 乘方运算的复杂度为：O(log n)，用动态规划来做会耗时比较多。
 */
~~~

~~~C++
int cutRope(int number) {

	if (number == 2) {
		return 1;
	}
	if (number == 3) {
		return 2;
	}
	int x = number % 3, y = number / 3;
	if (x == 0) {
		return pow(3, y);
	}
	else if (x == 1) {
		return 2 * 2 * pow(3, y - 1);
	}
	else 
		return 2 * pow(3, y);
	
}
~~~

##### 1-1、力扣上的一种讲解

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：6 MB, 在所有 C++ 提交中击败了100.00%的用户

~~~C++
    int cuttingRope(int n) {

    if(n<2) return 0;
    if(n<4) return n-1;
    int maxNum=1;
    while(n>4){
        maxNum*=3;
        n-=3;
    }
    maxNum*=n;
    return maxNum;
    }
~~~



##### 2、一种DP讲解方法

##### 讲解视频：

https://www.bilibili.com/video/BV18E411T7dU?from=search&seid=16580267998265505121

~~~C++
int cutRope(int number) {
		if (number == 2 || number == 3)
			return number - 1;
		vector<int> ans(number + 1,0);
		ans[0] = 1;
		ans[1] = 1;
		for (int i = 2; i <= number; ++i)
		{
			ans[i] = i - 1;//分为2 段 1 * （i-1）
			for (int j = 2; j < i; ++j)
			{
				ans[i] = max(ans[i], j * ans[i - j]); //一种是继续分割的情况
				ans[i] = max(ans[i], j * (i-j));//不在分割 就割成两段
			}
		}
		return ans[number];
}
~~~



##### 2、这种DP更容易懂一些

##### 讲解视频：https://www.bilibili.com/video/BV1C7411V7s6?from=search&seid=16580267998265505121

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：6 MB, 在所有 C++ 提交中击败了100.00%的用户

~~~C++
int cutRope(int number) {
		if (number < 2) return -1;
		if (number == 2 || number == 3)
			return number - 1;
		vector<int> ans(number + 1,0);
		int maxNum = -1;
		ans[1] = 1;
		ans[2] = 2;
		ans[3] = 3;//因为长度》=4，他们不需要拆，拆了反而会变小，对于小于4的情况我们直接开头处理
		for (int i = 4; i <= number; ++i)
		{
			for (int j = 1; j <= i/2; ++j)
			{
				maxNum = max(maxNum, ans[j] * ans[i - j]);
				ans[i] = maxNum;

			}
			
		}
		return ans[number];
}
~~~

j<=i/2 是因为 f(5) = f(1)*f(4)   f(5) = f(2)*****f(3)    f(5) = f(3)*****f(2)  

 f(5) = f(4)*****f(1)  ,可以看到走到后面去了有回来了，所以走一半即可，但一定要走到一半才行，不能小于i/2，必须是小于等于

#### 54-2、剪绳子-2（力扣上的题目）

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m] 。请问 k[0]*k[1]*...*k[m] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

 

示例 1：

输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1
示例 2:

输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36


提示：

2 <= n <= 1000

##### 1、DP会溢出，只能用上述规律这一种方法来做了

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：6.2 MB, 在所有 C++ 提交中击败了100.00%的用户

~~~C++
int cuttingRope(int n) {
    
    if(n<2) return 0;
    if(n<4) return n-1;
    long maxNum=1,mod = 1000000007;
    while(n>4){
        maxNum*=3;
        maxNum %=mod;
        n-=3;
    }
    maxNum*=n;
    maxNum %=mod;
    return maxNum;
    }
~~~

#### 55、孩子们的游戏（圆圈中最后剩下的数） 啥也别说了，背模板吧



每年六一儿童节,牛客都会准备一些小礼物去看望孤儿院的小朋友,今年亦是如此。HF作为牛客的资深元老,自然也准备了一些小游戏。其中,有个游戏是这样的:首先,让小朋友们围成一个大圈。然后,他随机指定一个数m,让编号为0的小朋友开始报数。每次喊到m-1的那个小朋友要出列唱首歌,然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,从他的下一个小朋友开始,继续0...m-1报数....这样下去....直到剩下最后一个小朋友,可以不用表演,并且拿到牛客名贵的“名侦探柯南”典藏版(名额有限哦!!^_^)。请你试着想下,哪个小朋友会得到这份礼品呢？(注：小朋友的编号是从0到n-1)



如果没有小朋友，请返回-1

##### 1、时间复杂度太大

~~~C++
class Solution {
public:
struct ListNode {
	int val;
	struct ListNode* next;
	ListNode(int v) :val(v), next(NULL) {

	}
};
    
    int LastRemaining_Solution(int n, int m)
    {
    ListNode* root=(ListNode*)malloc(sizeof(ListNode));
	root->val = 0;
	ListNode* node = (ListNode*)(malloc)(sizeof(ListNode));
	node=root;
	for (int i = 1; i < n; ++i) {
		ListNode* temp = (ListNode*)(malloc)(sizeof(ListNode));
		temp->val = i;
		node->next = temp;
		node = node->next;
	}
	node->next = root;

	int count = 0,result=-1;
	while (root != nullptr && n!=1) {
		if (++count == m - 1) {
			result = root->val;
			root = root->next;
			node->next = root;
			count = 0;
			n--;
			continue;
		}

		root = root->next;
		node = node->next;
		
	}
	result = root->val;
	return result;
    }
};
~~~



##### 2、约瑟夫环的问题，背模板吧

执行用时：4 ms, 在所有 C++ 提交中击败了99.81%的用户

内存消耗：5.8 MB, 在所有 C++ 提交中击败了100.00%的用户

~~~C++
    int lastRemaining(int n, int m) {

        if(n <= 0 || m < 0)
            return -1;
        int ans = 0;
        // 最后一轮剩下2个人，所以从2开始反推
        for (int i = 2; i <= n; ++i) {
            ans = (ans + m) % i;
        }
        return ans;
    }
~~~



##### 3、递归做法，不觉明厉

~~~C++
    int LastRemaining_Solution(int n, int m)
    {
	if(n==0)
 		return -1;
    if(n==1)
        return 0;
    else
        return (LastRemaining_Solution(n-1,m)+m)%n;
    }
~~~






