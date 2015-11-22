---
layout: post
title:  "PHPUnit: code coverage slow as hell. Speed it up!"
date:   2015-11-05 10:18:00
categories: 
---

As your codebase grow and you have written a lot of tests, code coverage using PHPUnit is a real pain: I mean, 2 hours and half just to run 120 integration tests and 320 assertions. Too much.

Luckly there is a way: run your tests in parallel and then merge all your code coverage. The first time I read about this idea (as a real alternative way) was inside [this excellent blog post from the Badoo tech team](https://techblog.badoo.com/blog/2015/03/09/code-coverage/): they claim to increase the speed of their tests by a factor of 30. They were right!

// work in progress..