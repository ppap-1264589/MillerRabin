## [Trang chủ](https://ppap-1264589.github.io/interesting-solution)

## Ý nghĩa

Một thuật toán rất khỏe để phân tích một số ra thừa số nguyên tố

## Tài liệu

[cp algorithms](https://cp-algorithms.com/algebra/primality_tests.html)


## Code : [MILLER.cpp](https://ideone.com/BjgW4k)

``` c++
#include <bits/stdc++.h>
#define up(i,a,b) for (int i = (int)a; i <= (int)b; i++)
#define u64 unsigned long long
#define u128 __uint128_t
using namespace std;

const vector<u64> Milbase = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37};

u64 fpow(u64 base, u64 d, u64 MOD){
    u64 res = 1;
    base %= MOD;
    for (; d; d >>= 1, base = (u128)base*base % MOD){
        if (d & 1) res =    (u128)res*base % MOD;
    }
    return res;
}

bool witness(u64 n, u64 base, u64 d, int cnt2){
    u64 x = fpow(base, d, n);
    if (x == 1 || x == n-1) return false;
    up(i,1,cnt2-1){
        x = (u128)x*x % n;
        if (x == n-1) return false;
    }
    return true;
}

bool MillerRabin(u64 n){
    if (n < 2) return false;

    u64 d = n-1;
    int cnt2 = 0;
    while ((d & 1) == 0){
        d >>= 1;
        ++cnt2;
    }

    for (auto base : Milbase){
        if (n == base) return true;
        if (n % base == 0) return false;
        if (witness(n, base, d, cnt2)){
            return false;
        }
    }
    return true;
}


signed main(){
    ios_base::sync_with_stdio(false);
    cin.tie(0);
    #define Task "A"
    if (fopen(Task".inp", "r")){
        freopen(Task".inp", "r", stdin);
        freopen(Task".out", "w", stdout);
    }

    u64 x;
    cin >> x;
    cout << MillerRabin(x);
}
```

### Chú ý

Nếu phải kiểm tra tính nguyên tố của một số lớn khoảng 1e18, việc nhân hai số có thể tràn số, nên ta phải dùng kiểu số ngầm trong C++ là __uint128_t

Tuy nhiên không phải trình chấm nào cũng chấp nhận kiểu số này. Ví dụ như trình chấm Themis trong các kì thi HSG ở Việt Nam thì thậm chí còn không được sử dụng

Thuật này chạy ổn trên các trình chấm online
