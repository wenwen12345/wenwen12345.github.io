> 函数模板 `async` 异步地运行函数 `f` （潜在地在可能是线程池一部分的分离线程中），并返回最终将保有该函数调用结果的 [std::future](https://zh.cppreference.com/w/cpp/thread/future "cpp/thread/future") 。

> 任何情况下，对 `std::async` 的调用_同步于_（定义于 [std::memory_order](https://zh.cppreference.com/w/cpp/atomic/memory_order "cpp/atomic/memory order") ）对 `f` 的调用，且 `f` 的完成_先序于_令共享状态就绪。若选择 `async` 策略，则关联线程的完成_同步于_首个等待于共享状态上的函数的成功返回，或最后一个释放共享状态的函数的返回，两者的先到来者。若 [std::decay](http://zh.cppreference.com/w/cpp/types/decay)<Function>::type 或 [std::decay](http://zh.cppreference.com/w/cpp/types/decay)<Args>::type 中的每个类型不能从其对应的实参构造，则程序为谬构。

> 指代此次调用 `std::async` 所创建的共享状态的 [std::future](https://zh.cppreference.com/w/cpp/thread/future "cpp/thread/future") 。

> 若运行策略等于 [std::launch::async](https://zh.cppreference.com/w/cpp/thread/launch "cpp/thread/launch") 且实现无法开始新线程（该情况下，若运行策略为 `async|deferred` 或设置了额外位，则它将回退到 deferred 或实现定义的策略），则抛出以 [std::errc::resource_unavailable_try_again](https://zh.cppreference.com/w/cpp/error/errc "cpp/error/errc") 为错误条件的 [std::system_error](https://zh.cppreference.com/w/cpp/error/system_error "cpp/error/system error") ，或者若无法分配内部数据结构所用的内存，则为 [std::bad_alloc](https://zh.cppreference.com/w/cpp/memory/new/bad_alloc "cpp/memory/new/bad alloc") 。

> 实现可以通过在默认运行策略中启用额外（实现定义的）位，扩展 **std::async** 第一重载的行为。

> 实现定义的运行策略的例子是同步策略（在 async 调用内立即执行）和任务策略（类似 async ，但不清理线程局域对象）。

> 若从 `std::async` 获得的 `std::future` 未被移动或绑定到引用，则在完整表达式结尾， [std::future](https://zh.cppreference.com/w/cpp/thread/future "cpp/thread/future") 的析构函数将阻塞直至异步计算完成，实质上令如下代码同步：

> (注意，以调用 std::async 以外的方式获得的 std::future 的析构函数决不阻塞)