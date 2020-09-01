*******************
冒泡排序
*******************

.. code-block:: c++

  #include <iostream>
  #include <vector>
  
  using namespace std;
  
  class BubbleSort {
   public:
    void sort(vector<int>& nums) {
      bool exchanges = true;
      for (int length = nums.size() - 1; length > 0 && exchanges; --length) {
        exchanges = false;
        for (int i = 0; i < length; ++i)
          if (nums[i] > nums[i + 1]) {
            swap(nums[i], nums[i + 1]);
            exchanges = true;
          }
      }
    }
  };
  
  std::ostream& operator<<(std::ostream& stream, const vector<int>& nums) {
    for (const auto& num : nums) stream << num << " ";
    return stream;
  }
  
  int main() {
    BubbleSort bs;
    vector<int> nums{2, 4, 5, 8, 1, 3, 9, 19, 10};
    bs.sort(nums);
    cout << nums << endl;
  }