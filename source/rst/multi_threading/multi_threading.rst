############################
多线程
############################

知识点
======

1. `std::mutex::unlock <https://en.cppreference.com/w/cpp/thread/mutex/unlock>`_: The mutex must be locked by the current thread of execution, otherwise, the behavior is undefined.
2. `std::condition_variable::wait <http://www.cplusplus.com/reference/condition_variable/condition_variable/wait/>`_

例题
====

`Leetcode 1114. 按序打印 <https://leetcode-cn.com/problems/print-in-order/>`_

.. code-block:: c++

  class Foo {
   public:
    Foo() {
      lock2_.lock();
      lock3_.lock();
    }
  
    void first(function<void()> printFirst) {
      // printFirst() outputs "first". Do not change or remove this line.
      printFirst();
      lock2_.unlock();
    }
  
    void second(function<void()> printSecond) {
      lock2_.lock();
      // printSecond() outputs "second". Do not change or remove this line.
      printSecond();
      lock3_.unlock();
    }
  
    void third(function<void()> printThird) {
      lock3_.lock();
      // printThird() outputs "third". Do not change or remove this line.
      printThird();
      lock3_.unlock();
    }
  
   private:
    std::mutex lock2_;
    std::mutex lock3_;
  };
  
`Leetcode 1115. 交替打印FooBar <https://leetcode-cn.com/problems/print-foobar-alternately/>`_

.. code-block:: c++

  class FooBar {
   private:
    int n;
    std::mutex lk1_, lk2_;
  
   public:
    FooBar(int n) {
      this->n = n;
      lk2_.lock();
    }
  
    void foo(function<void()> printFoo) {
      for (int i = 0; i < n; i++) {
        lk1_.lock();
        // printFoo() outputs "foo". Do not change or remove this line.
        printFoo();
        lk2_.unlock();
      }
    }
  
    void bar(function<void()> printBar) {
      for (int i = 0; i < n; i++) {
        lk2_.lock();
        // printBar() outputs "bar". Do not change or remove this line.
        printBar();
        lk1_.unlock();
      }
    }
  };

.. code-block:: c++

  class FooBar {
   private:
    int n;
    mutex mu_;
    condition_variable cond_;
    bool state = false;
  
   public:
    FooBar(int n) { this->n = n; }
  
    void foo(function<void()> printFoo) {
      for (int i = 0; i < n; i++) {
        {
          unique_lock<mutex> lk(mu_);
          cond_.wait(lk, [this]() { return state == false; });
          // printFoo() outputs "foo". Do not change or remove this line.
          printFoo();
          state = true;
        }
  
        cond_.notify_one();
      }
    }
  
    void bar(function<void()> printBar) {
      for (int i = 0; i < n; i++) {
        {
          unique_lock<mutex> lk(mu_);
          cond_.wait(lk, [this]() { return state == true; });
          // printBar() outputs "bar". Do not change or remove this line.
          printBar();
          state = false;
        }
  
        cond_.notify_one();
      }
    }
  };

.. code-block:: c++

  class FooBar {
   private:
    int n;
    std::atomic_bool flag = false;
  
   public:
    FooBar(int n) { this->n = n; }
  
    void foo(function<void()> printFoo) {
      for (int i = 0; i < n; i++) {
        while (flag.load(std::memory_order_acquire)) {
          std::this_thread::yield();
        }
        // printFoo() outputs "foo". Do not change or remove this line.
        printFoo();
        flag.store(true, std::memory_order_release);
      }
    }
  
    void bar(function<void()> printBar) {
      for (int i = 0; i < n; i++) {
        while (!flag.load(std::memory_order_acquire)) {
          std::this_thread::yield();
        }
        // printBar() outputs "bar". Do not change or remove this line.
        printBar();
        flag.store(false, std::memory_order_release);
      }
    }
  };

`Leetcode 1116. 打印零与奇偶数 <https://leetcode-cn.com/problems/print-zero-even-odd/>`_

