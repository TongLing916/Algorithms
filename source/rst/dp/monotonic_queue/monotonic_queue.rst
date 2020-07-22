*******************
单调队列优化DP
*******************

`Leetcode 410. 分割数组的最大值 <https://leetcode-cn.com/problems/split-array-largest-sum/>`_
==============================================================================================

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
==============================================================================================

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
==============================================================================================

`AcWing 299. 裁剪序列 <https://www.acwing.com/problem/content/301/>`_
==============================================================================================