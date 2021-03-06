Kotlin 版本：Kotlin 1.3

```.kt
[@JvmField] [public|protected|private] [open] <var|val> <propertyName>
```

中括号的为可选修饰符
如果要实现静态属性可采用

```.kt
companion object{
	[@JvmField] [public|protected|private] [open] <var|val> <propertyName>
}
```

先做一个总结吧

非静态属性

open 修饰符等价于 Java 中子类能不能重写 get、set 方法，即在 Java 中 get、set 有 final 修饰。默认有 final 修饰。

var 和 val 的区别就是在 Java 中属性是否有 final 修饰，前者没有 final。

使用 @JvmField 注解和 Java 直接声明属性没什么区别，即没有自动生成 get、set 方法。可见性修饰符默认是 public。没有使用的时候 get、set 的可见性由声明时决定，且属性始终是 private。默认 get、set 方法为 public，当声明为 private 时，没有 get、set 方法。

静态属性

open 修饰无效

var 和 val 区别和非静态属性一样

使用 @JvmField 注解的效果也是和非静态属性一样，唯一不同的是没使用注解在 Java 中是使用静态内部类实现静态属性和方法。

顶级属性

隶属于文件的属性（这种说法似乎不太准确），详细看代码吧（最后一个例子）

各种修饰的情况对应到 Java 当中 (非静态属性带注解没列举出来，不过可以用静态属性的例子来类比，或者手动用一下 Android Studio 的 Kotlin 转 Java 看一下)

```.kt
var a : Int

private int a;
public final int getA(){
	return a;
}
public final int setA(int a){
	this.a = a;
}
//--------------------
val a : Int

private final int a;
public final int getA(){
	return this.a;
}
//--------------------
private var a : Int

private int a;
//--------------------
private val a : Int

private final int a;
//--------------------
protected var a : Int

private int a;
protected final int getA() {
   return this.a;
}
protected final void setA(int a) {
   this.a = a;
}
//--------------------
protected val a : Int

private final int a;
protected final int getA() {
   return this.a;
}
//--------------------
public var a : Int

private int a;
public final int getA() {
   return this.a;
}
public final void setA(int a) {      
   this.a = a;
}
//--------------------
public val a : Int

private final int a;
public final int getA() {
   return this.a;
}
//--------------------
open var a : Int

private int a;
public int getA(){
	return a;
}
public int setA(int a){
	this.a = a;
}
//--------------------
open val a : Int

private final int a;
public int getA(){
	return this.a;
}
//--------------------
/*
  Compile Error
  这两个写法编译报错，private 和 open 不能一起用
*/
private open var a : Int
private open val a : Int
//--------------------
protected open var a : Int

private int a;
protected int getA() {
   return this.a;
}
protected void setA(int a) {
   this.a = a;
}
//--------------------
protected open val a : Int

private final int a;
protected int getA() {
   return this.a;
}
//--------------------
public open var a : Int

private int a;
public int getA() {
   return this.a;
}
public void setA(int a) {      
   this.a = a;
}
//--------------------
public open val a : Int

private final int a;
public int getA() {
   return this.a;
}
//--------------------
companion object{
    var a : Int					//open 对伴生对象的成员不起作用，public 也不起作用
}

private static int a;
public static final class Companion {
    public final int getA() {
       return BaseFragment.a;
    }
    public final void setA(int a) {
       BaseFragment.a = a;
    }
    private Companion() {
    }
    // $FF: synthetic method
    public Companion(DefaultConstructorMarker $constructor_marker) {        		this();
    }
}
//--------------------
companion object{
    val a : Int
}

private static final int a;
public static final class Companion {
   public final int getA() {
      return BaseFragment.a;
   }
   private Companion() {
   }
   // $FF: synthetic method
   public Companion(DefaultConstructorMarker $constructor_marker) {
      this();
   }
}
//--------------------
companion object{
    private var a : Int
}

private static int a;
public static final class Companion {
   private Companion() {
   }
   // $FF: synthetic method
   public Companion(DefaultConstructorMarker $constructor_marker) {
       this();
   }
}
//--------------------
companion object{
    private val a : Int
}

private static final int a;
public static final class Companion {
   private Companion() {
   }
   // $FF: synthetic method
   public Companion(DefaultConstructorMarker $constructor_marker) {
       this();
   }
}
//--------------------
companion object{
   protected var a : Int
}

private static int a;
public static final class Companion {
   protected final int getA() {
      return BaseFragment.a;
   }
   protected final void setA(int a) {
      BaseFragment.a = a;
   }
   private Companion() {
   }
   // $FF: synthetic method
   public Companion(DefaultConstructorMarker $constructor_marker) {
      this();
   }
}
//--------------------
companion object{
    protected val a : Int
}

private static final int a;
public static final class Companion {
   protected final int getA() {
      return BaseFragment.a;
   }
   private Companion() {
   }
   // $FF: synthetic method
   public Companion(DefaultConstructorMarker $constructor_marker) {
      this();
   }
}
//--------------------
companion object{
    @JvmField var a : Int		//通过 @JvmField 注解可以实现真正的静态方法和属性，不能和 private 修饰符一起使用
}

public static int a;
//--------------------
companion object{
    @JvmField val a : Int
}

public static final int a;
//--------------------
companion object{
    @JvmField protected var a : Int
}

protected static int a;
//--------------------
companion object{
    @JvmField protected val a : Int
}

protected static final int a;
//--------------------
const val a

/*
  类的名字由变量声明所在的文件名
  例如在 BaseFragment.kt 中声明类的名字就是 BaseFragmentKt
 */
public final class BaseFragmentKt{	
	public static final int a;
}
```

##### 