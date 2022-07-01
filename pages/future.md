> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [zh.cppreference.com](https://zh.cppreference.com/w/cpp/thread/future)

> 定义于头文件

> （通过 [std::async](https://zh.cppreference.com/w/cpp/thread/async "cpp/thread/async") 、 [std::packaged_task](https://zh.cppreference.com/w/cpp/thread/packaged_task "cpp/thread/packaged task") 或 [std::promise](https://zh.cppreference.com/w/cpp/thread/promise "cpp/thread/promise") 创建的）异步操作能提供一个 `std::future` 对象给该异步操作的创建者。

> 然后，异步操作的创建者能用各种方法查询、等待或从 `std::future` 提取值。若异步操作仍未提供值，则这些方法可能阻塞。

> 准备

> *   异步操作准备好发送结果给创建者时，它能通过修改链接到创建者的 `std::future` 的_共享状态_（例如 [std::promise::set_value](https://zh.cppreference.com/w/cpp/thread/promise/set_value "cpp/thread/promise/set value") ）进行。

> 状态

> 注意， `std::future` 所引用的共享状态不与另一异步返回对象共享（与 [std::shared_future](https://zh.cppreference.com/w/cpp/thread/shared_future "cpp/thread/shared future") 相反）。

> <table><tbody><tr><td><p><a href="https://zh.cppreference.com/w/cpp/thread/future/future" title="cpp/thread/future/future">(构造函数)</a></p></td><td>构造 future 对象<br>(公开成员函数) <a rel="nofollow" href="https://zh.cppreference.com/mwiki/index.php?title=Template:cpp/thread/future/dsc_constructor&amp;action=edit">[编辑]</a></td></tr><tr><td><p><a href="https://zh.cppreference.com/w/cpp/thread/future/~future" title="cpp/thread/future/~future">(析构函数)</a></p></td><td>析构 future 对象<br>(公开成员函数) <a rel="nofollow" href="https://zh.cppreference.com/mwiki/index.php?title=Template:cpp/thread/future/dsc_destructor&amp;action=edit">[编辑]</a></td></tr><tr><td><p><a href="https://zh.cppreference.com/w/cpp/thread/future/operator%3D" title="cpp/thread/future/operator=">operator=</a></p></td><td>移动 future 对象<br>(公开成员函数) <a rel="nofollow" href="https://zh.cppreference.com/mwiki/index.php?title=Template:cpp/thread/future/dsc_operator%3D&amp;action=edit">[编辑]</a></td></tr><tr><td><p><a href="https://zh.cppreference.com/w/cpp/thread/future/share" title="cpp/thread/future/share">share</a></p></td><td>从 <code>*this</code> 转移共享状态给 <a href="https://zh.cppreference.com/w/cpp/thread/shared_future" title="cpp/thread/shared future"><tt>shared_future</tt></a> 并返回它<br>(公开成员函数) <a rel="nofollow" href="https://zh.cppreference.com/mwiki/index.php?title=Template:cpp/thread/future/dsc_share&amp;action=edit">[编辑]</a></td></tr><tr><td colspan="2"><h5>获取结果</h5></td></tr><tr><td><p><a href="https://zh.cppreference.com/w/cpp/thread/future/get" title="cpp/thread/future/get">get</a></p></td><td>返回结果<br>(公开成员函数) <a rel="nofollow" href="https://zh.cppreference.com/mwiki/index.php?title=Template:cpp/thread/future/dsc_get&amp;action=edit">[编辑]</a></td></tr><tr><td colspan="2"><h5>状态</h5></td></tr><tr><td><p><a href="https://zh.cppreference.com/w/cpp/thread/future/valid" title="cpp/thread/future/valid">valid</a></p></td><td>检查 future 是否拥有共享状态<br>(公开成员函数) <a rel="nofollow" href="https://zh.cppreference.com/mwiki/index.php?title=Template:cpp/thread/future/dsc_valid&amp;action=edit">[编辑]</a></td></tr><tr><td><p><a href="https://zh.cppreference.com/w/cpp/thread/future/wait" title="cpp/thread/future/wait">wait</a></p></td><td>等待结果变得可用<br>(公开成员函数) <a rel="nofollow" href="https://zh.cppreference.com/mwiki/index.php?title=Template:cpp/thread/future/dsc_wait&amp;action=edit">[编辑]</a></td></tr><tr><td><p><a href="https://zh.cppreference.com/w/cpp/thread/future/wait_for" title="cpp/thread/future/wait for">wait_for</a></p></td><td>等待结果，如果在指定的超时间隔后仍然无法得到结果，则返回。<br>(公开成员函数) <a rel="nofollow" href="https://zh.cppreference.com/mwiki/index.php?title=Template:cpp/thread/future/dsc_wait_for&amp;action=edit">[编辑]</a></td></tr><tr><td><p><a href="https://zh.cppreference.com/w/cpp/thread/future/wait_until" title="cpp/thread/future/wait until">wait_until</a></p></td><td>等待结果，如果在已经到达指定的时间点时仍然无法得到结果，则返回。<br>(公开成员函数) <a rel="nofollow" href="https://zh.cppreference.com/mwiki/index.php?title=Template:cpp/thread/future/dsc_wait_until&amp;action=edit">[编辑]</a></td></tr></tbody></table>

> ```cpp
> #include <iostream>
> #include <future>
> #include <thread>
>  
> int main()
> {
>     // 来自 packaged_task 的 future
>     std::packaged_task<int()> task([](){ return 7; }); // 包装函数
>     std::future<int> f1 = task.get_future();  // 获取 future
>     std::thread(std::move(task)).detach(); // 在线程上运行
>  
>     // 来自 async() 的 future
>     std::future<int> f2 = std::async(std::launch::async, [](){ return 8; });
>  
>     // 来自 promise 的 future
>     std::promise<int> p;
>     std::future<int> f3 = p.get_future();
>     std::thread( [&p]{ p.set_value_at_thread_exit(9); }).detach();
>  
>     std::cout << "Waiting..." << std::flush;
>     f1.wait();
>     f2.wait();
>     f3.wait();
>     std::cout << "Done!\nResults are: "
>               << f1.get() << ' ' << f2.get() << ' ' << f3.get() << '\n';
> }
> ```