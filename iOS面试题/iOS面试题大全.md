# iOS面试题大全

###### 2015-12-28 达内纪老师

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
			[name retain];
			[_name release];	//把之前指针指向的内容内存计数-1
			_name = name; //指向新内容，内存计数+1
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
	   alloc，new   内存计数 = 1
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


##### 10. #import和#include有什么区别？@class呢？#import<>和#import” ”有什么区别？

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
	4.retain 表示持有特性，setter方法将传入参数先保留，再赋值，传入参数的retaincount会+1;
	5.copy 表示拷贝特性，setter方法将传入对象复制一份；需要完全一份新的变量时。
	6.nonatomic 非原子操作，决定编译器生成的setter和getter方法是否是原子操作。
		- atomic表示多线程安全，需要对方法加锁，保证同一时间只有一个线程访问属性，
		  因为有等待过程，所以影响执行效率
		- 一般使用nonatomic。不加锁。效率会更高。但是线程不安全。

##### 12. 写一个setter方法用于完成@property(nonatomic, retain)NSString *name, 写一个setter方法用于完成@property(nonatomic, copy)NSString *name

	- (void) setName:(NSString*) str //retain
	　{
	　　[str retain];
	　　[name release];
	　　name = str;
	　}
	- (void)setName:(NSString *)str //copy
	　{
	　　id t = [str copy];
	　　[name release];
	　　name = t;
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
	
	可以看到，在64位操作系统上，NSInteger是 C语言的long类型。 在32位操作系统上，则是int类型。








[^PointSyntax]: 点语法: "self.属性 = obj" 调用属性的setter方法。"self.属性" 调用属性的getter方法区别在于是否有等号

[^MRC]: MRC:手动内存释放。遵循谁申请谁释放的原则，需要手动的处理内存计数的增加和修改。从12年开始，逐步被ARC(自动内存释放)模式取代。



















