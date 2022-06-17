# ZCU106_AURORA
在ZCU106上使用AURORA协议通过光纤接口通信
*** *** ***
从ZCU106的原理图上可以看到SFP的P1连接到BANK225，坐标是X0Y10；对应的发管脚是Y4Y3,收管脚是AA2AA1
坐标的查询方法：UG1075 p56图1-17上可以看到ZU7EV一个有5组bank223-X0Y0toX0Y3,bank224-X0Y4toX0Y7,bank225,X0Y8toX0Y11,bank226-X0Y12tox0y15,bank227-X0Y16toX0Y19,具体每对管脚对应X0Y几请看原理图，特别直观。

目前自己看文档，发现BANK225的REFCLK貌似有问题，但是有人调通了：
https://blog.csdn.net/quhai1340/article/details/107400764
还不清楚是如何修改时钟管脚的。
虽然不清楚但是，只要选择BANK225 的参考1时钟，频率设置为156.25MHz，并且，真正的输入时钟使用BANK226的REFCLK1(U9,U10)，即在XDC里加入如下约束：
set_property PACKAGE_PIN U10 [get_ports GTHQ0_P],
GTHQ0_N 并没有设置，应该是IP核自己进行了设置。
这样就可以进行通信，不需要关心IP内部怎么连接。
注意:xdc中，请给init_pma 和 reset 分配管脚，因为zcu106的switch的不按低电平，按高电平，所以他俩绑定在switch上就行。
