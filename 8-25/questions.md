给定一个字符串 s ，请将 s 分割成一些子串，使每个子串都是 回文串 ，返回 s 所有可能的分割方案。

回文串 是正着读和反着读都一样的字符串。

示例 1：

输入：s = "google"
输出：[["g","o","o","g","l","e"],["g","oo","g","l","e"],["goog","l","e"]]
示例 2：

输入：s = "aab"
输出：[["a","a","b"],["aa","b"]]
示例 3：

输入：s = "a"
输出：[["a"]]
 
提示：
1 <= s.length <= 16
s 仅由小写英文字母组成

**题解：**
将abc,分开为逗号，a,b,c
![image.png](http://sbcsimulate.top:8090/upload/2023/08/image-94d34c907aa44503aab46b1ae6475d59.png)

![image.png](http://sbcsimulate.top:8090/upload/2023/08/image-023759a1b4a5428eb9bbb0e3d2f78b55.png)

**判断回文串用双向指针来处理，或者直接将字符串反转，判断是否和原串相同**

假设每对相邻字符之间有个逗号，那么就看每个逗号是选还是不选。

也可以理解成：是否要把 s[i]s[i]s[i] 当成分割出的子串的最后一个字符。

```java
class Solution {
    private final List<List<String>> ans = new ArrayList<>();
    private final List<String> path = new ArrayList<>();
    private String s;

    public List<List<String>> partition(String s) {
        this.s = s;
        dfs(0, 0);
        return ans;
    }

    private boolean isPalindrome(int left, int right) {
        while (left < right)
            if (s.charAt(left++) != s.charAt(right--))
                return false;
        return true;
    }

    // start 表示当前这段回文子串的开始位置
    private void dfs(int i, int start) {
        if (i == s.length()) {
            ans.add(new ArrayList<>(path)); // 复制 path
            return;
        }

        // 不选 i 和 i+1 之间的逗号（i=n-1 时一定要选）
        if (i < s.length() - 1)
            dfs(i + 1, start);

        // 选 i 和 i+1 之间的逗号（把 s[i] 作为子串的最后一个字符）
        if (isPalindrome(start, i)) {
            path.add(s.substring(start, i + 1));
            dfs(i + 1, i + 1); // 下一个子串从 i+1 开始
            path.remove(path.size() - 1); // 恢复现场
        }
    }
}

```
