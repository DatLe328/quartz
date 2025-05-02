https://codeforces.com/blog/entry/94437
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;

ll start = chrono::steady_clock().now().time_since_epoch().count();
mt19937_64 rng(start);

int rand_int(int l, int r) {
    return uniform_int_distribution<int>(l, r)(rng);
}
double rand_double(double l, double r) {
    return uniform_real_distribution<double>(l, r)(rng);
}

double temperature = 1;   // nhiệt độ hiện tại trong Simulated Annealing
double initial_temperature = 1e9; // 'nhiệt độ ban đầu

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    temperature = initial_temperature;
    int best_solution = 0;  // giữ giá trị kết quả tốt nhất
    int current_solution = 0;  // lưu giá trị hiện tại
    vetor<int> best_solution_trace;
    while (true) {
        ll current = chrono::steady_clock().now().time_since_epoch().count();
        if (current - start >= 1.99 * 1e9) break;
		// Random state
		int current_state_val;
		vetor<int> c;
		// If current_state_val better than current_solution
		if (current_state_val > best_solution) {
            best_solution = current_state_val;
            // asssign best_solution_trace
        } else {
            double acceptance_probability = exp((current_state_val - current_solution) / temperature);
            // current solution is in acceptance range
            if (rand_double(0.0, 1.0) <= acceptance_robability) {
                current_solution = current_state_val;
            } else {
                for (int i = 0; i < n; i++) {
                    current_state_val[i] = best_solution[i];
                }
            }
        }
        temperature *= 0.9999;  // Giảm nhiệt độ từ từ
    }

    cout << bestSolution << '\n';  // In kết quả tốt nhất

    return 0;
}

```