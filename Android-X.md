

### Android 6.0 （API 级别 23）

- ##### 运行时权限(动态申请权限)

  ```
  checkSelfPermission() //检查应用是否已被授予权限
  requestPermissions() //申请权限
  ```

### Android 7.0

- ##### 网络安全配置

```xml
    <?xml version="1.0" encoding="utf-8"?>
    <manifest ... >
        <application android:networkSecurityConfig="@xml/network_security_config"
                        ... >
            ...
        </application>
    </manifest>
    
```

##### res/xml/network_security_config.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config xmlns:android="http://schemas.android.com/apk/res/android">
    <base-config cleartextTrafficPermitted="true" />
</network-security-config>
```



- ##### 文件安全访问 FileProvider

  - 快速完成适配

  1. FileProvider的注册

  ```xml
  <application>
      <provider
          android:name="android.support.v4.content.FileProvider"
          android:authorities="${applicationId}.android7.fileprovider"
          android:exported="false"
          android:grantUriPermissions="true">
          <meta-data
              android:name="android.support.FILE_PROVIDER_PATHS"
              android:resource="@xml/file_paths" />
      </provider>
  </application>
  ```

  2. 编写res/xml/file_paths

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <paths xmlns:android="http://schemas.android.com/apk/res/android">
      <root-path
          name="root"
          path="" />
      <files-path
          name="files"
          path="" />
  
      <cache-path
          name="cache"
          path="" />
  
      <external-path
          name="external"
          path="" />
  
      <external-files-path
          name="external_file_path"
          path="" />
      <external-cache-path
          name="external_cache_path"
          path="" />
  
  </paths>
  ```

  - <root-path/> 代表设备的根目录new File("/");

  - <files-path/> 代表context.getFilesDir()

  - <cache-path/> 代表context.getCacheDir()

  - <external-path/> 代表Environment.getExternalStorageDirectory()

  - <external-files-path>代表context.getExternalFilesDirs()

  - <external-cache-path>代表getExternalCacheDirs()

  

  - 辅助类

  ```java
  public class FileProvider7 {
  
      public static Uri getUriForFile(Context context, File file) {
          Uri fileUri = null;
          if (Build.VERSION.SDK_INT >= 24) {
              fileUri = getUriForFile(context, file);
          } else {
              fileUri = Uri.fromFile(file);
          }
          return fileUri;
      }
  
      public static Uri getUriForFile(Context context, File file) {
          Uri fileUri = FileProvider.getUriForFile(context,
                  context.getPackageName() + ".android7.fileprovider",file);
          return fileUri;
      }
  
  
      public static void setIntentDataAndType(Context context,
                                              Intent intent,
                                              String type,
                                              File file,
                                              boolean writeAble) {
          if (Build.VERSION.SDK_INT >= 24) {
              intent.setDataAndType(getUriForFile(context, file), type);
              intent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
              if (writeAble) {
                  intent.addFlags(Intent.FLAG_GRANT_WRITE_URI_PERMISSION);
              }
          } else {
              intent.setDataAndType(Uri.fromFile(file), type);
          }
      }
  }
  ```

  - 使用
    1. 使用FileProvider兼容安装apk

使用FileProvider安装apk需要授予URI临时权限，也就是Context.grantUriPermission(package, Uri, mode_flags)，这将授予该内容的URI指定的包临时访问权限，根据该值mode_flags参数，该参数可以设置为 FLAG_GRANT_READ_URI_PERMISSION，FLAG_GRANT_WRITE_URI_PERMISSION,通过调用revokeUriPermission()或设备重新启动来撤消它

也可以直接使用以下方式授予临时权限



```undefined
intent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION |
Intent.FLAG_GRANT_WRITE_URI_PERMISSION);
```

安装apk实例：

```java
public void installApk(View view) {
    File file = new File(Environment.getExternalStorageDirectory(), "testandroid7-debug.apk");
    Intent intent = new Intent(Intent.ACTION_VIEW);
    setIntentDataAndType(this,intent,""application/vnd.android.package-archive",file,true);
            "application/vnd.android.package-archive");
    startActivity(intent);
}

    public static void setIntentDataAndType(Context context,
                                            Intent intent,
                                            String type,
                                            File file,
                                            boolean writeAble) {
        if (Build.VERSION.SDK_INT >= 24) {
            intent.setDataAndType(getUriForFile(context, file), type);
            intent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
            if (writeAble) {
                intent.addFlags(Intent.FLAG_GRANT_WRITE_URI_PERMISSION);
            }
        } else {
            intent.setDataAndType(Uri.fromFile(file), type);
        }
    }
```

​			2. 调用系统拍照并获取图片实例：

```java
public class ExistingCameraActivity extends AppCompatActivity {

    private static final int REQUEST_IMAGE_CAPTURE = 1122;
    @BindView(R.id.iv)
    ImageView mIv;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_existing_camera);
        ButterKnife.bind(this);
        // 该条件 resolveActivity()返回可以处理intent的第一个活动组件。
        // 因为如果startActivityForResult()使用无应用程序无法处理的意图进行调用，应用程序将崩溃
        Intent takePictureIntent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
        if (takePictureIntent.resolveActivity(getPackageManager()) != null) {

            File photoFile = createImageFile();

            if (photoFile != null) {
                Uri photoURI;
                //版本校验,或者grantUriPermission授权、revokeUriPermission移除权限
                if (Build.VERSION.SDK_INT >= 24) {
                    photoURI = FileProvider.getUriForFile(this, "com.fanfan.fileprovider", photoFile);
                } else {
                    photoURI = Uri.fromFile(photoFile);
                }

                takePictureIntent.putExtra(MediaStore.EXTRA_OUTPUT, photoURI);
                startActivityForResult(takePictureIntent, REQUEST_IMAGE_CAPTURE);
            }
        }
    }

    String currentPhotoPath;

    private File createImageFile() {
        String timeStamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
        String imageFileName = "JPEG_" + timeStamp + "_";
        File image = new File(Environment.getExternalStorageDirectory(), imageFileName + ".jpg");
        currentPhotoPath = image.getAbsolutePath();
        return image;
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        if (requestCode == REQUEST_IMAGE_CAPTURE && resultCode == RESULT_OK) {
            //获取拍摄到的图片缩略图
//          Bundle extras = data.getExtras();
//          Bitmap imageBitmap = (Bitmap) extras.get("data");
//          mIv.setImageBitmap(imageBitmap);

            //获取拍摄到的图片
            mIv.setImageBitmap(BitmapFactory.decodeFile(currentPhotoPath));

        }
    }
}
```



大家可能存在疑问，**为什么安卓7设备拍照没有增加临时权限？**

其实是因为Intent的action为ACTION_IMAGE_CAPTURE，当我们startActivity后，会辗转调用Instrumentation的execStartActivity方法，在该方法内部，会调用intent.migrateExtraStreamToClipData()，方法内部会
 给我们添加了WRITE和READ权限。（注意：addFlags方式对于ACTION_IMAGE_CAPTURE在5.0以下是无效的）

