*******************
快速排序
*******************

1. Worst-case time: :math:`O(n^2)`
2. Average time: :math:`O(n \lg n)`

.. code-block:: python

  def quickSort(alist):
    quickSortHelper(alist,0,len(alist)-1)  # How to choose pivot? Median of three

  def quickSortHelper(alist,first,last):
    if first<last:

        splitpoint = partition(alist,first,last)

        quickSortHelper(alist,first,splitpoint-1)
        quickSortHelper(alist,splitpoint+1,last)


  def partition(alist,first,last):
    pivotvalue = alist[first]

    leftmark = first+1
    rightmark = last

    done = False
    while not done:

        while leftmark <= rightmark and alist[leftmark] <= pivotvalue:
            leftmark = leftmark + 1

        while alist[rightmark] >= pivotvalue and rightmark >= leftmark:
            rightmark = rightmark -1

        if rightmark < leftmark:
            done = True
        else:
            temp = alist[leftmark]
            alist[leftmark] = alist[rightmark]
            alist[rightmark] = temp

    temp = alist[first]
    alist[first] = alist[rightmark]
    alist[rightmark] = temp


    return rightmark

  alist = [54,26,93,17,77,31,44,55,20]
  quickSort(alist)
  print(alist)


.. code-block:: c++

  #include <algorithm>
  #include <iostream>
  #include <vector>
  
  using namespace std;
  
  class QuickSort {
   public:
    void sort(vector<int>& nums) {
      int n = nums.size();
      if (n <= 1) return;
      random_shuffle(nums.begin(), nums.end());
      sort(nums, 0, n - 1);
    }
  
    // for small subarrays, we can consider use insertion sort
    void sort(vector<int>& nums, int lo, int hi) {
      if (hi <= lo) return;
      int pivot = partition(nums, lo, hi);
      sort(nums, lo, pivot - 1);
      sort(nums, pivot + 1, hi);
    }
  
    int partition(vector<int>& nums, int lo, int hi) {
      int i = lo, j = hi + 1;
      int v = nums[lo];  // we can find the median and put it in the lo position.
      while (1) {
        while (nums[++i] < v)
          if (i == hi) break;
        while (nums[--j] > v)
          if (j == lo) break;
        if (i >= j) break;
        swap(nums[i], nums[j]);
      }
      swap(nums[lo], nums[j]);
      return j;
    }
  };
  
  // median-of-3
  // small subarrays sorted using insertion sort
  class QuickSortOpt {
   public:
    const int INSERTION_SORT_CUTOFF = 8;
    void sort(vector<int>& nums) {
      int n = nums.size();
      if (n <= 1) return;
      random_shuffle(nums.begin(), nums.end());
      sort(nums, 0, n - 1);
    }
  
    // for small subarrays, we can consider use insertion sort
    void sort(vector<int>& nums, int lo, int hi) {
      if (hi <= lo) return;
  
      int n = hi - lo + 1;
      if (n <= INSERTION_SORT_CUTOFF) {
        insertionSort(nums, lo, hi);
        return;
      }
  
      int pivot = partition(nums, lo, hi);
      sort(nums, lo, pivot - 1);
      sort(nums, pivot + 1, hi);
    }
  
    int partition(vector<int>& nums, int lo, int hi) {
      int i = lo, j = hi + 1;
      int m = median3(nums, lo, lo + (j - i) / 2, hi);
      swap(nums[lo], nums[m]);
      int v = nums[lo];  // we can find the median and put it in the lo position.
      while (1) {
        while (nums[++i] < v)
          if (i == hi) break;
        while (nums[--j] > v)
          if (j == lo) break;
        if (i >= j) break;
        swap(nums[i], nums[j]);
      }
      swap(nums[lo], nums[j]);
      return j;
    }
  
    void insertionSort(vector<int>& nums, int lo, int hi) {
      for (int i = 1; i < nums.size(); ++i)
        for (int j = i; j > 0 && nums[j] < nums[j - 1]; --j)
          swap(nums[j - 1], nums[j]);
    }
  
    int median3(vector<int>& nums, int i, int j, int k) {
      if (nums[i] <= nums[j] && nums[j] <= nums[k])
        return j;
      else if (nums[j] <= nums[i] && nums[i] <= nums[k])
        return i;
      else
        return k;
    }
  };
  
  class Quick3Way  // Consider the equal elements
  {
   public:
    void sort(vector<int>& nums) {
      int n = nums.size();
      if (n <= 1) return;
      random_shuffle(nums.begin(), nums.end());
      sort(nums, 0, n - 1);
    }
  
    void sort(vector<int>& nums, int lo, int hi) {
      if (hi <= lo) return;
      int lt = lo, i = lo + 1, gt = hi;
      int v = nums[lo];
      while (i <= gt) {
        if (nums[i] < v)
          swap(nums[lt++], nums[i++]);
        else if (nums[i] > v)
          swap(nums[i], nums[gt--]);
        else
          ++i;
      }
      sort(nums, lo, lt - 1);
      sort(nums, gt + 1, hi);
    }
  };
  
  int main() {
    vector<int> test1{9, 7, 6, 5, 4, 3, 2, 1};
    vector<int> test2{5, 7, 6, 5, 4, 3, 5, 5};
    vector<int> test3{5, 7, 6,  5,  4,  3,  5,  5,  10, 9,
                      8, 2, 12, 15, 29, 10, 28, 30, 32};
    QuickSort quickSort;
    QuickSortOpt quickSortOpt;
    Quick3Way quick3Way;
    quickSort.sort(test1);
    quick3Way.sort(test2);
    quickSortOpt.sort(test3);
    for (auto& num : test3) cout << num << " ";
    cout << endl;
  }