### SharedPreference

```java
/**
 * SharedPreferences工具类
 */

public class BasePreference {

    private Context context;
    private SharedPreferences sp;
    private String SP_FILE_NAME = "CanYanNet";


    protected BasePreference(Context context) {
        this.context = context;
        sp = context.getSharedPreferences(SP_FILE_NAME, Context.MODE_PRIVATE);
    }

    /**
     * SP中写入String类型value
     *
     * @param key   键
     * @param value 值
     */
    public void putString(String key, String value) {
        sp.edit().putString(key, value).apply();
    }

    /**
     * SP中读取String
     *
     * @param key 键
     */
    public String getString(String key) {
        return sp.getString(key, "");
    }


    public void putInt(String key, int value) {
        sp.edit().putInt(key, value).apply();
    }

    public int getInt(String key) {
        return sp.getInt(key, 0);
    }

    public void putLong(String key, long value) {
        sp.edit().putLong(key, value).apply();
    }

    public long getLong(String key) {
        return sp.getLong(key, 0);
    }

    public void putFloat(String key, float value) {
        sp.edit().putFloat(key, value).apply();
    }

    public float getFloat(String key) {
        return sp.getFloat(key, -1F);
    }

    public void putBoolean(String key, boolean value) {
        sp.edit().putBoolean(key, value).apply();
    }

    public boolean getBoolean(String key) {
        return sp.getBoolean(key, false);
    }

    public void putStringSet(String key,   Set<String> set) {
        sp.edit().putStringSet(key, set).apply();

    }

    public Set<String> getStringSet(String key) {
        return sp.getStringSet(key,new LinkedHashSet<String>());
    }

    public void remove(String key) {
        sp.edit().remove(key).apply();
    }

    /**
     * 清除指定的信息
     *
     * @param name 键名
     * @param key  若为null 则删除name下所有的键值
     */
    public void clearPreference(String name, String key) {
        SharedPreferences sharedPreferences = context.getSharedPreferences(name, Context.MODE_PRIVATE);
        SharedPreferences.Editor editor = sharedPreferences.edit();
        if (key != null) {
            editor.remove(key);
        } else {
            editor.clear();
        }
        editor.apply();
    }
}
```

