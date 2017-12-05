---
layout: post
title: "《iOS进阶》-Core Foundation对象的内存管理"
date: '2015-09-22 17:14:48'
tags:
- ios
- iosjin-jie
- xue-xi-bi-ji
---

### Core Foundation对象的内存管理
底层的Core Foundation对象，大多数以XxxCreateWithXxx的方式创建如：`CFStringRef str = CFStringCreateWithCString(kCFAllocatorDefault,"Hello World",kCFStringEncodingUTF8);`,对于**CFRetain**和**CFRelease**两种方法，与OC中的Retain和Release大致一致。所以在处理底层Core Foundation对象，我们只需要延续以前手工管理引用计数的办法即可。

在ARC下，我们有时需要将一个Core Foundation对象转换成一个OC对象，这时候我们需要告诉编译器，转换过程中引用计数该如何调整，这就引入了bridge相关的关键字。

- **__bridge**:只做类型转换，不修改相关对象的引用计数，原来的Core Foundation对象在不用时，需要调用CFRelease方法。
- **__bridge_retained**:类型转换后，将相关对象的引用计数加1,原来的Core Foundation对象在不用时，需要调用CFRelease方法。
- **__bridge_transfer**:类型转换后，将该对象的引用计数交给ARC管理，Core Foundation对象在不用时，不再需要调用CFRelease方法。
