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

       