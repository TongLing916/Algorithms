*******************
插入排序
*******************

1. Worst-case time: :math:`O(n^2)`
2. Average time: :math:`O(n^2)`

.. code-block:: c++

    #include <iostream>
    #include <vector>
    
    using namespace std;
    
    ostream& operator<<(ostream& os, const vector<int>& nums) {
      for (const int num : nums) {
        os << num << " ";
      }
      return os;
    }
    
    class InsertionSort {
     public:
      static void sort(vector<int>& nums) { sort(0, nums.size() - 1, nums); }
    
     private:
      static void sort(const int lo, const int hi, vector<int>& nums) {
        for (int i = lo + 1; i <= hi; ++i) {
          for (int j = i; j > lo && nums[j] < nums[j - 1]; --j) {
            swap(nums[j - 1], nums[j]);
          }
        }
      }
    };
    
    int main() {
      vector<int> nums{2, 4, 5, 8, 1, 3, 9, 19, 10};
      InsertionSort::sort(nums);
      cout << nums << endl;
    }