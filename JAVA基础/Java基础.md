# Java基础

JVM：运行字节码的虚拟机，是字节码跨平台的实现，针对不同系统不同实现

JRE：运行时环境，运行已编译java程序的工具集，包括JVM，JAVA类库和其他一些基础构件，不能用于创建新程序

JDK：包括JRE，还包括编译器javac 和一些其他工具 jdb、javadoc，可以创建和编译程序

字节码：.class文件

![Untitled](Java/Untitled.png)

JVM的解释器逐行解释执行字节码（一句一句解释），后来又引入JIT运行时编译器（一次性编译完成），第一次编译完成后会把对应的机器码保存下来，对于热点代码进行编译，其他代码解释

编译与解释并存：因为 Java 程序要经过先编译，后解释两个步骤，由 Java 编写的程序需要先经过编译步骤，生成字节码（`.class`文件），这种字节码必须由 Java 解释器来解释执行。

java和c++的异同：

1、都是面向对象：封装、继承、多态

2、java 没有指针，内存安全

3、java类是单继承，更没有菱形继承，只是使用接口支持多继承

4、有GC机制，不需要析构函数来手动释放内存

5、java没有操作符重载

可变参数...：(String... s) 实际被读入为一个数组 String[] s1 = s; 是一种语法糖

![Untitled](Java/Untitled%201.png)

重载和重写：

重载（编译器）就是同样的一个方法能够根据输入数据的不同，做出不同的处理

重写（运行期）就是当子类继承自父类的相同方法，输入数据一样，但要做出有别于父类的响应时，你就要覆盖父类方法

1. 方法名、参数列表必须相同，**子类方法返回值类型应比父类方法返回值类型更小或相等，抛出的异常范围小于等于父类，访问修饰符范围大于等于父类。**
2. 如果父类方法访问修饰符为 `private/final/static` 则子类就不能重写该方法，但是被 `static` 修饰的方法能够被再次声明。
3. 构造方法无法被重写

**静态方法和变量可以被继承，但是会隐藏，不能被重写（子类可以写相同名字相同参数，但是不能加override，静态方法是属于类的），调用的时候如果子类实现了同名的方法就用子类的，否则调用隐藏的父类静态方法**

匿名内部类也就是没有名字的内部类正因为没有名字，所以匿名内部类只能使用一次，通常用来简化代码编写

![Untitled](Java/Untitled%202.png)

如果类没有重写equals方法，则等价于用”==”来比较类的对象。

String是有常量池的，因此创建String时会在常量池中查找有没有已经存在的值和要创建的值相同的对象，如果有就把它赋给当前引用。如果没有就在常量池中重新创建一个

基本类型和对应的引用类型用==比较，引用对象会进行自动拆箱再数值计算

