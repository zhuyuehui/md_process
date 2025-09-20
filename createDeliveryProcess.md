flowchart TD
      A[开始 createDeliveryProcess] --> B[获取项目订单ID和需求来源]
      B --> C[批量查询订单信息]
      C --> D{订单信息是否存在?}
      D -->|否| E[记录错误日志并返回]
      D -->|是| F[获取订单房屋详情]
      F --> G{房屋信息是否存在?}
      G -->|否| H[记录错误日志并返回]
      G -->|是| I[检查装修类型]
      I --> J{是否为整装/团装?}
      J -->|否| K[记录日志并返回]
      J -->|是| L[查询交付流程配置]
      L --> M{流程配置是否存在?}
      M -->|否| N[警告日志并返回]
      M -->|是| O[构建配置模板实体]
      O --> P[初始化调度中心任务]
      P --> Q[生成任务实例ID映射]
      Q --> R{配置模板是否有效?}
      R -->|否| S[警告日志并返回]
      R -->|是| T[获取节假日规则]
      T --> U[查询拆改后户型信息]
      U --> V[生成基础信息对象]
      V --> W[查询需求详情]
      W --> X[根据需求生成流程节点配置]
      X --> Y[过滤已有进展的任务]
      Y --> Z[转换为交付流程任务对象]
      Z --> AA[设置排程任务实例关联]
      AA --> BB{是否有需要处理的附加任务?}
      BB -->|是| CC[查询任务进展并合并]
      BB -->|否| DD[填充工艺包代码]
      CC --> DD
      DD --> EE[构造流程节点计算实体]
      EE --> FF[计算流程节点列表]
      FF --> GG{计算是否成功?}
      GG -->|否| HH[记录错误日志并返回]
      GG -->|是| II[存储交付流程]
      II --> JJ[发送任务变更通知]
      JJ --> KK[记录成功日志]
      KK --> LL[结束]

      %% 异常处理
      A --> MM[异常捕获]
      MM --> NN[记录失败记录]
      MM --> OO[完成失败记录]
      OO --> PP[结束]

      %% 样式定义
      classDef startEnd fill:#e1f5fe
      classDef process fill:#f3e5f5
      classDef decision fill:#fff3e0
      classDef error fill:#ffebee
      classDef success fill:#e8f5e8

      class A,LL,PP startEnd
      class B,C,F,I,L,O,P,Q,T,U,V,W,X,Y,Z,AA,CC,DD,EE,FF,II,JJ,KK,MM,NN,OO process
      class D,G,J,M,R,BB,GG decision
      class E,H,K,N,S,HH error
      class LL success

