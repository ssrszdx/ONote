作者：不知道该叫啥  
链接：https://www.zhihu.com/question/23357089/answer/2934967845  
来源：知乎  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。  
  

C/C++ 是应用最广泛的[编程语言](https://www.zhihu.com/search?q=%E7%BC%96%E7%A8%8B%E8%AF%AD%E8%A8%80&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)之一，无论是在操作系统、[嵌入式系统](https://www.zhihu.com/search?q=%E5%B5%8C%E5%85%A5%E5%BC%8F%E7%B3%BB%E7%BB%9F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)、游戏开发、[网络编程](https://www.zhihu.com/search?q=%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)、大数据处理、人工智能等领域都有广泛的应用。为了提高编程效率、[代码质量](https://www.zhihu.com/search?q=%E4%BB%A3%E7%A0%81%E8%B4%A8%E9%87%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)和[可维护性](https://www.zhihu.com/search?q=%E5%8F%AF%E7%BB%B4%E6%8A%A4%E6%80%A7&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)，我们需要使用一些专业的工具来辅助开发。下面是我个人推荐的几个值得使用的 C/C++ 编程工具。

## 一、[编译器](https://www.zhihu.com/search?q=%E7%BC%96%E8%AF%91%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)和[调试器](https://www.zhihu.com/search?q=%E8%B0%83%E8%AF%95%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)
**1. GCC**

GCC 是 GNU Compiler Collection 的缩写，是一个非常流行的开源 C/C++ 编译器。GCC 支持多种操作系统和平台，如 Linux、Windows、Mac OS X、ARM、MIPS、PowerPC 等，同时也支持多种语言的编译，包括 C、C++、Fortran、Ada、Objective-C、Java 等。GCC 的特点是稳定、可靠、高效，能够生成高质量的[可执行文件](https://www.zhihu.com/search?q=%E5%8F%AF%E6%89%A7%E8%A1%8C%E6%96%87%E4%BB%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)和[库文件](https://www.zhihu.com/search?q=%E5%BA%93%E6%96%87%E4%BB%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)。除此之外，GCC 还提供了丰富的编译选项和插件机制，可以根据不同的需求进行定制化的编译。

**2. Clang**

Clang 是一个基于 LLVM 架构的 C/C++ 编译器，被认为是 GCC 的一个良好[替代品](https://www.zhihu.com/search?q=%E6%9B%BF%E4%BB%A3%E5%93%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)。Clang 的特点是编译速度快、错误信息友好、[模块化](https://www.zhihu.com/search?q=%E6%A8%A1%E5%9D%97%E5%8C%96&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)设计、支持[静态分析](https://www.zhihu.com/search?q=%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)和[代码重构](https://www.zhihu.com/search?q=%E4%BB%A3%E7%A0%81%E9%87%8D%E6%9E%84&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)等特性。Clang 可以将代码编译成 LLVM [中间代码](https://www.zhihu.com/search?q=%E4%B8%AD%E9%97%B4%E4%BB%A3%E7%A0%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)，再转换为[目标代码](https://www.zhihu.com/search?q=%E7%9B%AE%E6%A0%87%E4%BB%A3%E7%A0%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)，因此可以支持多种平台和[体系结构](https://www.zhihu.com/search?q=%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)。Clang 的另一个优点是可以作为一个库使用，方便其他程序调用和[扩展](https://www.zhihu.com/search?q=%E6%89%A9%E5%B1%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)。

**3. GDB**

GDB 是 GNU Debugger 的缩写，是一个常用的 C/C++ 调试器。GDB 可以在代码[运行时](https://www.zhihu.com/search?q=%E8%BF%90%E8%A1%8C%E6%97%B6&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)暂停程序、查看变量的值、跟踪[函数调用](https://www.zhihu.com/search?q=%E5%87%BD%E6%95%B0%E8%B0%83%E7%94%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)、查看堆栈信息、设置断点等功能，能够帮助开发者快速定位代码中的问题。GDB 支持多种平台和体系结构，可以与不同的编译器配合使用，提供了丰富的命令和扩展机制，可以满足各种调试需求。

## 二、[集成开发环境](https://www.zhihu.com/search?q=%E9%9B%86%E6%88%90%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)

**1. Visual Studio**

Visual Studio 是[微软公司](https://www.zhihu.com/search?q=%E5%BE%AE%E8%BD%AF%E5%85%AC%E5%8F%B8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)开发的一款集成开发环境，支持多种编程语言和平台，包括 C/C++、C#、Visual Basic、.NET 等。Visual Studio 的特点是功能强大、界面友好、[可扩展性](https://www.zhihu.com/search?q=%E5%8F%AF%E6%89%A9%E5%B1%95%E6%80%A7&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)好、集成度高，能够提供全面的开发支持和[调试工具](https://www.zhihu.com/search?q=%E8%B0%83%E8%AF%95%E5%B7%A5%E5%85%B7&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)，包括代码[编辑器](https://www.zhihu.com/search?q=%E7%BC%96%E8%BE%91%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)、[自动完成](https://www.zhihu.com/search?q=%E8%87%AA%E5%8A%A8%E5%AE%8C%E6%88%90&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)、调试器、[性能分析器](https://www.zhihu.com/search?q=%E6%80%A7%E8%83%BD%E5%88%86%E6%9E%90%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)、单元测试等。Visual Studio 的社区版是免费的，但是专业版和企业版则需要付费。

**2. Eclipse**

Eclipse 是一款开源的集成开发环境，支持多种编程语言和平台，包括 C/C++、Java、Python、PHP、Android 等。Eclipse 的特点是插件丰富、可扩展性强、[跨平台](https://www.zhihu.com/search?q=%E8%B7%A8%E5%B9%B3%E5%8F%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)、免费开源，能够提供全面的开发支持和调试工具，包括代码编辑器、自动完成、调试器、性能分析器、单元测试等。Eclipse 的 C/C++ 开发插件叫做 CDT，可以提供更加专业的开发支持。

**3. Code::Blocks**

Code::Blocks 是一款开源的 C/C++ 集成开发环境，支持多种平台，包括 Windows、Linux、Mac OS X。Code::Blocks 的特点是[轻量级](https://www.zhihu.com/search?q=%E8%BD%BB%E9%87%8F%E7%BA%A7&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)、快速、易用，同时提供了丰富的开发支持和调试工具，包括代码编辑器、自动完成、调试器、性能分析器、单元测试等。Code::Blocks 还支持多种编译器和[构建系统](https://www.zhihu.com/search?q=%E6%9E%84%E5%BB%BA%E7%B3%BB%E7%BB%9F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)，能够满足不同的编程需求。

## 三、静态分析工具

**1.Clang Static Analyzer**

Clang Static Analyzer 是一个基于 Clang 的静态分析工具，可以检测 C/C++ 代码中的潜在错误和安全漏洞。Clang Static Analyzer 的特点是精确、高效、易用，能够[识别代码](https://www.zhihu.com/search?q=%E8%AF%86%E5%88%AB%E4%BB%A3%E7%A0%81&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)中的空指针、[内存泄漏](https://www.zhihu.com/search?q=%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)、数据竞争、格式错误等问题，并提供详细的报告和建议。Clang Static Analyzer 的运行速度很快，可以在编译时或者独立运行时进行静态分析。

**2. Coverity**

Coverity 是一款商业的静态分析工具，可以检测 C/C++ 代码中的缺陷、漏洞和安全风险。Coverity 的特点是准确、高效、可扩展，能够提供全面的代码审查和修复支持，帮助开发者提高代码质量和安全性。Coverity 还支持多种平台和集成开发环境，能够满足不同的开发需求。

**3. [PVS-Studio](https://www.zhihu.com/search?q=PVS-Studio&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)**

PVS-Studio 是一款商业的静态分析工具，可以检测 C/C++ 代码中的缺陷、漏洞和潜在问题。PVS-Studio 的特点是精确、高效、易用，能够快速定位代码中的问题，并提供详细的报告和建议。PVS-Studio 还支持多种编译器和构建系统，能够与不同的[开发工具](https://www.zhihu.com/search?q=%E5%BC%80%E5%8F%91%E5%B7%A5%E5%85%B7&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)集成使用。

## 四、性能分析工具

1. **Valgrind**

Valgrind 是一个开源的性能分析工具，可以检测 C/C++ 代码中的内存泄漏、访问越界、使用未初始化的变量等问题。Valgrind 的特点是强大、灵活、易用，能够提供全面的性能分析和调试支持，包括内存分析、[调用图分析](https://www.zhihu.com/search?q=%E8%B0%83%E7%94%A8%E5%9B%BE%E5%88%86%E6%9E%90&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)、CPU 分析、线程分析等。Valgrind 还支持多种[工具集](https://www.zhihu.com/search?q=%E5%B7%A5%E5%85%B7%E9%9B%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)，能够满足不同的分析需求。

**2. Perf**

Perf 是一个开源的性能分析工具，可以检测 C/C++ 代码中的 CPU、内存和 I/O 等性能问题。Perf 的特点是高效、精确、易用，能够提供全面的性能分析和调试支持，包括 CPU 分析、[内存分析](https://www.zhihu.com/search?q=%E5%86%85%E5%AD%98%E5%88%86%E6%9E%90&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)、事件分析等。Perf 还支持多种工具集和插件，能够与不同的开发工具集成使用。

**3. Intel VTune**

Intel VTune 是一款商业的性能分析工具，可以检测 C/C++ 代码中的 CPU、内存、I/O 和 GPU 等性能问题。Intel VTune 的特点是精确、高效、可扩展，能够提供全面的性能分析和调试支持，包括 CPU 分析、内存分析、线程分析、GPU 分析等。Intel VTune 还支持多种平台和集成开发环境，能够满足不同的性能分析需求。

## 五、构建工具

1. **CMake**

CMake 是一款跨平台的构建工具，可以管理 C/C++ 代码的构建过程。CMake 的特点是灵活、可扩展、易用，能够提供基本的构建支持和[依赖管理](https://www.zhihu.com/search?q=%E4%BE%9D%E8%B5%96%E7%AE%A1%E7%90%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)，包括自动化构建、依赖管理、[测试运行](https://www.zhihu.com/search?q=%E6%B5%8B%E8%AF%95%E8%BF%90%E8%A1%8C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)等。CMake 还支持多种编译器和构建工具，并提供多种插件和扩展，能够满足不同的构建需求。

**2. Make**

Make 是一款传统的构建工具，可以管理 C/C++ 代码的构建过程。Make 的特点是简单、易用、可扩展，能够提供基本的构建支持和依赖管理，包括自动化构建、依赖管理、测试运行等。Make 还支持多种编译器和构建工具，并提供多种插件和扩展，能够满足不同的构建需求。

**3. Ninja**

Ninja 是一款轻量级的构建工具，可以管理 C/C++ 代码的构建过程。Ninja 的特点是快速、可靠、易用，能够提供基本的构建支持和依赖管理，包括[自动化](https://www.zhihu.com/search?q=%E8%87%AA%E5%8A%A8%E5%8C%96&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)构建、依赖管理、测试运行等。Ninja 还支持多种编译器和构建工具，并提供多种插件和扩展，能够满足不同的构建需求。

**4. Bazel**

Bazel 是一款[谷歌](https://www.zhihu.com/search?q=%E8%B0%B7%E6%AD%8C&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2934967845%7D)开源的构建工具，可以管理 C/C++ 代码的构建过程。Bazel 的特点是高效、可扩展、易用，能够提供基本的构建支持和依赖管理，包括自动化构建、依赖管理、测试运行等。Bazel 还支持多种编译器和构建工具，并提供多种插件和扩展，能够满足不同的构建需求。