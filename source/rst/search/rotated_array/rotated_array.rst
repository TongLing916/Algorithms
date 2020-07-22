*******************
旋转数组
*******************

思路
----------
Note: 旋转数组仍是部分单调的，所以我们仍可以使用二分查找，
解决问题的关键重要的是找到正确的区间。 

寻找目标值
----------

如果 ``nums[mid]`` 小于 ``target``，那么该继续向左还是向右进行搜索？


如果 ``nums[mid]`` 大于 ``target``，那么该继续向左还是向右进行搜索？

`Leetcode 33. 搜索旋转排序数组 <https://leetcode-cn.com/problems/search-in-rotated-sorted-array/>`_

.. code-block:: c++


寻找最小值
----------

考虑数组中的最后一个元素 ``n``：在最小值右侧的元素，它们的值一定都小于等于 ``n``, 
而在最小值左侧的元素，它们的值一定都大于等于 ``n``。
因此，我们可以根据这一条性质，通过二分查找的方法找出最小值。

`Leetcode 153. 寻找旋转排序数组中的最小值 <https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/>`_

.. code-block:: c++

`Leetcode 154. 寻找旋转排序数组中的最小值 II <https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/>`_

.. code-block:: c++

  class Solution {
    public:
    int findMin(vector<int>& nums) {
      if (nums.empty()) {
        return -1;
      }
      int lo = 0, hi = nums.size() - 1;
      while (lo < hi) {
        const int mid = lo + ((hi - lo) >> 1);

        if (nums[mid] < nums[hi]) {
          hi = mid;
        } else if (nums[mid] > nums[hi]) {
          lo = mid + 1;
        } else {
          --hi;
        }
      }

      return nums[lo];
    }
  };


寻找最大值
----------




