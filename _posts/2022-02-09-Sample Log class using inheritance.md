---
layout: post
title: Sample Log Class using inheritance
date: 2022-02-09 17:42
category: C++
author: 
tags: []
summary: 
---

Today I looked into TensorRT sample codes(trtexec) and I figured out an useful code snippet.

```c++
#include <iostream>
#include <sstream>

class LogStream : public std::stringbuf {
 public:
    LogStream() : out_(std::cout) {}
    std::ostream& out_;
    int32_t sync() override {
        putOutput();
        return 0;
    }
    void putOutput()
    {
        out_ << "PREFIX: " << str();
        str("");
        out_.flush();
    }
};

LogStream test;

class Logger : public std::ostream {
  public:
    Logger() : std::ostream(&test) {}
};

int main() {
    Logger abc;
    abc << "Hello" << std::endl;
    abc << "Rung" << std::endl;
}

```

The result is as follows.

```
PREFIX: Hello
PREFIX: Rung
```

There may be lots of modification, e.g. change ostream to be std::cerr or change PREFIX to another string like a timestamp.