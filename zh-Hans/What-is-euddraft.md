## epScript、eudplib、euddraft 的关系

- [epScript](https://raw.githubusercontent.com/armoha/eudplib/master/docs/funclist.txt) 是由 [TriggerKing](https://github.com/phu54321) 创造的脚本语言，被设计用于《星际争霸：重制版》的 EUD 地图的运行时开发。
- [eudplib](https://github.com/armoha/eudplib) 是一个 Python 库，它是一个生成星际争霸重制版运行时触发器字节码的扩展库，是 epScript 的功能核心。
- [euddraft](https://github.com/armoha/euddraft)  是一个集成了 Python 的程序，它会将编写好的 epScript 脚本（`*.eps`）编译成对应的调用 eudplib 生成触发器字节码的 Python 脚本（`*.py`）；  
  然后 euddraft 会按照一定顺序执行这些 Python 脚本（`*.py`）生成与编写好的 epScript 脚本（`*.eps`）逻辑等效的实际的触发器字节码；  
  最终 euddraft 再将这些触发器字节码插入地图中，在游戏中地图加载时生效。

  



