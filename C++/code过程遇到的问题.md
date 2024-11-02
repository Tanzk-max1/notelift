![[Pasted image 20241102191744.png]]
报错：你尝试在没有对象实例的情况下调用了一个非静态成员函数
因为triggered 这个函数     `QAction::triggered()`   信号连接到一个槽函数，从而定义该动作触发时执行的操作。正常的情况下连接connect然后是不需要加上括号的，所以正确的应该是
![[Pasted image 20241102191957.png]]