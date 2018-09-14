activity一共有四种launcherModel:
standard
singleTop
singleTask
singleInstance

配置方法：
直接在AndroidMainfest.xml配置的android:launchMode属性。

standard:
standard模式是默认的启动模式，不用为配置android:launchMode属性。

如图所示：每次跳转系统都会在task中生成一个新的MainActivity实例，并且放于栈结构的顶部，当们按下后退键时，才能看到原来的MainActivity实例。

singleTop:

正如上图所示，跳转时系统会先在栈结构中寻找是否有一个MainActivity实例正位于栈顶，如果有则不在生成新的，而是直接使用。

我们看到，当从SecondActivity跳转到MainActivity时，系统发现存在有FirstActivity实例，但不是位于栈顶，于是重新生成一个实例。
这就是singleTop启动模式，如果发现有对应的activity实例正位于栈顶，则重复利用，不在生成新的实例。
这种启动模式通常适用于接受到消息后显示的界面，例如QQ接受到消息弹出activity，如果一次来10条消息，总不能弹出10个activity。

singleTask:

在上面的过程中，MainActivity的序列号是不变的，SecondActivity的序列号不是唯一的。说明从SecondActivity跳转到MainActivity时，没有生成新的实例，但是从MainActivity跳转到SecondActivity时生成了新的实例。
SecondActivity跳转到MainActivity后的栈结构发生变化的结果，我们注意到，SecondActivity消失了，没错，在这个跳转过程中系统发生有存在的FirstActivity实例，于是不在生成新的实例，而是将MainActivity之上的activity实例统统出栈，将MainActivity变为栈顶对象，显示在屏幕前。

singleInstance:
这种启动模式比较特殊，因为它会启用一个新的栈结构，将activity放置于这个新的栈结构中，并保证不在有其他activity实例进入。

应用场景： 
singleTop适合接收通知启动的内容显示页面。例如，某个新闻客户端的新闻内容页面，如果收到10个新闻推送，每次都打开一个新闻内容页面是很烦人的。

singleTask适合作为程序入口点。例如浏览器的主界面。不管从多少个应用启动浏览器，只会启动主界面一次，其余情况都会走onNewIntent，并且会清空主界面上面的其他页面。之前打开过的页面，打开之前的页面就ok，不再新建。

singleInstance适合需要与程序分离开的页面。例如闹铃提醒，将闹铃提醒与闹铃设置分离。singleInstance不要用于中间页面，如果用于中间页面，跳转会有问题，比如：A -> B (singleInstance) -> C，完全退出后，在此启动，首先打开的是B。


来源： https://blog.csdn.net/CodeEmperor/article/details/50481726
