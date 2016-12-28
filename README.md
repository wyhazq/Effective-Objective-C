## Effective Objective-C 52个有效方法

个人学习笔记，仅代表个人理解。

----

### 目录

#### 一、[熟悉Objective-C](#一熟悉objective-c-1)

>1. [Objective-C起源](#1objective-c起源)
>2. [类.h文件尽量少引入其他类的.h文件](#2类h文件尽量少引入其他类的h文件)
>3. [使用字面量语法](#3使用字面量语法)
>4. [多用常量，少用宏定义](#4多用常量少用宏定义)
>5. [使用枚举表示状态、选项、状态码](#5使用枚举表示状态选项状态码)

二、[对象、消息、运行期](#二对象消息运行期)

>6. [Property](#6property)
>7. [对象内部尽量访问实例变量？](#7对象内部尽量访问实例变量)
>8. [对象比较](#8对象比较)
>9. [类族模式](#9类族模式)
>10. [使用关联对象存放自定义数据](#10使用关联对象存放自定义数据)
>11. [objc_msgSend](#11objc_msgsend)
>12. [消息转发 message forwarding](#12消息转发-message-forwarding)
>13. [method swizzling](#13method-swizzling)
>14. [类对象](#14类对象)

三、[接口与API设计](#三接口与API设计)

> 15. [用前缀避免命名空间冲突](#15用前缀避免命名空间冲突)
> 16. [提供全能初始化方法](#16提供全能初始化方法)
> 17. [实现description方法](#17实现description方法)
> 18. [尽量使用不可变对象](#18尽量使用不可变对象)
> 19. [使用清晰而协调的命名方式](#19使用清晰而协调的命名方式)
> 20. [为私有方法名加前缀](#20为私有方法名加前缀)
> 21. [理解Objective-C错误类型](#21理解objective-c错误类型)
> 22. [理解NSCopying协议](#22理解nscopying协议)

四、[协议与分类](#四协议与分类)

> 23. [通过委托与数据源协议进行对象间通信](#23通过委托与数据源协议进行对象间通信)
> 24. [将类的实现代码分散到便于管理的数个分类之中](#24将类的实现代码分散到便于管理的数个分类之中)
> 25. [总是为第三方类的分类名称加前缀](#25总是为第三方类的分类名称加前缀)
> 26. [勿在分类中声明属性](#26勿在分类中声明属性)
> 27. [使用"class-continuation分类"隐藏实现细节](#27使用"class-continuation分类"隐藏实现细节)
> 28. [通过协议提供匿名对象](#28通过协议提供匿名对象)

五、[内存管理](#五内存管理)

> 29. [理解引用计数](#29理解引用计数)
> 30. [以ARC简化引用计数](#30以arc简化引用计数)
> 31. [在dealloc方法中只释放引用并解除监听](#31在dealloc方法中只释放引用并解除监听)
> 32. [编写“异常安全代码”时留意内存管理问题](#32编写“异常安全代码”时留意内存管理问题)
> 33. [以弱引用避免保留环](#33以弱引用避免保留环)
> 34. [以"自动释放池块"降低内存峰值](#34以"自动释放池块"降低内存峰值)
> 35. [用“僵尸对象”调试内存管理问题](#35用“僵尸对象”调试内存管理问题)
> 36. [不要使用retainCount](#36不要使用retainCount)

六、[Block && GCD](#六Block-&&-GCD)

> 37. [理解Block](#37理解block)
> 38. [为常用Block创建typedef](#38为常用block创建typedef)
> 39. [用handler block降低代码分散程度](#39用handler-block降低代码分散程度)
> 40. [用block引用其所属对象时不要出现保留环](#40用block引用其所属对象时不要出现保留环)
> 41. [多用派发队列，少用同步锁](#41多用派发队列少用同步锁)
> 42. [多用GCD，少用performSelector系列方法](#42多用gcd少用performselector系列方法)
> 43. [掌握GCD及NSOperationQueue的使用时机](#43掌握gcd及nsoperationqueue的使用时机)
> 44. [通过Dispatch Group机制，根据系统资源状况来执行任务](#44通过dispatch-group机制根据系统资源状况来执行任务)
> 45. [使用dispatch_once来执行只需运行一次的线程安全代码](#45使用dispatch_once来执行只需运行一次的线程安全代码)
> 46. [不要使用dispatch_get_current_queue](#46不要使用dispatch_get_current_queue)

七、[系统框架](#七系统框架)

> 47. [熟悉系统框架](#47熟悉系统框架)
> 48. [多用块枚举，少用for循环](#48多用块枚举少用for循环)
> 49. [对自定义其内存管理语义的collection使用无缝桥接](#49对自定义其内存管理语义的collection使用无缝桥接)
> 50. [构建缓存时选用NSCache而非NSDictionary](#50构建缓存时选用NSCache而非NSDictionary)
> 51. [精简initialize与load的实现代码](#51精简initialize与load的实现代码)
> 52. [别忘了NSTimer会保留其目标对象](#52别忘了NSTimer会保留其目标对象)

----

### 一、熟悉Objective-C

--------

#### 1.Objective-C起源

##### a.消息语言(运行环境) 

>* 区别：函数语言(编译器)
>* 方法，类型运行时处理(动态绑定)
>* 核心：runtime

b.C的超集

>* 必须理解C的核心(内存模型和指针)
>* 指针(变量)：栈    对象：堆    (32位：4byte，64位，8byte)
>* 栈内存在栈帧弹出时自动清理，堆内存必须直接管理，C：malloc，free    OC：引用计数
>* 基本类型，结构体等非对象类型使用栈内存(追求性能)  

-----

#### 2.类.h文件尽量少引入其他类的.h文件

>* 使用@class
>* import 和 遵循protocol 尽量在.m中去做

-----

#### 3.使用字面量语法

```objective-c
NSNumber *number = @1;
NSString *string = @"";
NSArray *array = @[]; array[0];
NSDictionary *dictionary = @{}; dictionary[key];
NSMutableString *mutableString = @"".mutableCopy; //多创建一个对象和多调用一个方法，但直观
NSMutableArray *mutableArray = @[].mutableCopy;	
NSMutableDictionary *mutableDictionary = @{}.mutableCopy;
```

------

#### 4.多用常量，少用宏定义

>* 宏定义的原理是替换，没有类型信息
>* 常量在公开，以类名为前缀；私有，以k开头
>* 私有用static，不加编译器会为其创建“外部符号”
>* 全局用extern

-------

#### 5.使用枚举表示状态、选项、状态码

>* 不组合：NS_ENUM
>* 组合：按位操作符，使用NS_OPTIONS定义。1<<0(1)  1<<1(10)  1<<2(100)，以此类推
>* switch枚举不应加default，以便新枚举加入时，编译器给出⚠️

------

### 二、对象、消息、运行期

---

#### 6.Property

>* 实例变量：偏移量，距离存放对象的内存区域的起始地址，应对运行时不兼容现象，OC的做法是把实例变量当做一种存储偏移量所用的特殊变量，交由类对象保管。(ABI)
>* 另一种解决方法是通过存取方法来做，OC的存取方法有严格的命名规范，于是便产生了@property
>* @property:编译器会自动生成get,set,ivar
>* @synthesize 可自定义实例变量名字，建议用默认
>* @dynamic不会自动合成
>* 属性特质：原子性，内存管理，读写权限、方法名

---

#### 7.对象内部尽量访问实例变量？

>* 读：_  写：self
>* 初始化和dialloc中：_
>* 惰性初始化：读：self  写：_ (本人比较倾向这种[iOS应用架构谈 view层的组织和调用方案](http://casatwy.com/iosying-yong-jia-gou-tan-viewceng-de-zu-zhi-he-diao-yong-fang-an.html)中最后的"关于Getter和Setter")

---

#### 8.对象比较

>* 使用isEqual
>* [a isEqual b] = YES;  a.hash == b.hash;    a.hash == b.hash;  [a isEqual b]不一定为YES
>* 编写hash方法时，应使用计算速度快并且哈希码碰撞几率低的算法
>* 不要盲目比较对象每个属性，根据具体需求定制
>* NSMutableSet？

---

#### 9.类族模式

>* 思想，用type去创建子类，跟工厂方法的思想一样
>* 比较类的类型 isKingOfClass

---

#### 10.使用关联对象存放自定义数据

|               关联类型                |   等效@property属性   |
| :-------------------------------: | :---------------: |
|      OBJC_ASSOCIATION_ASSIGN      |      assign       |
| OBJC_ASSOCIATION_RETAIN_NONATOMIC | nonatomic, retain |
|  OBJC_ASSOCIATION_COPY_NONATOMIC  |  nonatomic, copy  |
|      OBJC_ASSOCIATION_RETAIN      |      retain       |
|       OBJC_ASSOCIATION_COPY       |       copy        |

> * get：id objc_getAssociatedObject(id object, const void *key)
> * set：void objc_setAssociatedObject(id object, const void *key, id value, objc_AssociationPolicy policy)
> * remove：objc_removeAssociatedObjects(id object)

***

#### 11.objc_msgSend

> * id objc_msgSend(id self, SEL cmd, …)
> * 在方法列表objc_method_list中查询selector，找不到则沿着继承体系往上找，最后找不到则消息转发
> * struct objc_cache *cache：指向最近调用的方法，用于优化调用方法的速度
> * objc_msgSend_stret：for some struct return types
> * objc_msgSend_fpret： for some float return types
> * objc_msgSendSuper：向父类发送消息
> * 尾调用优化

---

#### 12.消息转发 message forwarding

对象收到无法解读的消息后，会启动消息转发

a. 动态方法解析

>* -(BOOL)resolveInstanceMethod:(SEL)sel;  是否能新增实例方法，处理sel。
>* -(BOOL)resolveClassMethod:(SEL)sel;  同上，处理类方法的
>* 前提：相关方法已实现好，等待运行时插入
>
>
>
>
>
>

b.备援接收者(备胎无处不在)

> * (id)forwardingTargetForSelector:(SEL)aSelector;当前接收者能找到备援对象，则将其返回，不能，返回nib
> * 通过此方案用组合模拟出多重继承？
> * 无法操作经由这一步所转发的消息，若想在发送给备援接收者之前先修改消息内容，得通过完整的消息转发机制

c.完整的消息转发

> * 创建NSInvocation对象，把尚未处理的那条消息有关的全部细节都封于其中，此对象包含selector，target，parameter。
> * 消息派发系统将消息指派给目标对象：-(void)forwardInvocation:(NSInvocation *)anInvocation
> * 比较有用的实现方式为：触发消息前，先以某种方式改变消息内容，比如追加另外一个参数，或是改换selector等等
> * 若发现调用操作不应由本类来处理，则需调用超类的同名方法，直至NSObject，如果最后调用了NSObject类的方法，那么该方法还会继而调用-(void)doesNotRecognizeSelector:(SEL)aSelector;抛出异常，表明selector最终未能得到处理。

d.步骤越往后，处理消息的代价越大，前面的步骤处理完，runtime能将此方法缓存起来，第三步开始还得创建并处理完整的NSInvocation对象

e.CALayer:兼容与键值编码的容器类

---

#### 13.method swizzling

既不需要源代码，也不需要通过继承重写方法就能改变这个类本身的功能。

id (*IMP)(id, SEL, ...);

a.交换

```objective-c
//selector
SEL originalSelector = @selector(viewDidLoad);
SEL swizzledSelector = @selector(sw_viewDidLoad);
//method
Method originalMethod = class_getInstanceMethod([self class], originalSelector);
Method swizzledMethod = class_getInstanceMethod([self class], swizzledSelector);
//exchange
method_exchangeImplementations(originalMethod, swizzledMethod);
```

b.安全写法

```objective-c
+ (void)load
{
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        Class class = [self class];
        
        SEL originalSelector = @selector(viewDidLoad);
        SEL swizzledSelector = @selector(sw_viewDidLoad);
        
        Method originalMethod = class_getInstanceMethod(class, originalSelector);
        Method swizzledMethod = class_getInstanceMethod(class, swizzledSelector);
        
        BOOL success = class_addMethod(class, originalSelector, method_getImplementation(swizzledMethod), method_getTypeEncoding(swizzledMethod));
        if (success) {
            class_replaceMethod(class, swizzledSelector, method_getImplementation(originalMethod), method_getTypeEncoding(originalMethod));
        } else {
            method_exchangeImplementations(originalMethod, swizzledMethod);
        }
    });
}

- (void)sw_viewDidLoad {
	[self sw_viewDidLoad];
  
  	//do something
}
```

勿滥用，控制不好会出各种问题的。

---

#### 14.类对象

```objective-c
struct objc_class {
    Class isa  OBJC_ISA_AVAILABILITY;

#if !__OBJC2__
    Class super_class                                        OBJC2_UNAVAILABLE;
    const char *name                                         OBJC2_UNAVAILABLE;
    long version                                             OBJC2_UNAVAILABLE;
    long info                                                OBJC2_UNAVAILABLE;
    long instance_size                                       OBJC2_UNAVAILABLE;
    struct objc_ivar_list *ivars                             OBJC2_UNAVAILABLE;
    struct objc_method_list **methodLists                    OBJC2_UNAVAILABLE;
    struct objc_cache *cache                                 OBJC2_UNAVAILABLE;
    struct objc_protocol_list *protocols                     OBJC2_UNAVAILABLE;
#endif

} OBJC2_UNAVAILABLE;
/// An opaque type that represents an Objective-C class.
typedef struct objc_class *Class;

struct objc_object {
    Class isa  OBJC_ISA_AVAILABILITY;
};
/// A pointer to an instance of a class.
typedef struct objc_object *id;
```

> * SomeClass instance, isa -> SomeClass class, isa -> SomeClass metaclass

---

### 三、接口与API设计

----

#### 15.用前缀避免命名空间冲突

> * Objective-C没有命名空间
> * Apple宣称其保留使用所有"两字母前缀"的权利(谁管它...)
> * 类和Category，Category的属性和方法都要使用前缀



---

#### 16.提供全能初始化方法

> * 如题：把必要初始化参数接在init方法后面，这样所有的init方法都可以调用此方法
> * 防止别人调用init或者new：- (instancetype)init UNAVAILABLE_ATTRIBUTE;
> * 实现NSCoding协议时要实现initWithCoder，这算是另一个全能初始化方法

---

#### 17.实现description方法

比较实用的是swizz一个NSDictionary的description，打印一下中文，不过假如是NSArray<NSDictionary *> *的打印不出来，暂时找不到好的解决方案

----

#### 18.尽量使用不可变对象

尽量把.h文件的属性设成readonly

---

#### 19.使用清晰而协调的命名方式

OC：尽量长...

---

#### 20.为私有方法名加前缀

加一下还是挺好的，但个人感觉业务类加起来感觉可读性有点差，一般都用pragma mark -分割了，VC，View，VM一般都规定了各自方法的固定格式，不关怎样，把代码的可维护性提高，即是好的解决方案。比如后面所讲的分类。

---

#### 21.理解Objective-C错误类型

> * 自动引用计数在默认情况下不是异常安全的。如果抛出异常，那么本应该在作用域末尾释放的对象现在却不会释放了。
> * OC采用的办法是，在极其罕见的情况下抛出异常，异常抛出后，无须考虑恢复问题，应用程序此时应该退出。(闪退咯)
> * 不严重的错误，返回nil
> * NSError(有待研究)

---

#### 22.理解NSCopying协议

> * -(id)copyWithZone:(NSZone *)zone; NSZone以前会把对象分区，现在只有一个默认分区了，忽略之。
> * Foundation框架中的所有collection类在默认情况下是浅拷贝
> * 若想令自定义对象具有拷贝功能，需实现NSCopying协议，可变版本需实现NSMutableCopying协议

---

### 四、协议与分类

---

#### 23.通过委托与数据源协议进行对象间通信

> * 总是应该把发起委托的实例也一并传入方法中
> * Delegate <- Class <- DataSource
> * 委托方法需要调用很多次时，可以在setDelegate方法中检查respondsToSelector，并用结构体或者其他标志位存储起来，以便提高速度，依据实际情况，遇到性能瓶颈可以考虑

---

#### 24.将类的实现代码分散到便于管理的数个分类之中

> * Category将大类分成几个部分，便于维护

---

#### 25.总是为第三方类的分类名称加前缀

如题，防止冲突

---

#### 26.勿在分类中声明属性

> * 把封装数据所用的全部属性都定义在主接口里
> * Category添加属性还是可以的，注意一点就好

---

#### 27.使用"class-continuation分类"隐藏实现细节

如题

---

#### 28.通过协议提供匿名对象

id<protocol>？(有待研究)

---

### 五、内存管理

---

#### 29.理解引用计数

> * 引用计数 retainCountz，retain递增引用计数，release递减保留计数，autorelease清理“自动释放池”(autorelease pool)时，再递减引用计数。
>
> * 避免悬挂指针 id obj = nil;
>
> * ```objective-c
>   - (void)setFoo:(id)foo {
>    	[foo retain];		//1
>     	[_foo release];		//2
>     	_foo = foo;
>   }
>   //1和2调换的话，假如原来self.foo = obja;现在再调用一次self.foo = obja; 那么obja就被释放了，再怎么retain都没用了，就可能会出事，应该是这么理解的，假如self.foo = objb应该不会出事。
>   ```
>
> * autorelease在下一次“事件循环event loop”时释放，类方法创建对象时常用，延长对象生命周期，使其在跨越方法调用边界后依然存活一段时间。
>
> * 保留环(循环引用) a引用了b, b引用a或者 a引用b, b引用c，c引用a之类的，造成无法释放，必须打破循环才能解决，比如用弱引用weak。

---

#### 30.以ARC简化引用计数

> * 返回对象归调用者所有：alloc，new，copy，mutableCopy
> * 优化强引用的实例变量：objc_autoreleaseReturnValue，objc_retainAutoreleaseReturnValue
> * __unsafe_unretained：不保留此值，这么做可能不安全，再次使用变量时，其对象可能已被回收
> * __weak：不保留此值，变量可安全使用，对象被回收时，变量会自动置空
> * CoreFoundation对象不归ARC管理：CFRetain，CFRelease

---

#### 31.在dealloc方法中只释放引用并解除监听

>* 移除CoreFoundation对象
>* 移除消息监听
>* 移除观察者

---

#### 32.编写“异常安全代码”时留意内存管理问题

> * 捕获异常时，一定要注意将try块内所创立的对象清理干净
> * 在默认情况下，ARC不生产安全处理异常所需的清理代码，开启编译器标志后可以生成这种代码，不过会导致应用程序变大，而且会降低运行效率。

---

#### 33.以弱引用避免保留环

> * __weak
> * __unsafe_unretained

----

#### 34.以"自动释放池块"降低内存峰值

自动释放池要等到下一次事件循环时才会清空，遇到内存峰值时，需主动创建

```objective-c
NSArray *databaseRecords = /*...*/;
NSMutableArray *people = [NSMutableArray new];
for (NSDictionary *record in databaseRecords) {
	@autoreleasepool {
     	EOCPerson *person = [[EOCPerson alloc] initWithRecord:record];
      	[people addObject:person];
	}
}
```

---

#### 35.用“僵尸对象”调试内存管理问题

> * 勾选Enable Zombie Objects选项打开
> * 系统回收对象时，检查此状态，若启用，则把对象转化为僵尸对象，不彻底回收。
> * message sent to deallocated

---

#### 36.不要使用retainCount

> * ARC已废弃
> * 给定时间的绝对保留计数，无法反映整个生命对象周期

---

### 六、Block && GCD

---

#### 37.理解Block

return_type (^block_name)(parameters)

```objective-c
struct Block_descriptor {
    unsigned long int reserved;
    unsigned long int size;
    void (*copy)(void *dst, void *src);
    void (*dispose)(void *);
};

struct Block_layout {
    void *isa;//指针，所有对象都有该指针，用于实现对象相关的功能。
    int flags;//用于按 bit 位表示一些 block 的附加信息。
    int reserved;//保留变量。
    void (*invoke)(void *, ...);//函数指针，指向具体的 block 实现的函数调用地址。
    struct Block_descriptor *descriptor;//表示该 block 的附加描述信息，主要是 size 大小，以及 copy 和 dispose 函数的指针。
  	//variables，capture 过来的变量，block 能够访问它外部的局部变量，就是因为将这些变量（或变量的地址）复制到了结构体中。
};
```



> * block可以实现闭包，是一个对象。这项语言特性是作为“扩展”而加入GCC编译器的。
> * 直接定义在函数里，和函数共享同一范围内的东西。
> * 语法与函数指针近似
> * __block：block外的变量需要在block内修改
> * weakSelf
> * block可分配在栈或堆上，也可以是全局的。分配在栈上的块可拷贝到堆里，使之与标准OC对象一样，具备引用计数。

---

#### 38.为常用Block创建typedef

如题

---

#### 39.用handler block降低代码分散程度

视情况而用，不要单纯为了可以降低分散代码而用。

----

#### 40.用block引用其所属对象时不要出现保留环

准确判断哪些需要weakSelf，哪些不需要

---

#### 41.多用派发队列，少用同步锁

> * 同步锁和NSLock可能造成死锁，GCD的话，加锁任务在内部处理
> * 串行队列保证数据同步，保证存取在同一队列中
> * 执行异步派发时，需要拷贝block
> * 栅栏 dispatch_barrier_async：并发队列如果发现接下来处理的block是栅栏block，那么就一直等到当前所有并发block都处理完，才会单独执行栅栏block

---

#### 42.多用GCD，少用performSelector系列方法

performSelector：

> * ARC不添加释放操作，可能导致内存泄露
> * 只能返回void或者id类型
> * 传入参数只能是id类型

GCD中有对应的代替方案

---

#### 43.掌握GCD及NSOperationQueue的使用时机

> * NSOperationQueue底层是用GCD实现的
> * NSOperation可以更精细地控制
> * 看情况选择

---

#### 44.通过Dispatch Group机制，根据系统资源状况来执行任务

> * 把多个任务合成一组

```objective-c
    dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
    dispatch_group_t group = dispatch_group_create();
    for (id obj in objs) {
        dispatch_group_async(group, queue, ^{
            [obj doSomething];
        });
    }
    dispatch_group_wait(group, DISPATCH_TIME_FOREVER);
    dispatch_queue_t mainQueue = dispatch_get_main_queue();
    dispatch_group_notify(group, mainQueue, ^{
        //do something on main queue
    });
```

---

#### 45.使用dispatch_once来执行只需运行一次的线程安全代码

单例

---

#### 46.不要使用dispatch_get_current_queue

> * iOS6.0已废弃
> * 使用：dispatch_queue_set_specific(<#dispatch_queue_t  _Nonnull queue#>, <#const void * _Nonnull key#>, <#void * _Nullable context#>, <#dispatch_function_t  _Nullable destructor#>)

---

### 七、系统框架

---

#### 47.熟悉系统框架

如题&掌握C语言

---

#### 48.多用块枚举，少用for循环

> * for循环、NSEnumerator遍历，快速遍历、块枚举
> * 块枚举本身能通过GCD来并发执行遍历操作，充分利用多核CPU
> * 若提前知道待遍历的collection含有何种对象，则应修改块签名，指出对象的具体类型

---

#### 49.对自定义其内存管理语义的collection使用无缝桥接

> * __bridge：NS转CF，ARC管理内存
> * __bridge_retained，NS转CF，手动管理
> * __bridge_transfer：CF转NS
> * 在CoreFoundation层面创建特殊的collection，在转成Objective-C collection，比较少用吧。

---

#### 50.构建缓存时选用NSCache而非NSDictionary

> * NSCache在系统资源将要耗尽时会自动删减缓存
> * 请使用缓存框架

---

#### 51.精简initialize与load的实现代码

> * load里面使用其他类是不安全的，其他类不一定加载好了
> * initialize：懒加载，且只调用一次，只应该用来设置内部数据
> * 无法在编译期设定的全局常量，可以放在initialize里初始化

---

#### 52.别忘了NSTimer会保留期目标对象

```objective-c
_timer = [NSTimer timerWithTimeInterval:1 target:self selector:@selector(onTimer:) userInfo:nil repeats:YES];
[[NSRunLoop currentRunLoop] addTimer:self.timer forMode:NSRunLoopCommonModes];
```

> * NSTimer对象会保留其目标对象，知道计时器本身失效为止，调用invalidate可使计时器失效，一次性计时器在触发任务之后也会失效。
> * 搞个NSTimer的category，把target设为NSTimer的类对象来解决保留问题。

---

