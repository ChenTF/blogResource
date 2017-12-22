# 相关知识
## workspace — project — targets 讲解
一个工作空间可以包含多个项目，一个项目可以包含多个目标（生成物）。    
一个项目中根据运行的targets不同，可以进行不同的编译设置，project是基础父类，targets是子类，targets的设置会覆盖project的设置。


**与单元测试的关系**

单元测试是在一个新的target上进行的设置，这样就不会影响程序开发，编译。    
在XCode7中创建一个项目时默认是选中创建测试target的，如果没有，创建方法如下：File -> New -> target -> UITest/UnitTest，创建完成后会自动创建对应的文件夹。

# 1. Unitest

XCTest是苹果官方提供的测试类, 只需要引入`#import <XCTest/XCTest.h>` 就可以使用.
## 1.1 XCTest使用示例
使用该UnitTest测试一些代码逻辑，使用UITest测试UI的点击交互逻辑。
### 1.1.1 测试目标代码


1.1.2. 创建测试类

说明：
* 任何测试类都需要继承自 XCTestCase 类
* setUp，tearDown是系统默认方法
* 命名：测试的目标类名+Tests

3.编写测试方法

说明：
* 测试方法必须以testXXX开头，Xcode会自动识别出所有的测试方法
* 在一个类中测试方法的调用顺序是按照方法的顺序来调用的，本例中1 -> 2 -> 3。
* 需要测试某些逻辑性操作时，要主要测试方法的编写顺序，比如 1先插入10条数据 -> 2根据id查询数据 -> 3查询所有数据
* 每个测试方法是分开到，调用顺序 setUp -> test1 -> tearDown, setUp -> test2 -> tearDown，这样保证各个测试用例直接不会互相影响
调用：
* 执行所有测试方法：command + u
* 只执行某个测试方法：点击方法前的菱形（目前是对号/错号）
* 执行某个类的所有测试方法：点击类前的菱形（目前是对号/错号）

4.测试结果与XCTAssert断言函数
     一个测试是否通过，是需要通过XCTassert类来进行验收的，XCTAssert是 一系列宏方法，提供了很多的判断（下面会列举）。
     如果通过则是在方法前是√，没有打印；如果失败则方法前是×，打印。（看上图）

打印：


测试用例列表：


XCTAssert宏方法
参考地址：http://my.oschina.net/u/1418722/blog/340194?fromerr=RUMiSWBO
XCTFail(...)
任何尝试都会测试失败，...是输出的提示文字。（后面都是这样）

XCTAssertNil(expression, ...)
expression为空时通过，否则测试失败。
expression接受id类型的参数。

XCTAssertNotNil(expression, ...)
expression不为空时通过，否则测试失败。
expression接受id类型的参数。

XCTAssert(expression, ...)
expression为true时通过，否则测试失败。
expression接受boolean类型的参数。

XCTAssertTrue(expression, ...)
expression为true时通过，否则测试失败。
expression接受boolean类型的参数。

XCTAssertFalse(expression, ...)
expression为false时通过，否则测试失败。
expression接受boolean类型的参数。

XCTAssertEqualObjects(expression1, expression2, ...)
expression1和expression1地址相同时通过，否则测试失败。
expression接受id类型的参数。

XCTAssertNotEqualObjects(expression1, expression2, ...)
expression1和expression1地址不相同时通过，否则测试失败。
expression接受id类型的参数。

XCTAssertEqual(expression1, expression2, ...)
expression1和expression1相等时通过，否则测试失败。
expression接受基本类型的参数（数值、结构体之类的）。

XCTAssertNotEqual(expression1, expression2, ...)
expression1和expression1不相等时通过，否则测试失败。
expression接受基本类型的参数。

XCTAssertEqualWithAccuracy(expression1, expression2, accuracy, ...)
expression1和expression2之间的任何值都大于accuracy时，测试失败。
expression1、expression2、accuracy都为基本类型。

XCTAssertNotEqualWithAccuracy(expression1, expression2, accuracy, ...)
expression1和expression2之间的任何值都小于等于accuracy时，测试失败。
expression1、expression2、accuracy都为基本类型。

XCTAssertGreaterThan(expression1, expression2, ...)
expression1 <= expression2时，测试失败。
expression为基本类型

XCTAssertGreaterThanOrEqual(expression1, expression2, ...)
expression1 < expression2时，测试失败。
expression为基本类型

XCTAssertLessThan(expression1, expression2, ...)
expression1 >= expression2时，测试失败。
expression为基本类型

XCTAssertLessThanOrEqual(expression1, expression2, ...)
expression1 > expression2时，测试失败。
expression为基本类型

XCTAssertThrows(expression, ...)
expression没抛异常，测试失败。
expression为一个表达式

XCTAssertThrowsSpecific(expression, exception_class, ...)
expression没抛指定类的异常，测试失败。
expression为一个表达式
exception_class为一个指定类

XCTAssertThrowsSpecificNamed(expression, exception_class, exception_name, ...)
expression没抛指定类、指定名字的异常，测试失败。
expression为一个表达式
exception_class为一个指定类
exception_name为一个指定名字

XCTAssertNoThrow(expression, ...)
expression抛出异常时，测试失败。
expression为一个表达式

XCTAssertNoThrowSpecific(expression, exception_class, ...)
expression抛出指定类的异常，测试失败。
expression为一个表达式

XCTAssertNoThrowSpecificNamed(expression, exception_class, exception_name, ...)
expression抛出指定类、指定名字的异常，测试失败。
expression为一个表达式
exception_class为一个指定类
exception_name为一个指定名字


测试环境的配置
1.Target Membership
参考：http://www.cnblogs.com/graphics/p/4117353.html

Target membership是指XCode中，一个文件属于哪一个工程，在XCode左侧的工程面板中选中一个文件，在XCode右侧的属性面板中会显示其Target Membership，如下图。
当前的文件AppDelegate.m属于书谱这个Target。

Target Membership的一些属性。
* .h  文件没有Target Membership
* 文件夹引用有Target Membership，其子文件继承该文件夹的Target Membership。但面板中不显示子文件的Target Membership。

以前遇到一个错误，就是UIImage创建的时候返回nil，仔细查看发现，图片的Target Membership选项没有勾上。这个错误比较难以发现，特此记之。

2.Link Binary With Libraries
     在测试本地存储是，如果需要一些二进制文件的支持，则test targert也需要引入相应的文件（配置和正常项目需一样）。




3.设置本地的支持文件路径

提醒：
     每次修改完配置文件，建议先Clean（Command+Shift+K）缓存，再编译。

4.PCH
     pch 和main target设置成一直, 注意Precompile Prefix Header选项


5.Pods设置
     当项目中有pod时, 在测试文件中引用pods的文件, 提示找不到, 错误如下:

     
     解决方案: 设置PROJECT的Configurations


6.plist设置
     两种方案
     一: 设置plist文件与build一致
     
     二: 将info.plist路径改成build target的路径


代码