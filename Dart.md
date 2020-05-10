# Dart 记录

#### 可选参数

1. 可选命名参数

   `void enableFlags(String name , {int age ,  int id}) {...}`

   - {}内的参数为可选参数

   - 函数调用方式: 

     ```
     enableFlags('Tom')	//可选参数不传递
     enableFlags('Tom' , age: 20)  //可选参数通过key: value的形式传递
     ```

     

2. 可选位置参数

   `void enableFlags(String name , [int age , int id]) {...}`

   - []内的参数为可选参数

   - 函数调用方式

     ```
     //不需要key : value这种形式传参 , 因为是相对于参数位置形式传参
     enableFlags('Tom' , 10)	//age=10 , id=null 
     enableFlags('Tom' , 20 , 20)  // 如果想传递id的值 , age必须有值
     ```

     

3. 默认值

   `void enableFlags(String name , [int age = 10 , int id = 01]) {...}`

   - 使用 ' = ' 设置默认值

     ```
     enableFlags('Tom') //age=10 , id=01 	age/id会有默认值
     enableFlags('Tom' , 20)	//age=20 , id=01
     enableFlags('Tom' , 20 , 20)  // 如果想传递id的值 , age还是必须有值
     ```

     

     