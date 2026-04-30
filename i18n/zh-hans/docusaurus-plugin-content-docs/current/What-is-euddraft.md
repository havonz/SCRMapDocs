---
sidebar_position: 3
---

# eps、eudplib、euddraft 的关系

- [epScript](https://raw.githubusercontent.com/armoha/eudplib/master/docs/funclist.txt) 是由 [TriggerKing](https://github.com/phu54321) 创建的脚本语言，设计目标是在《星际争霸：重制版》中开发 EUD 地图的运行时逻辑。
- [eudplib](https://github.com/armoha/eudplib) 是一个 Python 库，用于生成《星际争霸：重制版》运行时触发器字节码，也是 epScript 的功能核心。
- [euddraft](https://github.com/armoha/euddraft) 是一个集成了 Python 的程序，它会将编写好的 epScript 脚本（`*.eps`）编译成对应的 Python 脚本（`*.py`），由这些 Python 脚本调用 eudplib 生成触发器字节码；  
  然后 euddraft 会按一定顺序执行这些 Python 脚本（`*.py`），生成与 epScript 脚本（`*.eps`）逻辑等效的实际触发器字节码；  
  最终 euddraft 再将这些触发器字节码插入地图中，在游戏中地图加载时生效。

  


