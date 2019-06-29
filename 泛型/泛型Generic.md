# 泛型

```java
//什么是泛型Generic，就是类可以接受未指定类型的一个引用类型的类名如T，作为参数，在本类中可以使用T 将本类所能处理的数据进行抽象，泛化
//类型参数的类型可以是未指定的具体类型范围T ，也可以是继承了某具体类型<T extends String>,那么也就是说在使用的时候传入的类型参数必须是String类的子类，如 C<subString> c=new C(new subString);(假设subString是String的子类)

public class C<T>{

	T t;
	C(T t){
		this.t=t;
	}
	public T getParam(){
		return t;
	}
}
// 上面的类C被编译后在类中出现的类型类型参数T被替换为java.lang.Object，我觉得替换比擦除这个概念更准确
/* 比如 T t; 被替换为 Object t;
 * C（T t） 被替换为C(Object t)
 * T getParam() 被替换为 Object  getParam()
 */

//测试代码
public class D{
	public static void main(String ...args){
		C<String> c=new C("test");
		System.out.println(c.getParam());
	}
}

//在使用的时候c.getParam 会被插入checkcast 指令去类型转换，checkcast的操作数就是要转换的目标类型

  public static void main(java.lang.String...);
    descriptor: ([Ljava/lang/String;)V
    flags: ACC_PUBLIC, ACC_STATIC, ACC_VARARGS
    Code:
      stack=3, locals=2, args_size=1
         0: new           #2                  // class C
         3: dup
         4: ldc           #3                  // String test
         6: invokespecial #4                  // Method C."<init>":(Ljava/lang/Object;)V
         9: astore_1
        10: getstatic     #5                  // Field java/lang/System.out:Ljava/io/PrintStream;
        13: aload_1
        14: invokevirtual #6                  // Method C.getParam:()Ljava/lang/Object;
        17: checkcast     #7                  // class java/lang/String
        
//main 被编译后的字节码中在实例化C 类的时候根据传入的类型参数，就是checkcast的操作数，
//17： checkcast #7 就是将c.getParam()的类型转换为传入的类型参数java.lang.String


//javac 是根据什么数据去替换类中的类型参数呢？
Class C<T> 的反编译后的类型如下：
	public class C<T extends java.lang.Object> extends java.lang.Object
我们知道自定义类若没指定父类，则默认的的父类是Object,所以C extends Object，
但是我们传入的类型参数 T 也要遵循这个规则，T extend Object，为什么要说这个呢？ 因为javac 编译期就是根据类型参数的父类类型去替换掉代码中类型参数T,
Class C 就会变成如下：
public C<T extends Object> extends Object
{
	Object t;
	C(Object t){
		this.t=t;
	}
	public Object (Object t){
		return t;
	}
}

//那么我们就有一个结论了，如果定义类的时候，如果传入的类型参数的类型是不定的，那么默认继承Object,替换类型参数T的时候以Object代替，

如果在定义类的时候传入的类型参数继承了具体的类类型，如下：
public C<T extends Father>{}
	//那么C类中所所有的类型参数T 都会被替换为Father 如下：
{
	Father t;
	C(Father t){
		this.t=t;
	}
	public Father (Father t){
		return t;
	}
}
在实例化含有类型参数的类的时候，需要传入具体的参数类型，就是C<String> c=new C("test"); 所谓的具体的类型参数就是<String> 前面说过传入类型参数的作用就是作为checkcast 指令的操作数，用于类型转换的，

前面说的泛型，都是在编译期之间javac 搞得，和运行期无关，但是在运行期可以获取类型参数的信息，比如类的类型参数信息，字段的类型参数信息，方法的类型参数信息等等，
但是运行期，我们可以通过Class 的实例获取带有类型参数的类数据中的类型参数；如：
	如C<T>，在运行期中，我们可以获取类型参数T 的信息，
前面不是说了，泛型是指编译期搞的吗，为什么运行期还可以获取类型参数，在编译的时候，javac会把所有的，在与类名绑定的，与字段绑定的，与方法绑定的类型参数的信息会到各自的属性表中的signature中，如下，分别是类的属性表的signature属性，字段表中字段的属性表中的signature属性，方法表中方法的属性表的signature属性
```

