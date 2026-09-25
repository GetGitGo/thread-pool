[![Author: Barak Shoshany](https://img.shields.io/badge/author-Barak_Shoshany-009933)](https://baraksh.com/)
[![DOI: 10.1016/j.softx.2024.101687](https://img.shields.io/badge/DOI-10.1016%2Fj.softx.2024.101687-b31b1b)](https://doi.org/10.1016/j.softx.2024.101687)
[![arXiv:2105.00613](https://img.shields.io/badge/arXiv-2105.00613-b31b1b)](https://arxiv.org/abs/2105.00613)
[![License: MIT](https://img.shields.io/github/license/bshoshany/thread-pool)](https://github.com/bshoshany/thread-pool/blob/master/LICENSE.txt)
[![Language: C++17 / C++20 / C++23](https://img.shields.io/badge/Language-C%2B%2B17%20%2F%20C%2B%2B20%20%2F%20C%2B%2B23-yellow)](https://cppreference.com/)
[![GitHub stars](https://img.shields.io/github/stars/bshoshany/thread-pool?style=flat&color=009999)](https://github.com/bshoshany/thread-pool/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/bshoshany/thread-pool?style=flat&color=009999)](https://github.com/bshoshany/thread-pool/forks)
[![GitHub release](https://img.shields.io/github/v/release/bshoshany/thread-pool?color=660099)](https://github.com/bshoshany/thread-pool/releases)
[![Vcpkg version](https://img.shields.io/vcpkg/v/bshoshany-thread-pool?color=6600ff)](https://vcpkg.io/)
[![Meson WrapDB](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fraw.githubusercontent.com%2Fmesonbuild%2Fwrapdb%2Fmaster%2Freleases.json&query=%24%5B%22bshoshany-thread-pool%22%5D.versions%5B0%5D&label=wrapdb&color=6600ff)](https://mesonbuild.com/Wrapdb-projects.html)
[![Conan version](https://img.shields.io/conan/v/bshoshany-thread-pool?color=6600ff)](https://conan.io/center/recipes/bshoshany-thread-pool)
[![Open in Visual Studio Code](https://img.shields.io/badge/Open_in_Visual_Studio_Code-007acc)](https://vscode.dev/github/bshoshany/thread-pool)

# `BS::thread_pool`：快速、轻量、现代、易用的 C++17 / C++20 / C++23 线程池库

作者：**Barak Shoshany**\
邮箱：<baraksh@gmail.com>\
网站：<https://baraksh.com/>\
GitHub：<https://github.com/bshoshany>

本文是库 **v5.1.0**（发布于 **2026-01-03**）的完整文档中文译本。英文原文见 [README.md](README.md)。代码示例、命令、标识符保持原文。

* [引言](#引言)
    * [动机](#动机)
    * [功能概览](#功能概览)
* [入门](#入门)
    * [安装](#安装)
    * [编译与兼容性](#编译与兼容性)
    * [构造函数](#构造函数)
    * [获取和重置线程数](#获取和重置线程数)
* [向队列提交任务](#向队列提交任务)
    * [提交无参任务并取得 future](#提交无参任务并取得-future)
    * [提交带参任务并取得 future](#提交带参任务并取得-future)
    * [分离任务并等待](#分离任务并等待)
    * [带超时地等待任务](#带超时地等待任务)
    * [把类成员函数当作任务](#把类成员函数当作任务)
* [并行化循环](#并行化循环)
    * [自动并行化循环](#自动并行化循环)
    * [选择块数](#选择块数)
    * [公共下标类型](#公共下标类型)
    * [不返回 future 的循环并行化](#不返回-future-的循环并行化)
    * [按单个下标还是按块并行](#按单个下标还是按块并行)
    * [带返回值的循环](#带返回值的循环)
    * [并行化序列](#并行化序列)
    * [关于 `BS::multi_future`](#关于-bsmulti_future)
    * [不经过循环批量提交任务](#不经过循环批量提交任务)
* [工具类](#工具类)
    * [用 `BS::synced_stream` 同步向流打印](#用-bssynced_stream-同步向流打印)
* [管理任务](#管理任务)
    * [监视任务](#监视任务)
    * [清除队列中的任务](#清除队列中的任务)
    * [异常处理](#异常处理)
    * [查询当前线程](#查询当前线程)
    * [线程初始化函数](#线程初始化函数)
    * [线程清理函数](#线程清理函数)
    * [以常量引用传递任务参数](#以常量引用传递任务参数)
* [可选功能](#可选功能)
    * [启用功能](#启用功能)
    * [设置任务优先级](#设置任务优先级)
    * [暂停线程池](#暂停线程池)
    * [避免等待死锁](#避免等待死锁)
* [原生扩展](#原生扩展)
    * [启用原生扩展](#启用原生扩展)
    * [设置线程优先级](#设置线程优先级)
    * [设置线程亲和性](#设置线程亲和性)
    * [设置线程名](#设置线程名)
    * [设置进程优先级](#设置进程优先级)
    * [设置进程亲和性](#设置进程亲和性)
    * [访问原生线程句柄](#访问原生线程句柄)
* [测试](#测试)
    * [自动化测试](#自动化测试)
    * [性能测试](#性能测试)
    * [查询库版本](#查询库版本)
* [作为 C++20 模块导入](#作为-c20-模块导入)
* [导入 C++23 标准库模块](#导入-c23-标准库模块)
* [用包管理器安装](#用包管理器安装)
* [完整参考](#完整参考)
* [开发工具](#开发工具)
* [关于本项目](#关于本项目)

## 引言

### 动机

多线程是现代高性能计算的基础。自 C++11 起，标准库提供了 `std::thread` 这类底层工具。但每次构造 `std::thread` 都会新建一条线程，开销很大；线程数超过硬件能同时运行的数量时，还会明显变慢。

本库的 `BS::thread_pool` 在程序生命期内只创建一组固定线程，并反复复用它们执行不同任务。默认线程数等于硬件能并行运行的最大线程数。

用户把任务放进队列。某条线程空闲时，就取出下一个任务执行。线程池可以选择为每个任务产生一个 `std::future`，用来等待结束和/或取得返回值。线程和任务由线程池在后台自行管理，用户只需提交任务。

设计遵循四条原则。第一是紧凑：整个库就是一个自包含头文件，没有别的组件或依赖。第二是可移植：只用 C++ 标准库，不依赖编译器扩展或第三方库，因此任何符合标准、支持 C++17 或更新标准的编译器都可以用。第三是易用：文档详细，各种水平的程序员都可以直接上手。

第四是性能：每一行都按最高性能来写，并在多种编译器和平台上验证过。这个库最初是为作者自己的计算密集型科学计算项目写的，既跑在高端桌面/笔记本上，也跑在高性能计算节点上。

在现有 C++ 线程池库里，`BS::thread_pool` 处在中间：一边是功能很少、只适合简单程序的小型线程池，另一边是功能很多但组件、依赖和 API 都复杂、学习成本高的大型库。它面向这样的用户：想要一个简单、轻量、只含头文件、容易学、容易嵌进现有或新项目的库，同时不想在性能或功能上妥协。

取得这个库很快：可以从 [GitHub 仓库](https://github.com/bshoshany/thread-pool) 手工下载，也可以用多种包管理器和构建系统安装。可以当作传统的[只含头文件的库](#安装)使用，也可以当作现代的 [C++20 模块](#作为-c20-模块导入)导入。它在多个平台上经过大量测试，被全世界数以千计的 C++ 开发者用于从科学计算到游戏开发的各种场景。

### 功能概览

* **快：**
    * 从零开始，以[最高性能](#性能测试)为目标。
    * 适合 CPU 核心非常多的高性能计算节点。
    * 复用线程，避免为每个任务创建和销毁线程。
    * 任务队列保证并行运行的任务数不超过硬件允许的数量。
    * 所有可选功能都可以单独打开，以保持最小开销。
* **轻：**
    * 单个头文件：[`#include "BS_thread_pool.hpp"`](#安装) 即可。
    * 只含头文件：不必安装或构建库本身。
    * 自包含：没有外部要求或依赖。
    * 可移植：只用 C++ 标准库，任何符合 C++17 的编译器、任何平台都可以用。
    * 含全部可选功能和工具类，共 536 行代码（不计注释、空行、只有一个花括号的行、C++17/20 的 polyfill，以及原生扩展）。
* **现代：**
    * 完整支持 C++17、C++20、C++23。编译器提供新特性时会用上，以提高性能、可靠性和可用性。
    * 在 C++20 中可以用 [`import BS.thread_pool`](#作为-c20-模块导入) 作为模块导入，编译更快，也不会污染命名空间。
    * 在 C++23 中，受支持的编译器和平台上可以用 [`import std`](#导入-c23-标准库模块) 导入标准库模块。
    * 采用现代 C++ 写法，兼顾可读、可维护、性能、安全、可移植和可靠。
* **易用：**
    * 基本用法只需要少数成员函数；进阶用法另有更多成员函数、类和函数。
    * 用 [`submit_task()`](#向队列提交任务) 提交的每个任务都会自动产生 `std::future`，可用来等待结束、取得返回值，以及捕获抛出的异常。
    * 用 [`submit_loop()`](#并行化循环) 可以把循环自动拆成任意数量的任务，返回的 [`BS::multi_future`](#关于-bsmulti_future) 能一次跟踪全部并行任务。
    * 如果不需要 future，可以用 [`detach_task()`](#分离任务并等待) 提交任务，用 [`detach_loop()`](#不返回-future-的循环并行化) 并行化循环，用性能换便利。此时用 `wait()`、`wait_for()`、`wait_until()` 等待队列中的全部任务。
    * 文档在本文件和英文 [`README.md`](README.md) 中，含大量示例。
    * 源码用 Doxygen 注释写清了接口和实现，便于自行修改。
    * 可选的 Python 脚本 [`compile_cpp.py`](#compile_cpppy-脚本) 可以编译使用本库的程序，并在适用时支持 C++20 模块和 C++23 标准库模块。
* **其他功能：**
    * 用 [`get_thread_count()`](#获取和重置线程数) 取得当前线程数。
    * 用 [`reset()`](#获取和重置线程数) 在运行中安全地改变线程数。
    * 用 [`get_tasks_queued()`、`get_tasks_running()`、`get_tasks_total()`](#监视任务) 监视排队和/或正在运行的任务数。
    * 用 [`purge()`](#清除队列中的任务) 丢弃队列里还在等待的任务。
    * 把[初始化函数](#线程初始化函数)传给 `BS::thread_pool` 构造函数，让每条线程在执行提交的任务之前先运行它。
    * 用 [`set_cleanup_func()`](#线程清理函数) 设置每条线程销毁前运行的清理函数。
    * 用 [`detach_blocks()` 和 `submit_blocks()`](#按单个下标还是按块并行) 对并行循环做更底层的控制。
    * 用 [`detach_sequence()` 和 `submit_sequence()`](#并行化序列) 把按下标枚举的任务序列提交到队列。
    * 用 [`detach_bulk()` 和 `submit_bulk()`](#不经过循环批量提交任务) 从容器或迭代器范围批量提交任务。
    * 用 `BS::this_thread::get_index()` 和 `BS::this_thread::get_pool()` [查询当前线程](#查询当前线程)。
    * 用 [`get_thread_ids()`](#获取和重置线程数) 取得池中每条线程的唯一 ID。
    * 用 [`BS::synced_stream`](#用-bssynced_stream-同步向流打印) 让多个线程并行地向一个或多个流输出时保持同步。
* **可选功能：**
    * 向 `BS::thread_pool` 传入位掩码模板参数即可[启用可选功能](#启用功能)。
    * 可选的[任务优先级](#设置任务优先级)把优先级（-128 到 +127）作为所有 `submit` 和 `detach` 成员函数的最后一个参数。优先级更高的任务先执行。
    * 可选的[暂停](#暂停线程池)提供 `pause()`、`unpause()`、`is_paused()`。暂停时线程不再从队列取新任务。
    * 可选的[等待死锁检查](#避免等待死锁)在等待任务时发现死锁会抛出 `BS::wait_deadlock`。
* **原生扩展：**
    * 编译时定义宏 `BS_THREAD_POOL_NATIVE_EXTENSIONS` 可启用可选的[原生扩展](#原生扩展)。它们使用操作系统原生 API，不可移植。应能在大多数 Windows、Linux、macOS 上工作。
    * 用 [`BS::this_thread::get_os_thread_priority()` 和 `set_os_thread_priority()`](#设置线程优先级) 读取和设置当前线程优先级。
    * 用 [`get_os_thread_affinity()` 和 `set_os_thread_affinity()`](#设置线程亲和性) 读取和设置当前线程的处理器亲和性。
    * 用 [`get_os_thread_name()` 和 `set_os_thread_name()`](#设置线程名) 读取和设置当前线程名。
    * 用 [`BS::get_os_process_priority()` 和 `set_os_process_priority()`](#设置进程优先级) 读取和设置当前进程优先级。
    * 用 [`BS::get_os_process_affinity()` 和 `set_os_process_affinity()`](#设置进程亲和性) 读取和设置当前进程的处理器亲和性。
    * 用 [`get_native_handles()`](#访问原生线程句柄) 取得池中每条线程由实现定义的句柄。
* **经过充分测试：**
    * 附带的 [`BS_thread_pool_test.cpp`](#测试) 做数百项自动化测试，也是完整的用法示例。
    * 测试程序还用高度优化的多线程 Mandelbrot 集绘图算法做[基准测试](#性能测试)。
    * [`compile_cpp.py`](#compile_cpppy-脚本) 可以用一条命令在所有可用编译器上跑测试。
    * [兼容性](#编译与兼容性)在最新的 Windows、Ubuntu、macOS 上，用 Clang、GCC、MSVC 做过全面测试。
    * 持续开发。缺陷和功能请求请通过 [GitHub issues](https://github.com/bshoshany/thread-pool/issues) 提交。

## 入门

### 安装

下载 [GitHub 仓库](https://github.com/bshoshany/thread-pool) 的[最新发布](https://github.com/bshoshany/thread-pool/releases)，把 `include` 目录中的 `BS_thread_pool.hpp` 放到合适的位置，然后在程序里包含它：

```cpp
#include "BS_thread_pool.hpp"
```

之后通过 `BS::thread_pool` 使用线程池。也可以直接下载头文件本身：[这个 URL](https://raw.githubusercontent.com/bshoshany/thread-pool/master/include/BS_thread_pool.hpp)。这是单头文件库，不需要其他文件。

本库也发布在 [vcpkg](https://vcpkg.io/)、[Conan](https://conan.io/)、[Meson](https://mesonbuild.com/) 和 [CMake](https://cmake.org/) 上。详见[用包管理器安装](#用包管理器安装)。

如果有 C++20，也可以作为模块导入，把 `#include "BS_thread_pool.hpp"` 换成 `import BS.thread_pool;`。这需要多一个文件，而且模块必须先编译。说明见[作为 C++20 模块导入](#作为-c20-模块导入)。

### 编译与兼容性

本库正式支持 C++17、C++20、C++23。用 C++20 和/或 C++23 编译时会使用新特性，以获得更好的性能和可用性。库本身完全兼容 C++17，任何符合 C++17 的编译器都应能编译，与操作系统和架构无关，只要该平台有这样的编译器。

兼容性用附带测试程序 [`BS_thread_pool_test.cpp`](#测试) 验证。编译使用附带脚本 [`compile_cpp.py`](#compile_cpppy-脚本)，打开原生扩展；在适用时把库[作为 C++20 模块](#作为-c20-模块导入)导入，并把 [C++23 标准库作为模块](#导入-c23-标准库模块)导入。测试机是 24 核（8P+16E）/ 32 线程的 Intel i9-13900K。编译器、标准库和平台如下：

* Windows 11 25H2 build 26200.7462：
    * [Clang](https://clang.llvm.org/) v21.1.8 与 LLVM libc++ v21.1.8（[MSYS2 构建](https://www.msys2.org/)）
    * [GCC](https://gcc.gnu.org/) v15.2.0 与 GNU libstdc++ v15 (20250808)（[MSYS2 构建](https://www.msys2.org/)）
    * [MSVC](https://docs.microsoft.com/en-us/cpp/) v19.50.35721 与 Microsoft STL v145 (202508)
* Ubuntu 25.10：
    * [Clang](https://clang.llvm.org/) v21.1.8 与 LLVM libc++ v21.1.8
    * [GCC](https://gcc.gnu.org/) v15.2.0 与 GNU libstdc++ v15 (20250917)
* macOS 15.1 build 24B83：
    * [Clang](https://clang.llvm.org/) v21.1.8 与 LLVM libc++ v21.1.8（[Homebrew 构建](https://formulae.brew.sh/formula/llvm)）
    * 注意：目前不正式支持 Apple Clang，因为它不支持 C++20 模块。

本库需要 C++17，编译时必须打开相应支持：

* Clang 或 GCC：`-std=c++17`
* MSVC：`/std:c++17`，并加上 `/permissive-` 以保证符合标准。

要获得最高性能，应打开编译器优化：

* Clang 或 GCC：`-O3`
* MSVC：`/O2`

例如，先 `mkdir build`，再在仓库根目录编译测试程序 `BS_thread_pool_test.cpp`：

* Windows：
    * GCC：`g++ -std=c++17 -O3 -I include tests/BS_thread_pool_test.cpp -o build/BS_thread_pool_test.exe`
    * Clang：`clang++ -std=c++17 -O3 -I include tests/BS_thread_pool_test.cpp -o build/BS_thread_pool_test.exe`
    * MSVC：`cl /std:c++17 /permissive- /O2 /EHsc /I include tests/BS_thread_pool_test.cpp /Fo:build/BS_thread_pool_test.obj /Fe:build/BS_thread_pool_test.exe`（在对应 CPU 架构的 Visual Studio Developer PowerShell 中）
* Linux/macOS：
    * GCC：`g++ -std=c++17 -O3 -I include tests/BS_thread_pool_test.cpp -o build/BS_thread_pool_test`
    * Clang：`clang++ -std=c++17 -O3 -I include tests/BS_thread_pool_test.cpp -o build/BS_thread_pool_test`

如果编译器和代码库支持 C++20 和/或 C++23，建议打开它们，让线程池用上最新特性：

* Clang 或 GCC：`-std=c++20` 或 `-std=c++23`
* MSVC：C++20 用 `/std:c++20`，C++23 用 `/std:c++latest`

另外，有 C++20 时可以把库作为模块导入，说明见[下文](#作为-c20-模块导入)。

### 构造函数

默认构造函数创建的线程数等于实现通过 `std::thread::hardware_concurrency()` 报告的、硬件能同时运行的线程数。这通常由 CPU 核心数决定。超线程核心算两条线程。例如：

```cpp
// Constructs a thread pool with as many threads as are available in the hardware.
BS::thread_pool pool;
```

也可以在构造函数参数里指定不同于硬件并发数的线程数。线程数超过硬件能处理的数量**不会**提高性能，多半还会变差。这个选项是为了使用**更少**的线程，把一些硬件线程留给其他进程。例如：

```cpp
// Constructs a thread pool with only 12 threads.
BS::thread_pool pool(12);
```

通常，主线程只负责提交任务并等待，自己不做计算密集的工作。这时应用默认线程数，这样主线程等待时，硬件上所有线程都在干活。

如果主线程自己也做计算密集的工作，线程数可以比硬件并发数少 1，留一条硬件线程给主线程。程序里同时有多个线程池时，所有池的线程总数不应超过硬件并发数。

注意：如果启用了[原生扩展](#原生扩展)，默认构造的池只使用进程可用的线程数，该数值来自 [`BS::get_os_process_affinity()`](#设置进程亲和性)，可能小于硬件线程数。

### 获取和重置线程数

`get_thread_count()` 返回池中的线程数。使用默认构造函数时，它等于 `std::thread::hardware_concurrency()`；启用[原生扩展](#原生扩展)时等于 [`BS::get_os_process_affinity()`](#设置进程亲和性) 给出的数量。

线程池的意义就是只创建一次线程，创建之后一般不必再改线程数。如果需要，可以在运行中用 `reset()` 安全地修改。

`reset()` 会先等所有任务完成，包括正在线程上运行的和还在队列里等待的，然后销毁线程池，按参数指定的新线程数再建一个空队列的池。不传参数时行为与默认构造函数相同。

如果启用了暂停（[见下文](#暂停线程池)），`reset()` 在销毁池之前只等待正在运行的任务；重置之后会继续执行队列里剩下的任务以及新提交的任务。如果重置前池处于暂停状态，新池也会暂停。`reset()` 还可以更换线程初始化函数（[见下文](#线程初始化函数)）。

`get_thread_ids()` 返回一个向量，元素是各线程由 `std::thread::get_id()` 得到的唯一标识。这些值本身用处不大，但可以用来区分线程，或用来分配资源。

## 向队列提交任务

### 提交无参任务并取得 future

本节说明如何提交没有参数、但可以有返回值的任务。任务提交后，一旦有线程空闲就会执行。除非启用了任务优先级（[见下文](#设置任务优先级)），任务按提交顺序执行（先进先出）。

例如池有 8 条线程且队列为空，提交 16 个任务时，前 8 个会并行执行，其余任务在各线程完成手头任务后被逐个取走，直到队列空。

`submit_task()` 把任务提交到队列。它只接受一个参数，即要提交的任务。该任务必须是没有参数的函数，但可以有返回值。

`submit_task()` 返回与该任务关联的 `std::future`。若任务返回类型 `T`，future 的类型是 `std::future<T>`，任务结束时被设为返回值。若任务没有返回值，future 是 `std::future<void>`，不含值，但仍可用来等待结束。

用 future 的 `wait()` 等待结束。用 `get()` 取得返回值；若任务还没结束，`get()` 会先等待。示例：

```cpp
#include "BS_thread_pool.hpp" // BS::thread_pool
#include <future>             // std::future
#include <iostream>           // std::cout

int the_answer()
{
    return 42;
}

int main()
{
    BS::thread_pool pool;
    std::future<int> my_future = pool.submit_task(the_answer);
    std::cout << my_future.get() << '\n';
}
```

这里提交了返回 `int` 的 `the_answer()`，因此 `submit_task()` 返回 `std::future<int>`。随后用 `get()` 取得返回值并打印。

除了预先写好的函数，也可以用 [lambda 表达式](https://zh.cppreference.com/w/cpp/language/lambda) 就地定义任务。把上例改成 lambda：

```cpp
#include "BS_thread_pool.hpp" // BS::thread_pool
#include <future>             // std::future
#include <iostream>           // std::cout

int main()
{
    BS::thread_pool pool;
    std::future<int> my_future = pool.submit_task([]{ return 42; });
    std::cout << my_future.get() << '\n';
}
```

lambda `[]{ return 42; }` 有两部分：

1. 空捕获列表 `[]`，告诉编译器这里开始定义 lambda。
2. 代码块 `{ return 42; }`，返回 `42`。

提交 lambda 通常比提交预先写好的函数更简单、更快，尤其是可以捕获局部变量。下一节会讲到。

任务也可以没有返回值。下例提交一个无返回值的函数，再用 future 等待它结束：

```cpp
#include "BS_thread_pool.hpp" // BS::thread_pool
#include <chrono>             // std::chrono
#include <future>             // std::future
#include <iostream>           // std::cout
#include <thread>             // std::this_thread

int main()
{
    BS::thread_pool pool;
    const std::future<void> my_future = pool.submit_task(
        []
        {
            std::this_thread::sleep_for(std::chrono::milliseconds(500));
        });
    std::cout << "Waiting for the task to complete... ";
    my_future.wait();
    std::cout << "Done." << '\n';
}
```

这里把 lambda 拆成多行以便阅读。`std::this_thread::sleep_for(std::chrono::milliseconds(500))` 让任务睡 500 毫秒，用来模拟计算密集的工作。

### 提交带参任务并取得 future

上一节说过，`submit_task()` 提交的任务不能有参数。要用带参数的任务，可以把函数包进 lambda，或直接用 lambda 捕获。下例把预先写好的带参函数包进 lambda：

```cpp
#include "BS_thread_pool.hpp" // BS::thread_pool
#include <future>             // std::future
#include <iostream>           // std::cout

double multiply(const double lhs, const double rhs)
{
    return lhs * rhs;
}

int main()
{
    BS::thread_pool pool;
    std::future<double> my_future = pool.submit_task(
        []
        {
            return multiply(6, 7);
        });
    std::cout << my_future.get() << '\n';
}
```

向 `multiply()` 传参，就是在 lambda 里显式调用 `multiply(6, 7)`。如果参数不是字面量，可以用捕获列表从局部作用域捕获它们：

```cpp
#include "BS_thread_pool.hpp" // BS::thread_pool
#include <future>             // std::future
#include <iostream>           // std::cout

double multiply(const double lhs, const double rhs)
{
    return lhs * rhs;
}

int main()
{
    BS::thread_pool pool;
    constexpr double first = 6;
    constexpr double second = 7;
    std::future<double> my_future = pool.submit_task(
        [first, second]
        {
            return multiply(first, second);
        });
    std::cout << my_future.get() << '\n';
}
```

也可以不要 `multiply()`，把计算全部写进 lambda：

```cpp
#include "BS_thread_pool.hpp" // BS::thread_pool
#include <future>             // std::future
#include <iostream>           // std::cout

int main()
{
    BS::thread_pool pool;
    constexpr double first = 6;
    constexpr double second = 7;
    std::future<double> my_future = pool.submit_task(
        [first, second]
        {
            return first * second;
        });
    std::cout << my_future.get() << '\n';
}
```

### 分离任务并等待

通常最好用 `submit_task()` 提交任务，以便稍后等待结束和/或取得返回值。但有时不需要 future，例如只想“提交后不管”，或者任务已经用条件变量等方式和主线程或其他任务通信。

这时可以避开为任务分配 future 的开销，以提高性能。这叫做“分离”任务：任务离开主线程，独立运行。

`detach_task()` 把任务分离到队列，不为它产生 future。和 `submit_task()` 一样，任务不能有参数，但可以像上一节那样用 lambda 传入参数。通过 `detach_task()` 执行的任务不能有返回值，因为主线程没有办法取回它。

`detach_task()` 不返回 future，用户没有内建办法知道任务何时结束。在使用依赖其输出的东西之前，必须自己保证任务已经结束，否则会出问题。

`BS::thread_pool` 的 `wait()` 用来等待队列中的全部任务，不论它们是分离的还是带 future 提交的。它和 `std::future::wait()` 类似。看这段代码：

```cpp
#include "BS_thread_pool.hpp" // BS::thread_pool
#include <chrono>             // std::chrono
#include <iostream>           // std::cout
#include <thread>             // std::this_thread

int main()
{
    BS::thread_pool pool;
    int result = 0;
    pool.detach_task(
        [&result]
        {
            std::this_thread::sleep_for(std::chrono::milliseconds(100));
            result = 42;
        });
    std::cout << result << '\n';
}
```

程序先把局部变量 `result` 设为 `0`，再分离一个 lambda 任务。捕获列表里 `result` 前面的 `&` 表示**按引用**捕获，任务可以修改 `result`，修改会反映到主线程。

任务会把 `result` 改成 `42`，但先睡 100 毫秒。主线程打印 `result` 时任务还在睡，所以实际打印的是初始值 `0`，这不是想要的结果。

分离之后必须调用 `wait()`：

```cpp
#include "BS_thread_pool.hpp" // BS::thread_pool
#include <chrono>             // std::chrono
#include <iostream>           // std::cout
#include <thread>             // std::this_thread

int main()
{
    BS::thread_pool pool;
    int result = 0;
    pool.detach_task(
        [&result]
        {
            std::this_thread::sleep_for(std::chrono::milliseconds(100));
            result = 42;
        });
    pool.wait();
    std::cout << result << '\n';
}
```

现在会按预期打印 `42`。但 `wait()` 会等待队列中的**全部**任务，包括我们关心的那个任务之前或之后提交的其他任务。如果只想等**一个**任务，用 `submit_task()` 更合适。

### 带超时地等待任务

有时希望等待任务完成，但只等一段时间，或只等到某个时间点。例如超时后想通知用户发生了延迟。

对用 `submit_task()` 提交、带有 future 的任务，可以用 `std::future` 的两个成员函数：

* `wait_for()` 等待任务完成，但在给定的 `std::chrono::duration` 过去后停止等待。
* `wait_until()` 等待任务完成，但在给定的 `std::chrono::time_point` 到达后停止等待。

两种情况下，future 已就绪（任务结束，返回值如有也已取得）时返回 `std::future_status::ready`；超时仍未就绪时返回 `std::future_status::timeout`。

示例：

```cpp
#include "BS_thread_pool.hpp" // BS::thread_pool
#include <chrono>             // std::chrono
#include <future>             // std::future
#include <iostream>           // std::cout
#include <thread>             // std::this_thread

int main()
{
    BS::thread_pool pool;
    const std::future<void> my_future = pool.submit_task(
        []
        {
            std::this_thread::sleep_for(std::chrono::milliseconds(1000));
            std::cout << "Task done!\n";
        });
    while (true)
    {
        if (my_future.wait_for(std::chrono::milliseconds(200)) != std::future_status::ready)
            std::cout << "Sorry, the task is not done yet.\n";
        else
            break;
    }
}
```

输出大致如下：

```none
Sorry, the task is not done yet.
Sorry, the task is not done yet.
Sorry, the task is not done yet.
Sorry, the task is not done yet.
Task done!
```

分离的任务没有 future，不能用上面的方法。但 `BS::thread_pool` 自己也有 `wait_for()` 和 `wait_until()`，同样按一段时间或一个时间点等待，对象是**全部**任务（无论提交还是分离）。它们不返回 `std::future_status`：全部任务都跑完返回 `true`；时间到了仍有任务在跑返回 `false`。

同一个例子改用 `detach_task()` 和 `pool.wait_for()`：

```cpp
#include "BS_thread_pool.hpp" // BS::thread_pool
#include <chrono>             // std::chrono
#include <iostream>           // std::cout
#include <thread>             // std::this_thread

int main()
{
    BS::thread_pool pool;
    pool.detach_task(
        []
        {
            std::this_thread::sleep_for(std::chrono::milliseconds(1000));
            std::cout << "Task done!\n";
        });
    while (true)
    {
        if (!pool.wait_for(std::chrono::milliseconds(200)))
            std::cout << "Sorry, the task is not done yet.\n";
        else
            break;
    }
}
```

### 把类成员函数当作任务

考虑下面的程序：

```cpp
#include <iostream> // std::boolalpha, std::cout

class flag_class
{
public:
    [[nodiscard]] bool get_flag() const
    {
        return flag;
    }

    void set_flag(const bool arg)
    {
        flag = arg;
    }

private:
    bool flag = false;
};

int main()
{
    flag_class flag_object;
    flag_object.set_flag(true);
    std::cout << std::boolalpha << flag_object.get_flag() << '\n';
}
```

它创建 `flag_class` 对象 `flag_object`，用 `set_flag()` 把标志设为 `true`，再用 `get_flag()` 打印。

要把 `set_flag()` 作为任务提交，把语句 `flag_object.set_flag(true);` 包进 lambda，并按引用传入 `flag_object`：

```cpp
#include "BS_thread_pool.hpp" // BS::thread_pool
#include <iostream>           // std::boolalpha, std::cout

class flag_class
{
public:
    [[nodiscard]] bool get_flag() const
    {
        return flag;
    }

    void set_flag(const bool arg)
    {
        flag = arg;
    }

private:
    bool flag = false;
};

int main()
{
    BS::thread_pool pool;
    flag_class flag_object;
    pool.submit_task(
            [&flag_object]
            {
                flag_object.set_flag(true);
            })
        .wait();
    std::cout << std::boolalpha << flag_object.get_flag() << '\n';
}
```

改用 `detach_task()` 也可以，那时应对池本身调用 `wait()`，而不是对返回的 future。

这里没有先把 future 存下来再等，而是直接对 `submit_task()` 的返回值调用 `wait()`。手头没有别的事要做时，这是等待任务完成的常见写法。`flag_object` 按引用传入，是为了改同一个对象，而不是改它的副本。

也可以从对象自己的成员函数里调用另一个成员函数。语法类似，但 lambda 还必须捕获 `this`（指向当前对象的指针）：

```cpp
#include "BS_thread_pool.hpp" // BS::thread_pool
#include <iostream>           // std::boolalpha, std::cout

BS::thread_pool pool;

class flag_class
{
public:
    [[nodiscard]] bool get_flag() const
    {
        return flag;
    }

    void set_flag(const bool arg)
    {
        flag = arg;
    }

    void set_flag_to_true()
    {
        pool.submit_task(
                [this]
                {
                    set_flag(true);
                })
            .wait();
    }

private:
    bool flag = false;
};

int main()
{
    flag_class flag_object;
    flag_object.set_flag_to_true();
    std::cout << std::boolalpha << flag_object.get_flag() << '\n';
}
```

这个例子把线程池定义成全局对象，这样 `main()` 之外也能用到它。理论上可以把线程池的引用传进 `set_flag_to_true()`，但很多函数都要用同一个池时会很麻烦。把线程池做成全局对象是常见做法，所有函数都能访问它，不必当参数传来传去。

## 并行化循环

### 自动并行化循环

最常见、也最有效的并行方式之一，是把循环拆成更小的子循环并行跑。这在“令人尴尬的并行”计算里最有效，例如向量或矩阵运算：每次迭代都和其他迭代完全独立。

例如两个各有 1000 个元素的向量相加，有 10 条线程，可以把求和拆成 10 块、每块 100 个元素，全部并行，性能最多大约提高 10 倍。

`BS::thread_pool` 能自动并行化循环，不必自己处理细节。考虑这段通用循环：

```cpp
for (T i = start; i < end; ++i)
    loop(i);
```

其中：

* `T` 是任意有符号或无符号整数类型。
* 循环范围是 `[start, end)`，含 `start`，不含 `end`。
* `loop()` 对每个下标 `i` 执行一次，例如修改长度为 `end - start` 的数组。

用 `submit_loop()` 自动并行化并提交到队列：

```cpp
pool.submit_loop(start, end, loop, num_blocks);
```

其中：

* `start` 是范围的第一个下标。
* `end` 是最后一个下标的下一个，完整范围是 `[start, end)`。等价于上面的通用循环，但是并行的。若 `end <= start`，什么也不会发生；循环不能倒着走。
* `loop()` 是每次迭代要跑的函数。它必须恰好接受一个参数，即循环下标。它不能有返回值，因为每个任务会多次调用它，返回值没有意义。
* `num_blocks` 是要拆成的形如 `[a, b)` 的块数。例如范围是 `[0, 9)`、块数是 3，则三块是 `[0, 3)`、`[3, 6)`、`[6, 9)`。这个参数可以省略，省略时块数等于池中的线程数。

内部算法保证每块只有两种长度，相差 1，较长的块排在前面，任务尽量均匀，以优化性能。例如把 `[0, 100)` 拆成 15 块，结果是先提交 10 个长度为 7 的块，再提交 5 个长度为 6 的块。

每一块作为单独的任务提交。拆成 3 块就是 3 个可能并行的任务。只有 1 块时，整个循环作为一个任务，没有并行。

并行化上面的通用循环：

```cpp
BS::multi_future<void> loop_future = pool.submit_loop(start, end, loop, num_blocks);
loop_future.wait();
```

`submit_loop()` 返回辅助类 [`BS::multi_future<T>`](#关于-bsmulti_future)。它本质上是带额外成员函数的 `std::vector<std::future<T>>` 特化。`num_blocks` 个块各有一个 `std::future<T>`，全部存在返回的 `BS::multi_future<T>` 里。调用 `loop_future.wait()` 时，主线程只等到 `submit_loop()` 产生的**全部**任务结束，**不包括**队列里碰巧存在的其他任务。`BS::multi_future<T>` 的作用就是等待一组特定任务，这里是跑各块的那些任务。

简单例子：计算并打印 0 到 99 的平方表：

```cpp
#include <cstddef>  // std::size_t
#include <iomanip>  // std::setw
#include <iostream> // std::cout

int main()
{
    constexpr std::size_t max = 100;
    std::size_t squares[max];
    for (std::size_t i = 0; i < max; ++i)
        squares[i] = i * i;
    for (std::size_t i = 0; i < max; ++i)
        std::cout << std::setw(2) << i << "^2 = " << std::setw(4) << squares[i] << ((i % 5 != 4) ? " | " : "\n");
}
```

并行化如下：

```cpp
#include "BS_thread_pool.hpp" // BS::multi_future, BS::thread_pool
#include <cstddef>            // std::size_t
#include <iomanip>            // std::setw
#include <iostream>           // std::cout

int main()
{
    BS::thread_pool pool(10);
    constexpr std::size_t max = 100;
    std::size_t squares[max];
    const BS::multi_future<void> loop_future = pool.submit_loop(0, max,
        [&squares](const std::size_t i)
        {
            squares[i] = i * i;
        });
    loop_future.wait();
    for (std::size_t i = 0; i < max; ++i)
        std::cout << std::setw(2) << i << "^2 = " << std::setw(4) << squares[i] << ((i % 5 != 4) ? " | " : "\n");
}
```

有 10 条线程，又省略了 `num_blocks`，循环会分成 10 块，每块算 10 个平方。

这里并行的是计算，不是打印。原因有两个：

1. 我们希望按升序打印，但块的执行顺序没有保证。不要指望并行循环和串行循环的执行顺序一样。
2. 如果在并行任务里打印，10 个块会同时往标准输出写，结果会乱。[后文](#用-bssynced_stream-同步向流打印)会说明如何同步打印。

### 选择块数

并行循环时最重要的是块数 `num_blocks`。块数等于线程数通常**不是**最优。有些块会先结束；每条线程只有一块时，先完成的线程会空等到其余块结束，浪费 CPU。

一般应让块数大于线程数，让所有线程尽量满负荷。块太多又会因为开销增大而收益递减。经验法则是块数等于线程数的平方，但这并不总是最优。

最优块数取决于具体算法和循环下标总数，也会随编译器、操作系统和硬件变化。要获得最好性能，应对自己的场景做基准测试。附带测试程序里的[基准代码](#性能测试)是一个例子。

以上讨论只针对池里只有这一段并行循环的情况。如果还有很多其他任务在跑，空闲时间通常不用特别担心，线程会被其他任务占着。

### 公共下标类型

起点和终点的类型有一个细节。在[上面](#自动并行化循环)的例子里，起点 `0` 是 `int`，终点 `max` 是 `std::size_t`。两者符号不同，在 64 位系统上位宽也不同。这时 `submit_loop()` 用自定义类型特征 `BS::common_index_type` 决定下标的公共类型。

两个有符号整数或两个无符号整数的公共类型是较大的那个。一个有符号和一个无符号整数的公共类型，是能容纳两者完整范围的有符号整数。（这和 [`std::common_type`](https://zh.cppreference.com/w/cpp/types/common_type) 不同：后者在后一种情况下会选无符号整数，负起点加无符号终点会因整数溢出而失败。）

例外：其中一个是 64 位无符号整数，另一个是任意位宽的有符号整数。没有基本有符号类型能同时容纳两者的完整范围。这时公共类型选 64 位无符号整数，因为最常见的情况就是下标从 `0` 走到 `std::size_t`，如上节的例子。

如果第一个下标其实是负数，这会失败。因此**只有**在一个下标是负数、另一个是 `std::size_t` 这类 64 位无符号类型时，用户必须把两个下标都显式转换成想要的公共类型。其余情况都由 `BS::common_index_type` 在后台处理。

### 不返回 future 的循环并行化

和 [`detach_task()`](#分离任务并等待) 相对 [`submit_task()`](#提交无参任务并取得-future) 一样，有时要并行循环，但不需要 `BS::multi_future`。可以用 `detach_loop()` 代替 `submit_loop()`，参数相同，省掉生成 future 的开销（块数多时开销可能不小）。

平方例子可以这样分离：

```cpp
#include "BS_thread_pool.hpp" // BS::thread_pool
#include <cstddef>            // std::size_t
#include <iomanip>            // std::setw
#include <iostream>           // std::cout

int main()
{
    BS::thread_pool pool(10);
    constexpr std::size_t max = 100;
    std::size_t squares[max];
    pool.detach_loop(0, max,
        [&squares](const std::size_t i)
        {
            squares[i] = i * i;
        });
    pool.wait();
    for (std::size_t i = 0; i < max; ++i)
        std::cout << std::setw(2) << i << "^2 = " << std::setw(4) << squares[i] << ((i % 5 != 4) ? " | " : "\n");
}
```

**警告：** `detach_loop()` 不返回 `BS::multi_future`，没有内建办法知道循环何时结束。使用依赖其输出的东西之前，必须用 [`wait()`](#分离任务并等待)（如上）或条件变量等方法保证循环已经结束，否则会出问题。如果池里只有这个循环，`detach_loop()` 后面跟 `wait()` 通常是性能上的最优选择。

### 按单个下标还是按块并行

`detach_loop()` 和 `submit_loop()` 对每个下标 `i` 执行 `loop(i)`。后台会把循环拆成块，每块多次调用 `loop()`。每块内部是这种循环（`T` 是下标类型）：

```cpp
for (T i = start; i < end; ++i)
    loop(i);
```

每块的 `start` 和 `end` 由池自动决定。上节把 0 到 100 拆成 10 块、每块 10 个下标：`start = 0` 到 `end = 10`，`start = 10` 到 `end = 20`，依此类推。块不含最后一个下标，因为循环条件是 `i < end`，不是 `i <= end`。

这也意味着每块会多次调用 `loop()`，多次函数调用带来额外开销。短循环影响不大。下标达到数百万的很长循环，性能代价可能明显。

因此还有 `detach_blocks()` 和 `submit_blocks()`。`detach_loop()` / `submit_loop()` 每个下标调用一次 `loop(i)`、每块调用多次；`detach_blocks()` / `submit_blocks()` 每块只调用一次 `block(start, end)`。

主要好处是性能更好，主要代价是代码稍复杂：用户必须在每块里自己写从 `start` 到 `end` 的循环，保证块内所有下标都被处理。用 `detach_blocks()` 重写上例：

```cpp
#include "BS_thread_pool.hpp" // BS::thread_pool
#include <cstddef>            // std::size_t
#include <iomanip>            // std::setw
#include <iostream>           // std::cout

int main()
{
    BS::thread_pool pool(10);
    constexpr std::size_t max = 100;
    std::size_t squares[max];
    pool.detach_blocks(0, max,
        [&squares](const std::size_t start, const std::size_t end)
        {
            for (std::size_t i = start; i < end; ++i)
                squares[i] = i * i;
        });
    pool.wait();
    for (std::size_t i = 0; i < max; ++i)
        std::cout << std::setw(2) << i << "^2 = " << std::setw(4) << squares[i] << ((i % 5 != 4) ? " | " : "\n");
}
```

块函数接受两个参数，并包含内部循环。用了 `detach_blocks()`，所以必须用 `wait()` 等循环结束。也可以用 `submit_blocks()`，然后等待返回的 `BS::multi_future<void>`。

编译器优化通常能让 `detach_loop()` / `submit_loop()` 和 `detach_blocks()` / `submit_blocks()` 差不多快。但后两者本身总是更快，代价是稍难用。对每块做底层控制还可以做进一步优化，例如按块而不是按下标分配资源。应自己做基准测试，看哪种更适合。

### 带返回值的循环

和 `submit_task()` 不同，`submit_loop()` 只接受没有返回值的循环函数。每块会多次调用循环函数，返回值没有意义。`submit_blocks()` 则允许块函数有返回值，因为每块可以返回一个独立的值。

块函数每块执行一次，但块由线程池管理。用户只能选块数，不能选每块的范围，所以“每块返回一个值”的用途有限。求和或某些排序算法仍然用得上。这时 `submit_blocks()` 返回 `BS::multi_future<T>`，`T` 是返回值类型。

对给定范围里类型为 `T` 的元素求和：

```cpp
#include "BS_thread_pool.hpp" // BS::multi_future, BS::thread_pool
#include <cstdint>            // std::uint64_t
#include <future>             // std::future
#include <iostream>           // std::cout

BS::thread_pool pool;

template <typename T>
T sum(T min, T max)
{
    BS::multi_future<T> loop_future = pool.submit_blocks(
        min, max + 1,
        [](const T start, const T end)
        {
            T block_total = 0;
            for (T i = start; i < end; ++i)
                block_total += i;
            return block_total;
        },
        100);
    T result = 0;
    for (std::future<T>& future : loop_future)
        result += future.get();
    return result;
}

int main()
{
    std::cout << sum<std::uint64_t>(1, 1'000'000);
}
```

必须把 `T` 显式指定为 `std::uint64_t`，即 64 位无符号整数，因为结果 500,000,500,000 放不进 32 位整数。

`BS::multi_future<T>` 是 `std::vector<std::future<T>>` 的特化，所以可以用 range-based `for` 遍历 future，再对每个 future 调用 `get()`。这些值是各块的部分和，加起来就是总和。循环分成 100 块，共 100 个 future，每个是 10,000 个数的部分和。

这个 range-based `for` 多半会在整个循环结束前就开始。每次访问一个 future 时，就绪就取值，否则等到就绪再取。这样不必等整个循环结束就可以开始累加，只要等单个块，性能更好。

如果想等整个循环结束再求和，可以对 `BS::multi_future<T>` 本身调用 `get()`，它返回装着各 future 结果的 `std::vector<T>`。然后可以用 `std::reduce`：

```cpp
#include "BS_thread_pool.hpp" // BS::multi_future, BS::thread_pool
#include <cstdint>            // std::uint64_t
#include <iostream>           // std::cout
#include <numeric>            // std::reduce
#include <vector>             // std::vector

BS::thread_pool pool;

template <typename T>
T sum(T min, T max)
{
    BS::multi_future<T> loop_future = pool.submit_blocks(
        min, max + 1,
        [](const T start, const T end)
        {
            T block_total = 0;
            for (T i = start; i < end; ++i)
                block_total += i;
            return block_total;
        },
        100);
    std::vector<T> partial_sums = loop_future.get();
    T result = std::reduce(partial_sums.begin(), partial_sums.end());
    return result;
}

int main()
{
    std::cout << sum<std::uint64_t>(1, 1'000'000);
}
```

### 并行化序列

`detach_loop()`、`submit_loop()`、`detach_blocks()`、`submit_blocks()` 把循环拆成块，每块作为一个任务提交，任务内部遍历该块范围内的全部下标，下标可能很多。有时循环下标很少，或者更一般地，有一段按下标枚举的任务序列。这时可以不拆块，把每个下标作为独立任务提交。

用 `detach_sequence()` 和 `submit_sequence()`。语法和 `detach_loop()` / `submit_loop()` 类似，但末尾没有 `num_blocks`。序列函数只能有一个参数，即下标。

`detach_sequence()` 分离任务、不返回 future，需要等整段序列结束时用 `wait()`。`submit_sequence()` 返回 `BS::multi_future`。序列里的任务若有返回值，future 里就是那些值，否则是 `void` future。

每个任务计算自己下标的阶乘：

```cpp
#include "BS_thread_pool.hpp" // BS::multi_future, BS::thread_pool
#include <cstdint>            // std::uint64_t
#include <iostream>           // std::cout
#include <vector>             // std::vector

std::uint64_t factorial(const std::uint64_t n)
{
    std::uint64_t result = 1;
    for (std::uint64_t i = 2; i <= n; ++i)
        result *= i;
    return result;
}

int main()
{
    BS::thread_pool pool;
    constexpr std::uint64_t max = 20;
    BS::multi_future<std::uint64_t> sequence_future = pool.submit_sequence(0, max + 1, factorial);
    std::vector<std::uint64_t> factorials = sequence_future.get();
    for (std::uint64_t i = 0; i < max + 1; ++i)
        std::cout << i << "! = " << factorials[i] << '\n';
}
```

每个下标的阶乘存在 `BS::multi_future` 里，用 `get()` 取出向量；向量第 `i` 个元素就是该下标的阶乘，由序列里自己的任务算出。

**警告：** 每个下标都会作为单独任务提交，所以 `detach_sequence()` 和 `submit_sequence()` 只应在下标数量较小（大约在线程数的 1 到 2 个数量级以内）、且每个下标自己的计算量不小的时候使用。如果提交 100 万个下标、每个只算 1 毫秒，把每个下标单独提交的开销会远大于并行带来的收益。

### 关于 `BS::multi_future`

辅助类 `BS::multi_future<T>` 用来收集和访问一组 future。并行循环时池会自动创建它，也可以手动存放 `submit_task()` 或其他来源的 future。它是 `std::vector<std::future<T>>` 的特化，用法类似：

* 新建 `BS::multi_future<T>` 时，可以用默认构造函数创建空对象，以后再加入 future；也可以在构造时预先传入 future 的数量。
* 用 `[]` 访问指定下标的 future，用 `push_back()` 追加。如果数量事先已知，应先 `reserve()` 再 `push_back()`，否则会多次重新分配内存，效率很低。
* `size()` 返回当前存放的 future 数量。

另外还有专门处理 future 的成员函数：

* 全部存入后，用 `wait()` 一次等待全部，或用 `get()` 取得装着全部结果的 `std::vector<T>`。
* 用 `ready_count()` 查看有多少 future 已就绪。
* 用 `valid()` 检查存放的 future 是否全部有效。
* 用 `wait_for()` 按一段时间等待全部，或用 `wait_until()` 等到某个时间点。时间耗尽或时间点到达之前全部都等到了返回 `true`，否则返回 `false`。

除了跟踪并行循环，它也可以在有几组不同任务、又想分别跟踪每一组时使用。

### 不经过循环批量提交任务

有时要提交大量任务，但它们不属于循环或序列。可以用 `detach_bulk()` 或 `submit_bulk()` 一次提交。`detach_bulk()` 只是分离任务，`submit_bulk()` 返回 `BS::multi_future`。两种用法：

1. 传入可调用对象的容器。它们必须没有参数；有参数的函数要包进 lambda。`submit_bulk()` 时，可调用对象可以有返回值，结果存在返回的 `BS::multi_future` 里，但返回类型必须相同。
2. 传入迭代器范围，和标准库算法类似。例如只提交大容器里的一个子集。

用容器调用 `submit_bulk()`：

```cpp
#include "BS_thread_pool.hpp" // BS::multi_future, BS::thread_pool
#include <functional>         // std::function
#include <iostream>           // std::cout
#include <string>             // std::string
#include <vector>             // std::vector

int main()
{
    BS::thread_pool pool;
    std::vector<std::function<std::string()>> tasks;
    tasks.emplace_back([]
        {
            return "Do something.";
        });
    tasks.emplace_back([]
        {
            return "Do something else.";
        });
    tasks.emplace_back([]
        {
            return "Do another thing.";
        });
    BS::multi_future<std::string> results = pool.submit_bulk(tasks);
    for (const std::string& result : results.get())
        std::cout << result << '\n';
}
```

## 工具类

### 用 `BS::synced_stream` 同步向流打印

多个线程并行向输出流打印时，输出可能交错。试着跑这段代码：

```cpp
#include "BS_thread_pool.hpp" // BS::thread_pool
#include <iostream>           // std::cout

BS::thread_pool pool;

int main()
{
    pool.submit_sequence(0, 5,
            [](const unsigned int i)
            {
                std::cout << "Task no. " << i << " executing.\n";
            })
        .wait();
}
```

输出会乱成这样：

```none
Task no. Task no. Task no. 3 executing.
0 executing.
Task no. 41 executing.
Task no. 2 executing.
 executing.
```

原因是：对 `std::cout` 的**单次**插入是线程安全的，但没有机制保证同一线程后续几次插入连续打印。

`BS::synced_stream` 用来消除这类同步问题。要打印的流作为构造函数参数。不传参数时使用 `std::cout`：

```cpp
// Construct a synced stream that will print to std::cout.
BS::synced_stream sync_out;
// Construct a synced stream that will print to the output stream my_stream.
BS::synced_stream sync_out(my_stream);
```

`print()` 接受任意多个参数，按给定顺序逐个插入流。`println()` 相同，但末尾再打印换行符 `\n`。这个过程用互斥量同步，对同一个 `BS::synced_stream` 对象的其他 `print()` 或 `println()` 必须等到上一次调用结束。

例如：

```cpp
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::thread_pool

BS::synced_stream sync_out;
BS::thread_pool pool;

int main()
{
    pool.submit_sequence(0, 5,
            [](const unsigned int i)
            {
                sync_out.println("Task no. ", i, " executing.");
            })
        .wait();
}
```

会打印：

```none
Task no. 0 executing.
Task no. 1 executing.
Task no. 2 executing.
Task no. 3 executing.
Task no. 4 executing.
```

**警告：** 始终先创建 `BS::synced_stream`，再创建 `BS::thread_pool`，如上例。`BS::thread_pool` 离开作用域时会等待剩余任务。如果 `BS::synced_stream` 先离开作用域，还在用它的任务会崩溃。对象按构造的相反顺序析构，所以先构造 `BS::synced_stream` 可以保证即使池正在析构，任务仍然能用到它。

`<ios>` 和 `<iomanip>` 里的大多数流操纵符都可以作为 `print()` 和 `println()` 的参数，效果与插入到关联流相同，例如 `std::setw`（设置下一次输出的宽度）、`std::setprecision`（设置浮点数精度）、`std::fixed`（按固定位数显示浮点数）。

例外是刷新操纵符 `std::endl` 和 `std::flush`。编译器无法确定该用哪个模板特化，所以它们不能直接用。应改用 `BS::synced_stream::endl` 和 `BS::synced_stream::flush`。例如：

```cpp
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::thread_pool
#include <cmath>              // std::sqrt
#include <iomanip>            // std::setprecision, std::setw
#include <ios>                // std::fixed

BS::synced_stream sync_out;
BS::thread_pool pool;

int main()
{
    sync_out.print(std::setprecision(10), std::fixed);
    pool.submit_sequence(0, 16,
            [](const unsigned int i)
            {
                sync_out.print("The square root of ", std::setw(2), i, " is ", std::sqrt(i), ".", BS::synced_stream::endl);
            })
        .wait();
}
```

只有确实需要刷新时才用 `BS::synced_stream::endl`，否则用换行符。和 `std::endl` 一样，用得太频繁会强制每次都刷新缓冲区，影响性能。

`BS::synced_stream` 也可以同时同步打印到多个流。向构造函数传入一列输出流即可。下面的程序同时打印到 `std::cout` 和日志文件：

```cpp
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::thread_pool
#include <fstream>            // std::ofstream
#include <iostream>           // std::cout

BS::thread_pool pool;

int main()
{
    std::ofstream log_file("task.log");
    BS::synced_stream sync_out(std::cout, log_file);
    pool.submit_sequence(0, 5,
            [&sync_out](const unsigned int i)
            {
                sync_out.println("Task no. ", i, " executing.");
            })
        .wait();
}
```

必须在 `main()` 结束前等待 future，否则日志文件可能在任务结束前被析构。如果用没有 future 的 `detach_sequence()`，最后一行应调用 `pool.wait()`。

这个例子没有把 `BS::synced_stream` 做成全局对象，因为要把日志文件传给构造函数。也可以用 `add_stream()` 和 `remove_stream()` 给已有对象添加或移除流。下例先用默认构造函数创建全局对象（打印到 `std::cout`），再移除 `std::cout`，改加日志文件：

```cpp
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::thread_pool
#include <fstream>            // std::ofstream
#include <iostream>           // std::cout

BS::synced_stream sync_out;
BS::thread_pool pool;

int main()
{
    std::ofstream log_file("task.log");
    sync_out.remove_stream(std::cout);
    sync_out.add_stream(log_file);
    pool.submit_sequence(0, 5,
            [](const unsigned int i)
            {
                sync_out.println("Task no. ", i, " executing.");
            })
        .wait();
}
```

常见做法是创建一个全局 `BS::synced_stream`，程序任何地方都能用，不必传给每个想打印的函数。如果同时也有全局 `BS::thread_pool`，必须先定义全局 `BS::synced_stream`，再定义全局 `BS::thread_pool`，原因见上面的警告。

内部把流存在 `std::vector<std::ostream*>` 里。添加顺序就是打印顺序。需要更细的控制时，用 `get_streams()` 取得这个向量的引用，直接操作。

## 管理任务

### 监视任务

有时想知道提交的任务处于什么状态。用这三个成员函数：

* `get_tasks_queued()`：当前在队列里等待执行的任务数。
* `get_tasks_running()`：当前正在被线程执行的任务数。
* `get_tasks_total()`：未完成任务总数，包括还在队列里的和正在执行的。

注意 `get_tasks_total() == get_tasks_queued() + get_tasks_running()`。示例：

```cpp
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::thread_pool
#include <chrono>             // std::chrono
#include <thread>             // std::this_thread

BS::synced_stream sync_out;
BS::thread_pool pool(4);

void sleep_half_second(const unsigned int i)
{
    std::this_thread::sleep_for(std::chrono::milliseconds(500));
    sync_out.println("Task ", i, " done.");
}

void monitor_tasks()
{
    sync_out.println(pool.get_tasks_total(), " tasks total, ", pool.get_tasks_running(), " tasks running, ", pool.get_tasks_queued(), " tasks queued.");
}

int main()
{
    pool.wait();
    pool.detach_sequence(0, 12, sleep_half_second);
    monitor_tasks();
    std::this_thread::sleep_for(std::chrono::milliseconds(750));
    monitor_tasks();
    std::this_thread::sleep_for(std::chrono::milliseconds(500));
    monitor_tasks();
    std::this_thread::sleep_for(std::chrono::milliseconds(500));
    monitor_tasks();
    pool.wait();
}
```

假设至少有 4 条硬件线程（这样 4 个任务可以同时跑），输出大致如下：

```none
12 tasks total, 0 tasks running, 12 tasks queued.
Task 0 done.
Task 1 done.
Task 2 done.
Task 3 done.
8 tasks total, 4 tasks running, 4 tasks queued.
Task 4 done.
Task 5 done.
Task 6 done.
Task 7 done.
4 tasks total, 4 tasks running, 0 tasks queued.
Task 8 done.
Task 9 done.
Task 10 done.
Task 11 done.
0 tasks total, 0 tasks running, 0 tasks queued.
```

开头调用 `pool.wait()`，是因为创建线程池时每条线程都会跑一个初始化任务。不等待的话，第一行会显示总共 16 个任务，其中包含 4 个初始化任务。详见[下文](#线程初始化函数)。结尾也调用了 `pool.wait()`，保证程序结束前所有任务都已完成。

### 清除队列中的任务

用户可能在多线程操作还在进行时取消它。操作可能被拆成多个任务：一半正在线程上执行，另一半还在队列里等。

线程池不能终止已经在跑的任务。C++ 没有这个功能；而且在任务运行中突然终止，后果可能很严重，例如内存泄漏和数据损坏。还在队列里等待的任务可以用 `purge()` 清除。

调用 `purge()` 之后，仍在队列里等待的任务会被丢弃，线程永远不会执行它们。被清除的任务无法恢复。

例如：

```cpp
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::thread_pool
#include <chrono>             // std::chrono
#include <thread>             // std::this_thread

BS::synced_stream sync_out;
BS::thread_pool pool(4);

int main()
{
    pool.detach_sequence(0, 8,
        [](const unsigned int i)
        {
            std::this_thread::sleep_for(std::chrono::milliseconds(100));
            sync_out.println("Task ", i, " done.");
        });
    std::this_thread::sleep_for(std::chrono::milliseconds(50));
    pool.purge();
    pool.wait();
}
```

程序向队列提交 8 个任务。每个任务等 100 毫秒再打印。池有 4 条线程，所以先并行执行前 4 个，再执行后 4 个。程序先等 50 毫秒，确保前 4 个都已开始，然后 `purge()` 清除剩下的 4 个。这 4 个永远不会执行。`purge()` 调用时前 4 个仍在运行，它们会不受打断地结束；`purge()` 只丢弃还没开始的任务。因此输出只有前 4 个任务的消息：

```none
Task 0 done.
Task 1 done.
Task 2 done.
Task 3 done.
```

如上所述，线程池自己不能终止正在运行的任务。如果需要，必须在任务内部放一个能安全结束任务的机制。例如用原子标志，任务定期检查，标志被设置就自行返回。简单例子：

```cpp
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::thread_pool
#include <chrono>             // std::chrono
#include <thread>             // std::this_thread

BS::synced_stream sync_out;
BS::thread_pool pool(4);

int main()
{
    std::atomic<bool> stop_flag = false;
    pool.detach_sequence(0, 8,
        [&stop_flag](const unsigned int i)
        {
            std::this_thread::sleep_for(std::chrono::milliseconds(100));
            if (stop_flag)
                return;
            sync_out.println("Task ", i, " done.");
        });
    std::this_thread::sleep_for(std::chrono::milliseconds(50));
    stop_flag = true;
    pool.purge();
    pool.wait();
}
```

这个程序不会打印任何东西，因为 `stop_flag` 被设为 `true` 后任务会提前返回。这里其实不必调用 `purge()`，但调用它可以避免另外 4 个任务白白执行。

### 异常处理

`submit_task()` 会捕获任务抛出的异常，并转发给对应的 future。之后在 future 的 `get()` 上可以捕获。例如：

```cpp
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::thread_pool
#include <exception>          // std::exception
#include <future>             // std::future
#include <stdexcept>          // std::runtime_error

BS::synced_stream sync_out;
BS::thread_pool pool;

double inverse(const double x)
{
    if (x == 0)
        throw std::runtime_error("Division by zero!");
    return 1 / x;
}

int main()
{
    constexpr double num = 0;
    std::future<double> my_future = pool.submit_task(
        [num]
        {
            return inverse(num);
        });
    try
    {
        const double result = my_future.get();
        sync_out.println("The inverse of ", num, " is ", result, ".");
    }
    catch (const std::exception& e)
    {
        sync_out.println("Caught exception: ", e.what());
    }
}
```

输出为：

```none
Caught exception: Division by zero!
```

如果把 `num` 改成任何非零数，就不会抛异常，并打印倒数。

注意 `wait()` 不抛异常，只有 `get()` 会。因此即使任务没有返回值、future 是 `std::future<void>`，要捕获它抛出的异常，仍然必须对 future 调用 `get()`。例如：

```cpp
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::thread_pool
#include <exception>          // std::exception
#include <future>             // std::future
#include <stdexcept>          // std::runtime_error

BS::synced_stream sync_out;
BS::thread_pool pool;

void print_inverse(const double x)
{
    if (x == 0)
        throw std::runtime_error("Division by zero!");
    sync_out.println("The inverse of ", x, " is ", 1 / x, ".");
}

int main()
{
    constexpr double num = 0;
    std::future<void> my_future = pool.submit_task(
        [num]
        {
            print_inverse(num);
        });
    try
    {
        my_future.get();
    }
    catch (const std::exception& e)
    {
        sync_out.println("Caught exception: ", e.what());
    }
}
```

用 `BS::multi_future<T>` 一次处理多个 future 时，异常处理方式相同：如果其中某个 future 可能抛异常，调用 `get()` 时可以捕获，`BS::multi_future<void>` 也一样。

如果用 `detach_task()` 或其他 `detach` 成员函数，没有 future 返回，也就无法捕获任务抛出的异常。这时任务抛出的异常会被静默忽略，以免程序终止。要在分离任务里捕获异常，必须在任务内部自己捕获，例如：

```cpp
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::thread_pool
#include <exception>          // std::exception
#include <stdexcept>          // std::runtime_error

BS::synced_stream sync_out;
BS::thread_pool pool;

double inverse(const double x)
{
    if (x == 0)
        throw std::runtime_error("Division by zero!");
    return 1 / x;
}

int main()
{
    constexpr double num = 0;
    pool.detach_task(
        [num]
        {
            try
            {
                const double result = inverse(num);
                sync_out.println("The inverse of ", num, " is ", result, ".");
            }
            catch (const std::exception& e)
            {
                sync_out.println("Caught exception: ", e.what());
            }
        });
    pool.wait();
}
```

如果代码库显式禁用了异常，或者特性测试宏 `__cpp_exceptions` 因其他原因未定义，线程池会自动关闭异常处理。

### 查询当前线程

`BS::this_thread` 提供与 `std::this_thread` 类似的功能，让线程引用自身。它有这些静态成员函数：

* `BS::this_thread::get_index()` 返回当前线程的下标，类型是 `std::optional<std::size_t>`。
    * 如果这条线程属于某个 `BS::thread_pool`，返回值是 `[0, N)` 范围内的下标，其中 `N == BS::thread_pool::get_thread_count()`。
    * 否则，例如主线程或不属于任何池的独立线程，返回 `std::nullopt`。
* `BS::this_thread::get_pool()` 返回拥有当前线程的线程池指针，类型是 `std::optional<void*>`。
    * 如果这条线程属于某个 `BS::thread_pool`，返回指向该对象的 `void` 指针。
    * 否则返回 `std::nullopt`。

[`std::optional`](https://zh.cppreference.com/w/cpp/utility/optional) 是一个可能有值、也可能没有值的对象。[`std::nullopt`](https://zh.cppreference.com/w/cpp/utility/optional/nullopt) 表示没有值。访问 `std::optional` 时应先用 [`std::optional::has_value()`](https://zh.cppreference.com/w/cpp/utility/optional/operator_bool) 检查是否有值，有值再用 [`std::optional::value()`](https://zh.cppreference.com/w/cpp/utility/optional/value) 取出。`if (x.has_value())` 可以写成 `if (x)`，`x.value()` 可以写成 `*x`。

`BS::this_thread::get_pool()` 返回 `void*`，是因为 `BS::thread_pool` 是模板。拿到指针后，要用成员函数就必须转换成想要的模板实例。必须转成正确的类型。例如把指向 `BS::light_thread_pool` 的指针转成 `BS::priority_thread_pool*`，程序会有未定义行为。模板参数和别名见[可选功能](#可选功能)。

完整例子：

```cpp
#include "BS_thread_pool.hpp" // BS::light_thread_pool, BS::synced_stream, BS::this_thread
#include <atomic>             // std::atomic
#include <cstddef>            // std::size_t
#include <optional>           // std::optional
#include <thread>             // std::thread

BS::synced_stream sync_out;
BS::light_thread_pool p1;
BS::light_thread_pool p2;
std::atomic<char> ltr = 'A';

void check_this_thread(const char letter)
{
    const std::optional<void*> my_pool = BS::this_thread::get_pool();
    const std::optional<std::size_t> my_index = BS::this_thread::get_index();

    if (my_pool && my_index)
    {
        const std::size_t pool_number = *my_pool == &p1 ? 1 : 2;
        sync_out.println("Task ", letter, " is being executed by thread #", *my_index, " of pool #", pool_number, '.');
        static_cast<BS::light_thread_pool*>(*my_pool)->detach_task(
            [letter]
            {
                sync_out.println("-> Task ", ltr++, " was submitted by task ", letter, " using detach_task().");
            });
    }
    else
    {
        sync_out.println("Task ", letter, " is being executed by an independent thread, not in any thread pools.");
        std::thread(
            [letter]
            {
                sync_out.println("-> Task ", ltr++, " was submitted by task ", letter, " using a detached std::thread.");
            })
            .detach();
    }
}

int main()
{
    p1.submit_task(
          []
          {
              check_this_thread(ltr++);
          })
        .wait();
    p2.submit_task(
          []
          {
              check_this_thread(ltr++);
          })
        .wait();
    std::thread(
        []
        {
            check_this_thread(ltr++);
        })
        .join();
}
```

输出大致如下：

```none
Task A is being executed by thread #3 of pool #1.
-> Task B was submitted by task A using detach_task().
Task C is being executed by thread #7 of pool #2.
-> Task D was submitted by task C using detach_task().
Task E is being executed by an independent thread, not in any thread pools.
-> Task F was submitted by task E using a detached std::thread.
```

这个例子用三种方式执行 `check_this_thread()`：

1. 从线程池 `p1` 提交。
2. 从线程池 `p2` 提交。
3. 从独立的 `std::thread` 提交。

任务调用 `BS::this_thread::get_pool()` 和 `BS::this_thread::get_index()`，得到两个 `std::optional`：`my_pool` 和 `my_index`。两者都有值（求值为 `true`）时，任务知道自己跑在线程池里。“解引用”得到实际值：池指针是 `*my_pool`，线程下标是 `*my_index`。

任务把 `*my_pool` 和 `p1`、`p2` 的地址比较，判断自己在哪个池里，并从 `*my_index` 取得线程下标。然后它先把 `void*` 转成正确类型（这里是 `BS::light_thread_pool*`），再对该池调用 `detach_task()`，从自己的池里分离另一个任务（不等待它，否则可能死锁）。

如果 `my_pool` 和 `my_index` 没有值（求值为 `false`），任务知道自己跑在独立线程上。这时它用另一条独立线程分离额外任务。

### 线程初始化函数

有时需要在线程执行任何任务之前先初始化它们。可以把初始化函数传给 `BS::thread_pool` 构造函数或 `reset()`，作为唯一参数，或作为线程数之后的第二个参数。

初始化函数不能有返回值。它可以接受一个 `std::size_t` 类型的线程下标，也可以没有参数。没有参数时，函数可以用 `BS::this_thread::get_index()` 取得下标，也可以用 `BS::this_thread::get_pool()` 判断线程属于哪个池。

初始化函数实际上作为一组特殊任务提交，每条线程一个。它们绕过队列，但仍计入正在运行的任务数。因此池刚初始化后立刻查询，`get_tasks_total()` 和 `get_tasks_running()` 会报告这些任务正在运行。

这样做是为了让用户可以选择对池调用 `wait()` 等待初始化结束，也可以继续往下走。无论哪种情况，对应线程执行任何任务之前，初始化函数都会先跑完。除非它们有影响主线程的副作用，或者必须在**所有**线程上都跑完之后池才能开始执行任务，否则没有必要等待。

简单例子：

```cpp
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::thread_pool
#include <random>             // std::mt19937_64, std::random_device

BS::synced_stream sync_out;
thread_local std::mt19937_64 twister;

int main()
{
    BS::thread_pool pool(
        []
        {
            twister.seed(std::random_device()());
        });
    pool.submit_sequence(0, 4,
            [](int)
            {
                sync_out.println("I generated a random number: ", twister());
            })
        .wait();
}
```

这里创建一个 `thread_local` 的梅森旋转引擎，每条线程有自己独立的引擎。如果不播种，每条线程会生成完全相同的伪随机序列。因此向构造函数传入初始化函数，用（希望是）非确定性的 `std::random_device` 给每条线程的引擎播种。

传给 `submit_sequence()` 的 lambda 签名是 `[](int)`，有一个未命名的 `int` 参数，因为不用序列下标（范围是 `[0, 4)`）。这是把同一个任务提交多次的简便写法。

**警告：** 线程初始化函数不能抛异常，否则程序会终止。异常必须在函数内部显式处理。

### 线程清理函数

和初始化函数类似，也可以给池提供清理函数，在每条线程销毁之前运行。销毁发生在池析构或 `reset()` 时。清理函数同样不能有返回值，可以接受一个 `std::size_t` 类型的线程下标，也可以没有参数。每个池可以有自己的清理函数，用 `set_cleanup_func()` 设置。例如：

```cpp
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::this_thread, BS::thread_pool
#include <chrono>             // std::chrono
#include <cstddef>            // std::size_t
#include <fstream>            // std::ofstream
#include <string>             // std::to_string
#include <thread>             // std::this_thread

thread_local std::ofstream log_file;
thread_local BS::synced_stream sync_out(log_file);
constexpr std::size_t threads = 4;

int main()
{
    BS::thread_pool pool(threads,
        [](const std::size_t idx)
        {
            log_file.open("thread_" + std::to_string(idx) + ".log");
        });
    pool.set_cleanup_func(
        []
        {
            log_file.close();
        });
    pool.submit_sequence(0, threads * 10,
            [](const std::size_t idx)
            {
                std::this_thread::sleep_for(std::chrono::milliseconds(50));
                sync_out.println("Task ", idx, " is running on thread ", *BS::this_thread::get_index(), '.');
            })
        .wait();
}
```

这里创建 4 条线程，每条有自己的 thread-local `BS::synced_stream`，写入形如 `thread_N.log` 的日志文件，`N` 是线程下标。传给构造函数的初始化函数打开日志文件。用 `set_cleanup_func()` 设置的清理函数关闭日志文件。

用 `submit_sequence()` 提交 40 个任务，每个向日志打印自己跑在哪条线程上。`main()` 退出、`pool` 被销毁时，每条线程都会调用清理函数，保证日志文件正确关闭。

**警告：** 和初始化函数一样，清理函数不能抛异常，否则程序会终止。异常必须在函数内部显式处理。

### 以常量引用传递任务参数

C++ 里经常需要按引用或常量引用传参，而不是按值。这样函数直接访问被传入的对象，不必复制一份。[前面](#分离任务并等待)已经看到，按引用提交就是在 lambda 捕获列表里用 `&`。要按**常量**引用提交，可以用 `std::as_const()`，如下例：

```cpp
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::thread_pool
#include <utility>            // std::as_const

BS::synced_stream sync_out;

void increment(int& x)
{
    ++x;
}

void print(const int& x)
{
    sync_out.println(x);
}

int main()
{
    BS::thread_pool pool;
    int n = 0;
    pool.submit_task(
            [&n]
            {
                increment(n);
            })
        .wait();
    pool.submit_task(
            [&n = std::as_const(n)]
            {
                print(n);
            })
        .wait();
}
```

`increment()` 接受整数的**引用**并自增。按引用传参保证增加的是 `main()` 作用域里的 `n` 本身，而不是 `increment()` 里的副本。

`print()` 接受整数的**常量引用**并打印。按常量引用传参保证访问的是 `n` 本身，函数也不会意外修改它。如果把 `print()` 换成 `increment()`，程序无法编译，因为 `increment()` 不能接受常量引用。

一般不必非按常量引用传递，但如果要保证被引用的变量确实不会被修改，这样做更“正确”。

## 可选功能

### 启用功能

线程池有一些可选功能，默认关闭，以减少开销。创建池时向 `BS::thread_pool` 传入相应模板参数即可启用。模板参数是位掩码，可以用按位或 `|` 一次启用多项。标志是枚举类 `BS::tp` 的成员：

* `BS::tp::priority` 启用[任务优先级](#设置任务优先级)。
* `BS::tp::pause` 启用[暂停线程池](#暂停线程池)。
* `BS::tp::wait_deadlock_checks` 启用[等待死锁检查](#避免等待死锁)。
* 默认是 `BS::tp::none`，关闭全部可选功能。

例如同时启用任务优先级和暂停：

```cpp
BS::thread_pool<BS::tp::priority | BS::tp::pause> pool;
```

便利别名：

* `BS::light_thread_pool` 关闭全部可选功能（等价于默认模板参数的 `BS::thread_pool`，即 `BS::thread_pool<BS::tp::none>`）。
* `BS::priority_thread_pool` 启用任务优先级（等价于 `BS::thread_pool<BS::tp::priority>`）。
* `BS::pause_thread_pool` 启用暂停（等价于 `BS::thread_pool<BS::tp::pause>`）。
* `BS::wdc_thread_pool` 启用等待死锁检查（等价于 `BS::thread_pool<BS::tp::wait_deadlock_checks>`）。

没有同时启用多项的别名。需要时必须显式传模板参数，或自己定义别名，并用上面的按位或。

可选功能是按每个 `BS::thread_pool` 对象单独启用的，同一个程序里可以有启用了不同功能的多个池。例如不需要优先级的任务用 `BS::light_thread_pool`，需要优先级的用单独的 `BS::priority_thread_pool`。

### 设置任务优先级

在 `BS::thread_pool` 的模板参数里打开 `BS::tp::priority` 即启用任务优先级。库还定义了便利别名 `BS::priority_thread_pool`，等价于 `BS::thread_pool<BS::tp::priority>`。启用后，静态成员 `priority_enabled` 为 `true`。

任务或一组任务的优先级可以作为额外参数（放在参数列表末尾）传给 `detach_task()`、`submit_task()`、`detach_blocks()`、`submit_blocks()`、`detach_loop()`、`submit_loop()`、`detach_sequence()`、`submit_sequence()`、`detach_bulk()` 和 `submit_bulk()`。不指定时默认是 0。

优先级类型是 `BS::priority_t`，即有符号 8 位整数，范围是 -128 到 +127。任务按优先级从高到低执行。如果给块/循环/序列这类一次提交多个任务的函数指定优先级，这些任务的优先级都相同。

枚举 `BS::pr` 预定义了一些优先级，避免魔数，也便于以后改数值。按优先级从高到低：`BS::pr::highest`、`BS::pr::high`、`BS::pr::normal`、`BS::pr::low`、`BS::pr::lowest`。

简单例子：

```cpp
#include "BS_thread_pool.hpp" // BS::priority_thread_pool, BS::synced_stream

BS::synced_stream sync_out;
BS::priority_thread_pool pool(1);

int main()
{
    pool.detach_task(
        []
        {
            sync_out.println("This task will execute third.");
        },
        BS::pr::normal);
    pool.detach_task(
        []
        {
            sync_out.println("This task will execute fifth.");
        },
        BS::pr::lowest);
    pool.detach_task(
        []
        {
            sync_out.println("This task will execute second.");
        },
        BS::pr::high);
    pool.detach_task(
        []
        {
            sync_out.println("This task will execute first.");
        },
        BS::pr::highest);
    pool.detach_task(
        []
        {
            sync_out.println("This task will execute fourth.");
        },
        BS::pr::low);
}
```

程序会按正确的优先级顺序打印。为了简单，这里的池只有 1 条线程，任务一次只跑一个。如果池有 5 条或更多线程，这 5 个任务会大致同时跑：最高优先级的任务还在跑时，次高优先级的会被另一条线程取走。

这只是教学例子。实际使用中，可能希望立刻完成的任务用高优先级，跳过队列里已有的任务；不紧急的后台任务用低优先级，等高优先级的做完再执行。

任务优先级用 [`std::priority_queue`](https://zh.cppreference.com/w/cpp/container/priority_queue) 实现。存入新任务是 O(log n)，取出下一个（即最高优先级）任务是 O(1)。关闭优先级时用的是 [`std::queue`](https://zh.cppreference.com/w/cpp/container/queue)，存和取都是 O(1)。

因此启用优先队列可能略微降低性能，具体取决于用法，所以默认关闭。得到的是功能，付出的是性能。差别通常不大，编译器优化常常能把它降到可以忽略。

最后，使用优先队列时，任务的执行顺序**不一定**和提交顺序相同，**即使优先级全部相同**。这是因为 `std::priority_queue` 实现为[二叉堆](https://zh.wikipedia.org/wiki/二叉堆)，任务按二叉树存放，而不是按顺序存放。

### 暂停线程池

在模板参数里打开 `BS::tp::pause` 即启用暂停。便利别名 `BS::pause_thread_pool` 等价于 `BS::thread_pool<BS::tp::pause>`。启用后，静态成员 `pause_enabled` 为 `true`。

这会启用 `pause()`、`unpause()` 和 `is_paused()`。调用 `pause()` 后，工作线程暂时不再从队列取新任务。已经在执行的任务会继续跑完，因为线程池控制不了任务内部的代码。如果要在执行中途暂停某个任务，必须在任务自己里面写暂停机制。要恢复取任务，调用 `unpause()`。要查询当前是否暂停，调用 `is_paused()`。

例如：

```cpp
#include "BS_thread_pool.hpp" // BS::pause_thread_pool, BS::synced_stream
#include <chrono>             // std::chrono
#include <thread>             // std::this_thread

BS::synced_stream sync_out;
BS::pause_thread_pool pool(4);

void sleep_half_second(const unsigned int i)
{
    std::this_thread::sleep_for(std::chrono::milliseconds(500));
    sync_out.println("Task ", i, " done.");
}

void check_if_paused()
{
    if (pool.is_paused())
        sync_out.println("Pool paused.");
    else
        sync_out.println("Pool unpaused.");
}

int main()
{
    pool.detach_sequence(0, 8, sleep_half_second);
    sync_out.println("Submitted 8 tasks.");
    std::this_thread::sleep_for(std::chrono::milliseconds(250));
    pool.pause();
    check_if_paused();
    std::this_thread::sleep_for(std::chrono::milliseconds(1000));
    sync_out.println("Still paused...");
    std::this_thread::sleep_for(std::chrono::milliseconds(1000));
    pool.detach_sequence(8, 12, sleep_half_second);
    sync_out.println("Submitted 4 more tasks.");
    sync_out.println("Still paused...");
    std::this_thread::sleep_for(std::chrono::milliseconds(1000));
    pool.unpause();
    check_if_paused();
}
```

假设至少有 4 条硬件线程，输出大致如下：

```none
Submitted 8 tasks.
Pool paused.
Task 0 done.
Task 1 done.
Task 2 done.
Task 3 done.
Still paused...
Submitted 4 more tasks.
Still paused...
Pool unpaused.
Task 4 done.
Task 5 done.
Task 6 done.
Task 7 done.
Task 8 done.
Task 9 done.
Task 10 done.
Task 11 done.
```

这里先提交 8 个任务。前 4 个立刻开始（池只有 4 条线程）。等 250 毫秒后暂停。已经在跑的任务（各跑 500 毫秒）会继续跑完；暂停不影响正在运行的任务。另外 4 个暂时不会执行。暂停期间再提交 4 个任务，它们只是排在队列末尾。取消暂停后，先执行最初剩下的 4 个，再执行新的 4 个。

工作线程暂停时，`wait()` 只等待正在运行的任务，而不是全部任务（否则会永远等下去）。下面的程序演示这一点：

```cpp
#include "BS_thread_pool.hpp" // BS::pause_thread_pool, BS::synced_stream
#include <chrono>             // std::chrono
#include <thread>             // std::this_thread

BS::synced_stream sync_out;
BS::pause_thread_pool pool(4);

void sleep_half_second(const unsigned int i)
{
    std::this_thread::sleep_for(std::chrono::milliseconds(500));
    sync_out.println("Task ", i, " done.");
}

void check_if_paused()
{
    if (pool.is_paused())
        sync_out.println("Pool paused.");
    else
        sync_out.println("Pool unpaused.");
}

int main()
{
    pool.detach_sequence(0, 8, sleep_half_second);
    sync_out.println("Submitted 8 tasks. Waiting for them to complete.");
    pool.wait();
    pool.detach_sequence(8, 20, sleep_half_second);
    sync_out.println("Submitted 12 more tasks.");
    std::this_thread::sleep_for(std::chrono::milliseconds(250));
    pool.pause();
    check_if_paused();
    sync_out.println("Waiting for the ", pool.get_tasks_running(), " running tasks to complete.");
    pool.wait();
    sync_out.println("All running tasks completed. ", pool.get_tasks_queued(), " tasks still queued.");
    std::this_thread::sleep_for(std::chrono::milliseconds(1000));
    sync_out.println("Still paused...");
    std::this_thread::sleep_for(std::chrono::milliseconds(1000));
    sync_out.println("Still paused...");
    std::this_thread::sleep_for(std::chrono::milliseconds(1000));
    pool.unpause();
    check_if_paused();
    std::this_thread::sleep_for(std::chrono::milliseconds(250));
    sync_out.println("Waiting for the remaining ", pool.get_tasks_total(), " tasks (", pool.get_tasks_running(), " running and ", pool.get_tasks_queued(), " queued) to complete.");
    pool.wait();
    sync_out.println("All tasks completed.");
}
```

输出大致如下：

```none
Submitted 8 tasks. Waiting for them to complete.
Task 0 done.
Task 1 done.
Task 2 done.
Task 3 done.
Task 4 done.
Task 5 done.
Task 6 done.
Task 7 done.
Submitted 12 more tasks.
Pool paused.
Waiting for the 4 running tasks to complete.
Task 8 done.
Task 9 done.
Task 10 done.
Task 11 done.
All running tasks completed. 8 tasks still queued.
Still paused...
Still paused...
Pool unpaused.
Waiting for the remaining 8 tasks (4 running and 4 queued) to complete.
Task 12 done.
Task 13 done.
Task 14 done.
Task 15 done.
Task 16 done.
Task 17 done.
Task 18 done.
Task 19 done.
All tasks completed.
```

第一次 `wait()` 发生在未暂停时，等待全部 8 个任务，包括正在跑的和排队的。第二次 `wait()` 发生在暂停之后，只等待 4 个正在跑的任务，另外 8 个仍在队列里，因为池已暂停所以不会执行。第三次 `wait()` 发生在取消暂停之后，等待剩下的 8 个任务，包括正在跑的和排队的。

暂停会给等待函数和工作函数增加额外检查，开销很小但不是零，所以默认关闭。

**警告：** 如果线程池在暂停状态下被销毁，队列里剩下的任务永远不会执行！

### 避免等待死锁

在模板参数里打开 `BS::tp::wait_deadlock_checks` 即启用等待死锁检查。便利别名 `BS::wdc_thread_pool` 等价于 `BS::thread_pool<BS::tp::wait_deadlock_checks>`。启用后，静态成员 `wait_deadlock_checks_enabled` 为 `true`。

看这段程序就能明白它的用途：

```cpp
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::thread_pool

BS::synced_stream sync_out;
BS::thread_pool pool;

int main()
{
    pool.detach_task(
        []
        {
            pool.wait();
            sync_out.println("Done waiting.");
        });
}
```

程序创建一个线程池，再分离一个任务，让它等待同一个池里的任务完成。运行后永远不会打印 “Done waiting”，因为这个任务在等待**自己**完成。这就是**死锁**，程序会永远等下去。

简单程序里通常不会发生。更复杂的程序，例如同时跑多个线程池，就可能出现等待死锁。这时检查有用。启用后，`wait()`、`wait_for()`、`wait_until()` 会检查调用者是否就在同一个池的某条线程上；如果是，它们不进入等待，而是抛出 `BS::wait_deadlock`。

例如：

```cpp
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::wdc_thread_pool

BS::synced_stream sync_out;
BS::wdc_thread_pool pool;

int main()
{
    pool.detach_task(
        []
        {
            try
            {
                pool.wait();
                sync_out.println("Done waiting.");
            }
            catch (const BS::wait_deadlock&)
            {
                sync_out.println("Error: Deadlock!");
            }
        });
}
```

这次 `wait()` 会检测到死锁并抛异常，输出是 “Error: Deadlock!”。

默认关闭，因为等待死锁不常见，而且每次调用 `wait()`、`wait_for()` 或 `wait_until()` 都会增加一点开销。如果特性测试宏 `__cpp_exceptions` 未定义，等待死锁检查会自动关闭；这时若创建带 `BS::tp::wait_deadlock_checks` 的池，编译会失败。

## 原生扩展

### 启用原生扩展

可移植是本库的原则之一，但用操作系统原生 API 设置线程优先级这类不可移植的功能也很有用。因此库包含原生扩展，默认关闭，因为它们不可移植。（只要原生扩展保持关闭，库就是 100% 标准 C++。）

编译时定义宏 `BS_THREAD_POOL_NATIVE_EXTENSIONS` 即可启用。如果以头文件方式包含，必须在 `#include "BS_thread_pool.hpp"` **之前**定义该宏。即使定义了宏，若没有检测到受支持的操作系统（Windows、Linux 或 macOS），原生扩展仍会自动关闭。

如果[作为 C++20 模块](#作为-c20-模块导入)导入，在导入模块之前定义宏无效，因为模块看不到导入它的程序里定义的宏。必须作为编译器标志定义：Clang 和 GCC 用 `-D BS_THREAD_POOL_NATIVE_EXTENSIONS`，MSVC 用 `/D BS_THREAD_POOL_NATIVE_EXTENSIONS`。

[测试程序](#测试)只在编译时定义了 `BS_THREAD_POOL_NATIVE_EXTENSIONS` 时才测试原生扩展。如果[作为 C++20 模块](#作为-c20-模块导入)导入，编译模块时也必须启用该宏。

`constexpr` 标志 `BS::thread_pool_native_extensions` 表示库编译时是否启用了原生扩展。如果定义了 `BS_THREAD_POOL_NATIVE_EXTENSIONS` 但操作系统不受支持，该标志为 `false`。

**警告：** 原生扩展只在[编译与兼容性](#编译与兼容性)列出的操作系统上测试过。没有在这些系统的旧版本、其他 Linux 发行版或其他操作系统上测试，因此不保证在每个系统上都能工作。遇到问题请到 [GitHub 仓库](https://github.com/bshoshany/thread-pool) 报告。

### 设置线程优先级

原生扩展可以用操作系统原生 API 设置线程优先级。这和[设置任务优先级](#设置任务优先级)**不是**一回事。任务优先级是队列的功能，和池里的线程本身无关。任务优先级决定哪些任务先执行；线程优先级大致决定一条线程相对其他线程能分到多少 CPU 时间。原生扩展也可以设置任意线程的优先级，例如用 `std::thread` 创建的线程，不只是池里的线程。

对性能敏感的程序可能想提高线程优先级；应在后台运行的程序可能想降低它。不同操作系统处理优先级的方式差别很大，库用枚举类 `BS::os_thread_priority` 在原生 API 之上做了一层抽象，共 7 个成员：

* `BS::os_thread_priority::idle`
* `BS::os_thread_priority::lowest`
* `BS::os_thread_priority::below_normal`
* `BS::os_thread_priority::normal`
* `BS::os_thread_priority::above_normal`
* `BS::os_thread_priority::highest`
* `BS::os_thread_priority::realtime`

在 Windows 上，这些预定义优先级与 [Windows API 的线程优先级](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-setthreadpriority) 一一对应（`realtime` 对应 time critical）。在 Linux 和 macOS 上，线程优先级复杂得多，这些预定义值被映射到原生 API 可用的参数。

在 Linux（POSIX 线程）上，线程优先级由三个因素决定：[调度策略](https://www.man7.org/linux/man-pages/man3/pthread_setschedparam.3.html)、优先级数值，以及 [nice 值](https://www.man7.org/linux/man-pages/man2/setpriority.2.html)。库的抽象层为了简单和可移植，把它们归纳成上面的预定义等级。参数组合远不止这些，但更细的控制不可移植，实际用处也有限。精确映射见头文件 `BS_thread_pool.hpp` 的源码。

在 macOS 上同样使用 POSIX 线程，但和 Linux 不同，nice 值是按进程而不是按线程的（符合 POSIX 标准）。不过 macOS 在可用优先级范围上更自由。精确映射同样见源码。

大多数用户不必关心各操作系统的具体细节。抽象层的目的就是尽量简单、尽量可移植。但要注意：只有 Windows 允许非特权用户把线程优先级调高。在 Linux 和 macOS 上，非特权用户只能调低，只有 root 能调高；而且容易搞混的是，如果用户把优先级从 normal 降到更低，没有 root 权限就不能再调回 normal，即使 normal 是线程最初的优先级。

线程优先级由 `BS::this_thread` 的两个静态成员函数管理：

* `BS::this_thread::get_os_thread_priority()` 读取当前线程优先级，返回 `std::optional<BS::os_thread_priority>`。如果对象没有值，说明优先级无法确定，或者不是上面列出的预定义值之一。
* `BS::this_thread::set_os_thread_priority()` 设置当前线程优先级。成功返回 `true`，否则返回 `false`。`false` 通常表示用户没有设置该优先级所需的权限。

用[初始化函数](#线程初始化函数)最容易一次提高或降低池中全部线程的优先级。例如：

```cpp
#define BS_THREAD_POOL_NATIVE_EXTENSIONS
#include "BS_thread_pool.hpp" // BS::os_thread_priority, BS::synced_stream, BS::this_thread, BS::thread_pool
#include <cstddef>            // std::size_t
#include <map>                // std::map
#include <optional>           // std::optional
#include <string>             // std::string

BS::synced_stream sync_out;
BS::os_thread_priority target = BS::os_thread_priority::highest;

const std::map<BS::os_thread_priority, std::string> os_thread_priority_map = {{BS::os_thread_priority::idle, "idle"}, {BS::os_thread_priority::lowest, "lowest"}, {BS::os_thread_priority::below_normal, "below_normal"}, {BS::os_thread_priority::normal, "normal"}, {BS::os_thread_priority::above_normal, "above_normal"}, {BS::os_thread_priority::highest, "highest"}, {BS::os_thread_priority::realtime, "realtime"}};

std::string os_thread_priority_name(const BS::os_thread_priority priority)
{
    const std::map<BS::os_thread_priority, std::string>::const_iterator it = os_thread_priority_map.find(priority);
    return (it != os_thread_priority_map.end()) ? it->second : "unknown";
}

void set_priority(const std::size_t idx)
{
    const std::optional<BS::os_thread_priority> get_result = BS::this_thread::get_os_thread_priority();
    if (get_result)
        sync_out.println("The OS thread priority of thread ", idx, " is currently set to '", os_thread_priority_name(*get_result), "'.");
    else
        sync_out.println("Error: Failed to get the OS thread priority of thread ", idx, '!');
    const bool set_result = BS::this_thread::set_os_thread_priority(target);
    sync_out.println(set_result ? "Successfully" : "Error: Failed to", " set the OS priority of thread ", idx, " to '", os_thread_priority_name(target), "'.");
}

int main()
{
    BS::thread_pool pool(4, set_priority);
}
```

在 Linux 或 macOS 上请用 `sudo` 以 root 运行，否则会失败。这里用初始化函数 `set_priority()` 先打印每条线程的初始优先级（应为 “normal”），再把每条线程设为 “highest”。`os_thread_priority_name()` 把 `BS::os_thread_priority` 转成可读字符串。

### 设置线程亲和性

原生扩展可以用操作系统原生 API 设置线程的处理器亲和性。处理器亲和性有时叫 “pinning”，控制线程允许在哪些逻辑处理器上运行。一般而言，非超线程核心对应一个逻辑处理器，超线程核心对应两个。

这有助于性能优化，可以减少缓存未命中。但也可能降低性能，有时降得很厉害，因为在分配给它的核心空出来之前，线程根本不会运行。因此除了很特殊的情况，通常最好让操作系统调度器自己管理亲和性。

设置线程亲和性在 Windows 和 Linux 上可用，在 macOS 和 Android 上不可用，因为原生 API 不允许。各操作系统处理方式不同，库在原生 API 之上做了抽象：用 `std::vector<bool>` 控制亲和性，每个元素对应一个逻辑处理器。

由 `BS::this_thread` 的两个静态成员函数管理：

* `BS::this_thread::get_os_thread_affinity()` 读取当前线程亲和性，返回 `std::optional<std::vector<bool>>`。对象没有值表示无法确定。在 macOS 和 Android 上总是返回 `std::nullopt`。
* `BS::this_thread::set_os_thread_affinity()` 设置当前线程亲和性。成功返回 `true`，否则返回 `false`。在 macOS 和 Android 上总是返回 `false`。

线程亲和性必须是该线程所属进程的进程亲和性的子集。进程亲和性用 [`BS::get_os_process_affinity()`](#设置进程亲和性) 取得。

如果多条线程访问同一份数据，设置亲和性可以明显提高性能，因为数据可以留在这些线程所在核心的本地缓存里。下面的程序说明这一点：

```cpp
#define BS_THREAD_POOL_NATIVE_EXTENSIONS
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::this_thread
#include <atomic>             // std::atomic
#include <chrono>             // std::chrono
#include <cstdint>            // std::uint64_t
#include <thread>             // std::thread
#include <vector>             // std::vector

void do_test(const bool pin_threads)
{
    BS::synced_stream sync_out;
    constexpr std::uint64_t num_increments = 10'000'000;
    sync_out.println(pin_threads ? "With   " : "Without", " thread pinning:");
    std::atomic<std::uint64_t> counter = 0;
    auto worker = [&counter, pin_threads]
    {
        if (pin_threads)
        {
            std::vector<bool> affinity(std::thread::hardware_concurrency(), false);
            affinity[0] = true;
            BS::this_thread::set_os_thread_affinity(affinity);
        }
        for (std::uint64_t i = 0; i < num_increments; ++i)
            ++counter;
    };
    const std::chrono::steady_clock::time_point start = std::chrono::steady_clock::now();
    std::thread thread1(worker);
    std::thread thread2(worker);
    thread1.join();
    thread2.join();
    const std::chrono::steady_clock::time_point end = std::chrono::steady_clock::now();
    sync_out.println("Final count: ", counter, ", execution time: ", (std::chrono::duration_cast<std::chrono::milliseconds>(end - start)).count(), " ms.");
}

int main()
{
    do_test(false);
    do_test(true);
}
```

输出大致如下：

```none
Without thread pinning:
Final count: 20000000, execution time: 160 ms.
With thread pinning:
Final count: 20000000, execution time: 68 ms.
```

程序创建两条线程，各自把原子计数器加 1000 万次。先不做 pinning：操作系统多半会把它们放到两个不同核心上，原子变量的状态必须在两个核心之间同步，带来性能损失。再做 pinning：用 `BS::this_thread::set_os_thread_affinity()` 传入一个向量，下标 0 为 `true`、其余为 `false`，把每条线程钉在核心 0 上。这时原子变量留在核心 0 的本地缓存里，性能更高。

**警告：** 给池里的线程设置亲和性几乎从来不是好主意。任务提交到线程池后，你控制不了它实际跑在哪条线程上。亲和性的主要好处是减少缓存未命中，但提交到池里的任务无法保证访问同一份数据的任务跑在同一核心上。给池线程设置亲和性几乎肯定会降低性能，有时降很多，因为操作系统调度器不能再以最优方式把线程分配到核心。`BS::this_thread::set_os_thread_affinity()` 最常见的用途，是给独立于任何池创建的线程设置亲和性，例如用 `std::thread` 创建的线程。

### 设置线程名

原生扩展可以用操作系统原生 API 设置线程名。调试时有用，线程名会显示在调试器里（例如 Visual Studio Code 的调用堆栈）。

和原生扩展的其他功能一样，库在原生 API 之上做了抽象，由 `BS::this_thread` 的两个静态成员函数组成：

* `BS::this_thread::get_os_thread_name()` 读取当前线程名，返回 `std::optional<std::string>`。对象没有值表示无法确定名称。
* `BS::this_thread::set_os_thread_name()` 设置当前线程名。成功返回 `true`，否则返回 `false`。在 Linux 上，线程名最多 16 个字符，包括空终止符。

下面的程序演示该功能：

```cpp
#define BS_THREAD_POOL_NATIVE_EXTENSIONS
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::this_thread, BS::thread_pool
#include <cstddef>            // std::size_t
#include <optional>           // std::optional
#include <string>             // std::string, std::to_string

BS::synced_stream sync_out;

void set_name(const std::size_t idx)
{
    const std::string name = "Thread " + std::to_string(idx);
    const bool result = BS::this_thread::set_os_thread_name(name);
    sync_out.println(result ? "Successfully" : "Error: Failed to", " set the name of thread ", idx, " to '", name, "'.");
}

void get_name()
{
    const std::optional<std::string> result = BS::this_thread::get_os_thread_name();
    if (result)
        sync_out.println("This thread's name is set to '", *result, "'.");
    else
        sync_out.println("Error: Failed to get this thread's name!");
}

int main()
{
    const bool result = BS::this_thread::set_os_thread_name("Main Thread");
    sync_out.println(result ? "Successfully" : "Error: Failed to", " set the name of the main thread.");
    BS::thread_pool pool(4, set_name);
    pool.wait();
    // Place a breakpoint here to see the thread names in the debugger.
    pool.submit_task(get_name).wait();
}
```

在标出的那一行打断点，调试器里就能看到线程名。主线程名为 “Main Thread”，4 条池线程名为 “Thread 0” 到 “Thread 3”。最后一行会读取并打印某条随机线程的名字。

### 设置进程优先级

虽然和多线程没有直接关系，原生扩展也可以用操作系统原生 API 设置整个进程的优先级。和线程优先级一样，库用枚举类 `BS::os_process_priority` 做抽象，共 6 个成员：

* `BS::os_process_priority::idle`
* `BS::os_process_priority::below_normal`
* `BS::os_process_priority::normal`
* `BS::os_process_priority::above_normal`
* `BS::os_process_priority::high`
* `BS::os_process_priority::realtime`

在 Windows 上，这些预定义优先级与 [Windows API 的进程优先级类](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-setpriorityclass) 一一对应。在 Linux 和 macOS 上，进程优先级映射到 [nice 值](https://www.man7.org/linux/man-pages/man2/setpriority.2.html)，对应枚举成员的实际数值（数值越小，优先级越高）。

由两个函数管理：

* `BS::get_os_process_priority()` 读取进程优先级，返回 `std::optional<BS::os_process_priority>`。对象没有值表示无法确定，或者不是上面的预定义值之一。
* `BS::set_os_process_priority()` 设置进程优先级。成功返回 `true`，否则返回 `false`。`false` 通常表示没有所需权限。

示例：

```cpp
#define BS_THREAD_POOL_NATIVE_EXTENSIONS
#include "BS_thread_pool.hpp" // BS::get_os_process_priority, BS::os_process_priority, BS::set_os_process_priority, BS::synced_stream
#include <map>                // std::map
#include <optional>           // std::optional
#include <string>             // std::string

BS::synced_stream sync_out;
BS::os_process_priority target = BS::os_process_priority::high;

const std::map<BS::os_process_priority, std::string> os_process_priority_map = {{BS::os_process_priority::idle, "idle"}, {BS::os_process_priority::below_normal, "below_normal"}, {BS::os_process_priority::normal, "normal"}, {BS::os_process_priority::above_normal, "above_normal"}, {BS::os_process_priority::high, "high"}, {BS::os_process_priority::realtime, "realtime"}};

std::string os_process_priority_name(const BS::os_process_priority priority)
{
    const std::map<BS::os_process_priority, std::string>::const_iterator it = os_process_priority_map.find(priority);
    return (it != os_process_priority_map.end()) ? it->second : "unknown";
}

int main()
{
    const std::optional<BS::os_process_priority> get_result = BS::get_os_process_priority();
    if (get_result)
        sync_out.println("The OS process priority is currently set to '", os_process_priority_name(*get_result), "'.");
    else
        sync_out.println("Error: Failed to get the OS process priority!");
    const bool set_result = BS::set_os_process_priority(target);
    sync_out.println(set_result ? "Successfully" : "Error: Failed to", " set the OS process priority to '", os_process_priority_name(target), "'.");
}
```

在 Linux 或 macOS 上请用 `sudo` 以 root 运行，否则会失败。（这里其实不必用 `BS::synced_stream`，因为没有用线程池，只有主线程在打印；用它只是为了和其他例子一致。）

### 设置进程亲和性

原生扩展也可以用操作系统原生 API 设置整个进程的处理器亲和性。这在 Windows 和 Linux 上可用，在 macOS 上不可用，因为原生 API 不允许。和线程亲和性一样，抽象层是 `std::vector<bool>`，每个元素对应一个逻辑处理器。

由两个函数管理：

* `BS::get_os_process_affinity()` 读取进程亲和性，返回 `std::optional<std::vector<bool>>`。对象没有值表示无法确定。在 macOS 上总是返回 `std::nullopt`。
* `BS::set_os_process_affinity()` 设置进程亲和性。成功返回 `true`，否则返回 `false`。在 macOS 上总是返回 `false`。

统计 `BS::get_os_process_affinity()` 里值为 `true` 的元素个数，就能知道进程可用多少逻辑处理器。如果启用了原生扩展，默认构造的池会用这个方法决定进程可用的线程数（可能小于硬件线程数），并把它作为默认池线程数。示例：

```cpp
#define BS_THREAD_POOL_NATIVE_EXTENSIONS
#include "BS_thread_pool.hpp" // BS::get_os_process_affinity(), BS::set_os_process_affinity, BS::synced_stream, BS::thread_pool
#include <algorithm>          // std::count
#include <optional>           // std::optional
#include <thread>             // std::thread
#include <vector>             // std::vector

BS::synced_stream sync_out;

int main()
{
    sync_out.println("Total hardware threads: ", std::thread::hardware_concurrency());
    BS::thread_pool pool1;
    sync_out.println("Threads in first pool: ", pool1.get_thread_count());

    const bool success = BS::set_os_process_affinity({true, true, true});
    if (success)
    {
        const std::optional<std::vector<bool>> affinity = BS::get_os_process_affinity();
        if (affinity)
        {
            sync_out.println("Total threads now available to the process: ", std::count(affinity->begin(), affinity->end(), true));
            BS::thread_pool pool2;
            sync_out.println("Threads in second pool: ", pool2.get_thread_count());
            return 0;
        }
    }
    sync_out.println("ERROR: Failed to set or get process affinity.");
}
```

假设运行前没有预先设置进程亲和性（例如 Linux 上的 `taskset`），`pool1` 会按硬件线程总数创建。随后手工把进程亲和性设成只启用前 3 个逻辑处理器（传入 3 个 `true`，其余为 `false`）。因此 `pool2` 只会有 3 条线程。如果总共有 32 条硬件线程，输出是：

```none
Total hardware threads: 32
Threads in first pool: 32
Total threads now available to the process: 3
Threads in second pool: 3
```

### 访问原生线程句柄

启用原生扩展后，`BS::thread_pool` 增加成员函数 `get_native_handles()`，返回一个向量，元素是池中每条线程由实现定义的底层句柄。之后可以用与实现相关的方式在操作系统层面管理这些线程。

简例：

```cpp
#define BS_THREAD_POOL_NATIVE_EXTENSIONS
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::thread_pool
#include <thread>             // std::thread
#include <vector>             // std::vector

BS::synced_stream sync_out;
BS::thread_pool pool(4);

int main()
{
    std::vector<std::thread::native_handle_type> handles = pool.get_native_handles();
    for (std::size_t i = 0; i < handles.size(); ++i)
        sync_out.println("Thread ", i, " native handle: ", handles[i]);
}
```

输出取决于编译器和操作系统。例如：

```none
Thread 0 native handle: 00000000000000AC
Thread 1 native handle: 00000000000000B0
Thread 2 native handle: 00000000000000B4
Thread 3 native handle: 00000000000000B8
```

**警告：** 直接使用原生句柄写出的代码**不可移植**。如上所述，原生扩展已经为几种常用线程操作提供了抽象层，在受支持的平台上可移植，应优先使用。原生句柄是给需要做抽象层未覆盖的操作的用户准备的。

## 测试

### 自动化测试

[GitHub 仓库](https://github.com/bshoshany/thread-pool) `tests` 目录中的 `BS_thread_pool_test.cpp` 会对库的各个方面做自动化测试。这些代码同时也是完整的用法示例。

测试程序接受这些命令行参数：

* `help`：显示帮助并退出。其他参数会被忽略。
* `stdout`：打印到标准输出。
* `log`：打印到日志文件。文件名与可执行文件相同，后缀为当前日期时间 `-yyyy-mm-dd_hh.mm.ss.log`。
* `tests`：执行标准测试。
* `deadlock`：执行长时间的死锁测试。
* `benchmarks`：执行完整的 Mandelbrot 集基准测试。
* `plot`：执行快速的 Mandelbrot 集基准测试。
* `save`：把 Mandelbrot 集图像保存到文件。

不给选项时，默认是 `benchmarks log stdout tests`。如果同一目录或上级目录存在 `default_args.txt`，测试程序会从中读取默认参数（单行、空格分隔）。命令行参数仍可覆盖这些默认值。调试时很有用。

编译时可以定义这些宏（Clang 和 GCC 用 `-D`，MSVC 用 `/D`）以启用额外功能：

* `BS_THREAD_POOL_TEST_IMPORT_MODULE`：把线程池库[作为 C++20 模块](#作为-c20-模块导入)导入。模块必须事先编译，见对应章节。
* `BS_THREAD_POOL_NATIVE_EXTENSIONS`：测试[原生扩展](#原生扩展)。如果作为 C++20 模块导入，库编译时也必须定义同一个宏。

附带的 [`compile_cpp.py`](#compile_cpppy-脚本) 若以 `python scripts/compile_cpp.py tests/BS_thread_pool_test.cpp --run --try-all --type=release --verbose` 运行，会自动检测 Clang、GCC 和/或 MSVC 是否可用，并用每个可用编译器编译并运行测试程序 3 次：

1. C++17。
2. C++20，使用 `import BS.thread_pool`。
3. C++23，使用 `import BS.thread_pool` 和 `import std`。

任何测试失败时，请[提交缺陷报告](https://github.com/bshoshany/thread-pool/issues)，附上系统的精确规格（操作系统、CPU、编译器等）和生成的日志文件。注意：**只支持各编译器的最新版本**。

默认情况下，测试程序用 ANSI 转义码打印彩色输出，便于阅读。设置环境变量 `NO_COLOR` 可关闭。

### 性能测试

`BS_thread_pool_test.cpp` 也做基准测试，用高度优化的多线程算法绘制 [Mandelbrot 集](https://zh.wikipedia.org/wiki/曼德博集合)，采用归一化迭代计数和线性插值，做出平滑着色。如果启用了测试，只有全部测试通过才会做基准测试。

这些基准测试非常吃 CPU，多线程加速比很高，理想情况下会把每个核心和每条线程用满。因此它们适合优化库本身：结果对线程池自身的性能比对内存或缓存等因素更敏感。

完整基准测试用命令行参数 `benchmarks` 启用，默认开启。`plot` 只绘制一次 Mandelbrot 集，可以代替完整基准，也可以和它一起用。它会绘制 5 秒内能画出的最大图像，并且只测量整幅图的 pixels/ms。

测试程序会把生成的 Mandelbrot 集下采样后打印，以适合终端窗口（宽度 120 字符）。终端里是 24 位色，日志文件里是用 Unicode 方块画的单色图。如果设置了 `NO_COLOR`，终端输出也是单色。（注意：在 Windows Terminal 里，设置中的 `adjustIndistinguishableColors` 必须关闭，否则图显示不正确。）

想看全分辨率，传入 `save`，图像会保存为 `BS_thread_pool_benchmark_mandelbrot.bmp`（用 BMP 是为了不依赖第三方库）。默认关闭，因为文件可能很大。

程序通过测试需要多少像素才能在“任务数等于线程数”的并行循环下达到某个目标时长，来决定 Mandelbrot 图的最佳分辨率。这样各系统上每次基准测试的耗时（按每条线程计）大致相同，结果更一致、更可移植。

确定分辨率后开始绘图。算法细节见 `BS_thread_pool_test.cpp` 源码。同一操作既做单线程，也做多线程；多线程计算分散到提交给池的多个任务上。

多线程测试逐步增加任务数，池的线程数保持为硬件并发数，以获得最优性能。每项测试重复多次，对同一次测试的全部运行取平均时间。程序把任务数每次乘 2，直到出现收益递减。然后比较各次运行时间，并计算相对单线程测试的最大加速比。

如果启用了[原生扩展](#原生扩展)，程序会尽量把进程本身和池中全部线程的优先级提到最高，以免其他进程干扰基准测试。因此要得到最可靠的结果，建议以特权用户运行，尤其在 Linux 或 macOS 上，只有 root 能提高优先级。

下面是在 24 核（8P+16E）/ 32 线程的 Intel i9-13900K 上的结果。测试用 MSVC 的 C++23 模式编译，以利用最新 C++23 特性取得最高性能。优化标志是 `/O2`。基准测试跑了 5 次，取加速比中位数的那一次：

```none
Generating a 3927x3927 plot of the Mandelbrot set...
Each test will be repeated 30 times to collect reliable statistics.
   1 task:  [..............................]  (single-threaded)
|-> Mean:  500.8 ms, standard deviation:  1.3 ms, speed:  1026.5 pixels/ms.
   8 tasks: [..............................]
-> Mean:  146.0 ms, standard deviation:  0.3 ms, speed:  3520.9 pixels/ms.
  16 tasks: [..............................]
-> Mean:   82.2 ms, standard deviation:  1.5 ms, speed:  6256.1 pixels/ms.
  32 tasks: [..............................]
-> Mean:   49.8 ms, standard deviation:  1.2 ms, speed: 10322.2 pixels/ms.
  64 tasks: [..............................]
-> Mean:   26.9 ms, standard deviation:  1.2 ms, speed: 19109.5 pixels/ms.
 128 tasks: [..............................]
-> Mean:   22.8 ms, standard deviation:  0.9 ms, speed: 22545.8 pixels/ms.
 256 tasks: [..............................]
-> Mean:   21.4 ms, standard deviation:  0.5 ms, speed: 24058.2 pixels/ms.
 512 tasks: [..............................]
-> Mean:   20.7 ms, standard deviation:  0.6 ms, speed: 24833.1 pixels/ms.
1024 tasks: [..............................]
-> Mean:   21.0 ms, standard deviation:  0.4 ms, speed: 24478.3 pixels/ms.
Maximum speedup obtained by multithreading vs. single-threading: 24.2x, using 512 tasks.
```

这颗 CPU 有 24 个核心：8 个较快的性能核（最高 5.40 GHz），带超线程（共 16 条线程）；16 个较慢的能效核（最高 4.30 GHz），没有超线程。总共 32 条线程。

混合架构下，理论最大加速比不好直接算。粗略估计：E 核大约比 P 核慢 20%，超线程一般大约提供 30% 加速。因此相对单个 P 核的估计理论加速比是 8 × 1.3 + 16 × 0.8 = 23.2 倍。

实际中位数加速比 24.2 倍，比这个估计高 4.3%，说明线程池提供了最优性能，Mandelbrot 绘图算法用满了 CPU 的能力。

还要注意：虽然硬件线程数是 32，最大加速比不是在 32 个任务时达到的，而是在 512 个任务时达到的，大约是硬件线程数平方的一半。原因是把工作拆成比线程更多的任务可以消除线程空闲，见[上文](#选择块数)。到 1024 个任务时出现收益递减，提交任务的开销开始超过并行的收益。

### 查询库版本

从 v5.0.0 起，库定义了 `constexpr` 对象 `BS::thread_pool_version`，可在编译期检查版本。类型是 `BS::version`，成员为 `major`、`minor`、`patch`，比较运算符都是 `constexpr`。它还有 `to_string()`，并重载了 `operator<<`，便于运行时打印（[测试程序](#测试)会用到）。

因为 `BS::thread_pool_version` 是 `constexpr` 对象，凡是允许 `constexpr` 的地方都能用，例如 `static_assert()` 和 `if constexpr`。下面的程序在版本低于 5.1.0 时无法编译：

```cpp
#include "BS_thread_pool.hpp"

static_assert(BS::thread_pool_version >= BS::version(5, 1, 0), "This program requires version 5.1.0 or later of the BS::thread_pool library.");

int main()
{
    // ...
}
```

另一个例子会打印库版本（隐式使用 `BS::version` 的 `<<`），再按版本条件编译两条分支之一：

```cpp
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::thread_pool

BS::synced_stream sync_out;
BS::thread_pool pool;

int main()
{
    sync_out.println("Detected BS::thread_pool v", BS::thread_pool_version, '.');
    if constexpr (BS::thread_pool_version <= BS::version(5, 1, 0))
    {
        // Do something supported by BS::thread_pool v5.1.0 or earlier.
    }
    else
    {
        // Do something supported by newer versions of BS::thread_pool after v5.1.0.
    }
}
```

`BS::thread_pool_version` 从 v5.0.0 引入，是首选的版本检查方式。为了向后兼容，如果不确定拿到的是 v4 还是 v5，可以用 v4.0.1 引入的这些预处理宏：

* `BS_THREAD_POOL_VERSION_MAJOR`：主版本号。
* `BS_THREAD_POOL_VERSION_MINOR`：次版本号。
* `BS_THREAD_POOL_VERSION_PATCH`：修订号。

这些宏可以用 `#if` 做条件包含。例如 [`set_cleanup_func()`](#线程清理函数) 从 v5.0.0 引入。主版本号大于等于 5 时可以用它，否则必须另找清理办法：

```cpp
#include "BS_thread_pool.hpp" // BS::synced_stream, BS::thread_pool

BS::synced_stream sync_out;
BS::thread_pool pool;

int main()
{
#if BS_THREAD_POOL_VERSION_MAJOR >= 5
    pool.set_cleanup_func(
        []
        {
            sync_out.println("Doing cleanup...");
        });
#else
    // Do the cleanup in some other way.
#endif
}
```

如果库是[作为 C++20 模块](#作为-c20-模块导入)导入的，这些宏不可用，因为宏不能从模块导出。这时必须用 `BS::thread_pool_version`。（这也正是引入它的原因。）

## 作为 C++20 模块导入

### 编译模块

如果有 C++20，可以用 `import BS.thread_pool` 把库作为 C++20 模块导入。这是官方推荐的用法，好处很多：编译更快、封装更好、不污染命名空间、没有包含顺序问题、更容易维护、依赖管理更简单，等等。`constexpr` 标志 `BS::thread_pool_module` 表示库是否作为模块编译。关于 C++20 模块，见 [cppreference.com](https://zh.cppreference.com/w/cpp/language/modules)。

模块文件是 `modules` 目录中的 `BS.thread_pool.cppm`，只是头文件 `BS_thread_pool.hpp` 外面的一层薄包装。C++20 标准没有办法让同一个文件既当模块又当头文件，所以要作为模块编译，两个文件都需要。（只当头文件用时，只需要 `BS_thread_pool.hpp`。）

头文件 `BS_thread_pool.hpp` 在 `BS` 后面用下划线 `_`，是为了兼容旧版本。模块文件 `BS.thread_pool.cppm` 在 `BS` 后面用点 `.`，符合 C++20 模块命名约定：点表示层次。本库作者写的所有模块都用 `BS.` 前缀。

该功能已用最新的 Clang、GCC 和 MSVC 测试。写作时，C++20 模块在各编译器里仍未完全实现，而且各编译器的实现方式不同。

编译模块本身、以及导入它的程序，最简单的办法是用 [GitHub 仓库](https://github.com/bshoshany/thread-pool) 里的 Python 脚本 `compile_cpp.py`，它会自动为每个编译器选出合适的标志。详见[下一节](#用-compile_cpppy-和-import-bsthread_pool-编译)。

如果更想手工编译，必须先把模块编译成各编译器专用格式的二进制文件，见后面各节。一旦编译完成，导入库只需要这个二进制文件（MSVC 还需要一个目标文件）；`.cppm` 和 `.hpp` 不再需要。但使用模块的程序必须带一个标志，告诉编译器去哪里找那个二进制文件。

模块编译好之后，用 `import BS.thread_pool` 导入。上面所有例子里，都可以把 `#include "BS_thread_pool.hpp"` 换成 `import BS.thread_pool;`。唯一例外是[原生扩展](#原生扩展)：例子里用宏启用它们，但如该节所述，宏必须作为编译器标志定义，因为模块看不到导入它的程序里定义的宏。

简例：

```cpp
import BS.thread_pool;

BS::synced_stream sync_out;
BS::thread_pool pool;

int main()
{
    pool.submit_task(
            []
            {
                sync_out.println("Thread pool library successfully imported using C++20 modules!");
            })
        .wait();
}
```

下面给出用 Clang、GCC、MSVC 以及 CMake，先把库编译成模块、再用该模块编译[测试程序](#测试) `BS_thread_pool_test.cpp` 的命令。在 [GitHub 仓库](https://github.com/bshoshany/thread-pool) 里，相关文件是这样组织的：

```
├── README.md                     <- this documentation file
├── include
│   └── BS_thread_pool.hpp        <- the header file
├── modules
│   └── BS.thread_pool.cppm       <- the module file
├── scripts
│   └── compile_cpp.py            <- the compile script (optional)
└── tests
    └── BS_thread_pool_test.cpp   <- the test program
```

下面的例子假定命令在仓库根目录（含 `README.md` 的目录）执行。编译产物放在 `build` 子目录，应事先创建。

### 用 `compile_cpp.py` 和 `import BS.thread_pool` 编译

附带的 Python 脚本 [`compile_cpp.py`](#compile_cpppy-脚本) 可以方便地编译把库作为模块导入的程序。脚本会自动为每个编译器选出合适的标志，不必自己处理细节。例如在仓库根目录编译测试程序 `BS_thread_pool_test.cpp`，并让它导入 `BS.thread_pool` 模块：

```bash
python scripts/compile_cpp.py tests/BS_thread_pool_test.cpp -s=c++20 -i=include -t=release -m="BS.thread_pool=modules/BS.thread_pool.cppm,include/BS_thread_pool.hpp" -o=build/BS_thread_pool_test -d=BS_THREAD_POOL_TEST_IMPORT_MODULE -v
```

命令行参数的说明见[下文](#compile_cpppy-脚本)。`-d` 定义宏 `BS_THREAD_POOL_TEST_IMPORT_MODULE`，告诉测试程序把库作为模块导入，而不是包含头文件。**这个宏只给测试程序用；编译你自己的程序时不需要。** 要启用[原生扩展](#原生扩展)，再加上 `-d=BS_THREAD_POOL_NATIVE_EXTENSIONS`。要用 C++23，把 `-s=c++20` 换成 `-s=c++23`。

因为用了 `-t=release`，优化标志会自动加上。然后运行 `build/BS_thread_pool_test` 即可；也可以加 `-r`，编译后自动运行。如果模块导入成功，测试程序会打印：

```none
Thread pool library imported using: import BS.thread_pool (C++20 modules).
```

若要进一步定制，建议按[下文](#compile_cpppy-脚本)创建 `compile_cpp.yaml`。

### 用 Clang 和 `import BS.thread_pool` 编译

注意：以下说明只在写作时的最新版本 Clang v21.1.8 上测试过，旧版本可能不行。

用 Clang 编译模块文件 `BS.thread_pool.cppm`：先 `mkdir build`，再在仓库根目录执行：

```bash
clang++ modules/BS.thread_pool.cppm --precompile -std=c++20 -I include -o build/BS.thread_pool.pcm
```

参数说明：

* `modules/BS.thread_pool.cppm`：要编译的模块文件。它会自动包含 `include/BS_thread_pool.hpp`。
* `--precompile`：不运行链接器，只编译模块。
* `-std=c++20`：使用 C++20。C++23 用 `-std=c++23`。
* `-I include`：把 `include` 加入包含路径，让模块找到头文件 `BS_thread_pool.hpp`。
* `-o build/BS.thread_pool.pcm`：把编译好的模块输出到 `build/BS.thread_pool.pcm`。Clang 用扩展名 `.pcm` 表示预编译模块。

要启用[原生扩展](#原生扩展)，加上 `-D BS_THREAD_POOL_NATIVE_EXTENSIONS`。

模块编译好之后，这样编译测试程序：

```bash
clang++ tests/BS_thread_pool_test.cpp -fmodule-file="BS.thread_pool=build/BS.thread_pool.pcm" -std=c++20 -o build/BS_thread_pool_test -D BS_THREAD_POOL_TEST_IMPORT_MODULE
```

参数说明：

* `tests/BS_thread_pool_test.cpp`：要编译的程序。
* `-fmodule-file="BS.thread_pool=build/BS.thread_pool.pcm"`：指定模块 `BS.thread_pool` 位于 `build/BS.thread_pool.pcm`。
* `-std=c++20`：同上。
* `-o build/BS_thread_pool_test`：输出到 `build/BS_thread_pool_test`（Windows 上是 `build/BS_thread_pool_test.exe`）。
* `-D BS_THREAD_POOL_TEST_IMPORT_MODULE`：定义宏，告诉测试程序把库作为模块导入。**这个宏只给测试程序用；编译你自己的程序时不需要。**

若要测试原生扩展，再加上 `-D BS_THREAD_POOL_NATIVE_EXTENSIONS`。不必用 `-I`，因为不再需要头文件，只需要 `.pcm`。然后运行 `build/BS_thread_pool_test`。如果模块导入成功，会打印：

```none
Thread pool library imported using: import BS.thread_pool (C++20 modules).
```

按需要给上面的命令加上警告、调试、优化和其他编译器标志。Clang 使用 C++20 模块的更多信息见[官方文档](https://clang.llvm.org/docs/StandardCPlusPlusModules.html)。

**注意：** 写作时，Clang 配合 libc++ 在 C++20 模块里使用 `std::jthread` 会编译失败。在缺陷修好之前，如果检测到 Clang 和 libc++ 与 C++20 模块一起使用，线程池库会自动退回 `std::thread`。编译模块时定义 `BS_THREAD_POOL_DISABLE_WORKAROUNDS` 可关闭这个变通。

**注意：** 在 macOS 上，Apple Clang 不支持 C++20 模块。请用 [Homebrew](https://formulae.brew.sh/formula/llvm) 安装最新的 LLVM Clang，或者把头文件方式包含库。

### 用 GCC 和 `import BS.thread_pool` 编译

注意：以下说明只在写作时的最新版本 GCC v15.2.0 上测试过，旧版本可能不行。

用 GCC 编译模块文件 `BS.thread_pool.cppm`：先 `mkdir build`，再在仓库根目录执行：

```bash
g++ -x c++ modules/BS.thread_pool.cppm -c "-fmodule-mapper=|@g++-mapper-server -r build" -fmodule-only -fmodules -std=c++20 -I include
```

参数说明：

* `-x c++`：把输入文件当作 C++ 文件。文件扩展名是 `.cppm`，GCC 不认识，所以需要这个标志。
* `modules/BS.thread_pool.cppm`：要编译的模块文件。它会自动包含 `include/BS_thread_pool.hpp`。
* `-c`：不运行链接器，只编译模块。
* `"-fmodule-mapper=|@g++-mapper-server -r build"`：告诉模块映射器把编译好的模块放到 `build` 目录。会生成 `build/BS.thread_pool.gcm`。GCC 用扩展名 `.gcm` 表示编译好的模块。
* `-fmodule-only`：不为模块生成目标文件。
* `-fmodules`：启用 C++20 模块。
* `-std=c++20`：使用 C++20。C++23 用 `-std=c++23`。
* `-I include`：把 `include` 加入包含路径，让模块找到头文件 `BS_thread_pool.hpp`。

要启用[原生扩展](#原生扩展)，加上 `-D BS_THREAD_POOL_NATIVE_EXTENSIONS`。

模块编译好之后，这样编译测试程序：

```bash
g++ tests/BS_thread_pool_test.cpp "-fmodule-mapper=|@g++-mapper-server -r build" -fmodules -std=c++20 -o build/BS_thread_pool_test -D BS_THREAD_POOL_TEST_IMPORT_MODULE
```

参数说明：

* `tests/BS_thread_pool_test.cpp`：要编译的程序。
* `"-fmodule-mapper=|@g++-mapper-server -r build"`：告诉模块映射器编译好的模块在 `build` 目录，会查找 `build/BS.thread_pool.gcm`。
* `-fmodules`、`-std=c++20`：同上。
* `-o build/BS_thread_pool_test`：输出到 `build/BS_thread_pool_test`（Windows 上是 `build/BS_thread_pool_test.exe`）。
* `-D BS_THREAD_POOL_TEST_IMPORT_MODULE`：告诉测试程序把库作为模块导入。**这个宏只给测试程序用；编译你自己的程序时不需要。**

若要测试原生扩展，再加上 `-D BS_THREAD_POOL_NATIVE_EXTENSIONS`。不必用 `-I`，因为不再需要头文件，只需要 `.gcm`。然后运行 `build/BS_thread_pool_test`。如果模块导入成功，会打印：

```none
Thread pool library imported using: import BS.thread_pool (C++20 modules).
```

按需要给上面的命令加上警告、调试、优化和其他编译器标志。GCC 使用 C++20 模块的更多信息见[官方文档](https://gcc.gnu.org/onlinedocs/gcc/C_002b_002b-Modules.html)。

### 用 MSVC 和 `import BS.thread_pool` 编译

注意：以下说明只在写作时的最新版本 MSVC v19.50.35721 上测试过，旧版本可能不行。

用 MSVC 编译模块文件 `BS.thread_pool.cppm`：先打开对应 CPU 架构的 Visual Studio Developer PowerShell。例如在 Visual Studio 2026 上编译 x64，在仓库根目录的 PowerShell 里执行：

```pwsh
& 'C:\Program Files\Microsoft Visual Studio\18\Community\Common7\Tools\Launch-VsDevShell.ps1' -Arch amd64 -HostArch amd64 -SkipAutomaticLocation
```

ARM64 把 `amd64` 换成 `arm64`。（不要用开始菜单里的 “Developer PowerShell for VS” 快捷方式，它默认可能不是正确的 CPU 架构。）

先 `mkdir build`，再在仓库根目录执行：

```pwsh
cl modules/BS.thread_pool.cppm /c /EHsc /interface /nologo /permissive- /std:c++20 /TP /Zc:__cplusplus /I include /ifcOutput build/BS.thread_pool.ifc /Fo:build/BS.thread_pool.obj
```

参数说明：

* `modules/BS.thread_pool.cppm`：要编译的模块文件。它会自动包含 `include/BS_thread_pool.hpp`。
* `/c`：不运行链接器，只编译模块。
* `/EHsc`：启用 C++ 异常。
* `/interface`：把输入文件当作模块接口单元。MSVC 期望模块文件用 `.ixx` 扩展名，本库用的是 `.cppm`，所以需要这个标志。
* `/nologo`：不显示编译器横幅。
* `/permissive-`：关闭宽松行为，强制严格符合 C++ 标准。
* `/std:c++20`：使用 C++20。C++23 用 `/std:c++latest`。
* `/TP`：把输入文件当作 C++ 文件。扩展名是 `.cppm`，MSVC 不认识，所以需要这个标志。
* `/Zc:__cplusplus`：让预处理宏 `__cplusplus` 正确反映正在使用的 C++ 标准。
* `/I include`：把 `include` 加入包含路径，让模块找到头文件 `BS_thread_pool.hpp`。
* `/ifcOutput build/BS.thread_pool.ifc`：把编译好的模块输出到 `build/BS.thread_pool.ifc`。MSVC 用扩展名 `.ifc` 表示模块接口文件。
* `/Fo:build/BS.thread_pool.obj`：把编译好的目标文件输出到 `build/BS.thread_pool.obj`。

要启用[原生扩展](#原生扩展)，加上 `/D BS_THREAD_POOL_NATIVE_EXTENSIONS`。

模块编译好之后，这样编译测试程序：

```pwsh
cl tests/BS_thread_pool_test.cpp build/BS.thread_pool.obj /reference BS.thread_pool=build/BS.thread_pool.ifc /EHsc /nologo /permissive- /std:c++20 /Zc:__cplusplus /Fo:build/BS_thread_pool_test.obj /Fe:build/BS_thread_pool_test.exe /D BS_THREAD_POOL_TEST_IMPORT_MODULE
```

参数说明：

* `tests/BS_thread_pool_test.cpp`：要编译的程序。
* `build/BS.thread_pool.obj`：要链接进程序的模块目标文件。
* `/reference BS.thread_pool=build/BS.thread_pool.ifc`：指定模块 `BS.thread_pool` 位于 `build/BS.thread_pool.ifc`。
* `/EHsc`、`/nologo`、`/permissive-`、`/std:c++20`、`/Zc:__cplusplus`：同上。
* `/Fo:build/BS_thread_pool_test.obj`：目标文件输出到 `build/BS_thread_pool_test.obj`。
* `/Fe:build/BS_thread_pool_test.exe`：程序输出到 `build/BS_thread_pool_test.exe`。
* `/D BS_THREAD_POOL_TEST_IMPORT_MODULE`：告诉测试程序把库作为模块导入。**这个宏只给测试程序用；编译你自己的程序时不需要。**

若要测试原生扩展，再加上 `/D BS_THREAD_POOL_NATIVE_EXTENSIONS`。不必用 `/I`，因为不再需要头文件，只需要 `.obj` 和 `.ifc`。然后运行 `build/BS_thread_pool_test`。如果模块导入成功，会打印：

```none
Thread pool library imported using: import BS.thread_pool (C++20 modules).
```

按需要给上面的命令加上警告、调试、优化和其他编译器标志。MSVC 使用 C++20 模块的更多信息见[这篇博客](https://devblogs.microsoft.com/cppblog/using-cpp-modules-in-msvc-from-the-command-line-part-1/)。

### 用 CMake 和 `import BS.thread_pool` 编译

注意：以下说明只在写作时的最新版本 CMake v4.2.1 上测试过，旧版本可能不行。另外，目前不是所有 CMake 生成器都支持模块，详见 CMake 文档。

如果使用 [CMake](https://cmake.org/)，可以用 `target_sources()` 的 `CXX_MODULES` 包含模块文件 `BS.thread_pool.cppm`。CMake 会自动编译模块并链接到程序。下面的 `CMakeLists.txt` 用来构建测试程序，并把线程池库作为模块导入：

```cmake
cmake_minimum_required(VERSION 4.2.1)
project(BS_thread_pool_test LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if(MSVC)
    add_compile_options(/permissive- /Zc:__cplusplus)
endif()

add_library(BS_thread_pool)
target_sources(BS_thread_pool PRIVATE FILE_SET CXX_MODULES FILES modules/BS.thread_pool.cppm)
target_include_directories(BS_thread_pool PRIVATE include)

add_executable(${PROJECT_NAME} tests/BS_thread_pool_test.cpp)
target_link_libraries(${PROJECT_NAME} PRIVATE BS_thread_pool)
target_compile_definitions(${PROJECT_NAME} PRIVATE BS_THREAD_POOL_TEST_IMPORT_MODULE)
```

MSVC 必须加 `/permissive-`，否则测试程序不会编译；必须加 `/Zc:__cplusplus`，否则测试程序检测不到正确的 C++ 版本。`if(MSVC)` 块会自动处理。

要启用[原生扩展](#原生扩展)，加上 `add_compile_definitions(BS_THREAD_POOL_NATIVE_EXTENSIONS)`。要用 C++23，把 `CMAKE_CXX_STANDARD 20` 换成 `23`。

把这个文件放在仓库根目录，然后执行：

```bash
cmake -B build
cmake --build build
build/BS_thread_pool_test
```

MSVC 把最后一条换成 `build/Debug/BS_thread_pool_test`。如果模块导入成功，会打印：

```none
Thread pool library imported using: import BS.thread_pool (C++20 modules).
```

按需要给上面的配置加上警告、调试、优化和其他编译器标志。CMake 使用 C++20 模块的更多信息见[官方文档](https://cmake.org/cmake/help/latest/manual/cmake-cxxmodules.7.html)。

也可以让 CMake 自动从 GitHub 仓库下载库，见下文的 [CPM](#用-cmake-和-cpm-安装) 或 [`FetchContent`](#用-cmake-和-fetchcontent-安装)。

## 导入 C++23 标准库模块

### 启用 `import std`

如果有 C++23，线程池库可以用 `import std` 把 C++ 标准库作为模块导入。好处和[上文](#作为-c20-模块导入)把本库作为模块导入一样，例如编译更快。编译时定义宏 `BS_THREAD_POOL_IMPORT_STD` 即可启用。

写作时，把 C++ 标准库作为模块导入只被以下编译器和标准库组合正式支持：

* 较新的 LLVM Clang（**不是** Apple Clang）配合 LLVM libc++。
* 较新的 GCC 配合 libstdc++。
* 较新的 MSVC 配合 Microsoft STL。

如果定义了 `BS_THREAD_POOL_IMPORT_STD`，线程池库本身也必须作为模块导入。如果库是以头文件包含的，包含它的程序也会被迫 `import std`，这通常不是想要的结果；如果程序还 `#include` 了任何标准库头文件，还可能导致编译错误。

在导入模块之前定义宏无效，因为模块看不到导入它的程序里定义的宏。必须作为编译器标志定义：Clang 和 GCC 用 `-D BS_THREAD_POOL_IMPORT_STD`，MSVC 用 `/D BS_THREAD_POOL_IMPORT_STD`。

如果编译时定义了 `BS_THREAD_POOL_IMPORT_STD`，[测试程序](#测试)也会导入 `std` 模块。这时还应启用 `BS_THREAD_POOL_TEST_IMPORT_MODULE`，把线程池库作为模块导入。

`constexpr` 标志 `BS::thread_pool_import_std` 表示库编译时是否使用了 `import std`。如果定义了 `BS_THREAD_POOL_IMPORT_STD` 但编译器没有启用 C++23，该标志为 `false`。

写作时，导入 `std` 模块需要先编译它。如[上一节](#作为-c20-模块导入)所述，用附带的 `compile_cpp.py` 最简单，[下一节](#用-compile_cpppy-和-import-std-编译)会说明。想手工编译的话，后面几节分别说明 Clang、MSVC 和 CMake。假定读者已经读过把 `BS.thread_pool` 作为模块导入的章节，这里省略部分细节。

### 用 `compile_cpp.py` 和 `import std` 编译

附带的 Python 脚本 [`compile_cpp.py`](#compile_cpppy-脚本) 可以方便地编译把 C++ 标准库作为模块导入的程序。脚本会自动为每个编译器选出合适的标志。例如在仓库根目录编译测试程序，同时导入 `BS.thread_pool` 和 `std`：

```bash
python scripts/compile_cpp.py tests/BS_thread_pool_test.cpp -s=c++23 -i=include -t=release -m="BS.thread_pool=modules/BS.thread_pool.cppm,include/BS_thread_pool.hpp" -o=build/BS_thread_pool_test -d=BS_THREAD_POOL_TEST_IMPORT_MODULE -d=BS_THREAD_POOL_IMPORT_STD -u=auto -v
```

命令行参数见[下文](#compile_cpppy-脚本)。和[把线程池库作为模块导入](#用-compile_cpppy-和-import-bsthread_pool-编译)那条命令的差别是：

* `-s=c++20` 改成 `-s=c++23`，以便使用 C++23。
* 加上 `-d=BS_THREAD_POOL_IMPORT_STD` 定义所需宏。
* 加上 `-u=auto` 自动检测 `std` 模块的位置。如果不行，需要手工指定路径。

要启用[原生扩展](#原生扩展)，再加上 `-d=BS_THREAD_POOL_NATIVE_EXTENSIONS`。然后运行 `build/BS_thread_pool_test`。如果 `std` 模块导入成功，会打印：

```none
C++ Standard Library imported using:
* Thread pool library: import std (C++23 std module).
* Test program: import std (C++23 std module).
```

若要进一步定制，建议按[下文](#compile_cpppy-脚本)创建 `compile_cpp.yaml`。

### 用 Clang、LLVM libc++ 和 `import std` 编译

注意：以下说明只在写作时的最新版本 Clang v21.1.8 和 LLVM libc++ v21.1.8 上测试过，旧版本可能不行。

编译 `std` 模块之前，先找到 `std.cppm`：

* 在 Windows 上，libc++ 多半通过 [MSYS2](https://www.msys2.org/) 安装，`std` 模块应在 `C:\msys64\clang64\share\libc++\v1\std.cppm`。如果 MSYS2 不在 `C:\msys64`，换成实际路径。如果不是通过 MSYS2 安装的，在安装目录里手工找 `std.cppm`。
* 在 Linux 上，`std` 模块应在 `/usr/lib/llvm-<LLVM major version>/share/libc++/v1/std.cppm`。把 `<LLVM major version>` 换成 libc++ 的主版本号。如果装在别的目录，在那个目录里手工找。
* 在 macOS 上（[Homebrew 构建](https://formulae.brew.sh/formula/llvm)），`std` 模块应在 `/usr/local/Cellar/llvm/<LLVM full version>/share/libc++/v1/std.cppm`。把 `<LLVM full version>` 换成 libc++ 的完整版本号。如果装在别的目录，在那个目录里手工找。

用 Clang 编译 `std.cppm`：先 `mkdir build`，再在仓库根目录执行：

```bash
clang++ "path to std.cppm" --precompile -std=c++23 -o build/std.pcm -Wno-reserved-module-identifier
```

把 `"path to std.cppm"` 换成实际路径。编译器参数见[上文](#用-clang-和-import-bsthread_pool-编译)。额外的 `-Wno-reserved-module-identifier` 用来压掉一条误报警告。

然后按[上文](#用-clang-和-import-bsthread_pool-编译)编译 `BS.thread_pool` 模块，但改用 `-std=c++23`，并加上：

* `-fmodule-file="std=build/std.pcm"`：指定模块 `std` 位于 `build/std.pcm`。
* `-D BS_THREAD_POOL_IMPORT_STD`：让库导入 `std` 模块。

```bash
clang++ modules/BS.thread_pool.cppm --precompile -fmodule-file="std=build/std.pcm" -std=c++23 -I include -o build/BS.thread_pool.pcm -D BS_THREAD_POOL_IMPORT_STD
```

若要启用[原生扩展](#原生扩展)，加上 `-D BS_THREAD_POOL_NATIVE_EXTENSIONS`。模块编译好之后，这样编译测试程序：

```bash
clang++ tests/BS_thread_pool_test.cpp -fmodule-file="std=build/std.pcm" -fmodule-file="BS.thread_pool=build/BS.thread_pool.pcm" -std=c++23 -o build/BS_thread_pool_test -D BS_THREAD_POOL_TEST_IMPORT_MODULE -D BS_THREAD_POOL_IMPORT_STD
```

若要测试原生扩展，再加上 `-D BS_THREAD_POOL_NATIVE_EXTENSIONS`。然后运行 `build/BS_thread_pool_test`。如果 `std` 模块导入成功，会打印：

```none
C++ Standard Library imported using:
* Thread pool library: import std (C++23 std module).
* Test program: import std (C++23 std module).
```

### 用 GCC、GNU libstdc++ 和 `import std` 编译

注意：以下说明只在写作时的最新版本 GCC v15.2.0 和 GNU libstdc++ v15 (20250917) 上测试过，旧版本可能不行。

在 GNU libstdc++ 里，`std` 模块文件始终作为系统模块 `bits/std.cc` 提供。用 GCC 编译它：先 `mkdir build`，再在仓库根目录执行：

```bash
g++ -fsearch-include-path bits/std.cc -c "-fmodule-mapper=|@g++-mapper-server -r build" -fmodule-only -fmodules -std=c++23 -I include
```

编译器参数见[上文](#用-gcc-和-import-bsthread_pool-编译)。额外的 `-fsearch-include-path` 告诉编译器在包含路径里找 `bits/std.cc`（否则它会以为文件在当前目录）。

然后按[上文](#用-gcc-和-import-bsthread_pool-编译)编译 `BS.thread_pool` 模块，但改用 `-std=c++23`，并加上：

* `-D BS_THREAD_POOL_IMPORT_STD`：让库导入 `std` 模块。

```bash
g++ -x c++ modules/BS.thread_pool.cppm -c "-fmodule-mapper=|@g++-mapper-server -r build" -fmodule-only -fmodules -std=c++23 -I include -D BS_THREAD_POOL_IMPORT_STD
```

若要启用[原生扩展](#原生扩展)，加上 `-D BS_THREAD_POOL_NATIVE_EXTENSIONS`。模块编译好之后，这样编译测试程序：

```bash
g++ tests/BS_thread_pool_test.cpp "-fmodule-mapper=|@g++-mapper-server -r build" -fmodules -std=c++23 -o build/BS_thread_pool_test -D BS_THREAD_POOL_TEST_IMPORT_MODULE -D BS_THREAD_POOL_IMPORT_STD
```

若要测试原生扩展，再加上 `-D BS_THREAD_POOL_NATIVE_EXTENSIONS`。然后运行 `build/BS_thread_pool_test`。如果 `std` 模块导入成功，会打印：

```none
C++ Standard Library imported using:
* Thread pool library: import std (C++23 std module).
* Test program: import std (C++23 std module).
```

**注意：** 写作时，在 Windows 上通过 MSYS2 使用 GCC 和 libstdc++，如果同时启用原生扩展和 `import std`，`BS.thread_pool` 模块无法编译。在缺陷修好之前，如果检测到在 Windows 上 GCC 和 libstdc++ 与 C++23 `std` 模块一起使用，线程池库会自动退回头文件。编译模块时定义 `BS_THREAD_POOL_DISABLE_WORKAROUNDS` 可关闭这个变通。

### 用 MSVC、Microsoft STL 和 `import std` 编译

注意：以下说明只在写作时的最新版本 MSVC v19.50.35721 和 Microsoft STL v145 (202508) 上测试过，旧版本可能不行。

编译 `std` 模块之前，先找到 `std.ixx`。如果装了 Visual Studio 2026，它应在 `C:\Program Files\Microsoft Visual Studio\18\Community\VC\Tools\MSVC\<MSVC runtime version>\modules`。把 `<MSVC runtime version>` 换成 MSVC 运行库的完整版本号；写作时最新是 `14.50.35717`。如果 Visual Studio 装在别的目录，在那个目录里手工找 `std.ixx`。

用 MSVC 编译 `std.ixx`：先按[上文](#用-msvc-和-import-bsthread_pool-编译)打开对应 CPU 架构的 Visual Studio Developer PowerShell。进入仓库目录，`mkdir build`，再在仓库根目录执行：

```pwsh
cl "path to std.ixx" /c /EHsc /nologo /permissive- /std:c++latest /Zc:__cplusplus /ifcOutput build/std.ifc /Fo:build/std.obj
```

把 `"path to std.ixx"` 换成实际路径。编译器参数见[上文](#用-msvc-和-import-bsthread_pool-编译)。

然后按[上文](#用-msvc-和-import-bsthread_pool-编译)编译 `BS.thread_pool` 模块，并加上：

* `/reference std=build/std.ifc`：指定模块 `std` 位于 `build/std.ifc`。
* `/D BS_THREAD_POOL_IMPORT_STD`：让库导入 `std` 模块。

```pwsh
cl modules/BS.thread_pool.cppm /reference std=build/std.ifc /c /EHsc /interface /nologo /permissive- /std:c++latest /TP /Zc:__cplusplus /I include /ifcOutput build/BS.thread_pool.ifc /Fo:build/BS.thread_pool.obj /D BS_THREAD_POOL_IMPORT_STD
```

若要启用[原生扩展](#原生扩展)，加上 `/D BS_THREAD_POOL_NATIVE_EXTENSIONS`。模块编译好之后，这样编译测试程序（注意加上了 `build/std.obj`，以便链接 `std` 模块）：

```pwsh
cl tests/BS_thread_pool_test.cpp build/std.obj build/BS.thread_pool.obj /reference std=build/std.ifc /reference BS.thread_pool=build/BS.thread_pool.ifc /EHsc /nologo /permissive- /std:c++latest /Zc:__cplusplus /Fo:build/BS_thread_pool_test.obj /Fe:build/BS_thread_pool_test.exe /D BS_THREAD_POOL_TEST_IMPORT_MODULE /D BS_THREAD_POOL_IMPORT_STD
```

若要测试原生扩展，再加上 `/D BS_THREAD_POOL_NATIVE_EXTENSIONS`。然后运行 `build/BS_thread_pool_test`。如果 `std` 模块导入成功，会打印：

```none
C++ Standard Library imported using:
* Thread pool library: import std (C++23 std module).
* Test program: import std (C++23 std module).
```

### 用 CMake 和 `import std` 编译

注意：以下说明只在写作时的最新版本 CMake v4.2.1 上测试过，旧版本可能不行。另外，目前不是所有 CMake 生成器都支持模块，详见 CMake 文档。

如果使用 [CMake](https://cmake.org/)，可以打开 `CMAKE_EXPERIMENTAL_CXX_IMPORT_STD`，在编译器和标准库支持的前提下自动编译 `std` 模块。下面的 `CMakeLists.txt` 用来构建测试程序，把线程池库和 C++ 标准库都作为模块导入：

```cmake
cmake_minimum_required(VERSION 4.2.1)
project(BS_thread_pool_test LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_EXPERIMENTAL_CXX_IMPORT_STD ON)

add_compile_definitions(BS_THREAD_POOL_IMPORT_STD)

if(MSVC)
    add_compile_options(/permissive- /Zc:__cplusplus)
endif()

add_library(BS_thread_pool)
target_sources(BS_thread_pool PRIVATE FILE_SET CXX_MODULES FILES modules/BS.thread_pool.cppm)
target_include_directories(BS_thread_pool PRIVATE include)

add_executable(${PROJECT_NAME} tests/BS_thread_pool_test.cpp)
target_link_libraries(${PROJECT_NAME} PRIVATE BS_thread_pool)
target_compile_definitions(${PROJECT_NAME} PRIVATE BS_THREAD_POOL_TEST_IMPORT_MODULE)
```

`if(MSVC)` 块见[上文](#用-msvc-和-import-bsthread_pool-编译)。要启用[原生扩展](#原生扩展)，把宏 `BS_THREAD_POOL_NATIVE_EXTENSIONS` 加到 `add_compile_definitions()`。

把这个文件放在仓库根目录，然后执行：

```bash
cmake -B build
cmake --build build
build/BS_thread_pool_test
```

MSVC 把最后一条换成 `build/Debug/BS_thread_pool_test`。如果 `std` 模块导入成功，会打印：

```none
C++ Standard Library imported using:
* Thread pool library: import std (C++23 std module).
* Test program: import std (C++23 std module).
```

也可以让 CMake 自动从 GitHub 仓库下载库，见下文的 [CPM](#用-cmake-和-cpm-安装) 或 [`FetchContent`](#用-cmake-和-fetchcontent-安装)。

## 用包管理器安装

### 用 vcpkg 安装

如果使用 [vcpkg](https://vcpkg.io/) C/C++ 包管理器，可以这样安装 `BS::thread_pool`：

```bash
vcpkg install bshoshany-thread-pool
```

更新到最新版本：

```bash
vcpkg upgrade
```

更多信息见 [vcpkg.io 上的包页面](https://vcpkg.io/en/package/bshoshany-thread-pool)。

### 用 Meson 安装

如果使用 [Meson](https://mesonbuild.com/) 构建系统，可以从 [WrapDB](https://mesonbuild.com/Wrapdb-projects.html) 安装 `BS::thread_pool`。在项目里创建 `subprojects` 目录（如果还没有），然后执行：

```bash
meson wrap install bshoshany-thread-pool
```

然后在 `meson.build` 里用 `dependency('bshoshany-thread-pool')` 包含这个包。更新到最新版本：

```bash
meson wrap update bshoshany-thread-pool
```

### 用 Conan 安装

如果使用 [Conan](https://conan.io/) C/C++ 包管理器，在 `conanfile.txt` 里加上：

```ini
[requires]
bshoshany-thread-pool/5.1.0
```

更新版本时改版本号即可。更多信息见 [ConanCenter 上的包页面](https://conan.io/center/recipes/bshoshany-thread-pool)。

### 用 CMake 和 CPM 安装

注意：以下说明只在写作时的最新版本 CMake v4.2.1 和 CPM v0.42.0 上测试过，旧版本可能不行。

如果使用 [CMake](https://cmake.org/)，用 [CPM](https://github.com/cpm-cmake/CPM.cmake) 安装 `BS::thread_pool` 最方便。如果 CPM 已经装好，在项目的 `CMakeLists.txt` 里加上：

```cmake
CPMAddPackage(
    NAME BS_thread_pool
    GITHUB_REPOSITORY bshoshany/thread-pool
    VERSION 5.1.0
    EXCLUDE_FROM_ALL
    SYSTEM
)
add_library(BS_thread_pool INTERFACE)
target_include_directories(BS_thread_pool INTERFACE ${BS_thread_pool_SOURCE_DIR}/include)
```

这会自动从 [GitHub 仓库](https://github.com/bshoshany/thread-pool) 下载指定版本并包含进项目。

GitHub 包还有一种简写，`CPMAddPackage()` 只接受一个形如 `"gh:user/name@version"` 的参数。之后 `CPM_LAST_PACKAGE_NAME` 会被设成包名，用这个变量定义包含目录。配置更紧凑：

```cmake
CPMAddPackage("gh:bshoshany/thread-pool@5.1.0")
add_library(BS_thread_pool INTERFACE)
target_include_directories(BS_thread_pool INTERFACE ${${CPM_LAST_PACKAGE_NAME}_SOURCE_DIR}/include)
```

也可以不预先安装 CPM，在 `CPMAddPackage()` 之前加上：

```cmake
set(CPM_DOWNLOAD_LOCATION ${CMAKE_BINARY_DIR}/CPM.cmake)
if(NOT(EXISTS ${CPM_DOWNLOAD_LOCATION}))
    file(DOWNLOAD https://github.com/cpm-cmake/CPM.cmake/releases/latest/download/CPM.cmake ${CPM_DOWNLOAD_LOCATION})
endif()
include(${CPM_DOWNLOAD_LOCATION})
```

下面是一份完整的 `CMakeLists.txt`，会自动下载并编译测试程序 [`BS_thread_pool_test.cpp`](#测试)：

```cmake
cmake_minimum_required(VERSION 4.2.1)
project(BS_thread_pool_test LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if(MSVC)
    add_compile_options(/permissive- /Zc:__cplusplus)
endif()

set(CPM_DOWNLOAD_LOCATION ${CMAKE_BINARY_DIR}/CPM.cmake)
if(NOT(EXISTS ${CPM_DOWNLOAD_LOCATION}))
    file(DOWNLOAD https://github.com/cpm-cmake/CPM.cmake/releases/latest/download/CPM.cmake ${CPM_DOWNLOAD_LOCATION})
endif()
include(${CPM_DOWNLOAD_LOCATION})

CPMAddPackage("gh:bshoshany/thread-pool@5.1.0")
add_library(BS_thread_pool INTERFACE)
target_include_directories(BS_thread_pool INTERFACE ${${CPM_LAST_PACKAGE_NAME}_SOURCE_DIR}/include)

add_executable(${PROJECT_NAME} ${${CPM_LAST_PACKAGE_NAME}_SOURCE_DIR}/tests/BS_thread_pool_test.cpp)
target_link_libraries(${PROJECT_NAME} PRIVATE BS_thread_pool)
```

`if(MSVC)` 块见[上文](#用-msvc-和-import-bsthread_pool-编译)。要启用[原生扩展](#原生扩展)，加上 `add_compile_definitions(BS_THREAD_POOL_NATIVE_EXTENSIONS)`。

要用 C++20 或 C++23，把 `CMAKE_CXX_STANDARD 17` 分别换成 `20` 或 `23`。按需要给上面的配置加上警告、调试、优化和其他编译器标志。

把这份 `CMakeLists.txt` 放在空目录里，然后构建并运行：

```bash
cmake -B build
cmake --build build
build/BS_thread_pool_test
```

MSVC 把最后一条换成 `build/Debug/BS_thread_pool_test`。用 CMake 把库作为 C++20 模块导入见[这里](#用-cmake-和-import-bsthread_pool-编译)，用 CMake 导入 C++ 标准库模块见[这里](#用-cmake-和-import-std-编译)。

### 用 CMake 和 `FetchContent` 安装

注意：以下说明只在写作时的最新版本 CMake v4.2.1 上测试过，旧版本可能不行。

如果使用 [CMake](https://cmake.org/) 但不想用第三方工具，可以用内置的 [`FetchContent`](https://cmake.org/cmake/help/latest/module/FetchContent.html) 模块安装 `BS::thread_pool`。下面的完整 `CMakeLists.txt` 和上一节一样会自动下载并编译测试程序，但直接使用 `FetchContent`：

```cmake
cmake_minimum_required(VERSION 4.2.1)
project(BS_thread_pool_test LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if(MSVC)
    add_compile_options(/permissive- /Zc:__cplusplus)
endif()

include(FetchContent)
set(FETCHCONTENT_UPDATES_DISCONNECTED ON)
FetchContent_Declare(
    bshoshany_thread_pool
    GIT_REPOSITORY https://github.com/bshoshany/thread-pool.git
    GIT_TAG v5.1.0
    DOWNLOAD_EXTRACT_TIMESTAMP TRUE
    EXCLUDE_FROM_ALL
    SYSTEM
)
FetchContent_MakeAvailable(bshoshany_thread_pool)
add_library(BS_thread_pool INTERFACE)
target_include_directories(BS_thread_pool INTERFACE ${bshoshany_thread_pool_SOURCE_DIR}/include)

add_executable(${PROJECT_NAME} ${bshoshany_thread_pool_SOURCE_DIR}/tests/BS_thread_pool_test.cpp)
target_link_libraries(${PROJECT_NAME} PRIVATE BS_thread_pool)
```

## 完整参考

本节列出库中全部类和函数，以及其他重要信息。函数给出的是简化原型（例如去掉了 `const`），便于阅读。说明保持简短，只作速查；详细信息和用法示例见上文。

每一项的说明也写在源码里的 [Doxygen](https://www.doxygen.nl/) 注释中。现代 IDE，例如 [Visual Studio Code](https://code.visualstudio.com/)，可以在鼠标悬停或自动补全时用这些注释给出文档。

### `BS::thread_pool` 类模板

`BS::thread_pool` 是主线程池类。它创建一组线程，持续执行提交到队列的任务。它可以接受模板参数，用来启用[下文](#可选功能与模板参数)所述的可选功能。不使用模板参数时，默认可用的成员函数如下。

* 构造函数：
    * `thread_pool()`：新建线程池。线程数等于 `std::thread::hardware_concurrency()`；如果启用了原生扩展，则等于 `BS::get_os_process_affinity()` 给出的、进程可用的线程数。
    * `thread_pool(std::size_t num_threads)`：用指定线程数新建线程池。
    * `thread_pool(F&& init)`：用默认线程数和指定的初始化函数新建线程池。`F` 是模板参数。
    * `thread_pool(std::size_t num_threads, F&& init)`：用指定线程数和指定初始化函数新建线程池。
* 重置：
    * `void reset()`：按默认线程数重置池（如同用默认构造函数构造）。先等待全部任务；如果启用了暂停，只等待正在运行的任务，重置后队列里的任务会继续执行。如果重置前池处于暂停状态，新池也会暂停。
    * `void reset(std::size_t num_threads)`：用新的线程数重置池。
    * `void reset(F&& init)`：用默认线程数和新的初始化函数重置池。`F` 是模板参数。
    * `void reset(std::size_t num_threads, F&& init)`：用新的线程数和新的初始化函数重置池。
* 设置：
    * `void set_cleanup_func(F&& cleanup)`：设置线程池的清理函数。`F` 是模板参数。
* 查询：
    * `std::size_t get_tasks_queued()`：当前在队列里等待执行的任务数。
    * `std::size_t get_tasks_running()`：当前正在被线程执行的任务数。
    * `std::size_t get_tasks_total()`：未完成任务总数，包括还在队列里等待的和正在线程上运行的。注意 `get_tasks_total() == get_tasks_queued() + get_tasks_running()`。
    * `std::size_t get_thread_count()`：池中的线程数。
    * `std::vector<std::thread::id> get_thread_ids()`：返回一个向量，元素是池中每条线程的唯一标识，由 `std::thread::get_id()` 取得（C++20 及以后是 `std::jthread::get_id()`）。
* 不返回 future 的任务提交（`T1`、`T2`、`F`、`C`、`I` 是模板参数）：
    * `void detach_task(F&& task)`：把没有参数、没有返回值的函数提交到任务队列。有参数的函数要包进 lambda。
    * `void detach_blocks(T1 first_index, T2 index_after_last, F&& block, std::size_t num_blocks = 0)`：自动把循环拆成块来并行化。块函数接受两个参数，即块的起点和终点，因此每块只调用一次；用户必须保证块函数正确处理块内的全部下标。
    * `void detach_loop(T1 first_index, T2 index_after_last, F&& loop, std::size_t num_blocks = 0)`：自动把循环拆成块来并行化。循环函数接受一个参数，即循环下标，因此每块会调用多次。
    * `void detach_sequence(T1 first_index, T2 index_after_last, F&& sequence)`：把按下标枚举的任务序列提交到队列。序列函数接受一个参数，即任务下标，每个下标调用一次。
    * `void detach_bulk(C& container)`：把一容器没有参数、没有返回值的函数提交到队列。
    * `void detach_bulk(I first, I last)`：把一个迭代器范围内没有参数、没有返回值的函数提交到队列。
* 返回 future 的任务提交（`T1`、`T2`、`F`、`R`、`C`、`I` 是模板参数）：
    * `std::future<R> submit_task(F&& task)`：把没有参数的函数提交到任务队列。有参数的函数要包进 lambda。
    * `BS::multi_future<R> submit_blocks(T1 first_index, T2 index_after_last, F&& block, std::size_t num_blocks = 0)`：自动把循环拆成块来并行化。块函数接受两个参数，即块的起点和终点，因此每块只调用一次；用户必须保证块函数正确处理块内的全部下标。返回的 `BS::multi_future` 装着所有块的 future。
    * `BS::multi_future<void> submit_loop(T1 first_index, T2 index_after_last, F&& loop, std::size_t num_blocks = 0)`：自动把循环拆成块来并行化。循环函数接受一个参数，即循环下标，因此每块会调用多次。它不能有返回值。返回的 `BS::multi_future` 装着所有块的 future。
    * `BS::multi_future<R> submit_sequence(T1 first_index, T2 index_after_last, F&& sequence)`：把按下标枚举的任务序列提交到队列。序列函数接受一个参数，即任务下标，每个下标调用一次。返回的 `BS::multi_future` 装着所有任务的 future。
    * `BS::multi_future<R> submit_bulk(C& container)`：把一容器没有参数的函数提交到队列。返回的 `BS::multi_future` 装着所有任务的 future。
    * `BS::multi_future<R> submit_bulk(I first, I last)`：把一个迭代器范围内没有参数的函数提交到队列。返回的 `BS::multi_future` 装着所有任务的 future。
* 任务管理：
    * `void purge()`：清除队列里等待的全部任务。被清除的任务无法恢复。
* 等待任务（`R`、`P`、`C`、`D` 是模板参数）：
    * `void wait()`：等待全部任务完成，包括正在线程上运行的和还在队列里等待的。
    * `bool wait_for(std::chrono::duration<R, P>& duration)`：等待任务完成，但在指定时长过去后停止等待。全部任务都跑完返回 `true`；时间到了仍有任务在跑返回 `false`。
    * `bool wait_until(std::chrono::time_point<C, D>& timeout_time)`：等待任务完成，但在指定时间点到达后停止等待。全部任务都跑完返回 `true`；时间点到了仍有任务在跑返回 `false`。
* 析构函数：
    * `~thread_pool()`：等待全部任务完成，然后销毁全部线程。如果设置了清理函数，每条线程在销毁前会先运行它。

### 可选功能与模板参数

线程池有若干可选功能，必须通过模板参数显式启用。模板参数是位掩码，可以用按位或 `|` 一次启用多项。标志是枚举类 `BS::tp` 的成员。

* **任务优先级：** 在模板参数里打开 `BS::tp::priority`。启用后，静态成员 `priority_enabled` 为 `true`。
    * 启用后，所有 detach 和 submit 函数都可以在参数列表末尾额外指定任务或一组任务的优先级。不指定时默认是 0。
    * 优先级类型是 `BS::priority_t`，即有符号 8 位整数，范围是 -128 到 +127。任务按优先级从高到低执行。一组并行任务的优先级都相同。
    * 枚举 `BS::pr` 预定义了一些优先级：`BS::pr::highest`、`BS::pr::high`、`BS::pr::normal`、`BS::pr::low`、`BS::pr::lowest`。
* **暂停：** 在模板参数里打开 `BS::tp::pause`。启用后，静态成员 `pause_enabled` 为 `true`。增加这些成员函数：
    * `void pause()`：暂停池。工作线程暂时不再从队列取新任务，但已经在执行的任务会继续跑完。
    * `void unpause()`：取消暂停。工作线程恢复从队列取新任务。
    * `bool is_paused()`：查询池当前是否暂停。
* **等待死锁检查：** 在模板参数里打开 `BS::tp::wait_deadlock_checks`。启用后，静态成员 `wait_deadlock_checks_enabled` 为 `true`。
    * 启用后，`wait()`、`wait_for()`、`wait_until()` 会检查调用者是否就在同一个池的某条线程上，那样会造成死锁。如果是，它们不进入等待，而是抛出 `BS::wait_deadlock`。
    * 如果特性测试宏 `__cpp_exceptions` 未定义，等待死锁检查会自动关闭；这时若尝试启用该功能，编译会失败。

便利别名：

* `BS::light_thread_pool` 关闭全部可选功能（等价于默认模板参数的 `BS::thread_pool`，即 `BS::thread_pool<BS::tp::none>`）。
* `BS::priority_thread_pool` 启用任务优先级（等价于 `BS::thread_pool<BS::tp::priority>`）。
* `BS::pause_thread_pool` 启用暂停（等价于 `BS::thread_pool<BS::tp::pause>`）。
* `BS::wdc_thread_pool` 启用等待死锁检查（等价于 `BS::thread_pool<BS::tp::wait_deadlock_checks>`）。

### `BS::this_thread` 类

`BS::this_thread` 提供与 `std::this_thread` 类似的功能。静态成员函数：

* `static std::optional<std::size_t> get_index()`：取得当前线程的下标。如果线程不在池里，optional 没有值。
* `static std::optional<void*> get_pool()`：取得拥有当前线程的线程池指针。如果线程不在池里，optional 没有值。

如果启用了[原生扩展](#参考中的原生扩展)，该类还会有额外的静态成员函数。详见对应章节。

### 参考中的原生扩展

编译时定义宏 `BS_THREAD_POOL_NATIVE_EXTENSIONS` 即可启用原生扩展。如果以头文件方式包含，必须在 `#include "BS_thread_pool.hpp"` 之前定义该宏。如果作为 C++20 模块导入，必须作为编译器标志定义。原生扩展使用操作系统原生 API，因此不可移植；但应能在 Windows、Linux 和 macOS 上工作。

原生扩展向 `BS` 命名空间添加这些函数：

* `bool BS::set_os_process_affinity(std::vector<bool>& affinity)`：设置当前进程的处理器亲和性。参数是 `std::vector<bool>`，每个元素对应一个逻辑处理器。成功返回 `true`，否则返回 `false`。在 macOS 上不可用。
* `std::optional<std::vector<bool>> BS::get_os_process_affinity()`：读取当前进程的处理器亲和性。无法确定时 optional 没有值。在 macOS 上不可用。
* `bool BS::set_os_process_priority(BS::os_process_priority priority)`：设置当前进程优先级。参数必须是枚举 `BS::os_process_priority` 的成员，选项为 `idle`、`below_normal`、`normal`、`above_normal`、`high`、`realtime`。成功返回 `true`，否则返回 `false`。
* `std::optional<BS::os_process_priority> BS::get_os_process_priority()`：读取当前进程优先级。无法确定，或者不是 `BS::os_process_priority` 的预定义值时，optional 没有值。

原生扩展还向 `BS::this_thread` 添加这些静态成员函数：

* `bool BS::this_thread::set_os_thread_affinity(std::vector<bool>& affinity)`：设置当前线程的处理器亲和性。参数是 `std::vector<bool>`，每个元素对应一个逻辑处理器。线程亲和性必须是该线程所属进程的进程亲和性的子集。在 macOS 和 Android 上不可用。
* `std::optional<std::vector<bool>> BS::this_thread::get_os_thread_affinity()`：读取当前线程的处理器亲和性。无法确定时 optional 没有值。在 macOS 和 Android 上不可用。
* `bool BS::this_thread::set_os_thread_name(std::string& name)`：设置当前线程名。在 Linux 上，线程名最多 16 个字符，包括空终止符。成功返回 `true`，否则返回 `false`。
* `std::optional<std::string> BS::this_thread::get_os_thread_name()`：读取当前线程名。无法确定时 optional 没有值。
* `bool BS::this_thread::set_os_thread_priority(BS::os_thread_priority priority)`：设置当前线程优先级。参数必须是枚举 `BS::os_thread_priority` 的成员，选项为 `idle`、`lowest`、`below_normal`、`normal`、`above_normal`、`highest`、`realtime`。成功返回 `true`，否则返回 `false`。
* `std::optional<BS::os_thread_priority> BS::this_thread::get_os_thread_priority()`：读取当前线程优先级。无法确定，或者不是 `BS::os_thread_priority` 的预定义值时，optional 没有值。

最后，原生扩展向 `BS::thread_pool` 添加这个成员函数：

* `std::vector<std::thread::native_handle_type> get_native_handles()`：返回一个向量，元素是池中每条线程由实现定义的底层句柄。

### `BS::multi_future` 类

`BS::multi_future<T>` 用来一次等待多个 future 和/或取得它们的结果。它是 `std::vector<std::future<T>>` 的特化。因此能用在 [`std::vector<std::future<T>>`](https://zh.cppreference.com/w/cpp/container/vector) 上的成员函数也能用在 `BS::multi_future<T>` 上。例如它有迭代器，可以用 range-based for。

除了继承来的成员函数，`BS::multi_future<T>` 还有这些专用成员函数（`R`、`P`、`C`、`D` 是模板参数）：

* `[void 或 std::vector<T>] get()`：取得这个 `BS::multi_future` 里全部 future 的结果，并重新抛出其中保存的异常。如果 future 返回 `void`，本函数也返回 `void`。如果 future 返回类型 `T`，本函数返回装着结果的向量。
* `std::size_t ready_count()`：这个 `BS::multi_future` 里有多少 future 已就绪。
* `bool valid()`：这个 `BS::multi_future` 里的 future 是否全部有效。
* `void wait()`：等待这个 `BS::multi_future` 里的全部 future。
* `bool wait_for(std::chrono::duration<R, P>& duration)`：等待全部 future，但在指定时长过去后停止。时间耗尽之前全部都等到了返回 `true`，否则返回 `false`。
* `bool wait_until(std::chrono::time_point<C, D>& timeout_time)`：等待全部 future，但在指定时间点到达后停止。时间点到达之前全部都等到了返回 `true`，否则返回 `false`。

### `BS::synced_stream` 类

`BS::synced_stream` 用来让不同线程同步地向一个或多个输出流打印。成员函数如下（`T` 是模板参数包）：

* `synced_stream()`：新建同步流，打印到 `std::cout`。
* `synced_stream(T&... streams)`：新建同步流，打印到给定的输出流。
* `void add_stream(std::ostream& stream)`：向输出流列表添加一个流。
* `std::vector<std::ostream*>& get_streams()`：取得指向各输出流的指针向量的引用。
* `void print(T&... items)`：把任意多个项打印到输出流。只要各方都只用同一个 `BS::synced_stream` 对象打印，就能保证没有其他线程同时打印。
* `void println(T&&... items)`：把任意多个项打印到输出流，末尾再加换行符。
* `void remove_stream(std::ostream& stream)`：从输出流列表移除一个流。

另外还有两个流操纵符，用来帮助编译器确定该用哪个模板特化：

* `BS::synced_stream::endl`：`std::endl` 的显式转换。向流打印换行符，然后刷新。只有确实需要刷新时才用，否则用换行符。
* `BS::synced_stream::flush`：`std::flush` 的显式转换。用来刷新流。

### `BS::version` 类

`BS::version` 用来表示版本号。公有成员是 `major`、`minor`、`patch`，成员函数如下：

* `constexpr version(std::uint64_t major, std::uint64_t minor, std::uint64_t patch)`：用指定的主版本号、次版本号和修订号构造版本对象。
* `std::strong_ordering operator<=>(version&)`：C++20 及以后，两个版本号的三路比较运算符。在 C++17 中，改为显式定义 `==`、`!=`、`<`、`<=`、`>`、`>=`。
* `std::string to_string()`：把版本号转成 `"major.minor.patch"` 格式的字符串。
* `std::ostream& operator<<(std::ostream& stream, version& ver)`：把版本字符串输出到流。

库还定义了类型为 `BS::version` 的 `constexpr` 对象 `BS::thread_pool_version`，可在编译期检查库版本。

该功能从库的 v5.0.0 起可用。更早的版本使用宏 `BS_THREAD_POOL_VERSION_MAJOR`、`BS_THREAD_POOL_VERSION_MINOR`、`BS_THREAD_POOL_VERSION_PATCH`。这些宏仍为兼容而定义，但如果库是作为 C++20 模块导入的，就访问不到它们。

### 诊断变量

库定义了这些 `constexpr` 变量：

* `bool thread_pool_import_std`：库是否用 `import std` 导入了 C++23 标准库模块。
* `bool thread_pool_module`：库是否作为 C++20 模块编译。
* `bool thread_pool_native_extensions`：是否启用了原生扩展。

### C++20 模块导出的全部名称

用 `import BS.thread_pool` 作为 C++20 模块导入时，库按字母序导出这些名称：

* `BS::common_index_type_t`
* `BS::light_thread_pool`
* `BS::multi_future`
* `BS::pause_thread_pool`
* `BS::pr`
* `BS::priority_t`
* `BS::priority_thread_pool`
* `BS::synced_stream`
* `BS::this_thread`
* `BS::thread_pool`
* `BS::thread_pool_import_std`
* `BS::thread_pool_module`
* `BS::thread_pool_native_extensions`
* `BS::thread_pool_version`
* `BS::tp`（以及相关的按位运算符）
* `BS::version`
* `BS::wdc_thread_pool`

如果启用了异常，还会导出：

* `BS::wait_deadlock`

如果启用了原生扩展，还会导出：

* `BS::get_os_process_affinity`
* `BS::get_os_process_priority`
* `BS::os_process_priority`
* `BS::os_thread_priority`
* `BS::set_os_process_affinity`
* `BS::set_os_process_priority`

## 开发工具

### `compile_cpp.py` 脚本

Python 脚本 `compile_cpp.py` 位于 [GitHub 仓库](https://github.com/bshoshany/thread-pool) 的 `scripts` 目录，可以在不同平台上用不同编译器编译任意 C++ 源文件。它只在写作时的最新版本 Python v3.14.2 上测试过，旧版本可能不行。

脚本由库作者编写，用来配合内置的 Visual Studio Code 任务，方便用不同编译器、标准和平台的组合测试本库。它不能代替 CMake 或完整的构建系统，只是开发本库这类单头文件库或其他小项目时的便利脚本。

`compile_cpp.py` 也会透明地处理 C++20 模块，以及在 C++23 中把 C++ 标准库作为模块导入。想把本库作为 C++20 模块导入的用户会觉得它特别有用。

编译参数可以用命令行参数配置，也可以用可选的 YAML 配置文件 `compile_cpp.yaml`。命令行参数如下：

* 位置参数：要编译的源文件。
* `-h` 或 `--help`：显示帮助并退出。
* `-a` 或 `--arch`：目标架构（仅 MSVC）。必须是 `[amd64, arm64]` 之一，默认 `amd64`。
* `-b` 或 `--clear-output`：编译前清空输出目录。如果没有指定源文件，就只清空并退出。结果始终是一个空的输出目录。
* `-c` 或 `--compiler`：使用哪个编译器。必须是 `[cl, clang++, g++]` 之一。默认按平台自动判断。
* `-d` 或 `--define`：要定义的宏。多次使用可定义多个宏。`compile_cpp.yaml` 里还可以再定义宏。
* `-e` 或 `--force`：即使可执行文件已是最新，也强制重新编译。
* `-f` 或 `--flag`：额外的编译器标志。多次使用可添加多个标志。`compile_cpp.yaml` 里还可以再指定标志。
* `-g` 或 `--ignore-config`：忽略已存在的 `compile_cpp.yaml`。
* `-i` 或 `--include`：要使用的包含目录。多次使用可指定多个目录。`compile_cpp.yaml` 里还可以再指定包含目录。
* `-l` 或 `--as-module`：把文件作为 C++20 模块编译。
* `-m` 或 `--module`：要用的 C++20 模块文件，格式为 `module_name=module_file,dependencies,...`。多次使用可指定多个模块。`compile_cpp.yaml` 里还可以再指定模块。依赖只用来判断模块是否需要重新编译。
* `-n` 或 `--deps`：用来检测是否需要重新编译的依赖。这些文件被修改时，即使源文件没改，也会重新编译可执行文件。多次使用可添加多个依赖。`compile_cpp.yaml` 里还可以再指定依赖。注意这不用于 C++20 模块；模块有自己的依赖，在使用 `-m` 时列出。
* `-o` 或 `--output`：输出目录和/或可执行文件名。以 `/` 结尾时，目录不存在就创建。未指定时使用 `compile_cpp.yaml` 里定义的目录。未指定可执行文件名时，按 `{source}_[module_]{type}-{compiler}-{standard}` 自动生成，其中：
    * `source` 是第一个源文件的文件名（不含扩展名）。
    * 如果有 `module_`，表示该文件是 C++20 模块（启用了 `-l`/`--as-module`）。
    * `type` 是 `[debug, release]` 之一。
    * `compiler` 是 `[clang, gcc, msvc]` 之一。
    * `standard` 是 `[cpp17, cpp20, cpp23]` 之一。
* `-p` 或 `--pass`：如果指定了 `-r`/`--run`，把命令行参数传给编译出的程序。多次使用可传多个参数。`compile_cpp.yaml` 里还可以再指定参数。
* `-r` 或 `--run`：编译后运行程序。
* `-s` 或 `--std`：使用哪个 C++ 标准。必须是 `[c++17, c++20, c++23]` 之一。默认是 `c++23`。
* `-t` 或 `--type`：以哪种模式编译。必须是 `[debug, release]` 之一。默认是 `debug`。
* `-u` 或 `--std-module`：指定标准库模块的路径（仅 C++23）。未指定时取自 `compile_cpp.yaml`。用 `auto` 自动检测，用 `disable` 显式关闭。
* `-v` 或 `--verbose`：打印脚本的诊断信息。
* `-x` 或 `--disable-exceptions`：设为 `true` 时在编译器标志里关闭异常。设为 `false` 时启用异常。未指定时取自 `compile_cpp.yaml`。
* `-y` 或 `--try-all`：用系统上所有可用的编译器和 C++ 标准组合做测试编译。如果指定了 `-r`/`--run`，也会运行每个编译出的程序。其余参数会传给每一次编译尝试。不能和 `-c`/`--compiler` 或 `-s`/`--std` 一起用。

`compile_cpp.yaml` 包含这些字段：

* `defines`：编译源文件时要定义的宏列表。
* `deps`：依赖列表，例如头文件或库。这些文件中任何一个变化，用本脚本编译的全部源文件都会重新编译。
* `disable_exceptions`：是否在编译器标志里关闭异常。未指定时默认为 `false`。
* `flags`：传给各编译器的标志映射。编译器应是 `[cl, clang++, g++]` 之一。标志应是字符串列表。
* `includes`：包含目录列表。
* `modules`：C++20 模块映射，格式为 `module_name: [module_path, dependencies, ...]`。只在 C++20 或 C++23 模式下使用。依赖只用来判断模块是否需要重新编译。
* `output`：编译文件的输出目录。
* `pass_args`：编译后运行程序时要传入的参数列表。
* `std_module`：每种操作系统和编译器组合对应的标准库模块路径映射（仅 C++23）。操作系统应是 `[Windows, Linux, Darwin]` 之一。能自动判断时用 `auto`。

用法示例见 GitHub 仓库里的 `compile_cpp.yaml`。

默认情况下，脚本用 ANSI 转义码打印彩色输出，便于阅读。设置环境变量 `NO_COLOR` 可关闭。

### Visual Studio Code 任务

给 Visual Studio Code 用户准备了三个 `.vscode` 目录，都在 GitHub 仓库里：

* `.vscode-windows`：Windows，配合 Clang、GCC 和 MSVC。
* `.vscode-linux`：Linux，配合 Clang 和 GCC。
* `.vscode-macos`：macOS，配合 LLVM Clang（不是 Apple Clang）。

每个目录里有合适的 `c_cpp_properties.json`、`launch.json` 和 `tasks.json`，它们使用附带的 Python 脚本 [`compile_cpp.py`](#compile_cpppy-脚本)。欢迎在自己的项目里使用这些文件，但在特定系统上可能需要修改。

## 关于本项目

### 缺陷报告和功能请求

本库持续积极开发。遇到缺陷，或想请求新功能，请[在 GitHub 上开一个 issue](https://github.com/bshoshany/thread-pool/issues)，作者会尽快查看。

### 贡献和 pull request 政策

欢迎贡献。作者在本地编辑和测试之后，以累积更新的方式发布项目，因此**政策是不接受任何 pull request**。如果开了 pull request，而作者决定采纳建议，会先按项目的编码约定（格式、语法、命名、注释、编程实践等）修改代码，并做一些测试，确保改动不会破坏现有功能。然后并入下一次发布，可能和其他改动放在一起。新发布还会在 `CHANGELOG.md` 里注明，并链接到该 pull request，必要时也会修改 `README.md` 的文档。

### 给仓库加星

如果这个项目有用，请考虑[在 GitHub 上加星](https://github.com/bshoshany/thread-pool/stargazers)。这样作者能看到有多少人在用这些代码，也更有动力继续改进。

### 致谢

许多 GitHub 用户通过 issue、pull request、评论和/或私人通信，直接或间接地帮助改进了这个项目。对帮助最大的具体 issue 和 pull request，链接见 `CHANGELOG.md`。感谢各位的贡献。

### 版权与引用

Copyright (c) 2021-2026 [Barak Shoshany](https://baraksh.com/)。采用 [MIT 许可证](https://github.com/bshoshany/thread-pool/blob/master/LICENSE.txt)。

在任何软件中使用本库时，请在源代码和文档里提供 [GitHub 仓库](https://github.com/bshoshany/thread-pool) 的链接。

在已发表的研究中使用本库时，请按如下方式引用：

* Barak Shoshany, *"A C++17 Thread Pool for High-Performance Scientific Computing"*, [doi:10.1016/j.softx.2024.101687](https://doi.org/10.1016/j.softx.2024.101687), [SoftwareX 26 (2024) 101687](https://www.sciencedirect.com/science/article/pii/S235271102400058X), [arXiv:2105.00613](https://arxiv.org/abs/2105.00613)

BibTeX：

```bibtex
@article{Shoshany2024_ThreadPool,
    archiveprefix = {arXiv},
    author        = {Barak Shoshany},
    doi           = {10.1016/j.softx.2024.101687},
    eprint        = {2105.00613},
    journal       = {SoftwareX},
    pages         = {101687},
    title         = {{A C++17 Thread Pool for High-Performance Scientific Computing}},
    url           = {https://www.sciencedirect.com/science/article/pii/S235271102400058X},
    volume        = {26},
    year          = {2024}
}
```

注意：[SoftwareX](https://www.sciencedirect.com/science/article/pii/S235271102400058X) 和 [arXiv](https://arxiv.org/abs/2105.00613) 上的论文没有跟上库的最新版本。这些出版物只是为了方便科研人员发现本库，并在科学研究中引用它。最新版本文档只由 [GitHub 仓库](https://github.com/bshoshany/thread-pool) 里的 `README.md` 提供。中文译本是 `README.zh.md`。

### 关于作者

作者 Barak Shoshany 是理论、数学和计算物理学家，加拿大安大略省 Brock University 物理学助理教授，也是 McMaster University 的兼职讲师。研究聚焦于广义相对论和量子力学中时间与因果性的本质，以及符号计算和高性能科学计算。更多介绍见[个人网站](https://baraksh.com/)。

### 进一步学习 C++

C++ 初学者可能对作者在 McMaster University 研究生课程上的[讲义](https://baraksh.com/CSE701/notes/)感兴趣。讲义从零讲授现代 C 和 C++，包括开发本库时用到的一些高级技巧和编程实践。这门课自 2020 年起每年开设，讲义根据学生反馈持续更新。

### 其他项目

物理或天文学读者可能对 [OGRe](https://github.com/bshoshany/OGRe) 感兴趣：面向 Mathematica 的面向对象广义相对论包。它的 Python 移植是 [OGRePy](https://github.com/bshoshany/OGRePy)。




