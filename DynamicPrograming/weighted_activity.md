#### 题目描述
给定`𝑛`个活动，活动`𝑎_𝑖`表示为一个三元组`(𝑠_𝑖,𝑓_𝑖,𝑣_𝑖)`  
其中`𝑠_𝑖`表示活动开始时间，`𝑓_𝑖`表示活动的结束时间，`𝑣_𝑖`表示活动的权重  
带权活动选择问题是选择一些活动使得任意被选择的两个活动`𝑎_𝑖`和`𝑎_𝑗`执行时间互不相交  
即区间`[𝑠_𝑖,𝑓_𝑖]`和`[𝑠_𝑗,𝑓_𝑗]`互不重叠，并且被选择的活动的权重和最大  
请设计一种方法求解带权活动选择问题

#### 解题思路
对所有活动按结束时间进行排序  
设`dp(i)`表示活动`a_1`到`a_i`所选活动的最优解  
此时分为两种情况选活动`a_i`和不选活动`a_i`   
```
dp(i) = max{dp(i-1), dp(k) + v_i} ，  0 < k < i
```
`a_k`为距离`a_i`最近且与其不相交的活动  

#### 代码实现

[code](/DynamicPrograming/weighted_activity.cpp)
```cpp
struct Activity
{
	int s;
	int f;
	int v;

	const bool operator < (const Activity a) const
	{
		return f < a.f;
	}

	Activity(int s, int e, int w): s(s), f(e), v(w){}
};
```
```cpp
int solve(vector<Activity> a)
{
	int n = a.size();
	int r[n+1];

	sort(a.begin(), a.end());

	r[0] = 0;
	r[1] = a[0].v;

	for(int i=2; i<=n; ++i)
	{
		int k = i - 1;
		while(k > 0 && a[k-1].f > a[i-1].s)	
			--k;
		r[i] = max(r[i-1], r[k] + a[i-1].v);
	}

	return r[n];
}
```

