# 游戏外挂原理调研

## 背景

对游戏的逆向比较感兴趣，想要探索「外挂」是如何开发出来的

感谢网友「春宝」自制的「DNF 60版本」，让我简单探索了找到游戏相对简单的基质

## 寻找基质

以下为询问gpt的回答


### 倍攻

实现“倍攻”功能大约可以通过以下几个步骤：

1. 找到代表攻击力的内存地址。这个过程可能涉及到一些内存查找和编辑的步骤，你可能需要使用像Cheat Engine这样的内存扫描工具来帮助你找到这个地址。基本的做法就是在游戏中改变你的攻击力，然后在Cheat Engine中搜索有变动的数值。重复这个过程，直到找到代表攻击力的内存地址。
2. 修改这个内存地址的值。一旦找到了正确的内存地址，你就可以修改其中存储的数值。将原始攻击力乘以一个倍数，就可以实现“倍攻”。


### 如何找到游戏的基质？

找到游戏的基址通常涉及到一些内存编辑和调试工具。

这里以CE(Cheat Engine)为例，通过以下步骤：

1. 首先，运行你想找基址的游戏（确保游戏是运行状态）。打开CE。
2. 在CE的工具栏上选择“选定进程列表”，找到你的游戏进程，通常它的名字与游戏相同或者很相似，选择它之后点击“打开”。
3. 回到游戏，找一个数值你能修改的属性，比如生命值、金钱等。记下这个数值。
4. 切回CE，把这个数值输入到"值"的文本框中，点击“首次扫描”。可能会找到大量结果。
5. 回到游戏，改变那个数值（比如更改金币数或生命值等）。
6. 切回CE，将新的数值输入到"值"的文本框中，点击“下次扫描”。继续上述繁琐的过程，直到筛选出一个或几个地址。
7. 这些地址就是你要找的基址。你可以在CE中修改数值，然后回到游戏中检查是否生效（如金币数量的增加或生命值的增加），从而验证这个地址是否正确。
这只是一种最基础的寻找基址的方法。

### 有没有一些更高级的方法可以更快地找到游戏的基址？

找到游戏基址的高级方法有很多，提到的只想重点说明一些思路和技巧，因为具体使用的工具和操作可能因游戏和系统不同而有所不同。

1. <b>指针扫描</b>: 如果游戏基址会变动，我们需要用到CE的指针扫描功能。一开始的步骤与基础版本相同，但是在找到候选地址后，你要对其进行指针扫描以找到指向这个地址的指针，这个指针就是基址。
2. <b>使用断点调试</b>: 先让你的数值发生变化，然后在CE中设定断点，看哪些指令更改了该数据。这样可以从后端找到负责更改数据的函数，进一步观察其行为，以确定是否可以通过修改这个函数或者相关代码来达到我们的目的。
3. <b>数据类型和数组的扫描</b>: CE可以扫描不同的数据类型，包括字符串，浮点数和数组等。这种类型的扫描在面对复杂的情况或者无法直接的参数时候很有用。

### 如何使用CE进行指针扫描？

使用CE进行指针扫描过程比较复杂，需要一些基本的清楚和理解。不过我会详细解释此过程。下面是一般的步骤：

1. 首先，运行你的游戏，然后打开CE。
2. 在CE的菜单上选择“选定进程”，找到并点击你的游戏进程。
3. 在游戏中找一个数值并改变它，在CE中使用“扫描”来确诠定数值所在的地址。
4. 当你找到该地址后，在CE的地址列表中点击右键，选择“查找什么读取/写入这个地址…”。此时会弹出一个新的窗口。
5. 在开启后的窗口中，选择指向该地址的一个指令或线程，一般选择写入指令更好，然后点击“显示中断所在位置”按钮。
6. 提示新窗口，这是CE的反汇编窗口。在这个窗口中，点击右键，然后选择"查找指针扫描适用的指针"
7. 在弹出的“指针扫描”窗口中，你可以设置最小和最大的偏移量和层级。扫描完毕后，CE将显示一个或多个与之前地址相对应的指针地址。

以上就是使用CE进行指针扫描的步骤。这个技术通常用于查找动态地址，即每次游戏启动时都会改变的地址。不过需要注意的是，指针扫描可能会占用大量的计算机资源，因此你需要有耐心等待。

### CE里面对基址对应的值进行active，为什么游戏里的数据就不变了？

在CE中，激活（Active）一个地址是指将此地址上锁，使得其数值不会发生改变。这通常用于实现无限生命、无限弹药等效果。具体步骤为：

1. 扫描到你想要锁定的数值的地址后，点击复选框 "Active"，这将锁定此地址的数值。
2. 即使游戏实际上在修改这个值，但在CE的内存视图中这个地址的数值依旧维持不变。这是因为CE持续将此数值修改回你锁定时的数值，所以你在游戏中看到的是一个恒定不变的数值。

需要注意的是，这种操作不一定在所有的游戏中都能成功，因为有些游戏会有反作弊机制，对一些关键数据的修改频率、修改方式甚至连续修改的数量进行检测。此外，这种操作可能会导致游戏不稳定或者禁止你的账户。
