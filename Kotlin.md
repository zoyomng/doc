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

     