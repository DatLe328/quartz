```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;

const int MAX_N = 51;
int state_save[MAX_N]; 
int a[MAX_N];     
int state[MAX_N];      
int dp[MAX_N];            
double temp = 1;   
double START_TEMP = 1e9; 

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    freopen("subrev.in", "r", stdin);
    freopen("subrev.out", "w", stdout);
    srand(2); 
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        state[i] = rand() % 2; 
        cin >> a[i];       
    }

    temp = START_TEMP;
    int best_score = 0; 
    int prev_score = 0;  

    while (clock() / (double) CLOCKS_PER_SEC <= 1.99) {
        vector<int> pos;  
        for (int i = 0; i < n; i++) {
            state_save[i] = state[i];  
        }

        for (int it = 0; it < n * temp / START_TEMP; it++) {
            state[rand() % n] ^= 1;  
        }

        for (int i = 0; i < n; i++) {
            if (state[i]) {
                pos.push_back(i);
            }
        }

        int sz = pos.size();
        for (int i = 0; i < sz / 2; i++) {
            swap(a[pos[i]], a[pos[sz - 1 - i]]);
        }

        vector<int> lis;
        for (int i = 0; i < n; i++) {
            auto it = upper_bound(lis.begin(), lis.end(), a[i]);
            if (it == lis.end()) lis.push_back(a[i]);
            else *it = a[i];
        }
        int new_score = (int)lis.size();

        best_score = max(best_score, new_score);

        for (int i = 0; i < sz / 2; i++) {
            swap(a[pos[i]], a[pos[sz - 1 - i]]);
        }

        if (new_score > prev_score) {
            prev_score = new_score;
        } else {
            double acceptance = exp((new_score - prev_score) / temp);
            int x = 1 + rand();
            int y = 1 + rand();
            x %= y;
            if (x / (double) y <= acceptance) {
                prev_score = new_score;
            } else {
                for (int i = 0; i < n; i++) {
                    state[i] = state_save[i];
                }
            }
        }
        temp *= 0.9999; 
    }

    cout << best_score << '\n';  

    return 0;
}

```