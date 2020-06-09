### Android-selector

- TextView 按压后字体颜色selector

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

  

- TextView 选中后固定字体颜色及背景

  - @drawable/selector_text_bg

  ```xml
  <selector xmlns:android="http://schemas.android.com/apk/res/android">
      <item  android:drawable="@drawable/shape_corner_white_blue"   		 	  				   android:state_selected="true"/>
      <item  android:drawable="@drawable/shape_corner_white"    			            		   android:state_selected="false"/>
  </selector>
  ```

  ​	@drawable/shape_corner_white_blue

  ```xml
  <shape xmlns:android="http://schemas.android.com/apk/res/android"
      android:shape="rectangle">
      <corners android:radius="5dp"/>
      <solid android:color="@android:color/white"/>
      <stroke android:width="1dp" android:color="@color/colorGradientStart"/>
  </shape>
  ```

  ​	@drawable/shape_corner_white

  ```xml
  <shape xmlns:android="http://schemas.android.com/apk/res/android">
      <corners android:radius="5dp" />
      <solid android:color="@android:color/white"/>
  </shape>
  ```

  

  - @drawable/selector_text_color

  ```xml
  <selector xmlns:android="http://schemas.android.com/apk/res/android">
      <item android:color="#1788FF" android:state_selected="true" />
      <item android:color="#999" android:state_selected="false" />
  </selector>
  ```

  - TextView

  ```xml
  <TextView
  	android:id="@+id/tv_one"
  	android:background="@drawable/selector_text_bg"
  	android:textColor="@drawable/selector_text_color" />
  ```

  ```java
  tvOne.setSelected(true);
  ```

  - 如果是少量/数量不变TextView的单选操作(会有1S的延迟,建议使用Recyclerview可以避免延迟)

  ```java
    		switch (view.getId()) {
              case R.id.tv_one:
  //                updateState(tvOne.isSelected() ? 000 : 001);
                  updateState(tvOne.isSelected() ? 0 : 1);
                  break;
              case R.id.tv_two:
  //                updateState(tvTwo.isSelected() ? 000 : 010);
                  updateState(tvTwo.isSelected() ? 0 : 2);
                  break;
              case R.id.tv_three:
  //                updateState(tvThree.isSelected() ? 000 : 100);
                  updateState(tvThree.isSelected() ? 0 : 4);
                  break;
  ```

  ```java
   private void updateState(int code) {
          tvOne.setSelected(false);
          tvTwo.setSelected(false);
          tvThree.setSelected(false);
          switch (code) {
              case 1:
                  tvOne.setSelected(true);
                  break;
              case 2:
                  tvTwo.setSelected(true);
                  break;
              case 4:
                  tvThree.setSelected(true);
                  break;
              default:
                  break;
          }
      }
  ```

  

