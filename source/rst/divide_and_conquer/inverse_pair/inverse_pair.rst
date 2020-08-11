*******************
逆序对
*******************

题型
====

给定一个数组，计算右边元素比左边元素大的对数。

思路
====

.. code-block:: c++

  using LL = long long;
  class Solution {
  public:
    int solve(vector<int>& nums) {
      res_ = 0;
      mergeSort(nums, 0, nums.size() - 1);
      return res_;
    }

  private:
    void mergeSort(vector<int>& nums, const int lo, const int hi) {
      if (lo >= hi) {
        return;
      }

      const int mid = lo + ((hi - lo) >> 1);
      mergeSort(nums, lo, mid);
      mergeSort(nums, mid + 1, hi);
      merge(nums, lo, mid, hi);
    }

    void merge(vector<int>& nums, const int lo, const int mid, const int hi) {
      if (lo >= hi) {
        return;
      }

      vector<LL> aux(hi - lo + 1);
      for (int i = lo; i <= hi; ++i) {
        aux[i - lo] = nums[i];
      }

      // ---------------------- Code here ----------------------
      int right = mid + 1;
      for (int k = lo; k <= mid; ++k) {
        while (right <= hi && /** [TODO] Specific condition */) {
          ++right;
        }
        /** [TODO] Change res */
      }
      // -------------------------------------------------------

      int i = lo, j = mid + 1;
      for (int k = lo; k <= hi; ++k) {
        const int a = i - lo;
        const int b = j - lo;
        if (i > mid) {
          nums[k] = aux[b];
          ++j;
        } else if (j > hi) {
          nums[k] = aux[a];
          ++i;
        } else if (aux[a] >= aux[b]) {
          nums[k] = aux[b];
          ++j;
        } else {
          nums[k] = aux[a];
          ++i;
        }
      }
    }

  private:
    int res_;
  };

例题
====

`AcWing 65. 数组中的逆序对 <https://www.acwing.com/problem/content/description/61/>`_
-------------------------------------------------------------------------------------

.. code-block:: c++

  class Solution {
  public:
    int inversePairs(vector<int>& nums) {
      ans_ = 0;
      mergeSort(nums, 0, nums.size() - 1);
      return ans_;
    }

  private:
    void mergeSort(vector<int>& nums, const int lo, const int hi) {
      if (lo >= hi) {
        return;
      }
      const int mid = lo + ((hi - lo) >> 1);
      mergeSort(nums, lo, mid);
      mergeSort(nums, mid + 1, hi);
      merge(nums, lo, mid, hi);
    }

    void merge(vector<int>& nums, const int lo, const int mid, const int hi) {
      if (lo >= hi) {
        return;
      }

      vector<int> aux(hi - lo + 1);
      for (int i = lo; i <= hi; ++i) {
        aux[i - lo] = nums[i];
      }

      int right = mid + 1;
      for (int k = lo; k <= mid; ++k) {
        while (right <= hi && aux[k - lo] > aux[right - lo]) {
          ++right;
        }
        ans_ += right - mid - 1;
      }

      int i = lo, j = mid + 1;
      for (int k = lo; k <= hi; ++k) {
        const int a = i - lo;
        const int b = j - lo;
        if (i > mid) {
          nums[k] = aux[b];
          ++j;
        } else if (j > hi) {
          nums[k] = aux[a];
          ++i;
        } else if (aux[a] >= aux[b]) {
          nums[k] = aux[b];
          ++j;
        } else {
          nums[k] = aux[a];
          ++i;
        }
      }
    }

  private:
    int ans_;
  };


`Leetcode 315. 计算右侧小于当前元素的个数 <https://leetcode-cn.com/problems/count-of-smaller-numbers-after-self/>`_
------------------------------------------------------------------------------------------------------------------

.. code-block:: c++

  class Solution {
  public:
    vector<int> countSmaller(vector<int>& nums) {
      res_.resize(nums.size(), 0);
      vector<pair<int, int>> nums2;
      for (int i = 0; i < nums.size(); ++i) {
        nums2.push_back({i, nums[i]});
      }
      mergeSort(nums2, 0, nums.size() - 1);
      return res_;
    }

  private:
    void mergeSort(vector<pair<int, int>>& nums, const int lo, const int hi) {
      if (lo >= hi) {
        return;
      }

      const int mid = lo + ((hi - lo) >> 1);
      mergeSort(nums, lo, mid);
      mergeSort(nums, mid + 1, hi);
      merge(nums, lo, mid, hi);
    }

    void merge(vector<pair<int, int>>& nums, const int lo, const int mid,
              const int hi) {
      vector<pair<int, int>> aux(hi - lo + 1);
      for (int i = lo; i <= hi; ++i) {
        aux[i - lo] = nums[i];
      }

      int right = mid + 1;
      for (int k = lo; k <= mid; ++k) {
        while (right <= hi && aux[k - lo].second > aux[right - lo].second) {
          ++right;
        }
        res_[aux[k - lo].first] += right - mid - 1;
      }

      int i = lo, j = mid + 1;
      for (int k = lo; k <= hi; ++k) {
        const int a = i - lo;
        const int b = j - lo;
        if (i > mid) {
          nums[k] = aux[b];
          ++j;
        } else if (j > hi) {
          nums[k] = aux[a];
          ++i;
        } else if (aux[a].second > aux[b].second) {
          nums[k] = aux[b];
          ++j;
        } else {
          nums[k] = aux[a];
          ++i;
        }
      }
    }

  private:
    vector<int> res_;
  };

