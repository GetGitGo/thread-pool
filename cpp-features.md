# 项目 C++ 特性清单

范围：
- `include/BS_thread_pool.hpp`
- `modules/BS.thread_pool.cppm`
- `tests/BS_thread_pool_test.cpp`

行号是该特性的代表位置，同一特性多处出现时不逐行列出。Ctrl+点击链接打开文件并定位到该行；区间链接定位到起始行。

## 预处理与模块

| 特性术语 | 英文术语 | 所在文件行号 |
| --- | --- | --- |
| 头文件保护宏 | include guard | [include/BS_thread_pool.hpp:17-18](include/BS_thread_pool.hpp#L17) [include/BS_thread_pool.hpp:2510](include/BS_thread_pool.hpp#L2510) |
| 函数式宏 | function-like macro | [include/BS_thread_pool.hpp:273-281](include/BS_thread_pool.hpp#L273) [include/BS_thread_pool.hpp:373-375](include/BS_thread_pool.hpp#L373) [include/BS_thread_pool.hpp:444](include/BS_thread_pool.hpp#L444) |
| 记号粘贴 `##` | token pasting | [include/BS_thread_pool.hpp:278](include/BS_thread_pool.hpp#L278) |
| 条件编译 | conditional compilation | [include/BS_thread_pool.hpp:21-113](include/BS_thread_pool.hpp#L21)；[tests/BS_thread_pool_test.cpp:25-85](tests/BS_thread_pool_test.cpp#L25) |
| `#error` | `#error` directive | [include/BS_thread_pool.hpp:71](include/BS_thread_pool.hpp#L71) |
| `#include` / `#undef` | source inclusion / macro undefinition | [include/BS_thread_pool.hpp:23](include/BS_thread_pool.hpp#L23) [include/BS_thread_pool.hpp:30](include/BS_thread_pool.hpp#L30) [include/BS_thread_pool.hpp:74](include/BS_thread_pool.hpp#L74) [include/BS_thread_pool.hpp:117](include/BS_thread_pool.hpp#L117) |
| `__has_include` | `__has_include` | [include/BS_thread_pool.hpp:21-22](include/BS_thread_pool.hpp#L21)；[tests/BS_thread_pool_test.cpp:18-19](tests/BS_thread_pool_test.cpp#L18) [tests/BS_thread_pool_test.cpp:767](tests/BS_thread_pool_test.cpp#L767) |
| 特性测试宏 | feature-test macro | [include/BS_thread_pool.hpp:29](include/BS_thread_pool.hpp#L29) [include/BS_thread_pool.hpp:97](include/BS_thread_pool.hpp#L97) [include/BS_thread_pool.hpp:100](include/BS_thread_pool.hpp#L100) [include/BS_thread_pool.hpp:104](include/BS_thread_pool.hpp#L104) [include/BS_thread_pool.hpp:107](include/BS_thread_pool.hpp#L107) [include/BS_thread_pool.hpp:110](include/BS_thread_pool.hpp#L110) [include/BS_thread_pool.hpp:148](include/BS_thread_pool.hpp#L148) [include/BS_thread_pool.hpp:296](include/BS_thread_pool.hpp#L296) [include/BS_thread_pool.hpp:367](include/BS_thread_pool.hpp#L367)；[tests/BS_thread_pool_test.cpp:67-70](tests/BS_thread_pool_test.cpp#L67) [tests/BS_thread_pool_test.cpp:697-762](tests/BS_thread_pool_test.cpp#L697) |
| `__cplusplus` | predefined macro `__cplusplus` | [include/BS_thread_pool.hpp:28](include/BS_thread_pool.hpp#L28) [include/BS_thread_pool.hpp:66](include/BS_thread_pool.hpp#L66)；[tests/BS_thread_pool_test.cpp:25](tests/BS_thread_pool_test.cpp#L25) [tests/BS_thread_pool_test.cpp:78](tests/BS_thread_pool_test.cpp#L78) |
| 编译器 / 平台预定义宏 | predefined compiler and platform macros | [include/BS_thread_pool.hpp:28](include/BS_thread_pool.hpp#L28) [include/BS_thread_pool.hpp:35](include/BS_thread_pool.hpp#L35) [include/BS_thread_pool.hpp:43](include/BS_thread_pool.hpp#L43) [include/BS_thread_pool.hpp:51](include/BS_thread_pool.hpp#L51) |
| 全局模块片段 | global module fragment | [modules/BS.thread_pool.cppm:17-22](modules/BS.thread_pool.cppm#L17) |
| 模块接口单元 `export module` | module interface unit | [modules/BS.thread_pool.cppm:24](modules/BS.thread_pool.cppm#L24) |
| `import` 具名模块 | named module import | [tests/BS_thread_pool_test.cpp:79](tests/BS_thread_pool_test.cpp#L79) |
| `export namespace` 与 `using` 再导出 | exported namespace and using-declaration | [modules/BS.thread_pool.cppm:26-47](modules/BS.thread_pool.cppm#L26) |
| `import std`（C++23 标准库模块） | standard library module `import std` | [include/BS_thread_pool.hpp:69](include/BS_thread_pool.hpp#L69)；[tests/BS_thread_pool_test.cpp:26](tests/BS_thread_pool_test.cpp#L26) |

## 类型、对象与初始化

| 特性术语 | 英文术语 | 所在文件行号 |
| --- | --- | --- |
| 命名空间 | namespace | [include/BS_thread_pool.hpp:134](include/BS_thread_pool.hpp#L134) |
| 匿名命名空间 | unnamed namespace | [tests/BS_thread_pool_test.cpp:92](tests/BS_thread_pool_test.cpp#L92) |
| 类型别名 `using` | type alias | [include/BS_thread_pool.hpp:244](include/BS_thread_pool.hpp#L244) [include/BS_thread_pool.hpp:365](include/BS_thread_pool.hpp#L365) [include/BS_thread_pool.hpp:371](include/BS_thread_pool.hpp#L371)；[tests/BS_thread_pool_test.cpp:259](tests/BS_thread_pool_test.cpp#L259) |
| `auto` 类型推导 | `auto` type deduction | [tests/BS_thread_pool_test.cpp:891](tests/BS_thread_pool_test.cpp#L891) [tests/BS_thread_pool_test.cpp:1007](tests/BS_thread_pool_test.cpp#L1007) [tests/BS_thread_pool_test.cpp:1369](tests/BS_thread_pool_test.cpp#L1369) |
| `nullptr` | `nullptr` | [include/BS_thread_pool.hpp:358](include/BS_thread_pool.hpp#L358) [include/BS_thread_pool.hpp:1084](include/BS_thread_pool.hpp#L1084) [include/BS_thread_pool.hpp:2386](include/BS_thread_pool.hpp#L2386) |
| 定宽整数 | fixed-width integer types | [include/BS_thread_pool.hpp:145](include/BS_thread_pool.hpp#L145) [include/BS_thread_pool.hpp:244](include/BS_thread_pool.hpp#L244) [include/BS_thread_pool.hpp:392](include/BS_thread_pool.hpp#L392)；[tests/BS_thread_pool_test.cpp:269](tests/BS_thread_pool_test.cpp#L269) |
| std::size_t / std::ptrdiff_t | std::size_t / std::ptrdiff_t | [include/BS_thread_pool.hpp:493](include/BS_thread_pool.hpp#L493) [include/BS_thread_pool.hpp:1385](include/BS_thread_pool.hpp#L1385)；[tests/BS_thread_pool_test.cpp:121](tests/BS_thread_pool_test.cpp#L121) [tests/BS_thread_pool_test.cpp:146](tests/BS_thread_pool_test.cpp#L146) |
| 作用域枚举，指定底层类型 | scoped enumeration `enum class` with fixed underlying type | [include/BS_thread_pool.hpp:249](include/BS_thread_pool.hpp#L249)；[tests/BS_thread_pool_test.cpp:269](tests/BS_thread_pool_test.cpp#L269) |
| 非作用域枚举，指定底层类型 | unscoped enumeration with fixed underlying type | [include/BS_thread_pool.hpp:397](include/BS_thread_pool.hpp#L397) |
| 成员初始化列表 | member initializer list | [include/BS_thread_pool.hpp:145](include/BS_thread_pool.hpp#L145) [include/BS_thread_pool.hpp:323](include/BS_thread_pool.hpp#L323) [include/BS_thread_pool.hpp:417](include/BS_thread_pool.hpp#L417)；[tests/BS_thread_pool_test.cpp:132](tests/BS_thread_pool_test.cpp#L132) |
| 类内默认成员初始化 | default member initializer | [include/BS_thread_pool.hpp:358](include/BS_thread_pool.hpp#L358) [include/BS_thread_pool.hpp:439](include/BS_thread_pool.hpp#L439) [include/BS_thread_pool.hpp:640](include/BS_thread_pool.hpp#L640) [include/BS_thread_pool.hpp:2361](include/BS_thread_pool.hpp#L2361) [include/BS_thread_pool.hpp:2391](include/BS_thread_pool.hpp#L2391) |
| 委托构造函数 | delegating constructor | [include/BS_thread_pool.hpp:1457](include/BS_thread_pool.hpp#L1457) [include/BS_thread_pool.hpp:1464](include/BS_thread_pool.hpp#L1464) [include/BS_thread_pool.hpp:1472](include/BS_thread_pool.hpp#L1472) |
| 聚合初始化 | aggregate initialization | [include/BS_thread_pool.hpp:2169](include/BS_thread_pool.hpp#L2169) [include/BS_thread_pool.hpp:2204](include/BS_thread_pool.hpp#L2204) [include/BS_thread_pool.hpp:2237](include/BS_thread_pool.hpp#L2237)；[tests/BS_thread_pool_test.cpp:3304](tests/BS_thread_pool_test.cpp#L3304) [tests/BS_thread_pool_test.cpp:3348](tests/BS_thread_pool_test.cpp#L3348) |
| 列表初始化 | list initialization | [tests/BS_thread_pool_test.cpp:317-324](tests/BS_thread_pool_test.cpp#L317) |
| 值初始化 `{}` | value initialization | [include/BS_thread_pool.hpp:1104](include/BS_thread_pool.hpp#L1104) [include/BS_thread_pool.hpp:1154](include/BS_thread_pool.hpp#L1154) [include/BS_thread_pool.hpp:2391](include/BS_thread_pool.hpp#L2391) |
| 默认实参 | default arguments | [include/BS_thread_pool.hpp:417](include/BS_thread_pool.hpp#L417) [include/BS_thread_pool.hpp:1427](include/BS_thread_pool.hpp#L1427) [include/BS_thread_pool.hpp:1531](include/BS_thread_pool.hpp#L1531)；[tests/BS_thread_pool_test.cpp:170](tests/BS_thread_pool_test.cpp#L170) [tests/BS_thread_pool_test.cpp:427](tests/BS_thread_pool_test.cpp#L427) |
| 函数局部 `static` | function-local static | [tests/BS_thread_pool_test.cpp:557-558](tests/BS_thread_pool_test.cpp#L557) |
| `static_cast` | `static_cast` | [include/BS_thread_pool.hpp:276](include/BS_thread_pool.hpp#L276) [include/BS_thread_pool.hpp:290](include/BS_thread_pool.hpp#L290) [include/BS_thread_pool.hpp:588](include/BS_thread_pool.hpp#L588) [include/BS_thread_pool.hpp:920](include/BS_thread_pool.hpp#L920)；[tests/BS_thread_pool_test.cpp:337](tests/BS_thread_pool_test.cpp#L337) |
| 函数指针类型转换 | function pointer cast | [include/BS_thread_pool.hpp:2491](include/BS_thread_pool.hpp#L2491) [include/BS_thread_pool.hpp:2496](include/BS_thread_pool.hpp#L2496) |
| 整数后缀 `U` / `ULL` | integer literal suffixes | [include/BS_thread_pool.hpp:856](include/BS_thread_pool.hpp#L856) [include/BS_thread_pool.hpp:862](include/BS_thread_pool.hpp#L862) |
| 十六进制转义字符串 | hexadecimal escape in string literal | [tests/BS_thread_pool_test.cpp:3488](tests/BS_thread_pool_test.cpp#L3488) |

## 函数、运算符与属性

| 特性术语 | 英文术语 | 所在文件行号 |
| --- | --- | --- |
| `constexpr` 构造函数 / 函数 / 变量 | `constexpr` constructor, function, and variable | [include/BS_thread_pool.hpp:145](include/BS_thread_pool.hpp#L145) [include/BS_thread_pool.hpp:151](include/BS_thread_pool.hpp#L151) [include/BS_thread_pool.hpp:201](include/BS_thread_pool.hpp#L201) [include/BS_thread_pool.hpp:1434](include/BS_thread_pool.hpp#L1434)；[tests/BS_thread_pool_test.cpp:132](tests/BS_thread_pool_test.cpp#L132) [tests/BS_thread_pool_test.cpp:146](tests/BS_thread_pool_test.cpp#L146) [tests/BS_thread_pool_test.cpp:317](tests/BS_thread_pool_test.cpp#L317) |
| 内联变量 `inline` | inline variable | [include/BS_thread_pool.hpp:201](include/BS_thread_pool.hpp#L201) [include/BS_thread_pool.hpp:209](include/BS_thread_pool.hpp#L209) [include/BS_thread_pool.hpp:1347](include/BS_thread_pool.hpp#L1347)；[include/BS_thread_pool.hpp:2491](include/BS_thread_pool.hpp#L2491) |
| `static_assert` | `static_assert` | [include/BS_thread_pool.hpp:205](include/BS_thread_pool.hpp#L205) [include/BS_thread_pool.hpp:1447](include/BS_thread_pool.hpp#L1447)；[tests/BS_thread_pool_test.cpp:80-89](tests/BS_thread_pool_test.cpp#L80) [tests/BS_thread_pool_test.cpp:124](tests/BS_thread_pool_test.cpp#L124) |
| `noexcept` 说明符 | `noexcept` specifier | [include/BS_thread_pool.hpp:145](include/BS_thread_pool.hpp#L145) [include/BS_thread_pool.hpp:288](include/BS_thread_pool.hpp#L288) [include/BS_thread_pool.hpp:316](include/BS_thread_pool.hpp#L316) [include/BS_thread_pool.hpp:1495](include/BS_thread_pool.hpp#L1495) |
| 条件 `noexcept` | conditional `noexcept` | [include/BS_thread_pool.hpp:417](include/BS_thread_pool.hpp#L417) [include/BS_thread_pool.hpp:2125](include/BS_thread_pool.hpp#L2125) |
| 显式构造函数 `explicit` | `explicit` constructor | [include/BS_thread_pool.hpp:341](include/BS_thread_pool.hpp#L341) [include/BS_thread_pool.hpp:417](include/BS_thread_pool.hpp#L417) [include/BS_thread_pool.hpp:1464](include/BS_thread_pool.hpp#L1464)；[tests/BS_thread_pool_test.cpp:132](tests/BS_thread_pool_test.cpp#L132) |
| 默认实参与重载的调用运算符 | function call operator `operator()` | [include/BS_thread_pool.hpp:325](include/BS_thread_pool.hpp#L325) [include/BS_thread_pool.hpp:673](include/BS_thread_pool.hpp#L673) [include/BS_thread_pool.hpp:692](include/BS_thread_pool.hpp#L692) [include/BS_thread_pool.hpp:713](include/BS_thread_pool.hpp#L713) |
| 比较运算符重载 | comparison operator overloading | [include/BS_thread_pool.hpp:151-178](include/BS_thread_pool.hpp#L151) [include/BS_thread_pool.hpp:426](include/BS_thread_pool.hpp#L426) |
| 流插入运算符 | stream insertion operator `operator<<` | [include/BS_thread_pool.hpp:187](include/BS_thread_pool.hpp#L187)；[tests/BS_thread_pool_test.cpp:333](tests/BS_thread_pool_test.cpp#L333) [tests/BS_thread_pool_test.cpp:347](tests/BS_thread_pool_test.cpp#L347) |
| 位运算符重载（`& \| ^ ~` 及复合赋值） | bitwise and compound assignment operators | [include/BS_thread_pool.hpp:284-290](include/BS_thread_pool.hpp#L284) |
| 友元函数（隐藏友元） | hidden friend | [include/BS_thread_pool.hpp:151](include/BS_thread_pool.hpp#L151) [include/BS_thread_pool.hpp:187](include/BS_thread_pool.hpp#L187) [include/BS_thread_pool.hpp:426](include/BS_thread_pool.hpp#L426) |
| 友元类 | friend class | [include/BS_thread_pool.hpp:968-969](include/BS_thread_pool.hpp#L968) |
| 三路比较，默认生成 | three-way comparison `operator<=>` defaulted | [include/BS_thread_pool.hpp:149](include/BS_thread_pool.hpp#L149) |
| `std::strong_ordering` | `std::strong_ordering` | [include/BS_thread_pool.hpp:149](include/BS_thread_pool.hpp#L149) |
| 属性 `[[nodiscard]]` | attribute `[[nodiscard]]` | [include/BS_thread_pool.hpp:143](include/BS_thread_pool.hpp#L143) [include/BS_thread_pool.hpp:182](include/BS_thread_pool.hpp#L182) [include/BS_thread_pool.hpp:409](include/BS_thread_pool.hpp#L409) [include/BS_thread_pool.hpp:459](include/BS_thread_pool.hpp#L459) [include/BS_thread_pool.hpp:1428](include/BS_thread_pool.hpp#L1428) |
| 属性 `[[maybe_unused]]` | attribute `[[maybe_unused]]` | [include/BS_thread_pool.hpp:887](include/BS_thread_pool.hpp#L887) [include/BS_thread_pool.hpp:1048](include/BS_thread_pool.hpp#L1048) |
| 基于范围的 `for` | range-based `for` | [include/BS_thread_pool.hpp:474](include/BS_thread_pool.hpp#L474) [include/BS_thread_pool.hpp:1556](include/BS_thread_pool.hpp#L1556) [include/BS_thread_pool.hpp:2462](include/BS_thread_pool.hpp#L2462)；[tests/BS_thread_pool_test.cpp:353](tests/BS_thread_pool_test.cpp#L353) |
| `switch` | `switch` statement | [include/BS_thread_pool.hpp:924](include/BS_thread_pool.hpp#L924) [include/BS_thread_pool.hpp:1178](include/BS_thread_pool.hpp#L1178) [include/BS_thread_pool.hpp:1249](include/BS_thread_pool.hpp#L1249) |
| Lambda 表达式 | lambda expression | [include/BS_thread_pool.hpp:1457](include/BS_thread_pool.hpp#L1457) [include/BS_thread_pool.hpp:2303](include/BS_thread_pool.hpp#L2303) [include/BS_thread_pool.hpp:2361](include/BS_thread_pool.hpp#L2361)；[tests/BS_thread_pool_test.cpp:891](tests/BS_thread_pool_test.cpp#L891) [tests/BS_thread_pool_test.cpp:1375](tests/BS_thread_pool_test.cpp#L1375) |
| 按引用 / 按值捕获 | capture by reference and by copy | [include/BS_thread_pool.hpp:2089](include/BS_thread_pool.hpp#L2089)；[tests/BS_thread_pool_test.cpp:891](tests/BS_thread_pool_test.cpp#L891) [tests/BS_thread_pool_test.cpp:1466](tests/BS_thread_pool_test.cpp#L1466) |
| 初始化捕获 | init-capture | [include/BS_thread_pool.hpp:735](include/BS_thread_pool.hpp#L735) [include/BS_thread_pool.hpp:2072](include/BS_thread_pool.hpp#L2072)；[tests/BS_thread_pool_test.cpp:1007](tests/BS_thread_pool_test.cpp#L1007) |
| `mutable` lambda | `mutable` lambda | [include/BS_thread_pool.hpp:735](include/BS_thread_pool.hpp#L735) |
| 捕获 `this` | capturing `this` | [include/BS_thread_pool.hpp:2303](include/BS_thread_pool.hpp#L2303)；[tests/BS_thread_pool_test.cpp:158](tests/BS_thread_pool_test.cpp#L158) |

## 类与特殊成员函数

| 特性术语 | 英文术语 | 所在文件行号 |
| --- | --- | --- |
| `class` / `struct` | class and struct | [include/BS_thread_pool.hpp:143](include/BS_thread_pool.hpp#L143) [include/BS_thread_pool.hpp:312](include/BS_thread_pool.hpp#L312) [include/BS_thread_pool.hpp:459](include/BS_thread_pool.hpp#L459) [include/BS_thread_pool.hpp:1428](include/BS_thread_pool.hpp#L1428) [include/BS_thread_pool.hpp:2409](include/BS_thread_pool.hpp#L2409) |
| 访问控制 `public` / `private` | access specifiers | [include/BS_thread_pool.hpp:314](include/BS_thread_pool.hpp#L314) [include/BS_thread_pool.hpp:330](include/BS_thread_pool.hpp#L330) [include/BS_thread_pool.hpp:1430](include/BS_thread_pool.hpp#L1430) [include/BS_thread_pool.hpp:2052](include/BS_thread_pool.hpp#L2052) |
| 公有继承 | public inheritance | [include/BS_thread_pool.hpp:459](include/BS_thread_pool.hpp#L459) [include/BS_thread_pool.hpp:774](include/BS_thread_pool.hpp#L774) |
| 继承构造函数 | inheriting constructors | [include/BS_thread_pool.hpp:463](include/BS_thread_pool.hpp#L463) |
| 嵌套类 | nested class | [include/BS_thread_pool.hpp:331](include/BS_thread_pool.hpp#L331) [include/BS_thread_pool.hpp:338](include/BS_thread_pool.hpp#L338) |
| `virtual` 析构函数 | virtual destructor | [include/BS_thread_pool.hpp:333](include/BS_thread_pool.hpp#L333) |
| 纯虚函数 | pure virtual function | [include/BS_thread_pool.hpp:334](include/BS_thread_pool.hpp#L334) |
| `override` | `override` | [include/BS_thread_pool.hpp:343](include/BS_thread_pool.hpp#L343) |
| `final` | `final` | [include/BS_thread_pool.hpp:338](include/BS_thread_pool.hpp#L338) |
| 类型擦除 | type erasure | [include/BS_thread_pool.hpp:331-358](include/BS_thread_pool.hpp#L331) |
| 显式默认特殊成员 `= default` | explicitly defaulted special member functions | [include/BS_thread_pool.hpp:315-320](include/BS_thread_pool.hpp#L315)；[tests/BS_thread_pool_test.cpp:139](tests/BS_thread_pool_test.cpp#L139) [tests/BS_thread_pool_test.cpp:855](tests/BS_thread_pool_test.cpp#L855) |
| 显式删除特殊成员 `= delete` | explicitly deleted special member functions | [include/BS_thread_pool.hpp:318-319](include/BS_thread_pool.hpp#L318) [include/BS_thread_pool.hpp:1487-1490](include/BS_thread_pool.hpp#L1487)；[tests/BS_thread_pool_test.cpp:135-138](tests/BS_thread_pool_test.cpp#L135) |
| 仅可移动类型 | move-only type | [include/BS_thread_pool.hpp:316-319](include/BS_thread_pool.hpp#L316) |
| 右值引用 | rvalue reference | [include/BS_thread_pool.hpp:316](include/BS_thread_pool.hpp#L316) [include/BS_thread_pool.hpp:417](include/BS_thread_pool.hpp#L417) [include/BS_thread_pool.hpp:1472](include/BS_thread_pool.hpp#L1472) |
| 移动语义 `std::move` | move semantics | [include/BS_thread_pool.hpp:417](include/BS_thread_pool.hpp#L417) [include/BS_thread_pool.hpp:1559](include/BS_thread_pool.hpp#L1559) [include/BS_thread_pool.hpp:2255](include/BS_thread_pool.hpp#L2255) |
| `mutable` 数据成员 | `mutable` data member | [include/BS_thread_pool.hpp:434](include/BS_thread_pool.hpp#L434) [include/BS_thread_pool.hpp:2341](include/BS_thread_pool.hpp#L2341) [include/BS_thread_pool.hpp:2502](include/BS_thread_pool.hpp#L2502)；[tests/BS_thread_pool_test.cpp:243](tests/BS_thread_pool_test.cpp#L243) |
| `const` 成员函数 | const member function | [include/BS_thread_pool.hpp:182](include/BS_thread_pool.hpp#L182) [include/BS_thread_pool.hpp:493](include/BS_thread_pool.hpp#L493) [include/BS_thread_pool.hpp:509](include/BS_thread_pool.hpp#L509) |
| `static` 成员函数 | static member function | [include/BS_thread_pool.hpp:977](include/BS_thread_pool.hpp#L977) [include/BS_thread_pool.hpp:2125](include/BS_thread_pool.hpp#L2125)；[tests/BS_thread_pool_test.cpp:146](tests/BS_thread_pool_test.cpp#L146) [tests/BS_thread_pool_test.cpp:1344](tests/BS_thread_pool_test.cpp#L1344) |
| 成员函数指针 | pointer to member function | [tests/BS_thread_pool_test.cpp:1369](tests/BS_thread_pool_test.cpp#L1369) |
| 用户自定义拷贝 / 移动构造 | user-provided copy and move constructors | [tests/BS_thread_pool_test.cpp:857-859](tests/BS_thread_pool_test.cpp#L857) |

## 模板与编译期分支

| 特性术语 | 英文术语 | 所在文件行号 |
| --- | --- | --- |
| 函数模板 / 类模板 | function template / class template | [include/BS_thread_pool.hpp:312](include/BS_thread_pool.hpp#L312) [include/BS_thread_pool.hpp:458](include/BS_thread_pool.hpp#L458) [include/BS_thread_pool.hpp:534](include/BS_thread_pool.hpp#L534) [include/BS_thread_pool.hpp:1427](include/BS_thread_pool.hpp#L1427) [include/BS_thread_pool.hpp:2458](include/BS_thread_pool.hpp#L2458) |
| 成员函数模板 | member function template | [include/BS_thread_pool.hpp:322](include/BS_thread_pool.hpp#L322) [include/BS_thread_pool.hpp:1530](include/BS_thread_pool.hpp#L1530) [include/BS_thread_pool.hpp:2063](include/BS_thread_pool.hpp#L2063) |
| 变参模板 | variadic template | [include/BS_thread_pool.hpp:302](include/BS_thread_pool.hpp#L302) [include/BS_thread_pool.hpp:311](include/BS_thread_pool.hpp#L311) [include/BS_thread_pool.hpp:2426](include/BS_thread_pool.hpp#L2426) [include/BS_thread_pool.hpp:2458](include/BS_thread_pool.hpp#L2458)；[tests/BS_thread_pool_test.cpp:370](tests/BS_thread_pool_test.cpp#L370) [tests/BS_thread_pool_test.cpp:599](tests/BS_thread_pool_test.cpp#L599) |
| 形参包展开 | parameter pack expansion | [include/BS_thread_pool.hpp:327](include/BS_thread_pool.hpp#L327) [include/BS_thread_pool.hpp:2475](include/BS_thread_pool.hpp#L2475)；[tests/BS_thread_pool_test.cpp:374](tests/BS_thread_pool_test.cpp#L374) [tests/BS_thread_pool_test.cpp:388](tests/BS_thread_pool_test.cpp#L388) |
| 折叠表达式 | fold expression | [include/BS_thread_pool.hpp:2429](include/BS_thread_pool.hpp#L2429) [include/BS_thread_pool.hpp:2463](include/BS_thread_pool.hpp#L2463)；[tests/BS_thread_pool_test.cpp:602](tests/BS_thread_pool_test.cpp#L602) |
| 转发引用与完美转发 | forwarding reference and perfect forwarding | [include/BS_thread_pool.hpp:323](include/BS_thread_pool.hpp#L323) [include/BS_thread_pool.hpp:341](include/BS_thread_pool.hpp#L341) [include/BS_thread_pool.hpp:735](include/BS_thread_pool.hpp#L735) [include/BS_thread_pool.hpp:1472](include/BS_thread_pool.hpp#L1472) [include/BS_thread_pool.hpp:1632](include/BS_thread_pool.hpp#L1632)；[tests/BS_thread_pool_test.cpp:388](tests/BS_thread_pool_test.cpp#L388) |
| 非类型模板形参 | non-type template parameter | [include/BS_thread_pool.hpp:293](include/BS_thread_pool.hpp#L293) [include/BS_thread_pool.hpp:1427](include/BS_thread_pool.hpp#L1427) [include/BS_thread_pool.hpp:2157](include/BS_thread_pool.hpp#L2157)；[tests/BS_thread_pool_test.cpp:121](tests/BS_thread_pool_test.cpp#L121) |
| 默认模板实参 | default template argument | [include/BS_thread_pool.hpp:1427](include/BS_thread_pool.hpp#L1427) [include/BS_thread_pool.hpp:2157](include/BS_thread_pool.hpp#L2157)；[tests/BS_thread_pool_test.cpp:121](tests/BS_thread_pool_test.cpp#L121) |
| 类模板前向声明 | class template forward declaration | [include/BS_thread_pool.hpp:293-294](include/BS_thread_pool.hpp#L293) [include/BS_thread_pool.hpp:302-303](include/BS_thread_pool.hpp#L302) |
| 类模板偏特化 | partial specialization | [include/BS_thread_pool.hpp:312](include/BS_thread_pool.hpp#L312) [include/BS_thread_pool.hpp:1367](include/BS_thread_pool.hpp#L1367) [include/BS_thread_pool.hpp:1374](include/BS_thread_pool.hpp#L1374) [include/BS_thread_pool.hpp:1381](include/BS_thread_pool.hpp#L1381) |
| 别名模板 | alias template | [include/BS_thread_pool.hpp:1400](include/BS_thread_pool.hpp#L1400) |
| SFINAE 与 `std::enable_if_t` | SFINAE with `std::enable_if_t` | [include/BS_thread_pool.hpp:322](include/BS_thread_pool.hpp#L322) [include/BS_thread_pool.hpp:340](include/BS_thread_pool.hpp#L340) [include/BS_thread_pool.hpp:449-450](include/BS_thread_pool.hpp#L449) [include/BS_thread_pool.hpp:730](include/BS_thread_pool.hpp#L730) [include/BS_thread_pool.hpp:1367](include/BS_thread_pool.hpp#L1367) |
| 概念 `concept` | concept | [include/BS_thread_pool.hpp:445-446](include/BS_thread_pool.hpp#L445) |
| `requires` 子句 | requires-clause | [include/BS_thread_pool.hpp:444](include/BS_thread_pool.hpp#L444) |
| 标准概念 `std::invocable` | standard concept `std::invocable` | [include/BS_thread_pool.hpp:446](include/BS_thread_pool.hpp#L446) |
| `if constexpr` | `if constexpr` | [include/BS_thread_pool.hpp:345](include/BS_thread_pool.hpp#L345) [include/BS_thread_pool.hpp:472](include/BS_thread_pool.hpp#L472) [include/BS_thread_pool.hpp:741](include/BS_thread_pool.hpp#L741) [include/BS_thread_pool.hpp:1552](include/BS_thread_pool.hpp#L1552) [include/BS_thread_pool.hpp:1631](include/BS_thread_pool.hpp#L1631) [include/BS_thread_pool.hpp:1700](include/BS_thread_pool.hpp#L1700) [include/BS_thread_pool.hpp:2066](include/BS_thread_pool.hpp#L2066) |
| 类型特征 `_v` / `_t` | type trait variable templates and alias templates | [include/BS_thread_pool.hpp:276](include/BS_thread_pool.hpp#L276) [include/BS_thread_pool.hpp:322](include/BS_thread_pool.hpp#L322) [include/BS_thread_pool.hpp:345](include/BS_thread_pool.hpp#L345) [include/BS_thread_pool.hpp:470](include/BS_thread_pool.hpp#L470) [include/BS_thread_pool.hpp:1362](include/BS_thread_pool.hpp#L1362) [include/BS_thread_pool.hpp:1367](include/BS_thread_pool.hpp#L1367) [include/BS_thread_pool.hpp:2066](include/BS_thread_pool.hpp#L2066) |
| std::invoke | std::invoke | [include/BS_thread_pool.hpp:347](include/BS_thread_pool.hpp#L347) [include/BS_thread_pool.hpp:351](include/BS_thread_pool.hpp#L351) |
| std::conditional_t 选择成员类型 | std::conditional_t for member type selection | [include/BS_thread_pool.hpp:470](include/BS_thread_pool.hpp#L470) [include/BS_thread_pool.hpp:2371](include/BS_thread_pool.hpp#L2371) [include/BS_thread_pool.hpp:2391](include/BS_thread_pool.hpp#L2391) |

## 异常

| 特性术语 | 英文术语 | 所在文件行号 |
| --- | --- | --- |
| `try` / `catch` | `try` / `catch` | [include/BS_thread_pool.hpp:738-761](include/BS_thread_pool.hpp#L738) [include/BS_thread_pool.hpp:1498-1509](include/BS_thread_pool.hpp#L1498) [include/BS_thread_pool.hpp:2317-2326](include/BS_thread_pool.hpp#L2317) |
| 捕获所有异常 `catch (...)` | catch-all handler | [include/BS_thread_pool.hpp:752](include/BS_thread_pool.hpp#L752) [include/BS_thread_pool.hpp:758](include/BS_thread_pool.hpp#L758) [include/BS_thread_pool.hpp:1507](include/BS_thread_pool.hpp#L1507) [include/BS_thread_pool.hpp:2323](include/BS_thread_pool.hpp#L2323) |
| 自定义异常类 | user-defined exception type | [include/BS_thread_pool.hpp:774](include/BS_thread_pool.hpp#L774) |
| `std::runtime_error` | `std::runtime_error` | [include/BS_thread_pool.hpp:774-776](include/BS_thread_pool.hpp#L774) |
| `std::exception_ptr` 与 `std::current_exception` | `std::exception_ptr` and `std::current_exception` | [include/BS_thread_pool.hpp:756](include/BS_thread_pool.hpp#L756) |
| `std::promise::set_exception` / `set_value` | promise exception and value propagation | [include/BS_thread_pool.hpp:744](include/BS_thread_pool.hpp#L744) [include/BS_thread_pool.hpp:748](include/BS_thread_pool.hpp#L748) [include/BS_thread_pool.hpp:756](include/BS_thread_pool.hpp#L756) |

## 并发与同步

| 特性术语 | 英文术语 | 所在文件行号 |
| --- | --- | --- |
| `std::thread` | `std::thread` | [include/BS_thread_pool.hpp:381](include/BS_thread_pool.hpp#L381) |
| `std::jthread`（C++20） | `std::jthread` | [include/BS_thread_pool.hpp:371](include/BS_thread_pool.hpp#L371) |
| `std::stop_token`（C++20） | `std::stop_token` | [include/BS_thread_pool.hpp:373](include/BS_thread_pool.hpp#L373) [include/BS_thread_pool.hpp:375](include/BS_thread_pool.hpp#L375) [include/BS_thread_pool.hpp:2091](include/BS_thread_pool.hpp#L2091) |
| `hardware_concurrency` | `std::thread::hardware_concurrency` | [include/BS_thread_pool.hpp:2137-2138](include/BS_thread_pool.hpp#L2137)；[tests/BS_thread_pool_test.cpp:817](tests/BS_thread_pool_test.cpp#L817) |
| `native_handle` | `native_handle` | [include/BS_thread_pool.hpp:1645-1649](include/BS_thread_pool.hpp#L1645) |
| `std::thread::join` | `join` | [include/BS_thread_pool.hpp:2116](include/BS_thread_pool.hpp#L2116) |
| `std::mutex` | `std::mutex` | [include/BS_thread_pool.hpp:2341](include/BS_thread_pool.hpp#L2341) [include/BS_thread_pool.hpp:2502](include/BS_thread_pool.hpp#L2502)；[tests/BS_thread_pool_test.cpp:243](tests/BS_thread_pool_test.cpp#L243) |
| `std::scoped_lock` 与 CTAD | `std::scoped_lock` and class template argument deduction | [include/BS_thread_pool.hpp:1551](include/BS_thread_pool.hpp#L1551) [include/BS_thread_pool.hpp:1630](include/BS_thread_pool.hpp#L1630) [include/BS_thread_pool.hpp:2080](include/BS_thread_pool.hpp#L2080) [include/BS_thread_pool.hpp:2461](include/BS_thread_pool.hpp#L2461)；[tests/BS_thread_pool_test.cpp:173](tests/BS_thread_pool_test.cpp#L173) [tests/BS_thread_pool_test.cpp:186](tests/BS_thread_pool_test.cpp#L186) |
| `std::unique_lock` 与 CTAD | `std::unique_lock` and CTAD | [include/BS_thread_pool.hpp:2290](include/BS_thread_pool.hpp#L2290)；[tests/BS_thread_pool_test.cpp:156](tests/BS_thread_pool_test.cpp#L156) [tests/BS_thread_pool_test.cpp:206](tests/BS_thread_pool_test.cpp#L206) |
| `std::condition_variable` | `std::condition_variable` | [include/BS_thread_pool.hpp:2349](include/BS_thread_pool.hpp#L2349) [include/BS_thread_pool.hpp:2356](include/BS_thread_pool.hpp#L2356)；[tests/BS_thread_pool_test.cpp:248](tests/BS_thread_pool_test.cpp#L248) |
| `std::condition_variable_any` | `std::condition_variable_any` | [include/BS_thread_pool.hpp:2347](include/BS_thread_pool.hpp#L2347) |
| 带谓词的 `wait` / `wait_for` / `wait_until` | condition variable wait with predicate | [include/BS_thread_pool.hpp:2302-2309](include/BS_thread_pool.hpp#L2302)；[tests/BS_thread_pool_test.cpp:157](tests/BS_thread_pool_test.cpp#L157) [tests/BS_thread_pool_test.cpp:207](tests/BS_thread_pool_test.cpp#L207) [tests/BS_thread_pool_test.cpp:229](tests/BS_thread_pool_test.cpp#L229) |
| `notify_one` / `notify_all` | `notify_one` / `notify_all` | [include/BS_thread_pool.hpp:1565](include/BS_thread_pool.hpp#L1565) [include/BS_thread_pool.hpp:1636](include/BS_thread_pool.hpp#L1636) [include/BS_thread_pool.hpp:2114](include/BS_thread_pool.hpp#L2114) [include/BS_thread_pool.hpp:2295](include/BS_thread_pool.hpp#L2295)；[tests/BS_thread_pool_test.cpp:176](tests/BS_thread_pool_test.cpp#L176) |
| `std::atomic` | `std::atomic` | [tests/BS_thread_pool_test.cpp:790](tests/BS_thread_pool_test.cpp#L790) [tests/BS_thread_pool_test.cpp:1327](tests/BS_thread_pool_test.cpp#L1327) [tests/BS_thread_pool_test.cpp:1432](tests/BS_thread_pool_test.cpp#L1432) [tests/BS_thread_pool_test.cpp:2412](tests/BS_thread_pool_test.cpp#L2412) |
| `std::future` / `std::promise` | `std::future` / `std::promise` | [include/BS_thread_pool.hpp:459](include/BS_thread_pool.hpp#L459) [include/BS_thread_pool.hpp:733-766](include/BS_thread_pool.hpp#L733)；[tests/BS_thread_pool_test.cpp:970](tests/BS_thread_pool_test.cpp#L970) |
| `std::future_status` | `std::future_status` | [include/BS_thread_pool.hpp:498](include/BS_thread_pool.hpp#L498) |
| `future::get` / `wait` / `wait_for` / `wait_until` | future waiting and result retrieval | [include/BS_thread_pool.hpp:475](include/BS_thread_pool.hpp#L475) [include/BS_thread_pool.hpp:498](include/BS_thread_pool.hpp#L498) [include/BS_thread_pool.hpp:523](include/BS_thread_pool.hpp#L523) [include/BS_thread_pool.hpp:540](include/BS_thread_pool.hpp#L540) [include/BS_thread_pool.hpp:560](include/BS_thread_pool.hpp#L560) |
| `std::counting_semaphore` / `std::binary_semaphore`（C++20） | counting and binary semaphore | [tests/BS_thread_pool_test.cpp:113-114](tests/BS_thread_pool_test.cpp#L113) [tests/BS_thread_pool_test.cpp:121](tests/BS_thread_pool_test.cpp#L121) [tests/BS_thread_pool_test.cpp:259](tests/BS_thread_pool_test.cpp#L259) [tests/BS_thread_pool_test.cpp:1083](tests/BS_thread_pool_test.cpp#L1083) |
| `thread_local` | `thread_local` | [include/BS_thread_pool.hpp:1347-1348](include/BS_thread_pool.hpp#L1347) |
| `inline static thread_local` | inline static thread-local variable | [include/BS_thread_pool.hpp:1347-1348](include/BS_thread_pool.hpp#L1347) |

## 标准库组件

| 特性术语 | 英文术语 | 所在文件行号 |
| --- | --- | --- |
| `std::move_only_function`（C++23） | `std::move_only_function` | [include/BS_thread_pool.hpp:300](include/BS_thread_pool.hpp#L300)；[tests/BS_thread_pool_test.cpp:1394](tests/BS_thread_pool_test.cpp#L1394) |
| `std::function` | `std::function` | [tests/BS_thread_pool_test.cpp:1384](tests/BS_thread_pool_test.cpp#L1384) |
| `std::bind` / `std::ref` | `std::bind` / `std::ref` | [tests/BS_thread_pool_test.cpp:1410](tests/BS_thread_pool_test.cpp#L1410) |
| `std::optional` / `std::nullopt` | `std::optional` / `std::nullopt` | [include/BS_thread_pool.hpp:842](include/BS_thread_pool.hpp#L842) [include/BS_thread_pool.hpp:848](include/BS_thread_pool.hpp#L848) [include/BS_thread_pool.hpp:913](include/BS_thread_pool.hpp#L913) [include/BS_thread_pool.hpp:977](include/BS_thread_pool.hpp#L977) [include/BS_thread_pool.hpp:1248](include/BS_thread_pool.hpp#L1248) [include/BS_thread_pool.hpp:1347](include/BS_thread_pool.hpp#L1347) [include/BS_thread_pool.hpp:2130](include/BS_thread_pool.hpp#L2130) |
| `optional::has_value` / `value` | `optional::has_value` / `value` | [include/BS_thread_pool.hpp:1294-1295](include/BS_thread_pool.hpp#L1294) [include/BS_thread_pool.hpp:2131](include/BS_thread_pool.hpp#L2131) |
| `std::monostate` | `std::monostate` | [include/BS_thread_pool.hpp:2391](include/BS_thread_pool.hpp#L2391) |
| `std::tuple` 与 CTAD | `std::tuple` and CTAD | [include/BS_thread_pool.hpp:153](include/BS_thread_pool.hpp#L153) [include/BS_thread_pool.hpp:163](include/BS_thread_pool.hpp#L163) [include/BS_thread_pool.hpp:173](include/BS_thread_pool.hpp#L173) |
| `std::pair` | `std::pair` | [tests/BS_thread_pool_test.cpp:572](tests/BS_thread_pool_test.cpp#L572) [tests/BS_thread_pool_test.cpp:1669](tests/BS_thread_pool_test.cpp#L1669) |
| `std::string` / `std::wstring` / `std::to_string` | `std::string` / `std::wstring` / `std::to_string` | [include/BS_thread_pool.hpp:182-184](include/BS_thread_pool.hpp#L182) [include/BS_thread_pool.hpp:1090](include/BS_thread_pool.hpp#L1090) [include/BS_thread_pool.hpp:1124](include/BS_thread_pool.hpp#L1124) |
| `std::string_view`（C++17） | `std::string_view` | [tests/BS_thread_pool_test.cpp:427](tests/BS_thread_pool_test.cpp#L427) [tests/BS_thread_pool_test.cpp:441](tests/BS_thread_pool_test.cpp#L441) [tests/BS_thread_pool_test.cpp:885](tests/BS_thread_pool_test.cpp#L885) [tests/BS_thread_pool_test.cpp:2308](tests/BS_thread_pool_test.cpp#L2308) |
| `std::initializer_list` | `std::initializer_list` | [tests/BS_thread_pool_test.cpp:317](tests/BS_thread_pool_test.cpp#L317) [tests/BS_thread_pool_test.cpp:347](tests/BS_thread_pool_test.cpp#L347) [tests/BS_thread_pool_test.cpp:399](tests/BS_thread_pool_test.cpp#L399) |
| `std::vector` / `std::vector<bool>` | `std::vector` / `std::vector<bool>` | [include/BS_thread_pool.hpp:459](include/BS_thread_pool.hpp#L459) [include/BS_thread_pool.hpp:480](include/BS_thread_pool.hpp#L480) [include/BS_thread_pool.hpp:842](include/BS_thread_pool.hpp#L842) [include/BS_thread_pool.hpp:1647](include/BS_thread_pool.hpp#L1647) [include/BS_thread_pool.hpp:2201](include/BS_thread_pool.hpp#L2201) |
| `std::queue` / `std::priority_queue` | `std::queue` / `std::priority_queue` | [include/BS_thread_pool.hpp:2371](include/BS_thread_pool.hpp#L2371) |
| `std::array` | `std::array` | [tests/BS_thread_pool_test.cpp:3304](tests/BS_thread_pool_test.cpp#L3304) [tests/BS_thread_pool_test.cpp:3488](tests/BS_thread_pool_test.cpp#L3488) |
| `std::map` | `std::map` | [tests/BS_thread_pool_test.cpp:2794](tests/BS_thread_pool_test.cpp#L2794) [tests/BS_thread_pool_test.cpp:3967](tests/BS_thread_pool_test.cpp#L3967) |
| `std::unique_ptr` / `std::make_unique` | `std::unique_ptr` / `std::make_unique` | [include/BS_thread_pool.hpp:323](include/BS_thread_pool.hpp#L323) [include/BS_thread_pool.hpp:358](include/BS_thread_pool.hpp#L358) [include/BS_thread_pool.hpp:2078](include/BS_thread_pool.hpp#L2078) [include/BS_thread_pool.hpp:2386](include/BS_thread_pool.hpp#L2386) |
| `unique_ptr` 管理数组 | `unique_ptr` to array | [include/BS_thread_pool.hpp:2078](include/BS_thread_pool.hpp#L2078) [include/BS_thread_pool.hpp:2386](include/BS_thread_pool.hpp#L2386) |
| `std::shared_ptr` / `std::make_shared` | `std::shared_ptr` / `std::make_shared` | [include/BS_thread_pool.hpp:678](include/BS_thread_pool.hpp#L678) [include/BS_thread_pool.hpp:2163](include/BS_thread_pool.hpp#L2163) [include/BS_thread_pool.hpp:2198](include/BS_thread_pool.hpp#L2198) [include/BS_thread_pool.hpp:2233](include/BS_thread_pool.hpp#L2233) |
| `std::begin` / `std::end` | `std::begin` / `std::end` | [include/BS_thread_pool.hpp:1579](include/BS_thread_pool.hpp#L1579) |
| `std::min` / `std::count` / `std::remove` | `std::min` / `std::count` / `std::remove` | [include/BS_thread_pool.hpp:589](include/BS_thread_pool.hpp#L589) [include/BS_thread_pool.hpp:891](include/BS_thread_pool.hpp#L891) [include/BS_thread_pool.hpp:2133](include/BS_thread_pool.hpp#L2133) [include/BS_thread_pool.hpp:2485](include/BS_thread_pool.hpp#L2485) |
| `std::sort` / `std::shuffle` | `std::sort` / `std::shuffle` | [tests/BS_thread_pool_test.cpp:804](tests/BS_thread_pool_test.cpp#L804) [tests/BS_thread_pool_test.cpp:2297](tests/BS_thread_pool_test.cpp#L2297) |
| erase-remove 惯用法 | erase-remove idiom | [include/BS_thread_pool.hpp:2485](include/BS_thread_pool.hpp#L2485) |
| `std::chrono::duration` / `time_point` | `std::chrono::duration` / `time_point` | [include/BS_thread_pool.hpp:498](include/BS_thread_pool.hpp#L498) [include/BS_thread_pool.hpp:535](include/BS_thread_pool.hpp#L535) [include/BS_thread_pool.hpp:537](include/BS_thread_pool.hpp#L537) [include/BS_thread_pool.hpp:556](include/BS_thread_pool.hpp#L556)；[tests/BS_thread_pool_test.cpp:204](tests/BS_thread_pool_test.cpp#L204) [tests/BS_thread_pool_test.cpp:226](tests/BS_thread_pool_test.cpp#L226) |
| `steady_clock` / `system_clock` | `steady_clock` / `system_clock` | [include/BS_thread_pool.hpp:537](include/BS_thread_pool.hpp#L537)；[tests/BS_thread_pool_test.cpp:3836](tests/BS_thread_pool_test.cpp#L3836) |
| `std::ratio`（作为 duration 形参） | `std::ratio` as duration period | [include/BS_thread_pool.hpp:534-535](include/BS_thread_pool.hpp#L534)；[tests/BS_thread_pool_test.cpp:203](tests/BS_thread_pool_test.cpp#L203) |
| `time_point_cast` | `std::chrono::time_point_cast` | [tests/BS_thread_pool_test.cpp:3836](tests/BS_thread_pool_test.cpp#L3836) |
| `std::format`（C++20，含 chrono 格式） | `std::format` | [tests/BS_thread_pool_test.cpp:3836](tests/BS_thread_pool_test.cpp#L3836) |
| `std::bit_width`（C++20） | `std::bit_width` | [include/BS_thread_pool.hpp:850](include/BS_thread_pool.hpp#L850) [include/BS_thread_pool.hpp:1011](include/BS_thread_pool.hpp#L1011) |
| `std::ostream` / `std::cout` / `std::endl` / `std::flush` | iostream | [include/BS_thread_pool.hpp:187](include/BS_thread_pool.hpp#L187) [include/BS_thread_pool.hpp:2415](include/BS_thread_pool.hpp#L2415) [include/BS_thread_pool.hpp:2491](include/BS_thread_pool.hpp#L2491) [include/BS_thread_pool.hpp:2496](include/BS_thread_pool.hpp#L2496) |
| `std::setw` | `std::setw` | [tests/BS_thread_pool_test.cpp:697](tests/BS_thread_pool_test.cpp#L697) |
| `std::ofstream` 与 `std::ios::binary` | `std::ofstream` and binary open mode | [tests/BS_thread_pool_test.cpp:3368](tests/BS_thread_pool_test.cpp#L3368) [tests/BS_thread_pool_test.cpp:4041](tests/BS_thread_pool_test.cpp#L4041) |
| `std::complex` | `std::complex` | [tests/BS_thread_pool_test.cpp:3247-3251](tests/BS_thread_pool_test.cpp#L3247) [tests/BS_thread_pool_test.cpp:3348](tests/BS_thread_pool_test.cpp#L3348) |
| `std::numeric_limits` | `std::numeric_limits` | [tests/BS_thread_pool_test.cpp:121](tests/BS_thread_pool_test.cpp#L121) |
| `<random>`：`random_device`、`mt19937_64`、`uniform_int_distribution` | random number facilities | [tests/BS_thread_pool_test.cpp:557-559](tests/BS_thread_pool_test.cpp#L557) [tests/BS_thread_pool_test.cpp:2297](tests/BS_thread_pool_test.cpp#L2297) |
