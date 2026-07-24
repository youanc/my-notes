
## 題目大意

這是一個互動題，adaptive，電腦會生成1~200中其中三個數字作為答案，而你每次詢問可以問 K 個數字 $(1<=K<=200)$ ，電腦會告訴你這 K 個數字中有奇數還是偶數個答案中的數，而你要在 33 次以內猜到正確答案是哪三個數。

## 解題思路

### 奇偶性

注意到奇數等於奇數加偶數，所以只要用二分搜，每次切半後必定有一邊是奇數一邊偶數，這樣就可以至少找到其中一個答案。

### 將兩相異數分開

找到一個數以後就剩下兩個數，不過這時候就不能用前面的方法了因為切開後兩邊都會是偶數，沒辦法刪去其中一半，怎麼辦？ 這時候我們注意到：

- 兩相異數必有至少一位元相異  

所以只要枚舉每一個位元就可以把剩下兩個數分開，之後就只要用前面的方法二分搜就好，操作次數不會超過 $4\log_2{200}<33$  這樣就足以通過了。

## AC 程式碼

```cpp
#include <bits/stdc++.h>

using namespace std;

using ll=long long;

using pii=pair<int,int>;

const int N=200;

vector<int> an;

bool odd(int l,int r){

cout<<r-l+1<<'\n';

for(int i=l;i<=r;++i) cout<<i<<' ';

cout<<flush;

string s;cin>>s;

if(s=="correct") exit(0);

if(s=="odd") return true;

return false;

}

bool odd_(int l,int r,vector<int> &v){

cout<<r-l+1<<'\n';

for(int i=l;i<=r;++i){

cout<<v[i]<<' ';

}

cout<<flush;

string s;cin>>s;

if(s=="correct") exit(0);

if(s=="odd") return true;

return false;

}

void solve(vector<int> &v){

int len=v.size();

int le=0,ri=len-1;

while(le<=ri){

int mid=le+(ri-le)/2;

if(odd_(le,mid,v)) ri=mid-1;

else le=mid+1;

}

an.push_back(v[le]);

}

signed main(){

ios::sync_with_stdio(0),cin.tie(0);

int le=1,ri=N;

while(le<=ri){

int mid=le+(ri-le)/2;

if(odd(le,mid)) ri=mid-1;

else le=mid+1;

}

an.push_back(le);

int id;

for(int i=0;i<8;++i){

vector<int> v;

for(int j=1;j<=N;++j){

if(j==an[0]) continue;

if((j&(1<<i))) v.push_back(j);

}

cout<<v.size()<<'\n';

for(int x:v) cout<<x<<' ';

cout<<flush;

string s;cin>>s;

if(s=="correct") exit(0);

if(s=="even") continue;

id=i;

break;

}

vector<int> v1,v2;

for(int i=1;i<=N;++i){

if(i==an[0]) continue;

if(i&(1<<id)) v2.push_back(i);

else v1.push_back(i);

}

solve(v1);

solve(v2);

cout<<3<<'\n';

for(int a:an) cout<<a<<' ';

cout<<flush;

}
```