`Leetcode 327. 区间和的个数 <https://leetcode-cn.com/problems/count-of-range-sum/>`_
------------------------------------------------------------------------------------

.. code-block:: c++

	using LL = long long;
	class Solution {
	 public:
	  int countRangeSum(vector<int>& nums, int lower, int upper) {
	    if (nums.empty()) {
	      return 0;
	    }
	    lower_ = lower, upper_ = upper;
	    const int N = nums.size();
	    vector<LL> sums(N + 1, 0);
	    for (int i = 1; i <= N; ++i) {
	      sums[i] = sums[i - 1] + nums[i - 1];
	    }
	    mergeSort(sums, 0, N);
	    return ans_;
	  }

	 private:
	  void mergeSort(vector<LL>& nums, const int lo, const int hi) {
	    if (lo >= hi) {
	      return;
	    }
	    const int mid = lo + ((hi - lo) >> 1);
	    mergeSort(nums, lo, mid);
	    mergeSort(nums, mid + 1, hi);
	    merge(nums, lo, mid, hi);
	  }

	  void merge(vector<LL>& nums, const int lo, const int mid, const int hi) {
	    if (lo >= hi) {
	      return;
	    }

	    vector<LL> aux(hi - lo + 1);
	    for (int i = lo; i <= hi; ++i) {
	      aux[i - lo] = nums[i];
	    }

	    int last_right = mid + 1;
	    for (int k = lo; k <= mid; ++k) {
	      int right = last_right;
	      while (right <= hi && aux[right - lo] - aux[k - lo] < lower_) {
	        ++right;
	      }
	      last_right = right;
	      while (right <= hi && aux[right - lo] - aux[k - lo] <= upper_) {
	        ++right;
	        ++ans_;
	      }
	    }

	    int i = lo, j = mid + 1;
	    for (int k = lo; k <= hi; ++k) {
	      const int a = i - lo;
	      const int b = j - lo;
	      if (i > mid) {
	        nums[k] = aux[b];
	        ++j;
	      } else if (j > hi) {
	        nums[k] = aux[a];
	        ++i;
	      } else if (aux[a] >= aux[b]) {
	        nums[k] = aux[b];
	        ++j;
	      } else {
	        nums[k] = aux[a];
	        ++i;
	      }
	    }
	  }

	 private:
	  int ans_;
	  int lower_, upper_;
	};

`Leetcode 493. 翻转对 <https://leetcode-cn.com/problems/reverse-pairs/>`_
--------------------------------------------------------------------------

.. code-block:: c++

  using LL = long long;
  class Solution {
   public:
    int reversePairs(vector<int>& nums) {
      res_ = 0;
      mergeSort(nums, 0, nums.size() - 1);
      return res_;
    }

   private:
    void mergeSort(vector<int>& nums, const int lo, const int hi) {
      if (lo >= hi) {
        return;
      }

      const int mid = lo + ((hi - lo) >> 1);
      mergeSort(nums, lo, mid);
      mergeSort(nums, mid + 1, hi);
      merge(nums, lo, mid, hi);
    }

    void merge(vector<int>& nums, const int lo, const int mid, const int hi) {
      vector<LL> aux(hi - lo + 1);
      for (int i = 0; i < aux.size(); ++i) {
        aux[i] = nums[i + lo];
      }

      int i = lo, j = mid + 1;

      int right = mid + 1;
      for (int k = lo; k <= mid; ++k) {
        while (right <= hi && aux[k - lo] > aux[right - lo] * 2) {
          ++right;
        }
        res_ += right - mid - 1;
      }

      for (int k = lo; k <= hi; ++k) {
        if (i > mid) {
          nums[k] = aux[j - lo];
          ++j;
        } else if (j > hi) {
          nums[k] = aux[i - lo];
          ++i;
        } else if (aux[i - lo] >= aux[j - lo]) {
          nums[k] = aux[j - lo];
          ++j;
        } else {
          nums[k] = aux[i - lo];
          ++i;
        }
      }
    }

   private:
    int res_;
  };
