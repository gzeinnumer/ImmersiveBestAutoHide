# ImmersiveBestAutoHide

|<img src="https://github.com/gzeinnumer/ImmersiveBestAutoHide/blob/master/preview/example2.jpg" width="200"/>|<img src="https://github.com/gzeinnumer/ImmersiveBestAutoHide/blob/master/preview/example3.jpg" width="200"/>|<img src="https://github.com/gzeinnumer/ImmersiveBestAutoHide/blob/master/preview/example4.jpg" width="200"/>|
|---|---|---|
|Samsung A51(White Nav Bar)|Vivo 1719|Samsung A51(Black Nav Bar)|

- `Manifest.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest >

    <application
        android:theme="@style/Theme.ImmersiveBestAutoHide">
        <activity
            android:name=".MainActivity"
            android:theme="@style/Theme.VeryFullScreen.Splash">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

- `style.xml`
```xml
<style name="Theme.VeryFullScreen.Splash" parent="Theme.MaterialComponents.Light.NoActionBar">
    <item name="android:statusBarColor">@android:color/white</item>
    <item name="colorPrimary">@android:color/holo_red_light</item>
    <item name="colorPrimaryVariant">@android:color/holo_green_light</item>
    <item name="colorOnPrimary">@android:color/holo_blue_light</item>
    <item name="colorSecondary">@android:color/holo_red_light</item>
    <item name="colorSecondaryVariant">@android:color/holo_green_light</item>
    <item name="colorOnSecondary">@android:color/holo_blue_light</item>
    <item name="android:fitsSystemWindows">false</item>
    <item name="android:navigationBarColor">@color/teal_200</item>
    <item name="android:windowLightNavigationBar" tools:targetApi="27">true</item>
</style>
```

<p align="center">
  <img src="https://github.com/gzeinnumer/ImmersiveBestAutoHide/blob/master/preview/example1.jpg"/>
</p>

- `BaseAcivity.java`
```java
public class BaseActivity extends AppCompatActivity {

    ...

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    }
}
```

- `BaseActivityFull.java`
```java
public class BaseActivityFull extends BaseActivity{

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setFullScreen();
        setForceImmersiveScreen();
    }

    protected void setPaddingToTopParentView(View view){
        int statusBarHeight = getStatusBarHeight(this);
        view.setPadding(0, statusBarHeight, 0, 0);
    }

    public int getStatusBarHeight(Activity activity) {
        int result = 0;
        int resourceId = activity.getResources().getIdentifier("status_bar_height", "dimen", "android");
        if (resourceId > 0) {
            result = activity.getResources().getDimensionPixelSize(resourceId);
        }
        return result;
    }

    private void setForceImmersiveScreen() {
        final Handler forceImmersive = new Handler();
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                setFullScreen();

                forceImmersive.postDelayed(this, 5000);
            }
        };

        forceImmersive.postDelayed(runnable, 5000);
    }

    protected void setFullScreen() {
        //SYSTEM_UI_FLAG_IMMERSIVE_STICKY -> AUTO HIDE
        //SYSTEM_UI_FLAG_IMMERSIVE -> NOT AUTO HIDE | CAN SET BACK HOME COLOR
        int decore = View.SYSTEM_UI_FLAG_IMMERSIVE
                | View.SYSTEM_UI_FLAG_LAYOUT_STABLE
                | View.SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION
//                | View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN
                // Hide the nav bar and status bar
                | View.SYSTEM_UI_FLAG_HIDE_NAVIGATION;

        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
            //enable this tho maker icon status bar become black
            decore+= View.SYSTEM_UI_FLAG_LIGHT_STATUS_BAR;
        }

        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            //enable this tho maker icon Navigation bar become black
            decore +=  View.SYSTEM_UI_FLAG_LIGHT_NAVIGATION_BAR;
        }

        getWindow().getDecorView().setSystemUiVisibility(decore);
    }
}
```

- `MainActivity.java`
```java
public class MainActivity extends BaseActivityFull {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ConstraintLayout parent  = findViewById(R.id.parent);
        setPaddingToTopParentView(parent);
    }
}
```

- `activity_main.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/parent"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/purple_700"
    tools:context=".MainActivity">

    <EditText
        app:layout_constraintTop_toTopOf="parent"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Hello World!"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

---

```
Copyright 2021 M. Fadli Zein
```

