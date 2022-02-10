---
layout: post
title: Custom Log Class using inheritance
date: 2022-02-09 17:42
category: C++
author: 
tags: []
summary: 
---

I looked into TensorRT sample codes(trtexec) and I found an useful code snippet.

```c++
#include <iostream>
#include <sstream>

class LogStream : public std::stringbuf {
 public:
    LogStream() : out_(std::cout) {}
    int32_t sync() override {
        putOutput();
        return 0;
    }

 private:
    void putOutput()
    {
        out_ << "PREFIX[" << seq_++ << "]: " << str();
        str("");
        out_.flush();
    }
    std::ostream& out_;
    int seq_ = 0;
};

LogStream test;

class Logger : public std::ostream {
  public:
    Logger() : std::ostream(&test) {}
};

int main() {
    Logger abc;
    abc << "Hello" << std::endl;
    abc << "Nice to meet you" << std::endl;
    abc << "It's good" << std::endl;
}

```

The result is as follows.

```
PREFIX[0]: Hello
PREFIX[1]: Nice to meet you
PREFIX[2]: It's good
```

There may be lots of modification, e.g. change ostream to be std::cerr or change PREFIX to another string like a timestamp.

I used to use Google::GLog.
Even it is very useful, it might be heavy because I should install glog using a package manager such as apt-get.
So 
