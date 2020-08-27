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

`Leetcode 1116. 打印零与奇偶数 <https://leetcode-cn.com/problems/print-zero-even-odd/>`_

`Leetcode 1117. H2O 生成 <https://leetcode-cn.com/problems/building-h2o/>`_