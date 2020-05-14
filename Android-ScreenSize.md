### Android-ScreenSize

1. 第一种转换方式

```java
//转换dip为px 
public static int dp2px(Context context, int dp) { 
    float scale = context.getResources().getDisplayMetrics().density; 
    return (int)(dp*scale + 0.5f*(dp>=0?1:-1)); 
} 

//转换px为dip 
public static int px2dp(Context context, int px) { 
    float scale = context.getResources().getDisplayMetrics().density; 
    return (int)(px/scale + 0.5f*(px>=0?1:-1)); 
} 

public static int sp2px(Context context, float sp) {
    float fontScale = context.getResources().getDisplayMetrics().scaledDensity;
    return (int) (sp * fontScale + 0.5f);
}

public static int px2sp(Context context, float px) {
    float fontScale = context.getResources().getDisplayMetrics().scaledDensity;
    return (int) (px / fontScale + 0.5f);
}
```

2. 系统工具转换方式

```java
/**
* dp转px
**/
private int dp2px(float dpVal) {
	return (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, dpVal, 		    
    									 mContext.getResources().getDisplayMetrics());
}
```

- 源码(只有转化为px的方式)

  ```java
  public static float applyDimension(int unit, float value,DisplayMetrics metrics){
      switch (unit) {
          case COMPLEX_UNIT_PX:
              return value;
          case COMPLEX_UNIT_DIP:
              return value * metrics.density;
          case COMPLEX_UNIT_SP:
              return value * metrics.scaledDensity;
          case COMPLEX_UNIT_PT:
              return value * metrics.xdpi * (1.0f/72);
          case COMPLEX_UNIT_IN:
              return value * metrics.xdpi;
          case COMPLEX_UNIT_MM:
              return value * metrics.xdpi * (1.0f/25.4f);
      }
      return 0;
  }
  ```

  