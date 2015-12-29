# iOS面试题大全

###### 2015-12-28 达内纪老师 [GitHub](https://github.com/jiyingxin/Documents/tree/master/iOS面试题)，[CSDN博客](http://blog.csdn.net/jiyingxin)

![](http://7xpk73.com1.z0.glb.clouddn.com/8fb0f6bfd7bd50e285535532d4b45255.jpg)

	说明：
	最近为达内学员整理面试题。发现网上的面试题和答案基本都是抄来抄去的，甚至很多答案都是错误的。
	所以整理了常见的面试题，对答案重新进行了筛选整理。
	如果答案有错漏或者更好的答案，希望大家能够在GitHub或者CSDN通知我。
	此文档将会持续更新。GitHub会优先更新。


##### 1. OC中，与`alloc`语义相反的方法是`dealloc`还是`release`？与`retain`语义相反的方法是`dealloc`还是`release`？为什么？需要与`alloc`配对使用的方法是`dealloc`还是`release`，为什么？

		以下是针对MRC(手动内存释放)模式:
		与alloc语义相反的方法是dealloc，与retain语义相反的方法是release。
		alloc是为对象在内存中开辟空间，而dealloc则是对象销毁时释放空间。
		retain方法是对象开辟空间以后使对象的引用计数器加1，而release是对象的引用计数器减1。
		需要与alloc配对的方法是release，因为对象创建以后，对象的引用计数器自动加1，
		而调用release方法后，对象的引用计数器归0，系统会自动调用dealloc方法释放空间。

`MRC`[^MRC]

##### 2. 在一个对象的方法里面：`self.name = @"object";`和 `_name = @"object"`有什么不同吗？

		self.name = @"object"; 是通过点语法修改属性name的值。
		本质上使用的是name属性的setter方法进行的赋值操作，实际上执行的代码是
		
		[self setName:@"object"];
		
		例如：
		@property(nonatomic, strong) NSString *name;
		//根据@property关键词，系统自动生成setter方法。
		- (void)setName:(NSString *)name{
			//根据strong关键词
			[name retain];	//内存计数+1
			[_name release];	//把之前指针指向的内容内存计数-1
			_name = name; //指向新内容
		}
		
		_name = @“object”; 只是单纯的把‘_name’指针指向‘@"object"’字符串对象所在的地址，
		没有调用方法。

`点语法`[^PointSyntax]

##### 3. 这段代码有什么问题吗？  
>-(void)setAge:(int)newAge{   
self.age = newAge;   
} 

		在age属性的setter方法中，不能通过点语法给该属性赋值。
		会造成setter方法的循环调用。因为self.age = newAge; 
		本质上是在调用 [self setAge:newAge]; 方法。
		解决循环调用的方法是方法体修改为 _age = newAge;
		
		另外 变量名称不能使用new开头！

##### 4. 以下每行代码执行后，person对象的retain count分别是多少？
   >Person *person = [[Person alloc] init];  
   [person retain];  
   [person release];  
   [person release];  

	   Person *person = [[Person alloc] init];  =1
	   [person retain];  +1    = 2
	   [person release];  -1   = 1
	   [person release];  -1   = 0
	   
	   内存计数技术规律
	   alloc，new，copy   内存计数 = 1
	   retain +1
	   release -1
	   UIView  addSubview  +1
	   NSMutableArray  addObject   +1

##### 5. 这段代码有什么问题，如何修改？  
   >for(int i = 0; i < someLargeNumber; i++){  
	 NSString *string = @“Abc”;  
	string = [string lowercaseString];  
	string = [string stringByAppendingString:@“xyz”];  
	NSLog(@“%@“, string);  
   }  

	代码本身不会报错。
	但是猜测出题者的意思是要循环添加为 abcxyzxyzxyz.....这样的形式。
	如果是想在Abc后面拼接多个xyz字符串的话，
	则需要把"NSString *string = @“Abc”;" 这行代码放在循环语句外面。

##### 6. 简要叙述面向对象的特点，特别是多态。

	1. 封装
	封装是对象和类概念的主要特性。它是隐藏内部实现，提供外部接口，可以看作是“包装”。 
	封装，也就是把客观事物封装成抽象的类，并且类可以把自己的数据和方法只让可信的类或者对象操作，
	对不可信的进行信息隐藏。
	封装的目的是增强安全性和简化编程，使用者不必了解具体的实现细节，而只是要通过外部接口，
	以特定的访问权限来使用类的成员。
	好处：可以隐藏内部实现细节。通过大量功能类封装，加快后期开发速度。
	
	2. 继承
	面向对象编程 (OOP) 语言的一个主要功能就是“继承”。
	继承是指这样一种能力：它可以使用现有类的所有功能，并在无需重新编写原来的类的情况下
	对这些功能进行扩展。
	通过继承创建的新类称为“子类”或“派生类”，被继承的类称为“基类”、“父类”或“超类”。
	继承的过程，就是从一般到特殊的过程。在考虑使用继承时，有一点需要注意，
	那就是两个类之间的关系应该是“属于”关系。
	例如，Employee(雇员)是一个人，Manager(领导)也是一个人，因此这两个类都可以继承Person类。
	但是 Leg(腿) 类却不能继承 Person 类，因为腿并不是一个人。
	
	3. 多态
	多态性（polymorphism）是允许你将父对象设置成为和一个或更多的他的子对象相等的技术，
	赋值之后，父对象就可以根据当前赋值给它的子对象的特性以不同的方式运作。
	简单的说，就是一句话：允许将子类类型的指针赋值给父类类型的指针。
	不同对象以自己的方式响应相同的消息的能力叫做多态。
	意思就是假设生物类（life）都用有一个相同的 方法-eat;那人类属于生物，猪也属于生物，
	都继承了life后，实现各自的eat，但是调用是我们只需调用各自的eat方法。
	也就是不同的对象以 自己的方式响应了相同的消息（响应了eat这个选择器）。
	实现多态，有二种方式，覆盖，重载。
	•    覆盖(override)，是指子类重新定义父类的虚函数的做法。
	•    重载(overload)，是指允许存在多个同名函数，而这些函数的参数表不同
			（或许参数个数不同，或许参数类型不同，或许两者都不同）。
	** 这里注意：OC没有重载，因为OC只认函数名，不认参数类型。OC不允许存在多个同名函数。
						
	总结：封装可以隐藏实现细节，使得代码模块化；继承可以扩展已存在的代码模块（类）；
	它们的目的都是为了——代码重用。
	而多态则是为了实现另一个目的——接口重用！
	多态的作用，就是为了类在继承和派生的时候，保证使用“家谱”中任一类的实例的某一属性时的正确调用。
	在类层次中，子类只继承一个父类的数据结构和方法，则称为单重继承。 
	在类层次中，子类继承了多个父类的数据结构和方法，则称为多重继承。 

##### 7. objective-c 所有对象间的交互是如何实现的？

	在对象间交互中每个对象承担的角色不同，
	但总的来说无非就是”数据的发送者”或”数据的接收者”两种角色。
	消息的正向传递比较简单，直接拿到接受者的指针即可。
	消息的反响传递可以通过委托模式，观察者模式(本质是单例模式)，block语法，AppDelegagte来实现。
	其中委托模式，block语法都是1对1的消息传递。 观察者模式是1对多。
	AppDelegate比较特殊，这是一个生命周期与进程一致的对象。


##### 8. 什么叫数据结构？

	数据结构是计算机存储、组织数据的方式。是指相互之间存在一种或多种特定关系的数据元素的集合。
	通常，精心选择的数据结构可以带来更高的运行或者存储效率。

##### 9. OC的类可以多继承吗？可以实现多个接口吗？Category是什么？分类中能定义成员变量或属性吗？为什么？重写一个类的方式是继承好还是类别好？为什么？

	Object-c的类不可以多重继承;可以实现多个接口(协议)，通过实现多个接口可以完成C++的多重继承;
	Category是类别，推荐使用类别，用Category去重写类的方法，仅对本引入Category的类有效，
	不会影响到其他类与原有类的关系。


##### 10. #import和#include有什么区别？@class呢？#import<>和#import""有什么区别？

	#import是Objective-C导入头文件的关键字，#include是C/C++导入头文件的关键字,
	使用#import头文件会自动只导入一次，不会重复导入，相当于#include和#pragma once;
	@class告诉编译器某个类的声明，当执行时，才去查看类的实现文件，可以解决头文件的相互包含;
	#import<>用来包含系统的头文件，#import””用来包含用户头文件。
	例如：
	
	/* 如果这里不写@class，则报错。 原因是找不到MyVC的定义。因为代码执行顺序是由上至下的。
	当声明协议MyVCDelegate时， MyVC还没有声明。
	使用 @class 名称随便写，不管是否存在。 可以自己用代码尝试随意写个@class 例如：
	@class Hello; 
	就算项目中根本没有Hello类，这里也不会报错。 因为只有当项目运行起来，才会真的去检查Hello类
	是否声明了。
	*/
	
	@class MyVC;
	@protocol MyVCDelegate <NSObject>
	- (void)myVC:(MyVC *)myVC click:(id)sender;
	@end
	@interface MyVC : BaseVC
	@end
	
	ps：iOS7之后的新特性，可以使用@import 关键词来代理#import引入系统类库。
		使用@import引入系统类库，不需要到build phases中先添加添加系统库到项目中。

##### 11.属性readwrite, readonly, assign, retain, copy, nonatomic各是什么作用？在哪种情况下用？

	1.readwrite 是可读可写特性；需要生成getter方法和setter方法时
	（补充：默认属性，将生成不带额外参数的getter和setter方法（setter方法只有一个参数））
	2.readonly 是只读特性，只会生成getter方法，不会生成setter方法;不希望属性在类外改变
	3.assign 是赋值特性，setter方法将传入参数赋值给实例变量；仅设置变量时；
	4.retain(MRC)/strong(ARC) 表示持有特性，setter方法将传入参数先保留，
	  再赋值，传入参数的retaincount会+1;
	5.copy 表示拷贝特性，setter方法将传入对象复制一份；需要完全一份新的变量时。
	6.nonatomic 非原子操作，决定编译器生成的setter和getter方法是否是原子操作。
		- atomic表示多线程安全，需要对方法加锁，保证同一时间只有一个线程访问属性，
		  因为有等待过程，所以影响执行效率
		- 一般使用nonatomic。不加锁。效率会更高。但是线程不安全。

##### 12. 写一个setter方法用于完成@property(nonatomic, strong)NSString *name, 写一个setter方法用于完成@property(nonatomic, copy)NSString *name

	- (void)setName:(NSString*)str //retain
	　{
	　　[str retain];
	　　[_name release];
	　　_name = str;
	　}
	- (void)setName:(NSString *)str //copy
	　{
	　　id t = [str copy];
	　　[_name release];
	　　_name = t;
	　}

##### 13. 对于语句NSString *obj = [[NSData alloc] init]; obj在编译时和运行时分别是什么类型的对象？

	编译时是NSString的类型;运行时是NSData类型的对象。

##### 14. 常见的OC的数据类型有哪些？ 和C的基本数据类型有什么区别？ 如：NSInteger和int

	object-c的数据类型有NSString，NSNumber，NSArray，NSMutableArray，NSData等等。

	C语言的基本数据类型int，只是一定字节的内存空间，用于存放数值;
	NSInteger类型的定义是
	
	#if __LP64__ || (TARGET_OS_EMBEDDED && !TARGET_OS_IPHONE) 
	|| TARGET_OS_WIN32 	|| NS_BUILD_32_LIKE_64
	typedef long NSInteger;
	typedef unsigned long NSUInteger;
	#else	
	typedef int NSInteger;
	typedef unsigned int NSUInteger;
	#endif
	
	可以看到，在64位操作系统上，NSInteger是 C语言的long类型。 
	在32位操作系统上，则是int类型。

##### 15. id声明的对象有什么特性？

	id 声明的对象具有运行时的特性，即可以指向任意类型的objcetive-c的对象;
	可以作为返回值，也可以声明对象。
	例如
	- (id)initWithName:(NSString *)name;
	id obj = [NSObject new];
	
	现在我们使用苹果推荐使用的“instancetype”类型代替id类型作为返回值
	- (instancetype)initWithName:(NSString *)name;

	instancetype和id的区别在于， id可以声明对象 也可以作为返回值，
	instancetype只能作为返回值。

##### 16. OC如何对内存管理的，说说你的看法和解决方法。

	Objective-C的内存管理主要有三种方式ARC(自动内存计数)、手动内存计数、内存池。
	　　1. 自动内存计数ARC：由Xcode自动在App编译阶段，在代码中添加内存管理代码。
	　　2. 手动内存计数MRC：遵循内存谁申请，谁添加。谁释放的原则。
	　　3. 内存释放池 Release Pool：分为手动和自动两种
	　　	* 把需要同一时间点释放的内存统一放在一个池子中，当想要把池子中的对象释放时，
	　　	  可以手动调用drain方法。
	　　	* 池子也可以由系统来决定何时被释放，取决于runloop机制。
	　　	例如：[[[NSObject alloc] init] autorelease];
	　　			@autoreleasepool { 对象放在这里 }

##### 17. 你对@interface中的成员变量和@property声明的属性的理解。

	@interface AA: NSObject{
		NSString *_name; //成员变量
	}
	@property NSString *sex; //属性
	
	如上所示：
	属性拥有setter和getter方法 外加_sex成员变量。
	_name只是成员变量， 没有setter和getter方法。

##### 18. do while 和while do的区别？

	while do是先判断while中的表达式的真假，再执行循环。
	do while先进行循环一次，再判断while中的表达式的真假。

##### 19. 用预处理指令#define声明一个常数，用以表明一年中有多少秒(忽略闰年问题)。

	#define SECONDS_PER_YEAR (60 * 60 * 24 * 365)

##### 20. 浅拷贝和深拷贝的区别？


	浅拷贝：只复制指向对象的指针，而不复制引用对象本身。
	深拷贝：复制引用和对象本身。
	意思就是说我有个A对象，复制一份后得到A_copy对象后，
	对于浅复制来说，A和A_copy指向的是同一个内存资源，复制的只不过是是一个指针，
	对象本身资源还是只有一份。
	那如果我们对A_copy执行了修改操作,那么发现A引用的对象同样被修改，
	这其实违背了我们复制拷贝的一个思想。
	深复制就好理解了,内存中存在了两份独立对象本身。 	
	用网上一哥们通俗的话将就是： 	
	浅拷贝好比你和你的影子，你完蛋，你的影子也完蛋 	
	深拷贝好比你和你的克隆人，你完蛋，你的克隆人还活着。

##### 21. 类别的作用？继承和类别在实现中有何区别？

	Category可以向类中添加新的方法，或者重写已有方法。
	正常情况下不可以添加属性。但是实际应用中可以通过runtime机制添加属性。
	类别主要有3个作用：
	- 将类的实现分散到多个不同文件或多个不同框架中。降低耦合性。
	- 重写主类方法
	- 向类中添加协议，属性，方法。
	
	继承主要作用：
	- 重写父类方法
	- 在父类基础上增加属性，方法，协议
	
	区别：继承使用时，需要使用子类。 Category使用时只需要引入头文件。

##### 22. 我们说的OC是动态运行时语言是什么意思？

	`编译时`等价于`编码时`, `编码时`就是程序员写的代码的样子. 程序员为一个类编写代码, 
	便可以为一个类添加 “成员变量(实例变量)” . 程序员也可以在一个类中写一些函数, 被称作“方法”.
	 运行前编译, 编译器会把程序员写的代码编译成可执行文件, 里面便有之前写的类的信息, 
	 包括`实例变量`和`方法`, 这些信息并不能组成一个实际的数据类型. 
	 程序运行后, 会将这些信息拼凑成一个结构体, 这个结构体便是一个数据类型. 
	 同时, 在运行期间, 数据类型可以改变, 表现为:
		
	1. 可以动态增添方法
	2. 可以动态增添实例变量
	等等..做一些运行时数据类型修改.
		
	一旦做了运行时修改, 就会使得这个结构体与程序员当初编写的类不一样.

##### 23. 为什么很多内置类如UITableView的delegate属性都是assign而不是retain ？

	如果是retain会引起循环引用。
	所有的引用计数系统，都存在循环引用的问题。例如下面的引用关系：
	对象a创建并引用了对象b,对象b创建并引用了对象c,对象c创建并引用了对象b.
	这时候b和c的引用计数分别是2和1。当a不再使用b，调用release释放对b的所有权，因为c还引用了b。
	所以b的引用计数为1，b不会被释放。b不释放，c的引用计数就是1，c也不会被释放。
	从此，b和c永远留在内存中。
	
	这种情况，必须打断循环引用，通过其他规则来维护引用关系。
	比如，我们常见的delegate往往是assign方式的属性而不是retain方式的属性，
	赋值不会增加引用计数，就是为了防止delegation两端产生不必要的循环引用。
	如果一个UITableViewController对象a通过retain获取了UITableView对象b的所有权，
	这个UITableView对象b的delegate又是a，如果这个delegate是retain方式的，
	那基本上就没有机会释放这两个对象了。

##### 24. 什么时候用delegate，什么时候用Notification？

	Delegate(委托模式):
	1对1的反向消息通知功能。
	
	Notification(通知模式):
	只想要把消息发送出去，告知某些状态的变化。但是并不关心谁想要知道这个。

##### 25. 什么是KVC和KVO？

	KVC（Key-Value-Coding）:键 - 值编码是一种通过字符串间接访问对象的方式。
	而不是通过调用setter方法或通过实例变量访问的机制。很多情况下可以简化程序代码。
	例如：
	
	@interface MeiLing:NSObject
	
	@property NSString *name;
	@property UILabel *label;
	
	@end
	
	对于name的赋值 可以使用 meiLing.name = @"笑玲"; 这是点语法。调用的是setName方法。
	KVC的写法是  [meiLing setValue:@"梦玲" forKey:@"name"];  通过name字符串赋值。
	
	当然也可以跨层赋值，例如为label的text属性赋值
	点语法： meiLing.label.text = @"笑玲";
	KVC： [meiLing setValue:@"梦玲" forKeyPath:@"label.text"];

	KVO:键值观察机制，他提供了观察某一属性变化的方法，极大的简化了代码。
		KVO 只能被 KVC触发， 包括使用setValue：forKey：方法 和 点语法。
	通过下方方法为属性添加KVO观察
	- (void)addObserver:(NSObject *)observer 
					forKeyPath:(NSString *)keyPath 
					options:(NSKeyValueObservingOptions)options 
					context:(nullable void *)context;
	当被观察的属性发生变化时，会自动触发下方方法
	- (void)observeValueForKeyPath:(NSString *)keyPath 
							 ofObject:(id)object 
								 change:(NSDictionary *)change 
								context:(void *)context

##### 26. 设计模式是什么？你知道哪些设计模式，并简要叙述。

	- 单例模式：通过static关键词，声明全局变量。在整个进程运行期间只会被赋值一次。
	- 观察者模式：KVO是典型的通知模式，观察某个属性的状态，状态发生变化时通知观察者。
	- 委托模式：代理+协议的组合。实现1对1的反相传值操作。
	- 工厂模式：通过一个类方法，批量的根据已有模板生产对象。
	- MVC模式：Model View Control， 把模型 视图 控制器 层进行解耦合编写。
	- MVVM模式：Model View ViewModel 把 模型 视图 业务逻辑 层进行解耦合编写。

##### 27. 描述一下iOS SDK中如何实现MVC的开发模式。

	MVC即  Model-View-Control 
	Model称为模型层，主要负责数据结构，业务逻辑相关的操作
	View 称为视图层，主要负责视图的展示
	Control 称为控制层，主要负责把View和Model层结合起来的操作。例如点击视图上的某个按钮
	要执行Model层中的某个业务逻辑。 或者把Model中的数据展现在视图上。

##### 28. ViewController的didReceiveMemoryWarning是在什么时候调用的？默认的操作是什么？

		当系统内存不足时，首先UIViewController的didReceiveMemoryWarining方法会被调用。
		默认操作如果当前控制器不是window的根视图控制器，会自动将self.view释放。  

##### 29. delegate和Block的区别？

	delegate：
	- 需要定义协议方法并且实现协议方法，会使代码结构变复杂
	- 效率没有block高
	
	block：
	- 代码结构更加紧凑，不需要额外定义方法。
	- 需要注意防止循环引用，使用__weak 关键词修饰
	- 当需要在块中修改外部变量时，需要对外部变量使用__block 关键词修饰

##### 30. frame和bounds有什么不同？

	frame指的是：该view在父view坐标系统中的位置和大小。（参照点是父亲的坐标系统）
	bounds指的是：该view在本身坐标系统中 的位置和大小。（参照点是本身坐标系统）

##### 31.ViewController生命周期

	按照执行顺序排列
 	- initWithCoder：通过nib文件初始化时触发
	- awakeFromNib：nib文件被加载的时候，会发送一个awakeFromNib的消息到nib文件中的每个对象
	- loadView：开始加载视图控制器自带的view
	- viewDidLoad：视图控制器的view被加载完成
	- viewWillAppear：视图控制器的view将要显示在window上
	- updateViewConstraints：视图控制器的view开始更新AutoLayout约束
	- viewWillLayoutSubviews：视图控制器的view将要更新内容视图的位置
	- viewDidLayoutSubviews：视图控制器的view已经更新视图的位置
	- viewDidAppear：视图控制器的view已经展现到window上
	- viewWillDisappear：视图控制器的view将要从window上消失
	- viewDidDisappear：视图控制器的view已经从window上消失

##### 32. 如何将产品进行多语言发布，开发？

	国际化操作
	在Xcode中，Project里，找到Localization，点击+号，添加想要支持的语言
	通过新建Strings文件，把文件进行国际化处理。 通过键值对的形式，同一个key在不同的国际化
	文件中，对应不同的值。
	通过 NSLocalizedStringFromTable 等方法，通过key来自动根据iOS设备的当前语言，
	显示不同的字符串。

##### 33. OC中是如何实现线程同步的？
	@synchronized: 添加同步锁
	NSLock：加锁
	NSCondition：加条件锁
	dispatch_async(dispatch_get_main_queue(), ^{}); :异步主线程
	NSOperationQueue：添加线程依赖
	NSOperationQueue：设置最大并发数为1

##### 34. UDP和TCP的区别是什么？
	1.基于连接与无连接；
	2.对系统资源的要求（TCP较多，UDP少）；
	3.UDP程序结构较简单；
	4.流模式与数据报模式 ；
	5.TCP保证数据正确性，UDP可能丢包，TCP保证数据顺序，UDP不保证

##### 35. TCP/IP建立连接的过程？

	- 在TCP/IP 协议中，TCP协议提供可靠的连接服务，采用三次握手建立连接；
	- 第一次握手：建立连接时，客户端发送连接请求到服务器，并进入SYN_SEND状态，等待服务器确认；
	- 第二次握手：服务器收到客户端连接请求，向客户端发送允许连接应答，
	  此时服务器进入SYN_RECV状态；
	- 第三次握手：客户端收到服务器的允许连接应答，向服务器发送确认，客户端和服务器进入通信状态，
	  完成三次握手。
	 （所谓的三次握手，就是要有三次连接信息的发送、接收过程。
	   TCP连的建立需要进行三次连接信息的发送、接收。）

##### 36. 编程中，保存数据有哪几种方式？

	- 数据：Sqlite。 操作方式分为原生的sqlite3，FMDB，Coredata
	- 归档：Archive。 自定义类型需要注意遵循NSCoding协议
	- Plist：就是数组或字典，写入文件后的表现形式。
	- NSUserDefault：本质上就是Plist。
	- 写文件
	- 上传到服务器

##### 37. 介绍版本控制中Git与SVN。

	1、Git是一款免费、开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。
		主要区别于SVN工具的功能是 分支功能比SVN强大。 （常用）
	2、SVN是Subversion的简称，是一个开放源代码的版本控制系统，它采用了分支管理系统，
	   它的设计目标就是取代CVS。

##### 38. OC中创建线程的方法是什么？如果在主线程中执行代码，方法是什么？如果想延时执行代码，方法又是什么？

	创建线程的方法
	- [NSThread detachNewThreadSelector:nil toTarget:nil withObject:nil]
	- [self performSelectorInBackground:nil withObject:nil];
	- [[NSThread alloc] initWithTarget:nil selector:nil object:nil];
	- dispatch_async(dispatch_get_global_queue(0, 0), ^{});
	- [[NSOperationQueue new] addOperation:nil];

	主线程中执行代码的方法
	- [self performSelectorOnMainThread:nil withObject:nil waitUntilDone:YES]
	- dispatch_async(dispatch_get_main_queue(), ^{});
	- [[NSOperationQueue mainQueue] addOperation:nil];

	延迟执行代码
	sleep(2)  睡两秒钟
	NSTimer启动定时器

##### 39. iOS中有哪些多线程方案？

	常用的有三种: NSThread NSOperationQueue GCD。
		1、NSThread 是这三种范式里面相对轻量级的，但也是使用起来最负责的，
		你需要自己管理thread的生命周期，线程之间的同步。线程共享同一应用程序的部分内存空间，
		它们拥有对数据相同的访问权限。你得协调多个线程对同一数据的访问，
		一般做法是在访问之前加锁，这会导致一定的性能开销。
		2、NSOperationQueue 以面向对象的方式封装了用户需要执行的操作，
		我们只要聚焦于我们需要做的事情，而不必太操心线程的管理，同步等事情，
		因为NSOperation已经为我们封装了这些事情。 
		NSOperation 是一个抽象基类，我们必须使用它的子类。
		3、 GCD: iOS4 才开始支持，它提供了一些新的特性，以及运行库来支持多核并行编程，
		它的关注点更高：如何在多个cpu上提升效率。
		
		总结：
		- NSThread是早期的多线程解决方案，实际上是把C语言的PThread线程管理代码封装成OC代码。
		- GCD是取代NSThread的多线程技术，C语法+block。功能强大。
		- NSOperationQueue是把GCD封装为OC语法，额外比GCD增加了几项新功能。
			* 最大线程并发数
			* 取消队列中的任务
			* 暂停队列中的任务
			* 可以调整队列中的任务执行顺序，通过优先级
			* 线程依赖
			* NSOperationQueue支持KVO。 这就意味着你可以观察任务的状态属性。
		但是NSOperationQueue的执行效率没有GCD高，所以一半情况下，我们使用GCD来完成多线程操作。

##### 40. 线程与进程的区别和联系?

	1.什么是进程 
	进程是指在系统中正在运行的一个应用程序
	每个进程之间是独立的，每个进程均运行在其专用且受保护的内存空间内
	
	2.什么是线程
	1个进程要想执行任务，必须得有线程（每1个进程至少要有1条线程）
	线程是进程的基本执行单元，一个进程（程序）的所有任务都在线程中执行。

##### 41. 堆和栈的区别？
	　　一、堆栈空间分配区别：
	　　1、栈（操作系统）：由操作系统自动分配释放 ，存放函数的参数值，局部变量的值等。
	　　其操作方式类似于数据结构中的栈；
	　　2、堆（操作系统）： 一般由程序员分配释放， 若程序员不释放，程序结束时可能由OS回收，
	　　分配方式倒是类似于链表。
	　　
	　　二、堆栈缓存方式区别：
	　　1、栈使用的是一级缓存， 他们通常都是被调用时处于存储空间中，调用完毕立即释放；
	　　2、堆是存放在二级缓存中，生命周期由虚拟机的垃圾回收算法来决定。
	　　所以调用这些对象的速度要相对来得低一些。
	　　
	　　三、堆栈数据结构区别：
	　　堆（数据结构）：堆可以被看成是一棵树，如：堆排序；
	　　栈（数据结构）：一种先进后出的数据结构。

##### 42. 关键字`const`有什么含义？

	const修饰的数据类型是指常类型，常类型的变量或对象的值是不能被更新的。

##### 43. 简单阐述`static`关键字的作用



	

[^PointSyntax]: 点语法: "self.属性 = obj" 调用属性的setter方法。"self.属性" 调用属性的getter方法区别在于是否有等号

[^MRC]: MRC:手动内存释放。遵循谁申请谁释放的原则，需要手动的处理内存计数的增加和修改。从12年开始，逐步被ARC(自动内存释放)模式取代。