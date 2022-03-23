### Android-Toast

```java
/**
 * @Description: Toast的封装类
 * 解决以下问题:
 * 1.多次连续点击,多个吐司会一一显示消失,用户体验不好
 * 2.多次连续点击,会有一段时间不显示吐司
 * 3.自定义吐司
 * Application中初始化配置(可不初始化)
 * Toasty.Configs.getInstance()
 * .isTintIcon(true)
 * .setTextSize(16)
 * .setGravity(Gravity.CENTER, 0, 0)
 * .apply();
 */
public class Toasty {


    private static final Typeface DEFAULT_TYPEFACE = Typeface.create("sans-serif-condensed", Typeface.NORMAL);
    private static final int DEFAULT_GRAVITY = Gravity.CENTER;
    private static Typeface currentTypeface = DEFAULT_TYPEFACE;

    //  是否渲染图标
    private static boolean isTintIcon = true;
    private static int textSize = 16;
    private static int currentGravity = DEFAULT_GRAVITY;
    private static int currentXOffset = 0;
    private static int currentYOffset = 0;

    private static Toast lastToast;

    private Toasty() {
    }

    /**
     * @param text 如果是字符串资源:context.getText(stringRes)
     */
    public static Toast normal(@NonNull Context context, @NonNull CharSequence text, int duration) {
        return build(context, text, null, R.color.normalColor, duration);
    }

    public static Toast normal(@NonNull Context context, @NonNull CharSequence text, @DrawableRes int iconRes, int duration) {
        return build(context, text, ToastyUtil.getDrawable(context, iconRes), R.color.normalColor, duration);
    }

    /**
     * @param text 如果是字符串资源:context.getText(stringRes)
     */
    public static Toast success(@NonNull Context context, @NonNull CharSequence text, int duration) {
        return build(context, text, ToastyUtil.getDrawable(context, R.drawable.ic_success_white_24dp), R.color.successColor, duration);
    }

    public static Toast success(@NonNull Context context, @NonNull CharSequence text, Drawable icon, int duration) {
        return build(context, text, icon, R.color.successColor, duration);
    }

    /**
     * @param text 如果是字符串资源:context.getText(stringRes)
     */
    public static Toast error(@NonNull Context context, @NonNull CharSequence text, int duration) {
        return build(context, text, ToastyUtil.getDrawable(context, R.drawable.ic_error_white_24dp), R.color.errorColor, duration);
    }

    public static Toast error(@NonNull Context context, @NonNull CharSequence text, Drawable icon, int duration) {
        return build(context, text, icon, R.color.errorColor, duration);
    }

    /**
     * @param text 如果是字符串资源:context.getText(stringRes)
     */
    public static Toast info(@NonNull Context context, @NonNull CharSequence text) {
        return build(context, text, ToastyUtil.getDrawable(context, R.drawable.ic_info_outline_white_24dp), R.color.infoColor, Toast.LENGTH_SHORT);
    }

    public static Toast info(@NonNull Context context, @NonNull CharSequence text, int duration) {
        return build(context, text, ToastyUtil.getDrawable(context, R.drawable.ic_info_outline_white_24dp), R.color.infoColor, duration);
    }

    public static Toast info(@NonNull Context context, @NonNull CharSequence text, Drawable icon, int duration) {
        return build(context, text, icon, R.color.infoColor, duration);
    }

    /**
     * @param text 如果是字符串资源:context.getText(stringRes)
     */
    public static Toast warning(@NonNull Context context, @NonNull CharSequence text, int duration) {
        return build(context, text, ToastyUtil.getDrawable(context, R.drawable.ic_warning_outline_white_24dp), R.color.warningColor, duration);
    }

    public static Toast warning(@NonNull Context context, @NonNull CharSequence text, Drawable icon, int duration) {
        return build(context, text, icon, R.color.warningColor, duration);
    }

    public static Toast build(@NonNull Context context, @NonNull CharSequence text, Drawable icon, @ColorRes int tintColor, int duration) {
        return build(context, text, ToastyUtil.getColor(context, R.color.defaultTextColor), icon,
                ToastyUtil.getColor(context, tintColor), true, duration);
    }

    public static Toast build(@NonNull Context context, @NonNull CharSequence text, @ColorRes int textColor, Drawable icon, @ColorRes int tintColor, int duration) {
        return build(context, text, ToastyUtil.getColor(context, textColor), icon,
                ToastyUtil.getColor(context, tintColor), true, duration);
    }

    /**
     * @param context             上下文
     * @param text                文本
     * @param icon                图标
     * @param tintColor           渲染色值
     * @param textColor           文字颜色
     * @param duration            时长
     * @param isTintBackgroundRes 背景图片是否渲染
     * @return Toast对象
     */
    @SuppressLint("ShowToast")
    @CheckResult
    public static Toast build(@NonNull Context context, @NonNull CharSequence text, @ColorInt int textColor,
                              Drawable icon, @ColorInt int tintColor, boolean isTintBackgroundRes,
                              int duration) {
        Toast currentToast = Toast.makeText(context, "", duration);
        View contentView = ((LayoutInflater) context.getSystemService(Context.LAYOUT_INFLATER_SERVICE))
                .inflate(R.layout.toast_layout, null);
        ImageView ivToastIcon = contentView.findViewById(R.id.iv_toast_icon);
        TextView tvToastText = contentView.findViewById(R.id.tv_toast_text);

        Drawable backgroundRes;
        if (isTintBackgroundRes) {
            backgroundRes = ToastyUtil.tint9PatchDrawableFrame(context, tintColor);
        } else {
            backgroundRes = ToastyUtil.getDrawable(context, R.mipmap.toast_frame);
        }
        ToastyUtil.setBackground(contentView, backgroundRes);

        if (icon == null) {
            ivToastIcon.setVisibility(View.GONE);
        } else {
            //isTintIcon:图标是否进行渲染(目的:使icon与字体颜色一致);将图标设置成ImageView背景
            ToastyUtil.setBackground(ivToastIcon, isTintIcon ? ToastyUtil.tintIcon(icon, textColor) : icon);
        }

        tvToastText.setText(text);
        tvToastText.setTextColor(textColor);
        tvToastText.setTypeface(currentTypeface);
        tvToastText.setTextSize(TypedValue.COMPLEX_UNIT_SP, textSize);
        currentToast.setView(contentView);
        currentToast.setGravity(currentGravity, currentXOffset, currentYOffset);

        //解决问题1.多次连续点击,多个吐司会一一显示消失,用户体验不好
        if (lastToast != null) {
            lastToast.cancel();
        }
        lastToast = currentToast;

        return currentToast;
    }

    /**
     * 配置参数
     */
    public static class Config {
        private Typeface typeface = Toasty.currentTypeface;
        private int textSize = Toasty.textSize;
        private boolean isTintIcon = Toasty.isTintIcon;

        private int mGravity = Toasty.currentGravity;
        private int mX = Toasty.currentXOffset;
        private int mY = Toasty.currentYOffset;

        private Config() {
        }

        @CheckResult
        public static Config getInstance() {
            return new Config();
        }

        @CheckResult
        public Config setTypeface(@NonNull Typeface typeface) {
            this.typeface = typeface;
            return this;
        }

        @CheckResult
        public Config setTextSize(int textSize) {
            this.textSize = textSize;
            return this;
        }

        @CheckResult
        public Config setGravity(int gravity, int xOffset, int yOffset) {
            this.mGravity = gravity;
            this.mX = xOffset;
            this.mY = yOffset;
            return this;
        }

        @CheckResult
        public Config isTintIcon(boolean isTintIcon) {
            this.isTintIcon = isTintIcon;
            return this;
        }

        public static void reset() {
            Toasty.currentTypeface = DEFAULT_TYPEFACE;
            Toasty.textSize = 16;
            Toasty.isTintIcon = true;
            Toasty.currentGravity = DEFAULT_GRAVITY;
            Toasty.currentXOffset = 0;
            Toasty.currentYOffset = 0;
        }

        public void apply() {
            Toasty.currentTypeface = typeface;
            Toasty.isTintIcon = isTintIcon;
            Toasty.textSize = textSize;
            Toasty.currentGravity = mGravity;
            Toasty.currentXOffset = mX;
            Toasty.currentYOffset = mY;
        }
    }
}
```



```java
/**
 * @Description: Toasty工具类
 * 9.png-左上定义伸缩区域,右下定义文本区域
 * Drawable.setColorFilter()渲染图片颜色;可用于一张图片,选中与非选中两种状态的情形下轻松解决,减少图片的导入
 * PorterDuff.Mode多种渲染模式(简单说:取并集,合集...)
 * @CreateDate: 2019/9/25 14:55
 */
class ToastyUtil {
    public static Drawable tint9PatchDrawableFrame(@NonNull Context context, @ColorInt int tintColor) {
        NinePatchDrawable toastDrawable = (NinePatchDrawable) getDrawable(context, R.mipmap.toast_frame);
        return tintIcon(toastDrawable, tintColor);
    }

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

    public static Drawable getDrawable(@NonNull Context context, @DrawableRes int id) {
        return AppCompatResources.getDrawable(context, id);
    }

    /**
     * 设置背景图片
     *
     * @param view
     * @param drawable
     */
    public static void setBackground(View view, Drawable drawable) {
        view.setBackground(drawable);
    }

    public static int getColor(Context context, @ColorRes int color) {
        return ContextCompat.getColor(context, color);
    }
}
```