```java
public class PreferenceUtils extends BasePreference {
    private static final String USER_PHONE = "user_phone";
    private static final String USER_NICKNAME = "user_nickname";
    private static final String USER_HEAD_ICON = "user_head_icon";
    private static final String USER_BALANCE = "user_balance";
    private static final String USER_TOKEN = "user_token";
    private static final String IS_LOGINED = "is_logined";
    private static final String CITY_ID = "city_id";
    private static final String ISSET_PASSWORD = "isset_password";
    private static final String SAVE_SEARCH_RECORD = "save_search_record";
    private static final String PROVINCE_ID = "province_id";
    private static final String POPDAY_INHOME = "popday_inhome";
    private static final String SERVICEPHONE = "service_num";//客服电话
    private static final String USER_SEX = "user_sex";
    private static final String DEVICE_IMEI = "deviceIMEI";
    private static PreferenceUtils preferenceUtils;
    /**
     * 需要增加key就在这里新建
     */
    //用户名的key
    private String USER_NAME = "user_name";
    //是否首次启动的key
    private String IS_LAUNCHED = "is_launched";

    //userId
    private static final String USER_ID = "user_id";

    //经纬度
    private static final String LOC_LATITUDE = "loc_latitude";
    private static final String LOC_LONGITUDE = "loc_longitude";
    private static final String LOC_NAME = "loc_name";
    private static final String OLD_LATITUDE = "old_latitude";
    private static final String OLD_LONGITUDE = "old_longitude";
    private static String deviceIMEI;

    private PreferenceUtils(Context context) {
        super(context);
    }

    /**
     * 通过Application来获取Context对象，所以在获取preferenceUtils时不需要传入Context。
     *
     * @return
     */
    public synchronized static PreferenceUtils getInstance() {
        if (null == preferenceUtils) {
            preferenceUtils = new PreferenceUtils(App.getInstance());
        }
        return preferenceUtils;
    }


    //是否启动过
    public void setLaunched(boolean isLaunched) {
        putBoolean(IS_LAUNCHED, isLaunched);
    }

    public boolean isLaunched() {
        return getBoolean(IS_LAUNCHED);
    }

    //是否登陆过
    public void setLogined(boolean isLogined) {
        putBoolean(IS_LOGINED, isLogined);
    }

    public boolean isLogined() {
        return getBoolean(IS_LOGINED);
    }


    //userName
    public void setUserName(String name) {
        putString(USER_NAME, name);
    }

    public String getUserName() {
        return getString(USER_NAME);
    }

    //userId
    public void setUserId(String userId) {
        putString(USER_ID, userId);
    }

    public String getUserId() {
        return getString(USER_ID);
    }

    //userPhone
    public void setPhoneNumber(String phoneNum) {
        putString(USER_PHONE, phoneNum);
    }

    public String getPhoneNumber() {
        return getString(USER_PHONE);
    }

    //user_nickname
    public void setNickname(String nickname) {
        putString(USER_NICKNAME, nickname);
    }

    public String getNickname() {
        return getString(USER_NICKNAME);
    }

    //user_head_icon
    public void setHeadIcon(String headIcon) {
        putString(USER_HEAD_ICON, headIcon);
    }

    public String getHeadIcon() {
        return getString(USER_HEAD_ICON);
    }

    //user_balance
    public void setBalance(String balance) {
        putString(USER_BALANCE, balance);
    }

    public String getBalance() {
        return getString(USER_BALANCE);
    }

    //user_token
    public void setToken(String token) {
        putString(USER_TOKEN, token);
    }

    public String getToken() {
        return getString(USER_TOKEN);
    }

    public void setServicephone(String servicephone) {
        putString(SERVICEPHONE, servicephone);
    }

    public String getServicephone() {
        return getString(SERVICEPHONE);
    }

    //保存经度
    public void setLongitude(String longitude) {
        putString(LOC_LONGITUDE, longitude);
    }

    public String getLongitude() {
        return getString(LOC_LONGITUDE);
    }

    //保存纬度
    public void setLatitude(String latitude) {
        putString(LOC_LATITUDE, latitude);
    }

    public String getLatitude() {
        return getString(LOC_LATITUDE);
    }

    //保存定位城市
    public void setLocationName(String name) {
        putString(LOC_NAME, name);
    }

    public String getLocationName() {
        return getString(LOC_NAME);
    }

    //保存城市id
    public void setCityId(String cityId) {
        putString(CITY_ID, cityId);
    }

    public String getCityId() {
        return getString(CITY_ID);
    }

    public void setProvinceId(String province) {
        putString(PROVINCE_ID, province);
    }

    public String getProvinceId() {
        return getString(PROVINCE_ID);
    }


    public void isset_password(String isset_password) {
        putString(ISSET_PASSWORD, isset_password);
    }

    public String isset_password() {
        return getString(ISSET_PASSWORD);
    }

    public void saveSearchRecord(String record) {
        putString(SAVE_SEARCH_RECORD, record);
    }

    public String getSearchRecord() {
        return getString(SAVE_SEARCH_RECORD);
    }


    //设置首页"优惠弹出框"日
    public void setPopDayInHome(int day) {
        putInt(POPDAY_INHOME, day);
    }

    //获取首页"优惠弹出框"日
    public int getPopDayInHome() {
        return getInt(POPDAY_INHOME);
    }

    public String getOldLatitude() {
        return getString(OLD_LATITUDE);
    }

    public void setOldLatitude(String oldLatitude) {
        putString(OLD_LATITUDE, oldLatitude);
    }

    public String getOldLongitude() {
        return getString(OLD_LONGITUDE);
    }

    public void setOldLongitude(String oldLongitude) {
        putString(OLD_LONGITUDE, oldLongitude);
    }

    public void setDeviceIMEI(String deviceIMEI) {
        putString(DEVICE_IMEI, deviceIMEI);
    }

    public String getDeviceIMEI() {
        return getString(DEVICE_IMEI);
    }

    public String getUserSex() {
        return getString(USER_SEX);
    }

    public void setUserSex(String sex) {
        putString(USER_SEX, sex);
    }
}

```