![1561458718127](C:\Users\Administrator.MS-201605312249\AppData\Roaming\Typora\typora-user-images\1561458718127.png)



![1561458826289](C:\Users\Administrator.MS-201605312249\AppData\Roaming\Typora\typora-user-images\1561458826289.png)



![1561458866357](C:\Users\Administrator.MS-201605312249\AppData\Roaming\Typora\typora-user-images\1561458866357.png)



![1561458889673](C:\Users\Administrator.MS-201605312249\AppData\Roaming\Typora\typora-user-images\1561458889673.png)

```json
我们知道类加载的时候会提取class字节码中的数据，到方法表中建立一个便于搜索的数据结构存放类型信息，字段信息，方法信息等，各自的属性表的属性也会存放起来，所以运行期能获取类型参数的信息
```



> ### 关于泛型类的实例化

```java
//如果一个类型声明的时候是这样的
public class GenericClass<T>{}
//在实例化的时候
GenericClass gc=new GenericClass()；
//所谓的不传入类型化参数，就是不需要编译期插入类型转换，为什么不传入类型参数也可以呢？还是因为编译期的参数化类型的擦除，替换，就像一个普通类一样，比如
public class GenericClass<T extends Father>
//那么GenericClass类型所有返回值类型是T的被替换为Father,在调用方法的时候，返回的也是Father,

//重申一遍，实例化泛型类的时候传入类型参数，是编译器自动的插入类型转换指令的操作数的依据，而且编译期还会根据你传入的参数，在调用方法的时候会根据该参数，判断你传入的参数是否合法，场景如下：
```

```java
public class Generic<T>
{
	T getT(T t)｛....｝
	void setT(T t)｛...｝
	public static void main(String[] args) {
		Generic<String> lg=new Generic();
		lg.getT(1);
	}
}
Generic<String> lg=new Generic();	//这里讨论的重点是
其中Generic实例化的时候传入的是String，(T extneds Object，而String是继承Object的,所以T可以等于String)二替换后只剩下Object,
那么既然擦除后是
	Object getT(Object obj),
那么为什么lg.setT(1)，为什么编译不通过呢？因为编译期会根据lg实例化的时候传入的参数化类型String,去自动的插入checkcast指令，如果你传入的String子类型，Object getT(Object obj)在返回的时候返回Object,而编译器插入的自动转换指令后是这样的，(String)getT(1),(Integer--String)很明显类型转换失败，所以编译器为了让传入的数据和传出的数据的类型的一致，就会根据Generic<String> lg=new Generic();中指定的参数化类型去限制你传入的数据，这就是泛型的类型检查，具体的代码场景如下：
	Generic<Father> lg=new Generic();
	lg.set("aaa")；
	那么代码的执行就是这样的，set() 形参是Object 可以接受"aaa"，在方法处理完之后，返回Object,实际类型String,而编译器自动插入的类型转换是Father,String-->Father，类型转换注定失败，所以在编译的时候告诉你lg.set("aaa")是错误的，也就是说，单单的方法匹配是可以成功的，但是编译器插入的类型转换是注定失败的，所以编译器不让你调用，所以报错，看起来就像编译器实现了泛型的类型检查

这就是泛型保证，传入的数据与传出的数据的类型的一致性的类型检查
	

还有一种检查是这样的
public class ListStringGeneric<T extends Father>
{
	T getT(){
		return null;
	}
	void setT(T t) {
		
	}
	public static void main(String[] args) {
		ListStringGeneric lg=new ListStringGeneric();
//报错		lg.setT("aaaa");
	}
}

我们知道，泛型擦除后，T 会被改成Father,所以lg.setT("xx")的时候只是普通的方法匹配，根本没有这个方法，
也不存在泛型的传入数据前的类型检查，传出数据后的类型转换了
```

