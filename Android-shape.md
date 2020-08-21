### Android-shape

1. 底边

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

   右边框

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

   上边框

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

   

