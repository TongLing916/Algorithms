*******************
分割数组
*******************

`Leetcode 410. 分割数组的最大值 <https://leetcode-cn.com/problems/split-array-largest-sum/>`_
=============================================================================================

思路
    - 首先，答案肯定在 ``[l=max(nums), r=sum(nums) + 1)`` 的范围内。
    - 然后，我们可以尝试直接搜索答案。
    - 假定一个可能的答案 ``C``，然后我们至少需要分成多少组才有可能是 ``C``。
    - 假设至少需要 ``k`` 组，如果 ``k > m``，说明 ``C`` 太小了，我们得扩大答案 ``l = C + 1``，否则，``r = m``。

.. code-block:: c++

    class Solution {
     public:
      bool check(vector<int>& nums, int x, long long m) {
        long long sum = 0;
        int cnt = 1;
        for (int i = 0; i < nums.size(); ++i) {
          if (sum + nums[i] > x) {
            ++cnt;
            sum = nums[i];
          } else {
            sum += nums[i];
          }
        }
        return cnt <= m;
      }
    
      int splitArray(vector<int>& nums, int m) {
        long long left = 0, right = 0;
        for (int i = 0; i < nums.size(); ++i) {
          right += nums[i];
          if (left < nums[i]) {
            left = nums[i];
          }
        }
        while (left < right) {
          long long mid = (left + right) >> 1;
          if (check(nums, mid, m)) {
            right = mid;
          } else {
            left = mid + 1;
          }
        }
        return left;
      }
    };


