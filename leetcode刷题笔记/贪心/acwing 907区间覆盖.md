将所有区间按照左端点从小到大进行排序

从前往后枚举每个区间，在所有能覆盖start的区间中，选择右端点的最大区间，然后将start更新成右端点的最大值

```c++
#include<bits/stdc++.h>

using namespace std;

struct range{
    int l, r;  
};

int main()
{
    int start, end;
    cin >> start >> end;
    int n;
    cin >> n;
    vector<range> ranges(n);
    for(int i = 0; i < n; i ++)
    {
        cin >> ranges[i].l >> ranges[i].r;
    }
    
    sort(ranges.begin(), ranges.end(), [](range &r1, range &r2)
    {
        return r1.l < r2.l; 
    });
    
    int ans = 0;
    bool success = false;
    for(int i = 0; i < n; i ++)
    {
        int j = i, r = INT_MIN;
        while(j < n && ranges[j].l <= start)
        {
            r = max(r, ranges[j].r);
            j ++;
        }
        
        if(r < start)
        {
            ans = -1;
            break;
        }
        
        ans ++;
        
        if(r >= end)
        {
            success = true;
            break;
        }
        
        start = r;
        i = j - 1;
    }
    
    if(!success) ans = -1;
    cout << ans << endl;
    return 0;
}
```

