###  Kotlin

1. 数据类(javaBean)

   ```kotlin
   data class User(var id:Long,var name:String)
   ```

   - 这个数据类，它会自动生成所有属性和它们的访问器，以及一些有用的方法，比如，`toString()`

2. 空安全

   - 安全调用操作符(?)-明确指定一个对象是否能为空

     ```kotlin
     var notNullUser: User = null // × 不能赋值null
     var user: User? = null 		// √ user可以赋值null
     user.print()				// × 编译不通过,user为null时需要处理
     user?.print() 				// √
     
     val name = user?.name ?: "empty"
     
     ```




3. let/ with/ run/ apply/ also区别

| 函数名 | 定义inline的结构                                             | 函数体内使用的对象       | 返回值       | 是否是扩展函数 | 适用的场景                                                   |
| ------ | ------------------------------------------------------------ | ------------------------ | ------------ | -------------- | ------------------------------------------------------------ |
| let    | fun <T, R> T.let(block: (T) -> R): R = block(this)           | it指代当前对象           | 闭包形式返回 | 是             | 适用于处理不为null的操作场景                                 |
| with   | fun <T, R> with(receiver: T, block: T.() -> R): R = receiver.block() | this指代当前对象或者省略 | 闭包形式返回 | 否             | 适用于调用同一个类的多个方法时，可以省去类名重复，直接调用类的方法即可，经常用于Android中RecyclerView中onBinderViewHolder中，数据model的属性映射到UI上 |
| run    | fun <T, R> T.run(block: T.() -> R): R = block()              | this指代当前对象或者省略 | 闭包形式返回 | 是             | 适用于let,with函数任何场景。                                 |
| apply  | fun T.apply(block: T.() -> Unit): T { block(); return this } | this指代当前对象或者省略 | 返回this     | 是             | 1、适用于run函数的任何场景，一般用于初始化一个对象实例的时候，操作对象属性，并最终返回这个对象。 2、动态inflate出一个XML的View的时候需要给View绑定数据也会用到. 3、一般可用于多个扩展函数链式调用 4、数据model多层级包裹判空处理的问题 |
| also   | fun T.also(block: (T) -> Unit): T { block(this); return this } | it指代当前对象           | 返回this     | 是             | 适用于let函数的任何场景，一般可用于多个扩展函数链式调用      |



为了帮助你选择合适的作⽤域函数，我们提供了它们之间的主要区别表。 

**函数** 			**对象引⽤ 					返回值** 							**是否是扩展函数** 

let						 it 				Lambda 表达式结果 						是 

run 					this 			  Lambda 表达式结果						是 

run 					\- 				   Lambda 表达式结果						不是：调⽤⽆需上下⽂对象 

with 				this				 Lambda 表达式结果 						不是：把上下⽂对象当做参数 

apply 			this 					上下⽂对象										 是 

also 				it 						上下⽂对象 										是 

以下是根据预期⽬的选择作⽤域函数的简短指南： 

对⼀个⾮空（non-null）对象执⾏ lambda 表达式：let 

将表达式作为变量引⼊为局部作⽤域中：let 

对象配置：apply 

对象配置并且计算结果：run 

在需要表达式的地⽅运⾏语句：⾮扩展的 run 

附加效果：also 

⼀个对象的⼀组函数调⽤：with 

不同函数的使⽤场景存在重叠，你可以根据项⽬或团队中使⽤的特定约定选择函数。 

尽管作⽤域函数是使代码更简洁的⼀种⽅法，但请避免过度使⽤它们：这会降低代码的可读性并可能导 

致错误。避免嵌套作⽤域函数，同时链式调⽤它们时要⼩⼼：此时很容易对当前上下⽂对象及 this 或 

it 的值感到困惑。



![img](https://upload-images.jianshu.io/upload_images/6202959-757d95d7a54de330.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)





4. Kotlin Android Extensions(省去 findViewById() --> 查找控件)

   ```kotlin
   import kotlinx.android.synthetic.main.activity_main.*
   ```

   - Adapter中使用需要做以下操作

     - app下build

     ```groovy
     android {
       ...
     }
     //实验性
     androidExtensions {
         experimental = true
     }
     dependencies {
     	...
     }
     ```

     - 导入

       ```kotlin
       import kotlinx.android.synthetic.main.item_recycler.*
       ```

     - ViewHolder 需要实现 LayoutContainer

       ```kotlin
        class ViewHolder(override val containerView: View) : RecyclerView.ViewHolder(containerView), LayoutContainer {
        		...
        }
       ```

5. ```kotlin
   /**
    * @JvmOverloads:指示Kotlin编译器为这个函数生成替代默认参数值的重载
    * 如果一个方法有N个参数，其中M个参数有默认值，则会产生M个重载
    * 对应Java中重载,eg.自定义控件中有多个构造方法重载
    */
   class CustomView @JvmOverloads constructor(
       context: Context,
       attrs: AttributeSet? = null,
       defStyleAttr: Int = 0
   ) :View(context, attrs, defStyleAttr) {
   ```

6. 