字符串+解析  编译期可以确定下来的常量就会引用常量池，变量的话会调用Stringbuilder.append.tostring new一个[https://blog.nowcoder.net/n/3a3eb4670c974aa58b1276f59a2156a1](https://blog.nowcoder.net/n/3a3eb4670c974aa58b1276f59a2156a1)

**equals()与HashCode**()：

Hashcode()默认是返回类内存地址的哈希值，equals()默认是比较两个类的内存地址是否相同，所以想通过类的属性判断两个实例是否相同时，就需要重写equals()方法，而当对该类进行一些hash有关的容器操作时(如hashmap，hashset)，必须要重写hashcode()，否则会出现属性相同的类却被重复加入set中，因为这些容器是先比较hashcode（），如果有hashcode相同则继续调用equals()方法进一步检查是否相同(这样减少了equals的调用次数，提高了执行速度）。 所以这就是为什么重写equals一定也要重写hashcode → 为了在和hash相关的集合中有正确表现

总结就是：equals相同，那么hashcode一定要相同。hashcode相同，equals不一定相同(哈希碰撞)

**为什么hashcode方法大多选择31作为生成hash值的乘数**：

一、31=(1 << 5) - 1，可以被JVM优化

 二、31是质数中一个“不大不小”的存在，如果使用的是一个如2的较小质数，
那么得出的乘积会在一个很小的范围，很容易造成哈希值的冲突。而如果选择一个100以上的质数，得出的哈希值会超出int的最大范围，这两种都不合适。

8种基本数据类型

**需要注意的是java的char是2个字节**，和c++不同，char的包装类型为Character

**包装类型不赋值默认为null（防止NPE）**，基本类型不赋值有默认值

POJO类的成员变量建议用包装类型，而局部变量推荐用基本数据类型（没有NPE问题，在虚拟机栈上速度快）

![Untitled](Java/Untitled%203.png)

数值类型的包装类型基本都有常量池: Integer Long[-128,127] Character[0,127]

![Untitled](Java/Untitled%204.png)

基本类型可以用==比较，但是包装类型一定要用equals比较，哪怕有常量池

![Untitled](Java/Untitled%205.png)

基本类型的运算会会发生小字节类型向大字节类型转换的现象。对于short，byte,char 比int 字节数小的变量类型来说，运算结果会自动转换为int类型。Java编译器会在编译期或者运行期将byte和short类型的数据带符号扩展为相应的int类型数据，将boolean和char类型数据零位扩展为相应的int类型数据。因此，在处理boolean 、byte、short 和 char 类型的数组是，也会用相应的int类型的字节码指令来处理。因此，大多数对于上述类型数据的操作，实际上都是使用相应的 int 类型作为运算类型

![Untitled](Java/Untitled%206.png)

a+=b和a=a+b的区别：

![Untitled](Java/Untitled%207.png)

还有一个区别是+=会进行隐式类型转换，而a+b不会，所以对于short byte char这种进行运算时会转换为int运算的，a=a+b会报错，因为没法把int转为这些类型

![Untitled](Java/Untitled%208.png)

double float浮点数为什么不准确，bigdecimal怎么解决的：

IEEE754标准，尾数决定精度，近似数

bigdecimal把小数转换为大数运算，所以进行除法时如果出现无限循环小数就会抛出异常

[https://cloud.tencent.com/developer/article/1468551](https://cloud.tencent.com/developer/article/1468551)

构造方法没有返回值，但不能用 void 声明构造函数。且默认会有无参构造方法，但是如果添加了有参构造方法，就不会自动生成无参构造方法，因此如果重载了有参的构造方法，记得要把无参的构造方法也写出来（因为子类实例化时如果子类构造函数没有用super()指定调用父类的有参构造函数，会默认调用父类的无参构造函数，所以每一个类必须要有一个无参的构造函数）

访问权限：不加修饰符，表示包级可见，

protected只能修饰成员，因为在继承体系中表示成员对于子类可见

接口从java8开始可以拥有默认的方法实现，接口的字段和方法默认是public的，并且不能为private或protected，并且默认为static和final的

final:类不能被继承，方法不能被子类重写，基本类型不能变，引用类型不能再引用其他对象

面向对象三大特征：

封装：对象的属性隐藏在内部，不允许直接访问，提供一些方法来操作和访问

继承：

多态：表示一个对象具有多种的状态，具体表现为父类的引用指向子类的实例

![Untitled](Java/Untitled%209.png)

**深拷贝和浅拷贝及引用拷贝**：

![Untitled](Java/Untitled%2010.png)

![Untitled](Java/Untitled%2011.png)

**实现深拷贝的方法**：

实现cloneable接口，就可以调用object的clone方法进行浅拷贝，如果要深拷贝就重写clone方法，自定义深拷贝过程

也可以用构造函数进行深拷贝，需要手写拷贝构造函数

或者使用序列化，先将源对象进行序列化，再反序列化生成拷贝对象，也是深拷贝，不过需要类和成员类都实现Serializable接口

String为什么不能修改：虽然final修饰了 byte[]数组（final修饰的基本类型不能改变，修饰的引用类型不能再指向其他对象），但这个不是string不能修改的原因（引用还是可以变的)，原因是数组是private的且没有提供修改方法，而String类被final修饰不能被继承，所以不会有子类破坏String不可变性

![Untitled](Java/Untitled%2012.png)

![Untitled](Java/Untitled%2013.png)

1. 操作少量的数据: 适用 `String`
2. 单线程操作字符串缓冲区下操作大量数据: 适用 `StringBuilder`
3. 多线程操作字符串缓冲区下操作大量数据: 适用 `StringBuffer`

Java中只有值传递没有引用传递的原因：

![Untitled](Java/Untitled%2014.png)

匿名内部类 lambda

枚举：本质是创建一个final类继承Enum类

![Untitled](Java/Untitled%2015.png)

```java
public enum Fruit{
    APPLE(1),ORANGE(2),BANANA(3);
    int code;

    Fruit(int code){
        this.code=code;
    }
}

```

反编译class：

```java
public final class Fruit extends Enum
{

    public static Fruit[] values()
    {
        return (Fruit[])$VALUES.clone();
    }

    public static Fruit valueOf(String s)
    {
        return (Fruit)Enum.valueOf(Fruit, s);
    }

    private Fruit(String s, int i, int j)
    {
        super(s, i);
        code = j;
    }

    public static final Fruit APPLE;
    public static final Fruit ORANGE;
    public static final Fruit BANANA;
    int code;
    private static final Fruit $VALUES[];

    static
    {
        APPLE = new Fruit("APPLE", 0, 1);
        ORANGE = new Fruit("ORANGE", 1, 2);
        BANANA = new Fruit("BANANA", 2, 3);
        $VALUES = (new Fruit[] {
            APPLE, ORANGE, BANANA
        });
    }
}
```

抽象类不一定有抽象方法

JDK 1.8允许给接口添加非抽象的方法实现，但必须使用default关键字修饰；定义了default的方法可以不被实现子类所实现，但只能被实现子类的对象调用

JDK 1.8中允许使用static关键字修饰接口的一个方法，并提供实现，称为接口静态方法。接口静态方法只能通过接口调用（接口名.静态方法名）。

**接口和抽象类：**

**相同点**

（1）都不能被实例化 （2）接口的实现类或抽象类的子类都只有实现了接口或抽象类中的方法后才能实例化。

**不同点**

（1）接口只有定义，不能有方法的实现，java 1.8中可以定义default方法体，而抽象类可以有定义与实现，方法可在抽象类中实现。

（2）一个类可以实现多个接口，但一个类只能继承一个抽象类。所以，使用接口可以间接地实现多重继承。

（3）接口强调特定功能的实现，而抽象类强调所属关系。

（4）**接口成员变量默认为public static final，必须赋初值，不能被修改**；其所有的成员方法都是public、abstract的。抽象类中成员变量默认default，可在子类中被重新定义，也可被重新赋值；抽象方法被abstract修饰，不能被private、static、synchronized和native等修饰，必须以分号结尾，不带花括号。

**泛型**：提供了编译时类型安全检测机制，是编译期去掉的语法糖（类型擦除），本质是参数化类型，也就是说所操作的数据类型被指定为一个参数。同一泛型类共用一份字节码，共用静态变量。

限定通配符-集合中要写用super（比如参数类型限定），要读用extends（比如返回类型限定），都要就不用：

extends X的集合是不能调用add的，只能在初始化时赋一个X的子类的List给他，所以extends只能读，读的时候可以取X及X的超类，类型擦除后擦除到X，

super X的集合可以add X或X的子类，不能add X的父类（因为不知道具体是X的哪个父类，所以不允许加入X的超类），读取的时候因为此时不知道是什么类型，所以只能返回object，因此super可以写不能读。类型擦除后擦除到Object

![Untitled](Java/Untitled%2016.png)

h[ttps://blog.csdn.net/w372426096/article/details/78081552](https://blog.csdn.net/w372426096/article/details/78081552)

[https://princeyao.blog.csdn.net/article/details/86674481?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromBaidu~Rate-2.queryctrv4&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromBaidu~Rate-2.queryctrv4&utm_relevant_index=5](https://princeyao.blog.csdn.net/article/details/86674481?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-2.queryctrv4&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-2.queryctrv4&utm_relevant_index=5)

因为是语法糖，所以运行期间可以通过反射添加非泛型类元素

![Untitled](Java/Untitled%2017.png)

项目哪里用到了泛型？todo

**反射**：赋予了我们在运行时分析类以及执行类中方法的能力。通过反射可以获取任意一个类的所有属性和方法，还可以调用这些方法和属性

class field method（每种都有getannotation方法获取注解）

类设置accessable为true可以访问和修改私有变量、方法

![Untitled](Java/Untitled%2018.png)

*getDeclaredMethod()*获取的是**类自身**声明的所有方法，包含**public**、**protected**和**private**方法。*getMethod()*获取的是类的所有共有方法，这就包括自身的**所有public**方法，和从基类继承的、从接口实现的所有public方法。

**反射原理**：invoke方法的底层原理：Method的invoke方法进行了一些权限检查，最后调用了MethodAccessor的invoke方法，MethodAccessor是一个接口，我们需要去acquireMethodAccessor（）中看究竟返回的是哪个具体实现，在这里面是调用ReflectionFactory的newMethodAccessor方法来生成一个对象，首先生成了一个NativeMethodAccessorImpl对象，再将它作为参数调用DelegatingMethodAccessorImpl类的构造方法，这里使用了代理模式，将NativeMethodAccessorImpl交给DelegatingMethodAccessorImpl类代理，所以最终ReflectionFactory类生成的是DelegatingMethodAccessorImpl对象，但这个对象其实本质是代理的NativeMethodAccessorImpl，所以调用的是NativeMethodAccessorImpl的invoke方法，而这个invoke方法会判断历史调用次数是否超过某个阈值，如果超过则生成一个新的MethodAccessorImpl对象，并使代理类DelegatingMethodAccessorImpl代理这个新对象。原因是实际的MethodAccessor有native和java两个版本，Native 版本（c++相关代码）一开始启动快，但是随着运行时间边长，速度变慢。Java 版本一开始加载慢，但是随着运行时间边长，速度变快。正是因为两种存在这些问题，所以第一次加载的时候我们会发现使用的是 NativeMethodAccessorImpl 的实现，而当反射调用次数超过 15 次之后，则使用 MethodAccessorGenerator 生成的 MethodAccessorImpl 对象去实现反射。

总结：invoke→reflectionFactory→delegatingMethodAccessorImpl→nativeMethodAccessorImpl→ 调用超过一定次数则MthodAccessorImpl

![Untitled](Java/Untitled%2019.png)

**静态代理和动态代理**：

[https://www.cnblogs.com/gonjan-blog/p/6685611.html](https://www.cnblogs.com/gonjan-blog/p/6685611.html)

[https://www.cnblogs.com/jingmoxukong/p/12049112.html](https://www.cnblogs.com/jingmoxukong/p/12049112.html)

静态代理就是代理模式，为其他对象提供一种代理以控制对这个对象的访问

![Untitled](Java/Untitled%2020.png)

静态代理模式固然在访问无法访问的资源，增强现有的接口业务功能方面有很大的优点，但是大量使用这种静态代理，会使我们系统内的类的规模增大，并且不易维护；并且由于 Proxy 和 RealSubject 的功能本质上是相同的，Proxy 只是起到了中介的作用，这种代理在系统中的存在，导致系统结构比较臃肿和松散。

为了解决静态代理的问题，引入了动态代理，运行状态中，需要代理的地方，根据 Subject 和 RealSubject，动态地创建一个 Proxy，用完之后，就会销毁，这样就可以避免了 Proxy 角色的 class 在系统中冗杂的问题

**动态代理**：核心-InvocationHandler和Proxy。invocationhandler这个接口只有一个invoke方法，它的实现充当一个代理类的作用，InvocationhandlerImpl包含一个被代理的类，使用invoke方法来进行方法调用（这里其实也有AOP的影子，通过InvocationHandler的invoke来在调用方法的前后进行切面操作）

![Untitled](Java/Untitled%2021.png)

我们创建InvocationhandlerImpl后，使用这个impl作为参数，调用Proxy的newProxyInstance静态方法来产生一个动态代理类，这个类继承了proxy并且实现了被代理的接口（jdk的动态代理只能代理接口，即实现了某个接口的类，对于没有实现任何接口的类，需要使用cglib的动态代理技术,**cglib基于让newProxy去继承RealSubject**）

newProxyInstance生成的动态类继承了proxy且实现了被代理的接口，所以可以把接口引用指向它，而它内部又有一个Invocationhandler，所以对这个类的方法调用最终会变InvocationhandlerImpl invoke的调用，这个动态代理类是运行时动态在JVM中创建的，他的class叫做$ProxyX(X代表编号）

```java
//   *对接口 goWorking 的调用 转变成 super.h.invoke(this,m5,new Object[]{var1,var2});调用。
//           *h 就是Proxy.java类的一个 InvocationHandler 接口 属性，
//           *我们在创建 动态代理类实例时候都必须 传一个 InvocationHandler 接口的实例过去。 这里就是刚才我们定义的 PersonInvocationHandler 。
//           *回到过后是不是就回到了 PersonInvocationHandler.invoke方法里面，所以 PersonInvocationHandler 是我们生成的动态代理类的拦截器，拦截所有方法调用。
//           */
    public final void goWorking(String var1, String var2) throws {
        try {
            super.h.invoke(this, m5, new Object[]{var1, var2});
        } catch (RuntimeException | Error var4) {
            throw var4;
        } catch (Throwable var5) {
            throw new UndeclaredThrowableException(var5);
        }
    }

    public final int hashCode() throws {
        try {
            return ((Integer) super.h.invoke(this, m0, (Object[]) null)).intValue();
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }
}

/**
 * 静态代码块，根据动态代理实现的公共接口类接口方法 获取到所有接口方法 的 Method 实例
 */
static {try{
        m1=Class.forName("java.lang.Object").getMethod("equals",new Class[]{Class.forName("java.lang.Object")});
        m4=Class.forName("org.vincent.proxy.dynamicproxy.Person").getMethod("setName",new Class[]{Class.forName("java.lang.String")});
        m3=Class.forName("org.vincent.proxy.dynamicproxy.Person").getMethod("getName",new Class[0]);
        m2=Class.forName("java.lang.Object").getMethod("toString",new Class[0]);
        m5=Class.forName("org.vincent.proxy.dynamicproxy.Person").getMethod("goWorking",new Class[]{Class.forName("java.lang.String"),Class.forName("java.lang.String")});
        m0=Class.forName("java.lang.Object").getMethod("hashCode",new Class[0]);
        }catch(NoSuchMethodException var2){throw new NoSuchMethodError(var2.getMessage());
        }catch(ClassNotFoundException var3){throw new NoClassDefFoundError(var3.getMessage());
        }
        }
        }
```

![Untitled](Java/Untitled%2022.png)

JDK动态代理只提供接口的代理，不支持类的代理，因为生成的动态类需要继承proxy，不能多重继承，**只能实现类实现的接口所提供的的方法（类实现接口后自己添加的方法除了hashCode，equals 和 toString其他的方法都不能被代理类生成）**

newProxyInstance的具体操作：先生成动态类的class文件，再用反射调用构造函数实例化

首先调用Proxy.getProxyClass(classLoader（被代理对象的类加载器）,Interfaces（被代理对象所实现的接口）)来生成动态代理类的class文件. 再用反射来获取动态代理类的constructer，再调用这个constrctuer并且将handler传入作为参数来实例化动态代理类

引申：代理模式的意义：远程调用就用到了代理模式，客户端调用代理模式和远程对象建立联系，省去了直接调用的处理网络服务这一步。代理类最重要的功能就是做权限控制，在代理类进行权限判断，还有切面、日志和拦截器也是这样

**注解**：本质是继承了annotation接口的接口（**注意java里面接口也是可以继承的**），注解只有被解析之后才会生效，常见的解析方法有两种：

- **编译期直接扫描** ：编译器在编译 Java 代码的时候扫描对应的注解并处理，比如某个方法使用`@Override` 注解，编译器在编译的时候就会检测当前的方法是否重写了父类对应的方法。
- **运行期通过反射处理** ：像框架中自带的注解(比如 Spring 框架的 `@Value` 、`@Component`)都是通过反射来进行处理的。

[https://juejin.cn/post/6844904167517995022](https://juejin.cn/post/6844904167517995022)

[https://juejin.cn/post/6844904168491073543](https://juejin.cn/post/6844904168491073543)

调用 `Method`对象的 `getAnnotation`方法 即可获取该方法上的所有注解，但是注解是接口，并没有被实例化，是如何调用它的value（）方法来获取写在注解上的值的呢？

答案是动态代理，JVM初始化运行时会调用动态代理创建一个实现了annotation接口的proxy类，里面就包括了value方法。写在注解上的值，都放在这个proxy类里面的AnnotationInvocationHandler里的memberValues里。

```java
class AnnotationInvocationHandler implements InvocationHandler, Serializable {
   private static final long serialVersionUID = 6182022883658399397L;
   private final Class<? extends Annotation> type;
   private final Map<String, Object> memberValues;
   private transient volatile Method[] memberMethods = null;
   ...
}
```

自定义方法注解及用拦截器或切面加反射实现对注解的处理：

[https://juejin.cn/post/6844903949233815566](https://juejin.cn/post/6844903949233815566)

也可以自定义类和field的注解

数组的本质是什么：动态生成的新类型

数组本质是类，是虚拟机运行时自动创建的类型，这个类的命名以“[”开头，几个表示几维，接着是数组中元素的类型，比如String[]  getclass().getname()是[Ljava.lang.String。（[https://blog.csdn.net/zhangjg_blog/article/details/16116613#t1](https://blog.csdn.net/zhangjg_blog/article/details/16116613#t1)）

![Untitled](Java/Untitled%2023.png)

**异常**：所有的异常都有一个共同的祖先 `java.lang` 包中的 `Throwable` 类。`Throwable` 类有两个重要的子类:

- **`Exception`** :程序本身可以处理的异常，可以通过 `catch` 来进行捕获。`Exception` 又可以分为 Checked Exception (受检查异常，必须处理) 和 Unchecked Exception (不受检查异常，可以不处理)。
- **`Error`** ：`Error` 属于程序无法处理的错误 ，我们没办法通过 `catch` 来进行捕获 。例如Java 虚拟机运行错误（`Virtual MachineError`）、虚拟机内存不够错误(`OutOfMemoryError`)、类定义错误（`NoClassDefFoundError`）等 。这些异常发生时，Java 虚拟机（JVM）一般会选择线程终止。

![Untitled](Java/Untitled%2024.png)

Checked Exception在编译过程中必须被 `catch`/`throw` 处理

UnChecked Exception包括NPE和越界异常，都是运行时异常

**OOM和StackOverFlow区别**：

OOM是当JVM分配内存时 不够才会抛出异常，主要分为

Java heap space： 新建对象时 JVM内存不足 new（**堆内存溢出**）

unable to create new native thread：你的应用创建了太多线程，一个应用进程创建多个线程，超过系统承载极限（**栈内存溢出**）

Metaspce：元空间内存不足（****方法区内存溢出****）

GC overhead limit exceedec****：****GC回收时间过长时会抛出该异常过长的定义是，超过98%的时间用来做GC并且回收了不到2%的堆内存连续多次GC都只回收了不到2%的极端情况下才会抛出

StackOverFlow异常一般是虚拟机栈的嵌套层数太深，所有栈帧的大小的总和大于-Xss设置的值

内存泄露是造成内存溢出的其中一个原因，但是内存泄露不一定会造成内存溢出。 简单来说，内存溢出就是占用内存太大，超过了系统可以承受的范围；而内存泄露则是由于对程序运行分配的对象没有回收（长生命周期的对象持有短生命周期对象的引用，`尽管短生命周期对象已经不再需要，但是因为长生命周期对象持有它的引用而导致不能被回收`），久而久之，则在系统分配的堆空间里面产生了很多无用的引用

内存泄漏的解决是提高程序的健壮型，因为内存泄露是纯代码层面的问题。

**如果频繁FULL GC，那很可能就是代码没写好，导致内存泄漏了**，可以把堆dump下来或者用在线分析工具查看存活的对象情况及占用内存从而分析问题，有些分析工具还可以定位到代码（jstate jconsole）

![Untitled](Java/Untitled%2025.png)

**Try-with-resources**： 任何实现 `java.lang.AutoCloseable`或者 `java.io.Closeable`的对象都可以被称为资源，对于资源的使用，用Try-with-resources 替代 Try-catch-finally，可以自动close，并且可以很简单的处理多个资源的开关

![Untitled](Java/Untitled%2026.png)

处理异常：

在一个方法中如果发生异常，这个方法会创建一个异常对象，并转交给 JVM，该异常对象包含异常名称，异常描述以及异常发生时应用程序的状态。创建异常对象并转交给 JVM 的过程称为抛出异常。可能有一系列的方法调用，最终才进入抛出异常的方法，这一系列方法调用的有序列表叫做调用栈。J

VM 会顺着调用栈去查找看是否有可以处理异常的代码，如果有，则调用异常处理代码。当 JVM 发现可以处理异常的代码时，会把发生的异常传递给它。如果 JVM 没有找到可以处理该异常的代码块，JVM 就会将该异常转交给默认的异常处理器（默认处理器为 JVM 的一部分），默认异常处理器打印出异常信息并终止应用程序。

抛出和处理异常的开销是很大的，所以异常不要用来做流程控制，条件控制。 说明：异常设计的初衷是解决程序运行中的各种意外情况，且异常的处理效率比条件判断方式要低很多

如果有try块放到了事务代码中，catch异常后，如果需要回滚事务，一定要注意手动回滚事务。

不要在finally块中使用return。try块中的return语句执行成功后，并不马上返回，而是继续执行finally块中的语句，如果此处存在return语句，则在此直接返回，无情丢弃掉try块中的返回点

**序列化和IO**：Serializable接口

`transient`修饰类中不想序列化的字段（只能修饰变量），被修饰的变量反序列化后会被设置为类型的默认值。需要注意的是，**静态变量因为不属于任何对象，所以无论有没有transient，都不会被序列化**，

序列化对象里面的属性是对象的话也需要实现序列化接口

**反序列化时静态变量如何初始化**：

被反序列化时会先去JVM找有没有这个类的class文件，有的话输出这个类文件的静态变量值, JVM没有加载这个类的话就会为默认值

![Untitled](Java/Untitled%2027.png)

![Untitled](Java/Untitled%2028.png)

****有了字节流,为什么还要有字符流：****

字符流是由 Java 虚拟机将字节流转化为2个字节的Unicode字符得到的，问题就出在这个过程还算是非常耗时，并且，如果我们不知道编码类型就很容易出现乱码问题。所以， I/O 流就干脆提供了一个直接操作字符的接口，方便我们平时对字符进行流操作。如果音频文件、图片等媒体文件用字节流比较好，如果涉及到字符的话使用字符流比较好。字符流可以直接处理Unicode字符，

字节序：多字节的数据在内存中的存放顺序。分为大端序、小端序两种。
0x12345678
大端序：低地址存放高位字节 12345678
小端序：低地址存放低位字节 78563412
java是大端序，c++是小端序；网络传输的网络字节序一般都是大端序。

**BIO NIO AIO 到IO多路复用-POLL&EPOLL：**

BIO和NIO都是同步IO，它们字面的区别是阻塞/非阻塞，实质最大的区别在于分别**以流和块的方式处理数据**，块 I/O 的效率比流 I/O 高很多。**BIO处理多个连接需要分别开多个线程处理**，因为是阻塞的。

AIO是异步非阻塞，不需要有Selector去轮询，由操作系统完成后会自动通知程序启动线程去处理，基于操作系统底层的异步API

NIO的组成和原理：

JAVA的NIO其实叫New IO，在Non-blocking IO 的基础上还增加了多路复用的思想

NIO由Channel、buffer、Selector组成，实际上实现了IO多路复用中的Reactor思想，一个线程 Thread 使用一个选择器 Selector 通过轮询的方式去监听多个通道 Channel 上的事件，只有套接字Channel才能配置为非阻塞，Filechannel不能，因为也没有意义

**Buffer**: **一块连续内存，底层是一个数组**，**实现了零拷贝（mmap）**，读取和写入数据都需要通过buffer，用flip()切换读写模式。常用ByteBuffer

**Channel**：Channel是双向的，BIO的流不是双向的，只能读或者写。

channel.read(buffer)从通道中把数据读到buffer中

channel.write(buffer)从buffer中把数据写到通道中

FileChannel、DatagramChannel、SocketChannel、SocketServerChannel

**Selector**：

![Untitled](Java/Untitled%2029.png)

每个通道都需要注册到Selector上，注册时必须指定监听的具体事件（可以单个可以多个，不同位 或一下就可以）

![Untitled](Java/Untitled%2030.png)

Selector会轮询的去询问注册通道是否有指定事件到达，当使用者调用Selector.select()时会返回到达通道的数量，若都没有事件到达，则会阻塞到至少有一个事件到达或者到timeout（select函数是阻塞的）。看源码发现select的doselect有两个实现，一个是WEpoll 一个是windowSelector

 使用`selectedKeys`来获取到达的事件及对应通道

![Untitled](Java/Untitled%2031.png)

![Untitled](Java/Untitled%2032.png)

NIO的一个缺点是只能用单线程调用select来处理IO

Selector基于select/poll模型实现，是基于IO复用技术的非阻塞IO，不是异步IO。在JDK1.5 update10和linux core2.6以上版本，sun优化了Selctor的实现，底层使用epoll替换了select/poll

使用NIO 推荐使用成熟的框架 如Netty（解决jdk epoll空转bug：哪怕没有事件到达也返回，导致cpu空转）

Select/poll和epoll：

select的三个缺点：单个进程能打开文件句柄最多1024个，每次调用都要把句柄集合从用户态拷贝到内核态，扫描是轮询的线性扫描，

poll没有了句柄的限制，其他一样

epoll不需要轮询，做到了触发时回调，有新的事件来时会进行提示，且不需要内核态用户态拷贝

水平触发和边缘触发：一个只在新事件来时提醒一次，一个在队列中还有未处理事件时一直提醒。

epoll高效的原因：[https://www.jianshu.com/p/31cdfd6f5a48](https://www.jianshu.com/p/31cdfd6f5a48)

[https://mp.weixin.qq.com/s/YdIdoZ_yusVWza1PU7lWaw](https://mp.weixin.qq.com/s/YdIdoZ_yusVWza1PU7lWaw)

[https://www.cnblogs.com/xiaolincoding/p/13719610.html](https://www.cnblogs.com/xiaolincoding/p/13719610.html)

**零拷贝(重点)**：避免在用户态和内核态之间来回拷贝的技术，NIO的零拷贝有两种实现

FileChannnel是基于transferTo()这个方法，它调用了native的transferTo方法，基于操作系统的底层实现（sendfile），从内核直接传输到sokcet buffer，避免了用户态和内核态之间的数据拷贝

MappedBytebuffer这种是基于**内存直接映射（mmap）**的零拷贝，mmap允许代码将文件映射到内核内存并直接访问它，就好像它在用户空间中一样，从而避免了不必要的从用户态到内核态的复制，内核空间对这段区域的修改也直接反映到用户空间。**通过`mmap`也可以实现不同进程间的共享内存**

sendfile适合大文件，mmap适合小数据

[http://learn.lianglianglee.com/专栏/重学操作系统-完/14 用户态和内核态：用户态线程和内核态线程有什么区别？.md](http://learn.lianglianglee.com/%E4%B8%93%E6%A0%8F/%E9%87%8D%E5%AD%A6%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F-%E5%AE%8C/14%20%20%E7%94%A8%E6%88%B7%E6%80%81%E5%92%8C%E5%86%85%E6%A0%B8%E6%80%81%EF%BC%9A%E7%94%A8%E6%88%B7%E6%80%81%E7%BA%BF%E7%A8%8B%E5%92%8C%E5%86%85%E6%A0%B8%E6%80%81%E7%BA%BF%E7%A8%8B%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB%EF%BC%9F.md)

**为什么要隔离用户态和内核态**？

通过区分内核空间和用户空间的设计，**隔离了操作系统代码(操作系统的代码要比应用程序的代码健壮很多)与应用程序代码。**

**即便是单个应用程序出现错误也不会影响到操作系统的稳定性，这样其它的程序还可以正常的运行**

**什么是用户态，什么是内核态**？

- 内核空间（Kernal Space），这个空间只有内核程序可以访问；
- 用户空间（User Space），这部分内存专门给应用程序使用。

用户空间中的代码被限制了只能使用一个局部的内存空间，我们说这些程序在**用户态（User Mode）** 执行。内核空间中的代码可以访问所有内存，我们称这些程序在**内核态（Kernal Mode）** 执行。

用户线程和内核线程可以是多对一，多对多，一对一，**hotspot的用户线程和内核线程是一对一的**

**集合/容器**：

![Untitled](Java/Untitled%2033.png)

![Untitled](Java/Untitled%2034.png)

ArrayDeque双端队列的底层是数组+双指针实现循环队列（相比于linkedList有扩容问题，但是linkedList插入的是node，伴随着对象的创建，使得插入操作速度较慢，占用空间大以）

ArrayQueue队列底层也是数组+双指针

List：

ArrayList适用于顺序添加、频繁（随机）访问的场景，线程不安全

Vector是很古老的实现类，线程安全，效率相对差

LinkedList适合频繁的两端增删，也是线程不安全的

线程安全的list有一个****CopyOnWriteArrayList-****基于写时复制思想，添加元素时先将当前容器复制出一个新的容器，然后新的容器里添加元素，添加完元素之后，再将原容器的引用指向新的容器，这样并发读的时候不需要加锁。这其实也是一种读写分离的思想，读用原容器，写创建一个新容器，缺点是每次都要复制一个新的，内存占用大，且只能保证数据的最终一致性，不能保证数据的实时一致性-写入的数据不能被马上读到

Iterator 接口提供遍历任何 Collection 的接口。我们可以从一个 Collection 中使用迭代器方法来获取迭代器实例。

![Untitled](Java/Untitled%2035.png)

![Untitled](Java/Untitled%2036.png)

**List如何正确遍历删除**：

用增强for循环遍历判断删除会报ConcurrentModificationException。增强for循环是iterator的语法糖，iterator.next()时会调用checkForComodification比较modCount和expectedModCount的值是否相等，当我们删除了后这里就不相等了。

这里的modCount其实提供的就是Fail-Fast机制，对于非线程安全的集合都会有ModCount，在迭代过程中若不相同则抛出异常防止继续遍历.

与之相对的有**fail-safe机制**：保证任何对集合结构的修改都会在一个复制的集合上进行，基于**写时复制**。先复制原有集合内容，在拷贝集合上进行遍历，**且对增删等方法加锁（避免copy出n个副本，导致并发写问题）**；concurrent包下容器都实现了该机制

正确操作：1. iterator.remove()会把两个值重新赋值

![Untitled](Java/Untitled%2037.png)

1. for循环+list.get(i)正序删除，删除后将i=i-1
2. for循环+list.get(i)倒序删除

 4.  java8之后新特性:removeIf()

![Untitled](Java/Untitled%2038.png)

![Untitled](Java/Untitled%2039.png)

因为Arrays.asList返回的是java.util.Arrays的一个内部类，没有实现集合的修改方法

   ArrayList如果要在指定位置 i 插入和删除元素的话（`add(int index, E element)`）时间复杂度就为 O(n-i)。因为在进行上述操作的时候集合中第 i 和第 i 个元素之后的(n-i)个元素都要执行向后位/向前移一位的操作。

   `LinkedList` 采用链表存储，所以，如果是在头尾插入或者删除元素不受元素位置的影响（`add(E e)`、`addFirst(E e)`、`addLast(E e)`、`removeFirst()` 、 `removeLast()`），近似 O(1)，如果是要在指定位置 `i` 插入和删除元素的话（`add(int index, E element)`，`remove(Object o)`） 时间复杂度近似为 O(n) ，因为需要先移动到指定位置再插入。

   内存空间占用**：** ArrayList 的空 间浪费主要体现在在 list 列表的结尾会预留一定的容量空间，而 LinkedList 的空间花费则体现在它的每一个元素都需要消耗比 ArrayList 更多的空间（因为要存放直接后继和直接前驱以及数据）。

ArrayList扩容分析：

以无参数构造方法创建 `ArrayList` 时，**实际上初始化赋值的是一个空数组。当对数组进行添加元素操作时，才真正分配容量**。即向数组中添加第一个元素时，数组容量扩为 10（10是default_capacity）

size和capacity：一个是当前元素个数，一个是当前数组长度

每次minCapacity(当前最少需要的)大于当前capacity的时候，首先将capacity扩大为1.5倍，若还是小于minCapacaity，那么直接设置为minCapacity

为什么是1.5而不是像map一样为2？

**System.ArrayCopy**:是一个用于**数组复制的native方法**

在arraylist的add(index,object)中，要把index之后的元素都往后挪，就是使用这个native方法实现数组自己的移位复制

![Untitled](Java/Untitled%2040.png)

**Arrays.Copyof**： 调用的就是System.ArrayCopy

创建一个新的数组，将原数组复制到新数组

![Untitled](Java/Untitled%2041.png)

 ArrayList自我扩容调用的就是Arrays.copyof函数

![Untitled](Java/Untitled%2042.png)

ArrayDeque和LinkedList：arraydeque也是线程不安全

![Untitled](Java/Untitled%2043.png)

队列如果想要线程安全可以用BlockingQueue和ConcurrentLinkedQueue（非阻塞）

也可以通过 Collections 的 synchronizedList 方法将其转换成线程安全的容器后再使用

```java
List<String> synchronizedList = Collections.synchronizedList(list);
synchronizedList.add("aaa");
synchronizedList.add("bbb");

for (int i = 0; i < synchronizedList.size(); i++) {
    System.out.println(synchronizedList.get(i));
}
```

**HashMap ConcurrentHashMap HashTable**:

Hashtable是线程安全（synchronized）但这个类已经被淘汰了：get/put所有相关操作都是synchronized的，这相当于给整个哈希表加了一把**大锁**，多线程访问时候，只要有一个线程访问或操作该对象，那其他线程只能阻塞，相当于将所有的操作**串行化，性能非常差**

`HashMap` 可以存储 null 的 key 和 value

![Untitled](Java/Untitled%2044.png)

**哈希值要进行哈希扰动的原因**？

把哈希值右移16位再与原值异或

```java
// 把哈希值右移16位再与原值异或
return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
```

如果使用hashCode取余，那么相当于**参与运算的只有hashCode的低位**，高位是没有起到任何作用的。扰动混合了原哈希值中的高位和低位，增大了随机性，让数据元素更加均衡的散列，减少碰撞

**Jdk8哈希扩容的优化**？

![Untitled](Java/Untitled%2045.png)

在扩充HashMap的时候，不需要像JDK1.7的实现那样重新计算hash，只需要看看原来的hash值新增的那个bit是1还是0就好了，是0的话索引没变，是1的话索引变成“原索引+oldCap”

![Untitled](Java/Untitled%2046.png)

**哈希表为什么链表长了要用红黑树而不是二叉平衡树**？

红黑树是一种宽泛条件下的平衡树，平衡树严格要求左右绝对值高度小于等于1，因此插入删除时需要旋转的次数更多，红黑树可以理解为一种相对平衡的二叉树，避免了二叉树的O（n）最坏时间复杂度，又比二叉平衡树效率相对高

****为什么HashMap中String、Integer这样的包装类适合作为Key****

这些包装类都是`final`修饰，是不可变性的， 保证了`key`的不可更改性，不会出现放入和获取时哈希值不同的情况。

***hashmap的长度为什么要是2的幂次方***（初始化输入为其他数也会初始化为最小的大于它的2的幂）：

计算方便：因为计算插入位置时要用到 hashKey%mapSize，但是%号其实很慢，所以如果mapsize是2的幂次方的话，这个式子可以用位运算&来优化：hashKey&（mapSize-1）;

hash分布更均匀：并且2的幂次方是偶数，mapSize-1最后一位是1，如果是奇数的话那最后一位是0，任何 hash 值都**只会被**散列到数组的**偶数下标位置**上，浪费了很多空间，增大了哈希碰撞发生的概率（只要不是2的幂次方的数，都会有可能某一位被忽略，只有2的幂次方-1是全为1的）

jdk1.7 **hashmap的死循环问题**：插入第n个节点时，发生rehash，假设现在有两个线程同时进行，线程1和线程2，两个线程都会新建新的数组。最终可能会产生一个环。原因就是因为扩容的rehash。jdk1.8 扩容不红rehash 避免了这个问题

[http://www.justdojava.com/2019/12/18/java-collection-15.1/](http://www.justdojava.com/2019/12/18/java-collection-15.1/)

[https://blog.csdn.net/zzu_seu/article/details/106698150](https://blog.csdn.net/zzu_seu/article/details/106698150)

`ConcurrentHashMap`: jdk1.7分段锁： **Segment 数组 → Node 数组（jdk1.8），采用 CAS + synchronized+volatile 来保证并发安全:** 

key和value都是volatile的，保证可见性。

ConcurrentHashMap 中 synchronized 只锁定当前链表或红黑二叉树的首节点，只要节点 的hash 不冲突，就不会产生并发，效率又进行了提高

流程：put的时候， 首先计算hash，遍历node数组，如果node是空的话，**就通过CAS+自旋的方式初始化**，如果当前数组位置是空则直接通过CAS自旋写入数据，再判断当前是否在扩容，如果是那代表有别的线程在扩容，一起进行扩容，扩容后再进行数据的更改（helpTransfer），**否则使用synchronized锁定当前链表或红黑二叉树的首节点并写入数据**，写入数据同样判断链表、红黑树，链表写入和HashMap的方式一样，key hash一样就覆盖，反之就尾插法

`**ConcurrentHashMap` 的并发扩容**：hashmap扩容是阻塞的（非concurrent的容器扩容都是阻塞）`ConcurrentHashMap` 支持并发扩容。`**ConcurrentHashMap` 扩容也是不用重新哈希的，根据高位决定新位置**

![Untitled](Java/Untitled%2047.png)

核心方法是transfer，从数组队尾开始拷贝，拷贝槽点时会锁住槽点，拷贝完成后将槽点设置为转移节点（ForwardingNode）。所以槽点拷贝完成后将新数组赋值给容器。线程分段进行数据在新老数组中的迁移，扩容过程中发生的插入，只要插入的位置扩容线程还未迁移到，就可以插入，**当迁移到该插入的位置时**，就会阻塞等待插入操作完成再继续迁移。

![Untitled](Java/Untitled%2048.png)

![Untitled](Java/Untitled%2049.png)

[**ConcurrentHashMap](https://so.csdn.net/so/search?q=ConcurrentHashMap&spm=1001.2101.3001.7020)的键值对为什么不能为null，而HashMap却可以？**

当你通过get(test)获取对应的value时，如果获取到的是null时，你无法判断，它是put（k,v）的时候value为null，还是这个key从来没有做过映射

1. 可能没有test这个key
2. 可能有test这个key，只不过value为null

**HashMap是非并发的，可以通过contains(key)的true或者false来做这个判断。而支持并发的Map在先调用m.get(key)再m.contains(key)的时候，m可能已经不同了。**

![Untitled](Java/Untitled%2050.png)

HashMap：

初始大小16，负载因子0.75

![Untitled](Java/Untitled%2051.png)

![Untitled](Java/Untitled%2052.png)

hash扩容 resize在jdk8有优化（不需要重新计算哈希值，只需要判断每个元素新位是1还是0）

LRU- LinkedHashMap.removeEldestEntry

![Untitled](Java/Untitled%2053.png)

WaekhashMap：通过弱引用来管理entry entry 可能会被GC自动删除***WeakHashMap* 的这个特点特别适用于需要缓存的场景**。在缓存场景下，由于内存是有限的，不能缓存所有对象；对象缓存命中可以提高系统效率，但缓存MISS也不会造成错误，因为可以通过计算重新得到。

JAVA8新特性：

1.lambda表达式+函数式编程：最简单的运用就是PriorityQueue不用写Comparator了，直接用lambda表达式。还有就是list.forEach() list.steam().xxx.collect(Collectors.toList())

lambda本质：会生成一个私有静态方法+一个匿名内部类；在内部类中实现了函数式接口，在实现接口的方法中，会调用私有静态方法。在使用lambda表达式的地方，通过传递内部类实例，来调用函数式接口方法。所以，lambda表达式可以理解为函数式接口的简单实现

****@FunctionInterface函数式接口：****

Consumer< T> void accept(T t)

Supplier < T> T get()

Predicate<T> boolean test(T t)

Function<T,R> R apply(T t)

函数式接口(Functional Interface)就是一个有且仅有一个抽象方法的接口

函数式接口可以被隐式转换为 lambda 表达式。

```java
public class Test_Supplier {
    private static String test_Supplier(Supplier<String> suply) {
        return suply.get(); //供应者接口
    }
    public static void main(String[] args) {
         // 产生的数据作为 sout 作为输出
         System.out.println(test_Supplier(()->"产生数据"));
         
         System.out.println(String.valueOf(new Supplier<String>() {
             @Override
            public String get() {
                return "产生数据";
            }
        }));
    }
}
```

```java
public class Test_Comsumer {
    public static void generateX(Consumer<String> consumer) {
        consumer.accept("hello consumer");
    }
    public static void main(String[] args) {
        generateX(s->System.out.println(s));
    }
}
```

```java
public class Use_Predicate {
    // 判断字符串是否存在o  既是生产者 又是消费者接口
    private static void method_test(Predicate<String> predicate) {
         boolean b = predicate.test("OOM SOF");
         System.out.println(b);
    }
    // 判断字符串是否同时存在o h 同时
    private static void method_and(Predicate<String> predicate1,Predicate<String> predicate2) {
        boolean b = predicate1.and(predicate2).test("OOM SOF");
        System.out.println(b);
    }
    //判断字符串是否一方存在o h 
    private static void method_or(Predicate<String> predicate1,Predicate<String> predicate2) {
        boolean b = predicate1.or(predicate2).test("OOM SOF");
        System.out.println(b);
    }
    // 判断字符串不存在o 为真   相反结果
    private static void method_negate(Predicate<String> predicate) {
         boolean b = predicate.negate().test("OOM SOF");
         System.out.println(b);
    }
    public static void main(String[] args) {
        method_test((s)->s.contains("O"));
        method_and(s->s.contains("O"), s->s.contains("h"));
        method_or(s->s.contains("O"), s->s.contains("h"));
        method_negate(s->s.contains("O"));
    }
}
```

```java

// 将数字转换为String类型
    private static void numberToString(Function<Number, String> function) {
        String apply = function.apply(12);
        System.out.println("转换结果:"+apply);
    }
    public static void main(String[] args) {
        numberToString((s)->String.valueOf(s));
    }
```

2.Optional类处理NPE问题

```java
Optional empty= Optional.ofNullable(zoo).map(o -> o.getDog()).map(d -> d.getAge()).filter(v->v==1).orElse(3);
System.out.println(empty.orElseGet(() -> "Default Value"));
```

3.日期Date-Time类

temporal.TemporalAdjusters

4.接口允许defalut实现

java的语法糖：swtich、泛型、基本类型的自动装箱拆箱、可变参数、枚举、内部类、断言、加强for循环、lambda表达式

字符集: Ascii是英文字符集，unicode是包含了所有语言符号的字符集，本身也有进行编码，但是没有优化，单个字符最多需要6个字节。utf8是一种变长的unicode编码规则，可以用1-4个字节来表示一个符号，节省空间。  gbk是中文的编码规则