# 392. 判断子序列

[题目链接：力扣](https://leetcode.cn/problems/is-subsequence/description/?envType=study-plan-v2&envId=selected-coding-interview) · 难度：简单

## 题目描述

给定两个字符串 `s` 和 `t`，判断 `s` 是否是 `t` 的子序列。

子序列可以通过删除原字符串中的零个或多个字符得到，但保留下来的字符必须维持原来的相对顺序。例如，`ace` 是 `abcde` 的子序列，`aec` 则不是。

### 示例

```text
输入：s = "abc", t = "ahbgdc"
输出：true

输入：s = "axc", t = "ahbgdc"
输出：false
```

### 约束

- `0 <= s.length <= 100`
- `0 <= t.length <= 10^4`
- 两个字符串均只包含小写英文字母。

**进阶：** 如果需要对同一个 `t` 判断大量不同的 `s`，如何优化？

## 解题思路：双指针

用指针 `i` 指向 `s` 中下一个待匹配的字符，指针 `j` 从左到右扫描 `t`。每次比较时，若字符相同，则 `i` 向右移动；无论是否相同，`j` 都向右移动。扫描结束后，如果 `i` 已到达 `s` 的末尾，说明所有字符都按顺序匹配成功。

每次都选择 `t` 中最早出现的可匹配字符，能为后续字符留下尽可能多的空间，因此不会错过可行的匹配。空字符串无需匹配任何字符，所以始终是子序列。

```javascript
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var isSubsequence = function (s, t) {
  let i = 0;
  let j = 0;

  while (i < s.length && j < t.length) {
    if (s[i] === t[j]) i++;
    j++;
  }

  return i === s.length;
};
```

时间复杂度为 `O(|t|)`，额外空间复杂度为 `O(1)`。

## 进阶思路

当同一个 `t` 要被反复查询时，可以预处理每个位置之后各字母下一次出现的位置。对于每个 `s`，按顺序查表并跳到匹配字符之后；若某个字符找不到，则返回 `false`。字母表大小为 26，预处理的时间和空间复杂度均为 `O(26 × |t|)`，每次查询耗时 `O(|s|)`。

## 交互动画

在浏览器中直接打开 [index.html](./index.html)，可以自动播放或单步观察双指针的移动，也可以输入字符串尝试。演示页面将每个输入限制为 30 个字符，方便观察；题目的长度限制见上方约束。
