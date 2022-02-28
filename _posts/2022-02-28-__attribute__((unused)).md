---
layout: post
title: __attribute__((unused))
date: 2022-02-28 10:24
category: C++
author: 
tags: []
summary: 
---

Sometimes, there can be a keyword, `__attribute__((unused))`.
It tells C++ compiler the following after the keyword might be unused. 
So don't bother to give me a warn message for unused variables/functions!

**Example 1**
```c++
int square(int num) {
  int c __attribute__((unused));
  return num * num;
}

int main() {
  return square(3);
}
```

**Example 2**
```c++
int square(int num) {
    return num * num;
}

__attribute__((unused))
static int add(int a, int b) {
  return a + b;
}

int main() {
    return square(3);
}
```

You can try to compile with '-Wunused' option when there is with, or without `__attribute__` keyword.
