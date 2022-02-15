---
layout: post
title: Rule of All or Nothing
date: 2022-02-15 09:05
category: C++
author:
tags: []
summary:
---

When I began to study C++, one hard to understand is to know the difference between "copy" and "assignment".
And one is to know the concept of Lvalue and Rvalue.
Moreover, to know the concept of Lvalue reference, Rvalue reference.

So I post the basic Rule of All(Rule of Five) code snippet to catch up with a basic concept.

```c++
// The rule of all or nothing
#include <iostream>

class RuleFive {
public:
  ~RuleFive() {
    std::cout << "5. Destructing " << std::hex << this << std::endl;
    if (p_val_ != nullptr) delete p_val_;
  }
  RuleFive() {
    std::cout << "0. Default constructor" << std::endl;
    p_val_ = new int;
  }
  RuleFive(RuleFive& other) : p_val_(new int(*other.p_val_)) {
    std::cout << "1. Copy constructor" << std::endl;
  }
  RuleFive(RuleFive&& other) : p_val_(new int(*other.p_val_)) {
    std::cout << "2. Move constructor" << std::endl;
  }
  RuleFive& operator=(RuleFive& other) {
    std::cout << "3. Copy assignment" << std::endl;
    p_val_ = new int(*other.p_val_);
    return *this;
  }
  RuleFive& operator=(RuleFive&& other) {
    std::cout << "4. Move assignment" << std::endl;
    std::swap(p_val_, other.p_val_);
    return *this;
  }

private:
  int* p_val_ = nullptr;
};

int main() {
    RuleFive A;
    RuleFive B = std::move(A);

    RuleFive C;
    C = std::move(B);

    RuleFive D;
    D = B;

    std::cout << std::hex << &A << std::endl;
    std::cout << std::hex << &B << std::endl;
    std::cout << std::hex << &C << std::endl;
    std::cout << std::hex << &D << std::endl;
}
```

**Result of the above**
```bsh
0. Default constructor
2. Move constructor
0. Default constructor
4. Move assignment
0. Default constructor
3. Copy assignment
0x7ffe82698898
0x7ffe82698890
0x7ffe82698888
0x7ffe82698880
5. Destructing 0x7ffe82698880
5. Destructing 0x7ffe82698888
5. Destructing 0x7ffe82698890
5. Destructing 0x7ffe82698898
```

So why is this necessary?
If you write a non-default destructor(like the above code), there may be an issue when you leave copy constructor and copy assignment as default.
The reason issue come up with is "a shallow copy" (instead of a deep copy).
If you want to use an immutable object, it may be good to use a shallow copy. But you should know what you are doing exactly.

I would like to introduce Scott Myers' good blog posting, that Mr. Myers had a concern about the Rule of Zero.

[http://scottmeyers.blogspot.com/2014/03/a-concern-about-rule-of-zero.html](http://scottmeyers.blogspot.com/2014/03/a-concern-about-rule-of-zero.html).

Also, I recommend the following posting on the Cppreference.

[The rule of three/five/zero](https://en.cppreference.com/w/cpp/language/rule_of_three)

It is very worth reading.

Next time, I would like to recap the concept of Lvalue reference, Rvalue reference, or SOLID principles.
