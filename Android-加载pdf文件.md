### Android-加载pdf文件

- 使用方法

```java
try {
    //  sdcard/test1.pdf为本地pdf文件的路径
    File file = FileUtil.fileFromAsset(MyUserCenterActivity.this, "iCMES-移动端用户手册-A4.pdf");
    Intent intent = getPdfFileIntent(file);
    startActivity(intent);
} catch (Exception e) {
    //没有安装第三方的软件会提示
    ToastUtil.show("没有找到打开该文件的应用程序");
}
```

- 

```java
public Intent getPdfFileIntent(File file) {
    Intent intent = new Intent("android.intent.action.VIEW");
    intent.addCategory("android.intent.category.DEFAULT");
    intent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
    intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
    Uri uri;
    if (Build.VERSION.SDK_INT >= 24) {
    	uri = FileProvider.getUriForFile(MyUserCenterActivity.this, getPackageName(), file);
    } else {
    	uri = Uri.fromFile(file);
    }

    intent.setDataAndType(uri, "application/pdf");
    return Intent.createChooser(intent, "Open File");
}
```



```java
	/**
     * 获取assets下的文件并以文件的形式返回
     *
     * @param context
     * @param assetName
     * @return
     * @throws IOException
     */
public static File fileFromAsset(Context context, String assetName) throws IOException {
    File outFile = new File(context.getCacheDir(), assetName);
    copy(context.getAssets().open(assetName), outFile);
    return outFile;
}

public static void copy(InputStream inputStream, File output) throws IOException {
    OutputStream outputStream = null;
    try {
        outputStream = new FileOutputStream(output);
        int read = 0;
        byte[] bytes = new byte[1024];
        while ((read = inputStream.read(bytes)) != -1) {
            outputStream.write(bytes, 0, read);
        }
    } finally {
        try {
            if (inputStream != null) {
                inputStream.close();
            }
        } finally {
            if (outputStream != null) {
                outputStream.close();
            }
        }
    }
}
```

