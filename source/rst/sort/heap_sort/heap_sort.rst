*******************
堆排序
*******************

1. Worst-case time: :math:`O(n \lg n)`
2. Average time: :math:`O(n \lg n)`

.. code-block:: c++

  #include <iostream>
  #include <string>
  #include <vector>
  
  using namespace std;
  
  template <typename T>
  class HeapSort {
   public:
    static void sink(vector<T>& a, int k, int n) {
      while (2 * k <= n) {
        int j = 2 * k;
        if (j < n && a[j - 1] < a[j]) {
          ++j;
        }
        if (a[k - 1] >= a[j - 1]) {
          break;
        }
        swap(a[k - 1], a[j - 1]);
        k = j;
      }
    }
  
    static void sort(vector<T>& a) {
      int N = a.size();
      for (int k = N / 2; k >= 1; --k) {
        sink(a, k, N);
      }
      while (N > 1) {
        swap(a[0], a[--N]);
        sink(a, 1, N);
      }
    }
  };
  
  int main() {
    vector<int> v{3, 4, 5, 1, 2, 6, 10, 8, 7, 9};
    vector<string> vs{"April", "Tong", "Robert", "Sebastian"};
  
    HeapSort<int>::sort(v);
    for (auto e : v) cout << e << " ";
  
    cout << endl;
  
    HeapSort<string>::sort(vs);
    for (auto e : vs) cout << e << " ";
  }
