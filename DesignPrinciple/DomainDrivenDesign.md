# Domain-Driven Design 领域驱动设计

    一个领域就是一个问题域，只要是同一个领域，那问题域就相同。所以，只要确定了系统所属的领域，那这个系统的核心业务，即要解决的关键问题、问题的范围边界就基本确定了。
    每个子域（sub domain）对应一个边界上下文（bounded context），同一个边界上下文中的概念是明确的，没有任何歧义。
    找出聚合根，并找出每个聚合根包含的信息；
    画出领域模型图，圈出每个模型中的聚合边界；
## 领域驱动的分层架构
### 用户界面层/表示层Facade
    用户界面层负责向用户显示信息和解释用户指令。该层包含与其他应用系统交互的接口与通信设施。
    它负责Request的解释、验证以及转换，也负责Response的序列化，该层的主要职责是与外部用户包括Web服务、其他系统交互，如接受用户的访问，展示必要的数据信息。
    用户界面层facade目录：
        （1）api存放Controller类，接受用户或者外部系统的访问，展示必要的数据信息。
        （2）handle存放GlobalExceptionHandler全局异常处理类或者全局拦截器。
        （3）model存放DTO（Request、Reponse）、Factory（Assembler）类，Factory负责数据传输对象DTO与领域对象Domain相互转换。

### 应用层Application
    应用层定义了软件要完成的任务，并且指挥表达领域概念的对象来解决问题。
    应用层主要负责组织整个应用的流程，是面向用例设计的。该层非常适合处理事务，日志和安全等。相对于领域层，应用层应该是很薄的一层。它只是协调领域层对象执行实际的工作。
    应用层application目录：
        （1）service存放Service类，调用Domain执行命令操作，负责Domain的任务编排和分配工作。
        （2）external存放ExternalService类，负责与其他系统的应用层进行交互，通常是我们主动访问第三方服务。
        （3）model存放DTO（ExtRequest、ExtReponse）类。应用层application可以合并到领域层biz目录。
### 领域层/模型层Biz
    领域层主要负责表达业务概念，业务状态信息和业务规则。
    领域模型层主要包含以下的内容：
        1. 实体(Entities):具有唯一标识的对象值对象(Value Objects): 无需唯一标识。
        2. 领域服务(Domain): 与业务逻辑相关的，具有属性和行为的对象。
        3. 聚合/聚合根(Aggregates & Aggregate Roots): 聚合是指一组具有内聚关系的相关对象的集合。
        4. 工厂(Factories): 创建复杂对象，隐藏创建细节。
        5. 仓储(Repository): 提供查找和持久化对象的方法。

        领域层biz目录：
            （1）domain存放Domain类，Domain负责业务逻辑，调用Repository对象来执行数据库操作。Domain没有直接访问数据库的代码，具体的数据库操作是通过调用Repository对象完成的。注意，除了CQRS模式外，Repository都应该是由Domain调用的，而不是由Service调用。
            （2）repository存放Repository类，调用Dao或者Mapper对象类执行数据库操作。
            （3）factory存放Factory类，负责Domain和实体Entity的转换。

### 基础设施层Infrastructure
    基础设施层为上面各层提供通用的技术能力：为应用层传递消息，为领域层提供持久化机制，为用户界面层绘制屏幕组件。
    基础设施层Infrastructure目录：
    （1）commons存放通用工具类Utils、常量类Constant、枚举类Enum、BizErrCode错误码类等等。
    （2）persistence存放Dao或者Mapper类，负责把持久化数据映射成实体Entity对象。

作者：Johny Sinn
链接：https://www.zhihu.com/question/328870859/answer/1435876181
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。