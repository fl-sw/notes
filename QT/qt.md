* explicit关键字  
构造函数被explicit修饰后, 就不能再被隐式调用了

* friend
友元函数：给函数访问我私有成员的权限
友元类：给类访问我私有成员的权限

* new A 与new A() 的区别:  
    * POD是Plain old data的缩写，它是一个struct或者类，且不包含构造函数、析构函数以及虚函数。
    * POD class必须是一个aggregate  
        (aggregate:没有用户定义的构造函数，没有私有的或者保护的非静态数据，没有基类或虚函数。它只是一些字段值的集合，没有使用任何封装以及多态特性)
    * C++中的三种初始化方式：
        zero-initialization , default-initialization , value-initialization。
    * 在所有C++版本中，只有当A是POD类型的时候，new A和new A()才会有区别。而且，C++98和C++03会有区别。




Q_OBJECT //宏，允许类中使用信号和槽机制

return a.exec();	//让应用程序进入消息循环机制

对象树：构造时，从上向下；析构时，从下向上
在qt中，只要对象指定了父亲，都会在对象树中管理它的释放

## 信号和槽  
* connect（信号的发送者，信号的内容(信号函数)，信号的接收者，接收者对信号的处理方法（槽函数））  
* 信号槽的优点：松散耦合-》<u>信号的发送端</u> 和 <u>信号的接收端</u>  本身是没有关系的;通过connect将两者耦合在一起.
* `connect(btn, &QPushButton::clicked, this, &QMainWindow::close);`

## 自定义信号槽:
https://www.cnblogs.com/schips/p/12537360.html  
在程序中不可把信号与常规函数连接在一起，否则信号的释放不会引起对应函数的执行

* 信号:signals
    * 自定义信号写到signals: 下面
    * 返回值是void,只要声明,不要定义
    * 可以有参数,可以重载

* 槽:slots
    * 与信号函数不同，槽函数必须自己完成实现代码
    * 可以有参数,可以重载

* 触发:emit 
    * 发射信号

* 注:
    * siganls 没有 public、private、protected 等属性，这点不同于 slots。另外，signals、slots 关键字是 QT 自己定义的，不是 C++ 中的关键字（其实也是一个宏）
    * 发送者和接收者都需要是QObject的子类（当然，槽函数是全局函数、Lambda 表达式等无需接收者的时候除外）；
    * 使用 signals 标记信号函数，信号是一个函数声明，返回 void，不需要实现函数代码；
    * 槽函数是普通的成员函数，作为成员函数，会受到 public、private、protected 的影响；
    * 使用 emit 在恰当的位置发送信号；
    * 使用QObject::connect()函数连接信号和槽。 
    * 一个信号可以对应多个槽（但是槽的执行顺序不可控）
    * N个信号可以对应一个槽
    * 信号还可以连接信号

* 当信号和槽发生重载时
    * 需要用定义函数指针来作为connect()的参数
    * 定义与槽相同类型的函数指针，而且指针要标明 域名；
    * ```cpp
        void(Teacher:: *noPraTeacherSignal)() = &Teacher::hungry;
	    void(Teacher:: *isPraTeacherSignal)(QString) = &Teacher::hungry;

	    void(Student:: *noPraStudentSignal)() = &Student::treat;
	    void(Student:: *isPraStudentSignal)() = &Student::treat;
        ```

* 信号和槽的参数必须一一对应
    * 类型要一一匹配
    * 信号的参数个数可以多于槽的参数个数


* QString 转成 char* 的方法
    * QString 先转成 QByteArray （用toUtf8()方法） 再转成char* （用data()方法）；
    * `qDebug() << "this is my" << str.toUtf8().data();`

* 断开信号连接
    * disconnect(); //参数和连接的参数一摸一样

* lambda表达式
    * `[](){};` 
    * 方括号是函数对象参数；标识一个lambda表达式的开始
    * []()mutable{}; //加了mutable后，在大括号中修改的就是传进来的参数的拷贝，而不是参数本身。
    * 返回值：`int ret = []()->{return 100;}();`
    * 可以在connect()的第四个参数的地方，写lambda表达式，在lambda表达式中调用想调用的函数（比如发射信号，调用槽函数）