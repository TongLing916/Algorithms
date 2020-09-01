*******************
归并排序
*******************

1. Worst-case time: :math:`O(n \lg n)`
2. Average time: :math:`O(n \lg n)`

.. code-block:: c++

    #include <algorithm>
    #include <iostream>
    #include <vector>

    using namespace std;

    ostream& operator<<(ostream& os, const vector<int>& nums) {
      for (const int num : nums) {
        os << num << " ";
      }
      return os;
    }

    class MergeSort {
     public:
      void Merge(const int lo, const int mid, const int hi, vector<int>& a) {
        int i = lo;
        int j = mid + 1;

        for (int k = lo; k <= hi; ++k) {
          // Copy a[lo..hi] to aux[lo..hi].
          aux[k] = a[k];
        }
        for (int k = lo; k <= hi; ++k) {
          // Merge back to a[lo..hi]
          if (i > mid) {
            a[k] = aux[j++];
          } else if (j > hi) {
            a[k] = aux[i++];
          } else if (aux[j] < aux[i]) {
            a[k] = aux[j++];
          } else {
            a[k] = aux[i++];
          }
        }
      }

      virtual void Sort(vector<int>& a) = 0;

     protected:
      vector<int> aux;
    };

    class MergeSortTopDown : public MergeSort {
     public:
      void Sort(vector<int>& a) override {
        aux.resize(a.size(), -1);
        Sort(0, a.size() - 1, a);
      }

     private:
      void Sort(const int lo, const int hi, vector<int>& a) {
        if (hi <= lo) {
          return;
        }
        const int mid = lo + (hi - lo) / 2;
        Sort(lo, mid, a);
        Sort(mid + 1, hi, a);
        Merge(lo, mid, hi, a);
      }
    };

    class MergeSortBottomUp : public MergeSort {
     public:
      void Sort(vector<int>& a) override {
        const int N = a.size();
        aux.resize(a.size(), -1);
        for (int sz = 1; sz < N; sz = sz + sz) {
          for (int lo = 0; lo < N - sz; lo += sz + sz) {
            Merge(lo, lo + sz - 1, std::min(lo + sz + sz - 1, N - 1), a);
          }
        }
      }
    };

    int main() {
      vector<int> nums{
          364, 637, 341, 406, 747, 995, 234, 971, 571, 219, 993, 407, 416, 366, 315,
          301, 601, 650, 418, 355, 460, 505, 360, 965, 516, 648, 727, 667, 465, 849,
          455, 181, 486, 149, 588, 233, 144, 174, 557, 67,  746, 550, 474, 162, 268,
          142, 463, 221, 882, 576, 604, 739, 288, 569, 256, 936, 275, 401, 497, 82,
          935, 983, 583, 523, 697, 478, 147, 795, 380, 973, 958, 115, 773, 870, 259,
          655, 446, 863, 735, 784, 3,   671, 433, 630, 425, 930, 64,  266, 235, 187,
          284, 665, 874, 80,  45,  848, 38,  811, 267, 575};
      MergeSortTopDown mstd;
      MergeSortBottomUp msbu;
      msbu.Sort(nums);

      cout << nums << endl;
    }
