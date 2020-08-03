# google sanitizers

发现了一个 google 出品的 C++ 神器 [sanitizers][1]。包括：

 * [AddressSanitizer][2] (detects addressability issues) 
 * [LeakSanitizer][3] (detects memory leaks)
 * ThreadSanitizer (detects data races and deadlocks) for [C++][4] and [Go][5]
 * [MemorySanitizer][6] (detects use of uninitialized memory)

已经嵌入 llvm，gcc/clang 可用。

有空研究下。

还发现了一篇好文《[一个涉及编译器、高速缓存、内存管理、C/C++的bug][7]》，超级隐晦的 bug。

 * 很经典。只有了解了所有的细节（硬件、操作系统），才能知道问题所在。
 * c_str() 的一个简单优化，叠加上 操作系统 的行为，就变成了 bug。


[1]:https://github.com/google/sanitizers
[2]:https://github.com/google/sanitizers/wiki/AddressSanitizer
[3]:https://github.com/google/sanitizers/wiki/AddressSanitizerLeakSanitizer
[4]:https://github.com/google/sanitizers/wiki/ThreadSanitizerCppManual
[5]:https://github.com/google/sanitizers/wiki/ThreadSanitizerGoManual
[6]:https://github.com/google/sanitizers/wiki/MemorySanitizer
[7]:https://zhuanlan.zhihu.com/p/55575761
