### Android-Gson

```java
public class GsonUtils {

    public static <T> T json2Bean(String result, Class<T> clz) {

        try {
            if (TextUtils.isEmpty(result)) {
                return null;
            }

            Gson gson = new Gson();
            T t = gson.fromJson(result, clz);
            return t;
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }

    }

    public static <T> T json2Bean(Reader result, Class<T> clz) {
        if (result == null) {
            return null;
        }

        Gson gson = new Gson();
        T t = gson.fromJson(result, clz);
        return t;
    }

    public static String toJson(@NonNull Object obj) {

        try {
            Gson gson = new Gson();
            return gson.toJson(obj);
        } catch (Exception e) {
            e.printStackTrace();
            return "";
        }
    }

    /**
     * json字符串转List集合
     * 调用方法不会报错,需要类型转化的会报错
     *
     * @param <T>
     * @param jsonString
     * @return
     */
    public static <T> List<T> json2List(String jsonString) {
        Gson gson = new Gson();
        Type type = new TypeToken<List<T>>() {
        }.getType();

        return gson.fromJson(jsonString, type);
    }


    /**
     * json字符串转List集合(加强版)
     *
     * @param json
     * @param clazz
     * @param <T>
     * @return
     */
    public static <T> List<T> parseString2List(String json, Class clazz) {
        Type type = new ParameterizedTypeImpl(clazz);
        return new Gson().fromJson(json, type);
    }

    private static class ParameterizedTypeImpl implements ParameterizedType {
        Class clazz;

        ParameterizedTypeImpl(Class clz) {
            clazz = clz;
        }

        @NotNull
        @Override
        public Type[] getActualTypeArguments() {
            return new Type[]{clazz};
        }

        @NotNull
        @Override
        public Type getRawType() {
            return List.class;
        }

        @Override
        public Type getOwnerType() {
            return null;
        }
    }
}

```



- getActualTypeArguments 返回实际类型组成的数据，即new Type[]{String.class,Integer.class}

- getRawType 返回原生类型，即 HashMap

- getOwnerType 返回 Type 对象，表示此类型是其成员之一的类型。例如，如果此类型为 `O.I`，则返回 `O` 的表示形式。 如果此类型为顶层类型，则返回 null。这里就直接返回null就行了。


  链接：https://www.jianshu.com/p/701ae370f959










