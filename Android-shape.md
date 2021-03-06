### Android-shape

1. 单边框

   - 底边

   ```xml
   <layer-list xmlns:android="http://schemas.android.com/apk/res/android">  
     
       <item  
           android:top="-2dp"  
           android:right="-2dp"  
           android:left="-2dp">  
           <shape>  
               <solid android:color="#e4e4e4"/>  
               <stroke  
                   android:width="1dp"  
                   android:color="#D7D7D7"/>  
           </shape>  
       </item>  
     
   </layer-list>  
   ```

   ```xml
   <layer-list xmlns:android="http://schemas.android.com/apk/res/android">
       <!--背景白色-仅一条下边框-->
       <item
           android:left="-2dp"
           android:right="-2dp"
           android:top="-2dp">
           <shape>
               <solid android:color="@android:color/white" />
               <stroke
                   android:width="1dp"
                   android:color="#f2f2f2" />
               <padding android:bottom="@dimen/dp_3" />
           </shape>
       </item>
   
   </layer-list>
   ```

   - 右边框

   ```xml
   <layer-list xmlns:android="http://schemas.android.com/apk/res/android">
       <!--背景白色-仅一条右边框-->
       <item
           android:left="-2dp"
           android:bottom="-2dp"
           android:top="-2dp">
           <shape>
               <stroke
                   android:width="1dp"
                   android:color="#D7D7D7" />
               <padding android:bottom="@dimen/dp_5" />
           </shape>
       </item>
   
   </layer-list>  
   ```

   - 上边框

   ```xml
   <layer-list xmlns:android="http://schemas.android.com/apk/res/android">  
     <!--背景白色-仅一条上边框-->
       <item  
           android:bottom="-2dp"
           android:right="-2dp"  
           android:left="-2dp">  
           <shape>  
               <solid android:color="@android:color/transparent"/>  
               <stroke  
                   android:width="1dp"  
                   android:color="#D7D7D7"/>
           </shape>  
       </item>  
     
   </layer-list>  
   ```


2. 圈

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <shape xmlns:android="http://schemas.android.com/apk/res/android"
       android:thickness="1dp"
       android:shape="ring"
       android:useLevel="false">
       <size
           android:width="8dp"
           android:height="8dp" />
       <solid android:color="#1788FF" />
   </shape>
   ```

3. 单边框(另一种写法)

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <layer-list xmlns:android="http://schemas.android.com/apk/res/android"
       android:paddingStart="20dp"
       android:paddingEnd="20dp">
       <item>
           <shape>
               <solid android:color="@android:color/white" />
           </shape>
       </item>
   
       <item
           android:height="1dp"
           android:gravity="bottom"
           android:left="12dp"
           android:right="12dp">
           <color android:color="@color/lineColor" />
       </item>
   </layer-list>
   ```

   - 好处: 可以留出边框 居 边界的一段距离 , 即底边框不是填充底边界的

