# leetcode刷题思路

## 二分搜索（最左右边界）

> 左侧边界right=mid-1,返回left前要验证

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
      int first=left_bound(nums,target);
      int last=right_bound(nums,target);
      return new int[]{first,last};
    }
    int left_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，锁定左侧边界
            right = mid - 1;
        }
    }
    // 最后要检查 left 越界的情况
    if (left >= nums.length || nums[left] != target)
        return -1;
    return left;
}

int right_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，锁定右侧边界
            left = mid + 1;
        }
    }
    // 最后要检查 right 越界的情况
    if (right < 0 || nums[right] != target)
        return -1;
    return right;
}
}
```

## 滑动窗口（子串问题）

```java
public List<Integer> findAnagrams(String s, String t) {
    Map<Character, Integer> need = new HashMap<>();
    Map<Character, Integer> window = new HashMap<>();
    for (int i = 0; i < t.length(); i++) {
        char c = t.charAt(i);
        need.put(c, need.getOrDefault(c, 0) + 1);
    }

    int left = 0, right = 0;
    int valid = 0;
    List<Integer> res = new ArrayList<>(); // 记录结果
    while (right < s.length()) {
        char c = s.charAt(right);
        right++;
        // 进行窗口内数据的一系列更新
        if (need.containsKey(c)) {
            window.put(c, window.getOrDefault(c, 0) + 1);
            if (window.get(c).equals(need.get(c))) {
                valid++;
            }
        }
        // 判断左侧窗口是否要收缩
        while (right - left >= t.length()) {
            // 当窗口符合条件时，把起始索引加入 res
            if (valid == need.size()) {
                res.add(left);
            }
            char d = s.charAt(left);
            left++;
            // 进行窗口内数据的一系列更新
            if (need.containsKey(d)) {
                if (window.get(d).equals(need.get(d))) {
                    valid--;
                }
                window.put(d, window.get(d) - 1);
            }
        }
    }
    return res;
}
```

