############################
排列
############################

`31. 下一个排列 <https://leetcode-cn.com/problems/next-permutation/>`_

.. code-block:: c++

  class Solution {
   public:
    void nextPermutation(vector<int>& nums) {
      if (nums.size() <= 1) {
        return;
      }
  
      int i = nums.size() - 1;
      while (i > 0 && nums[i - 1] >= nums[i]) {
        --i;
      }
  
      if (i == 0) {
        // The array is already in order (large to low)
        reverse(nums.begin(), nums.end());
      } else {
        // Right part of i is from large to low
        // Swap [i] with first one larger than [i] (from right to left)
        // Reverse right part
        --i;
        int k = nums.size() - 1;
        while (k > i && nums[k] <= nums[i]) {
          --k;
        }
        swap(nums[k], nums[i]);
        reverse(nums.begin() + i + 1, nums.end());
      }
    }
  };

`46. 全排列 <https://leetcode-cn.com/problems/permutations/>`_

.. code-block:: c++

`47. 全排列 II <https://leetcode-cn.com/problems/permutations-ii/>`_

.. code-block:: c++

`60. 排列序列 <https://leetcode-cn.com/problems/permutation-sequence/>`_

.. code-block:: c++