
## 題目大意

有 n 張卡片，第 i 張的正面寫著 $a[i]$ ，背面寫著 $b[i]$ ，一開始所有卡片都是正面朝上，你可以進行以下操作至多 k 次：

-  選擇一個區間，將此區間所有卡片翻面。  

試問最後所有朝上的面的數字總和的最大值。

$1<=n<=2e5$   
$1<=k<=10$  
$1<=a[i],b[i]<=1e9$  

## 解題思路

### Solution 1 : DP

這題基本上就是 DP 裸題，我就懶得寫這個解法了。

### Solution 2 : 反悔貪心

跟 dp 一樣是複雜度最差 $O(NK)$ 的演算法，不過我覺得這方法蠻酷的而且大部分時候剪枝可以讓實際複雜度更好。

#### 什麼是反悔貪心？

如果這題要用一般貪心的話，大概只能用在 $k=1$ 的情況，也就是非常經典的最大連續和 dp ，不過當 k 更大的時候這個貪心就需要稍微改一下，讓每次貪心出來的最大連續和是未來可以悔改的。

#### 進入正題

我們做至多 k 次最大連續和，但是這邊注意到，假設我翻面的貢獻是 X ，那麼翻回來的貢獻就會是 -X ，因此，我們只需要在每一次 dp 完，把該次有翻面的區間都乘上負號，未來 dp 時就相當於給了他可以被翻回去的機會。

具體來說，我們一開始的初始獲利會是 $\sum_{i=1}^{n} a[i]$ ，新增一個陣列 arr ， $arr[i]=b[i]-a[i]$ ，這樣我們 dp 的時候都對 arr 做事，最後把初始獲利加上每一次 dp 的結果就好。

## AC 程式碼

```cpp
#include <bits/stdc++.h>

using namespace std;

using ll=long long;

using pii=pair<int,int>;

const int N=2e5+5;

ll an=0;

ll a[N],b[N];

ll arr[N];

ll dp[N];

signed main(){

    ios::sync_with_stdio(0),cin.tie(0);

    int n,k;cin>>n>>k;

    for(int i=1;i<=n;++i) cin>>a[i]>>b[i];

    for(int i=1;i<=n;++i){

        an+=a[i];

        arr[i]=b[i]-a[i];

    }

    for(int i=0;i<k;++i){

        int ml=-1,mr=-1;

        int le=-1;

        ll mx=-1e18;

        for(int j=1;j<=n;++j){

            dp[j]=arr[j]+max(dp[j-1],0ll);

            if(dp[j-1]<=0) le=j;

            if(dp[j]>mx){

                ml=le;mr=j;

                mx=dp[j];

            }

        }

        if(mx<=0) break;

        an+=mx;

        for(int j=ml;j<=mr;++j) arr[j]=-arr[j];

    }

    cout<<an<<'\n';

}
```



