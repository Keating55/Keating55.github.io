# DCL双锁检查机制

`synchronized`里外都设置一个if判断

单例模式-懒汉模式

```java
public class IpConfig {
    private static Properties properties;

    public static Properties getInstance() {
        if (properties == null) {
            synchronized (IpConfig.class) {
                if (properties == null) {
                    properties = Init.SetProperties("resource/ipConfig.properties");
                }
            }
        }
        return properties;
    }
}

```

