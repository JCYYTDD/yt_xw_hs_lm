[1. 两数之和](https://leetcode.cn/problems/two-sum/)

map

[49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/)

map

[128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)

set

unordered_set

[283. 移动零](https://leetcode.cn/problems/move-zeroes/)

vector

[3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

set

vector `<int>` m(128,0)

[560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/)

map容器默认排序规则为 按照key值进行 从小到大排序

unordered_map性能更高

[56. 合并区间](https://leetcode.cn/problems/merge-intervals/)

sort 应用多个指标,排序标准应该声明为静态类型

```cpp
static bool compare(vector<int>& p1, vector<int>& p2) {
	if (p1[0] == p2[0]) {
		return p1[1] < p2[1];
	}
	else {
		return p1[0] < p2[0];
	}
}


vector<vector<int>> merge(vector<vector<int>>& intervals) {

	vector<vector<int>> ans;
	if (intervals.size() == 0)return ans;
	sort(intervals.begin(),intervals.end(),compare);
	ans.push_back(intervals[0]);
	for (int i = 1; i < intervals.size(); i++) {
		if (ans[ans.size() - 1][1] >= intervals[i][0] && ans[ans.size() - 1][1] <= intervals[i][1]) {
			ans[ans.size() - 1][1] = intervals[i][1];
		}
		else if(ans[ans.size() - 1][1] < intervals[i][0]) {
			ans.push_back(intervals[i]);
		}
		else if (ans[ans.size() - 1][i - 1][1] >= intervals[i][1]) {

		}
	}

	return ans;


}
```
160. 相交链表
指针
结构体struct