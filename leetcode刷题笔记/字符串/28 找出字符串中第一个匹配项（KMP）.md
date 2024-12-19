思路

从每个字符串都从1开始

首先构造next数组

next[i] = j 的含义是以i为长度的串，前后缀相等的长度为j

next[1] = 0特判

```c++
class Solution {
public:
    int strStr(string s, string p) {
        int n = s.size(), m = p.size();
        s = ' ' + s, p = ' ' + p;
        vector<int> next(m + 1);
        for(int i = 2, j = 0; i <= m; i ++)
        {
            while(j && p[i] != p[j + 1]) j = next[j];
            if(p[i] == p[j + 1]) j ++;
            next[i] = j;
        }

        for(int i = 1, j = 0; i <= n; i ++)
        {
            while(j && s[i] != p[j + 1]) j = next[j];
            if(s[i] == p[j + 1]) j ++;
            if(j == m) return i - m;
        }

        return -1;
    }
};
```

