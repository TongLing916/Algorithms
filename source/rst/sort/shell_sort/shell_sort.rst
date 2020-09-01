*******************
希尔排序
*******************

.. code-block:: c++

  #include <iostream>
  #include <vector>
  
  using namespace std;
  
  template <typename T>
  class ShellSort {
   public:
    static void sort(vector<T>& v) {
      int n = v.size();
      for (int gap = n / 2; gap > 0; gap /= 2)
        for (int i = gap; i < n; ++i) {
          int tmp = v[i];
          int j = i;
          for (; j >= gap && v[j - gap] > tmp; j -= gap) {
            v[j] = v[j - gap];
          }
          v[j] = tmp;
        }
    }
  };
  
  int main() {
    vector<int> v{1, 2, 5, 9, 8, 7, 3, 4, 6, 0};
    ShellSort<int>::sort(v);
    for (auto e : v) cout << e << " ";
  }