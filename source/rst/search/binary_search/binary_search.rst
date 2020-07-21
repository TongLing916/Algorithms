*******************
二分查找
*******************

.. code-block:: c++

    #include <iostream>
    #include <vector>
    using namespace std;

    int binary_search(const vector<int>& A, const int val) {
      int l = 0;
      int r = static_cast<int>(A.size());
      while (l < r) {
        const int m = l + ((r - l) >> 1);
        if (A[m] == val) {
          return m;
        } else if (A[m] > val) {
          r = m;
        } else {
          l = m + 1;
        }
      }
      return -1;
    }

    int upper_bound(const vector<int>& A, int val, int l, int r) {
      while (l < r) {
        const int m = l + ((r - l) >> 1);
        if (A[m] > val) {
          r = m;
        } else {
          l = m + 1;
        }
      }
      return l;
    }

    int lower_bound(const vector<int>& A, const int val, int l, int r) {
      while (l < r) {
        const int m = l + ((r - l) >> 1);
        if (A[m] >= val) {
          r = m;
        } else {
          l = m + 1;
        }
      }
      return l;
    }

    int main() {
      vector<int> A{1, 2, 2, 2, 4, 4, 5};
      cout << lower_bound(A, 0, 0, A.size()) << endl;  // 0
      cout << lower_bound(A, 2, 0, A.size()) << endl;  // 1
      cout << lower_bound(A, 3, 0, A.size()) << endl;  // 4
      cout << upper_bound(A, 2, 0, A.size()) << endl;  // 4
      cout << upper_bound(A, 4, 0, A.size()) << endl;  // 6
      cout << upper_bound(A, 5, 0, A.size()) << endl;  // 7
    }