############################
多线程
############################

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

`Leetcode 1117. H2O 生成 <https://leetcode-cn.com/problems/building-h2o/>`_