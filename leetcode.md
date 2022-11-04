# leetcode刷题记录

> 计划：每日三题

## [Leetcode1 两数之和](https://leetcode.cn/problems/two-sum/)

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int n = nums.size();
        unordered_map<int, int> hash;
        for(int i = 0; i < n; i++) {
            int x = target - nums[i];
            if(hash.find(x) != hash.end())
                return {i, hash[x]};
            hash[nums[i]] = i;
        }
        return {};
        
    }
};
```

## [Leetcode2 两数相加](https://leetcode.cn/problems/add-two-numbers/)

```c++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(-1);
        ListNode* cur = dummy;
        int t = 0;
        while(l1 || l2 || t) {
            if(l1) t += l1->val, l1 = l1->next;
            if(l2) t += l2->val, l2 = l2->next;
            cur = cur->next = new ListNode(t % 10);
            t /= 10;
        }
        return dummy->next;

    }
};
```

## [Leetcode3 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

```c++
const int N = 5e4 + 10;
int f[N];
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n = s.size();
        f[0] = 1;
        for(int i = 1; i < n; i++) {
            int j = i - 1;
            while(s[j] != s[i] && j >= i - f[i - 1] && j >= 0) {
                j--;
                if(j < 0) break;
            }
            if(j < i - f[i - 1]) f[i] = f[i - 1] + 1;
            else f[i] = i - j;
        }
        int res = 0;
        for(int i = 0; i < n; i++) res = max(res, f[i]);
        return res;

    }
};
```