.. code-block:: c++

  class ZeroEvenOdd {
   private:
    int n;
    std::mutex lk_zero_, lk_even_, lk_odd_;
  
   public:
    ZeroEvenOdd(int n) {
      this->n = n;
      lk_even_.lock();
      lk_odd_.lock();
    }
  
    // printNumber(x) outputs "x", where x is an integer.
    void zero(function<void(int)> printNumber) {
      for (int i = 0; i < n; ++i) {
        lk_zero_.lock();
        printNumber(0);
        if (i & 1) {
          lk_even_.unlock();
        } else {
          lk_odd_.unlock();
        }
      }
    }
  
    void even(function<void(int)> printNumber) {
      for (int i = 2; i <= n; i += 2) {
        lk_even_.lock();
        printNumber(i);
        lk_zero_.unlock();
      }
    }
  
    void odd(function<void(int)> printNumber) {
      for (int i = 1; i <= n; i += 2) {
        lk_odd_.lock();
        printNumber(i);
        lk_zero_.unlock();
      }
    }
  };

.. code-block:: c++

  class ZeroEvenOdd {
   private:
    int n;
    condition_variable cv;
    mutex m;
  
    int state;
    bool print_odd;
  
   public:
    ZeroEvenOdd(int n) {
      this->n = n;
      this->state = 0;  // 0:zero 1:odd/even
      this->print_odd = true;
    }
  
    // printNumber(x) outputs "x", where x is an integer.
    void zero(function<void(int)> printNumber) {
      unique_lock<mutex> lock(m);
      for (int i = 0; i < n; ++i) {
        while (state != 0) {
          cv.wait(lock);
        }
        printNumber(0);
        state = 1;
        cv.notify_all();
      }
    }
  
    void even(function<void(int)> printNumber) {
      unique_lock<mutex> lock(m);
      for (int i = 2; i <= n; i += 2) {
        while (state == 0 || print_odd) {
          cv.wait(lock);
        }
        printNumber(i);
        state = 0;
        print_odd = true;
        cv.notify_all();
      }
    }
  
    void odd(function<void(int)> printNumber) {
      unique_lock<mutex> lock(m);
      for (int i = 1; i <= n; i += 2) {
        while (state == 0 || !print_odd) {
          cv.wait(lock);
        }
        printNumber(i);
        state = 0;
        print_odd = false;
        cv.notify_all();
      }
    }
  };

`Leetcode 1117. H2O 生成 <https://leetcode-cn.com/problems/building-h2o/>`_

.. code-block:: c++

  class H2O {
   public:
    std::mutex mtx, mtx2;
    int count;
    H2O() {
      count = 0;
      mtx2.lock();
    }
  
    void hydrogen(function<void()> releaseHydrogen) {
      mtx.lock();
      count++;
      // releaseHydrogen() outputs "H". Do not change or remove this line.
      releaseHydrogen();
      if (count % 2 == 1) {
        mtx.unlock();
      } else {
        mtx2.unlock();
      }
    }
  
    void oxygen(function<void()> releaseOxygen) {
      mtx2.lock();
      // releaseOxygen() outputs "O". Do not change or remove this line.
      releaseOxygen();
      mtx.unlock();
    }
  };

.. code-block:: c++

  class H2O {
    int nH, nO;
    mutex mut;
    condition_variable cv;

   public:
    H2O() : nH(0), nO(0) {}

    void clear() {
      if (nH == 2 && nO == 1) {
        nH = nO = 0;
      }
    }

    void hydrogen(function<void()> releaseHydrogen) {
      // releaseHydrogen() outputs "H". Do not change or remove this line.
      // cout << "H";
      unique_lock<mutex> lk(mut);
      cv.wait(lk, [=] { return nH < 2; });
      releaseHydrogen();
      ++nH;
      clear();
      cv.notify_all();
    }

    void oxygen(function<void()> releaseOxygen) {
      // releaseOxygen() outputs "O". Do not change or remove this line.
      // cout << "O";
      unique_lock<mutex> lk(mut);
      cv.wait(lk, [=] { return nO == 0; });
      releaseOxygen();
      ++nO;
      clear();
      cv.notify_all();
    }
  };

.. code-block:: c++

  class H2O {
    std::mutex m;
    int h = 2;
    int o = 0;
    std::condition_variable cvo;
    std::condition_variable cvh;
  
   public:
    H2O() {}
  
    void hydrogen(function<void()> releaseHydrogen) {
      unique_lock<std::mutex> l(m);
  
      while (h <= 0) cvh.wait(l);
      releaseHydrogen();
      --h;
      ++o;
  
      cvo.notify_one();
    }
  
    void oxygen(function<void()> releaseOxygen) {
      unique_lock<std::mutex> l(m);
  
      while (o <= 0) cvo.wait(l);
      releaseOxygen();
      o -= 2;
      h += 2;
  
      cvh.notify_all();
    }
  };