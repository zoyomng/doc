### Android-图片

1. 9.png-左上定义伸缩区域,右下定义文本区域
2. Drawable.setColorFilter()渲染图片颜色;可用于一张图片,选中与非选中两种状态的情形下轻松解决,减少图片的导入
   * PorterDuff.Mode多种渲染模式(简单说:取并集,合集...)

```java
/**
     * 对图标进行渲染
     *
     * @param drawable
     * @param tintColor
     * @return
     */
public static Drawable tintIcon(@NonNull Drawable drawable, @ColorInt int tintColor) {
    drawable.setColorFilter(tintColor, PorterDuff.Mode.SRC_IN);
    return drawable;
}
```

3. 本地图片显示(除使用三方框架外)

   ```java
   Uri uri = Uri.fromFile(mFile);
   imageView.setImageURI(uri);
   ```

   - Intent 传递Uri

     ```java
     //传递
     intent.putExtra("uri", uri.toString());
     
     //获取
     String uriStr = intent.getStringExtra("uri");
     Uri uri = Uri.parse(uriStr);
     ```

     

