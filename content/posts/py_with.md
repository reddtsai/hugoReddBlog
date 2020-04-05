---
title: "Py_with"
date: 2020-03-17T15:25:22+08:00
description: 
draft: true
hideToc: false
enableToc: true
enableTocContent: true
author: Redd Tsai
authorEmoji: 🐔
tags:
- Python
categories:
- Python
---

``` python
class MyTest:
    def __init__(self):
        self.name = "redd"
        self.repo = lotto649Repo()
        print("__init__")
    def __enter__(self):
        print("__enter__")
        return self
    def __exit__(self, exceptionType, exceptionValue, traceback):
        print("__exit__")
    def whoami(self):
        # raise Exception("Exception")
        print(self.name)
    def query(self):
        self.repo.query()
    def add(self):
        self.repo.add(109000028, 1, 12, 34, 37, 4, 25, 42)
 
with MyTest() as test:
    test.add()
    test.query()
```