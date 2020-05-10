### Android-selector

- TextView 字体颜色selector

  ```xml
  <selector xmlns:android="http://schemas.android.com/apk/res/android">
      <!--使用android:color属性-->
      <item android:color="@color/color_pressed" android:state_pressed="true"  />
      <item android:color="@color/color_common" android:state_pressed="false" />
  </selector>
  ```

  ```xml
  <TextView
  	 android:textColor="@drawable/selector_item_common_color"
  	 ...
  />
  
  ```

  ```java
   textview.setOnClickListener(null)	//需要设置点击事件
  ```

  

