# objc0909
Category extension protocal
Objective-c中提供了可以让我们扩展类定义的手段：类目，延展和协议。类目：为已知的类增加新的方法；延展：通知在本类的定义里使用类目来声明私有方法；协议：协议声明了可以被任何类实现的方法。
注意：这些手段只能增加类的方法，并不能用于增加实例变量，要增加类的实例变量，只能通过定义子类来间接实现。
1、类目
1)声明类目
@interface NSString (NumberConvenience)
-(NSNumber *)lengthAsNumber;
@end
该声明具有2个特点。首先，现有类位于@interface关键字之后，其后是位于圆括号中的一个新名称。该声明表示，类别的名称是NumberConvenience，而且该类别将向NSString类中添加方法。只要保证类别名称的唯一性，你可以向一个类中添加任意多得类别。
其次，你可以指定希望向其添加类别的类以及类别的名称，而且你还可以列出添加的方法，最后以@end结束。由于不能添加新实现变量，因此与类声明不同的是，类别的声明中没有实例变量部分。
2)类目的局限性
第一，无法向类中添加新的实例变量。类别没有位置容纳实例变量。
第二，名称冲突，即类别中得方法与现有的方法重名。当发生名称冲突时，类别具有更高的优先级。你得类别方法将完全取代初始方法，从而无法再使用初始方法。有些编程人员在自己的类别方法中增加一个前缀，以确保不发生名称冲突。
有一些技术可以克服类别无法增加新实例变量的局限。例如，可以使用全局字典存储对象与你想要关联的额外变量之间的映射。但此时你可能需要认真考虑一下，类别是否是完成当前任务的最佳选择。
3)类目的作用
cocoa中得类别主要用于3个目的：第一，将类的实现分散到不同文件或者不同框架中。第二，创建对私有方法的前向引用。第三，向对象添加非正式协议。
2、延展
类的延展可以看作是一种匿名的类目，类有时需要一些只为自己所见，所用的私有方法这种私有方法可以通过延展的方式来声明，延展中定义的方法在类本身的@implementation代码区域中进行实现。
定义延展
@interface MyObject : NSObject
 {     
NSNumber *number; }
 - (NSNumber *)number;
 @end @interface MyObject (Setter)
 - (void)setNumber:(NSNumber *)newNumber;
@end
 @implementation MyObject
 - (NSNumber *)number
{     
return number;
 }
- (void)setNumber:(NSNumber *)newNumber
{!//do something
}
@end
当在定义延展的时候不提供类目名时，延展中定义的方法既被视为“必须实现”的API在这种情况下，如果方法缺少实现代码，则编译器会报警告，此时方法的实现必须出现在类主体的@implementation代码块中。
3、协议和代理模式
协议只声明了方法，不具体实现，接受协议的对象负责实现。OC的协议是由@protocol声明的一组方法列表，要求其它的类去实现，相当于@interface部分的声明。
注意：
a.确认协议时应实现协议中 @required 修饰的方法
b.可以选择性实现 @optional 修饰的方法
c.使用[对象 conformsToProtocol:@protocol(Protocol)]判断是否遵循协议
d.协议写在提供协议类的.h文件里
协议的应用--代理
代理模式即本身不做时间的事情，而是要求其他人去做。
