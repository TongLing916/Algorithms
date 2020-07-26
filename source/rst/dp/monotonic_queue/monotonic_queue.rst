*******************
单调队列优化DP
*******************

题型
====

一个数组 ``nums`` 分成 ``k`` 组，求最大值，最小值，min-max，max-mean 等等。

思路
====

1. 状态表示：``dp[k][i] := 将nums[0] ... nums[i]分成k组``
2. 状态转移：思考最后一组的位置 ``j``，那么状态转移方程就变成类似 ``dp[k][i] = max(dp[k][i], dp[k - 1][j - 1] + sum(j .. i)``，其中，需要考虑 ``j`` 的取值范围，以及我们具体需要计算的东西。

例题
====

`Leetcode 410. 分割数组的最大值 <https://leetcode-cn.com/problems/split-array-largest-sum/>`_
---------------------------------------------------------------------------------------------

.. code-block:: c++

    typedef long long LL;
    class Solution {
     public:
      int splitArray(vector<int>& nums, int m) {
        const int n = nums.size();
        vector<LL> sums(n);
        vector<vector<LL>> dp(m + 1, vector<LL>(n, LLONG_MAX));
        sums[0] = nums[0];
        for (int i = 1; i < n; ++i) {
          sums[i] = sums[i - 1] + nums[i];
        }
        for (int i = 0; i < n; ++i) {
          dp[1][i] = sums[i];
        }
    
        // dp[i][j] := ans of sub-problem, splitting nums[0] ~ nums[j] into i groups
        for (int i = 2; i <= m; ++i) {
          for (int j = i - 1; j < n; ++j) {
            for (int k = 0; k < j; ++k) {
              dp[i][j] = min(dp[i][j], max(dp[i - 1][k], sums[j] - sums[k]));
            }
          }
        }
        return dp[m][n - 1];
      }
    };

`Leetcode 813. 最大平均值和的分组 <https://leetcode-cn.com/problems/largest-sum-of-averages/>`_
-----------------------------------------------------------------------------------------------

.. code-block:: c++

    class Solution {
     public:
      double largestSumOfAverages(vector<int>& A, int K) {
        const int N = A.size();
        vector<double> sum(N, 0.);
        sum[0] = A[0];
        for (int i = 1; i < N; ++i) {
          sum[i] = sum[i - 1] + A[i];
        }

        vector<vector<double>> dp(K + 1, vector<double>(N, 0.));
        for (int i = 0; i < N; ++i) {
          dp[1][i] = sum[i] / (i + 1);
        }

        // dp[k][i]: max when splitting [0, i] into k groups
        for (int k = 2; k <= K; ++k) {
          for (int i = k - 1; i < N; ++i) {
            for (int j = k - 1; j <= i; ++j) {
              dp[k][i] = max(
                  dp[k][i], dp[k - 1][j - 1] + (sum[i] - sum[j - 1]) / (i - j + 1));
            }
          }
        }

        return dp[K][N - 1];
      }
    };

`AcWing 298. 围栏 <https://www.acwing.com/problem/content/300/>`_
------------------------------------------------------------------

.. code-block:: c++

  #include <iostream>
  #include <vector>
  #include <algorithm>

  using namespace std;

  struct Worker {
    int l;  // max length
    int p;  // money per length
    int s;  // must contain
  };

  int solve(const int n, const int m, const vector<Worker>& workers) {
      // dp[i][n]: first i workers, first n woods
      vector<vector<int>> dp(m + 1, vector<int>(n + 1));

      for (int i = 1; i <= m; ++i) {
          for (int j = 1; j <= n; ++j) {
              // if worker i does not work
              dp[i][j] = dp[i - 1][j];

              // if worker i does not work at j
              dp[i][j] = max(dp[i][j], dp[i][j - 1]);

              // if worker (i - 1) works at j, start from k
              // then, j must be in [s, s + l - 1)
              // k must be in [max(1, j - l + 1), s]
              const int l = workers[i - 1].l, p = workers[i - 1].p, s = workers[i - 1].s;
              if (j < s || j > s + l - 1) {
                  continue;
              }

              const int lo = max(1, j - l + 1);
              for (int k = lo; k <= s; ++k) {
                  const int len = j - k + 1;
                  dp[i][j] = max(dp[i][j], dp[i - 1][k - 1] + p * len);
              }
          }
      }

      return dp[m][n];
  }

  int main() {
      int n, m;
      cin >> n >> m;
      vector<Worker> workers(m);
      for (int i = 0; i < m; ++i) {
          cin >> workers[i].l >> workers[i].p >> workers[i].s;
      }
      sort(workers.begin(), workers.end(), [](const Worker& a, const Worker& b) {
          return a.s < b.s; 
      });
      cout << solve(n, m, workers) << endl;
      return 0;
  }

`AcWing 299. 裁剪序列 <https://www.acwing.com/problem/content/301/>`_
----------------------------------------------------------------------

相关试题
========


