# 触发器的概念

《星际争霸1》地图中的触发器（Trigger）是指该地图在游戏过程中为一些特定游戏事件设定特定结果的配置。  
ScmDraft2 一类基础地图编辑器中可以在地图中添加基础的触发器。  
触发器可以设定地图在游戏过程中满足何种条件（Conditions）就发生何种动作（Actions）  
例如可以在地图中设定内容为 “当玩家1收集到 500 点水晶矿就获胜” 的触发器  
就是指 “玩家1收集到 500 点水晶矿” 这个条件会触发 “获胜” 这个动作  


## 触发器的设计结构
  《星际争霸1》触发器的设计结构分为三个部分：目标玩家组（Players），条件（Conditions），动作（Actions）  
  
  ### 目标玩家组（Players）
  表明这条触发器对哪些玩家生效，一个触发器可以同时指定多个生效玩家。  
  如果设定的玩家们全都不在游戏中（电脑玩家也算玩家），那么该条触发器不会被启用（条件满足也不会被触发）。  
  目标玩家只能是 P1~P8 中的一个或多个。  
  这里指定的生效玩家将限定其条件和动作中的当前玩家（CurrentPlayer）是谁。  
    
  ### 条件（Conditions）
  一个触发器的最多可以指定 16 个触发条件，所有条件全都满足时，将执行该触发器动作中的所有动作。  
  若需要设定部分条件满足就执行，则可使用多个触发器触发一个开关（Switch），然后指定一个触发器判断开关的状态执行动作。  
  常见触发器条件有资源收集条件、单位死亡数条件、倒数计时器条件、开关状态条件等等。  

  ### 动作（Actions）
  一个触发器最多可以指定 64 条动作，当该触发器的条件全部满足时，这些动作就会按顺序执行。  
  常见的触发器动作有资源设置动作、单位死亡数设置动作、倒数计时器状态调整动作、开关状态调整动作等等。  

  




