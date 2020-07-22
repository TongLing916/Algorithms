*******************
寻找数字
*******************

`260. 只出现一次的数字 III <https://leetcode-cn.com/problems/single-number-iii/>`_

.. code-block:: c++

  class Solution {
  public:
      vector<int> singleNumber(vector<int>& nums) {
          int diff = accumulate(nums.begin(), nums.end(), 0, bit_xor<int>());
          diff &= -diff;
          vector<int> rets(2, 0);
          for (int num : nums)
              rets[!(num & diff)] ^= num;
          return rets;
      }
  };