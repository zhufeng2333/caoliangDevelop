### lab5	MainActivity.java

``` java
package com.example.lab5;

import androidx.appcompat.app.AppCompatActivity;
import android.content.Intent;
import android.graphics.Bitmap;
import android.net.Uri;
import android.os.Bundle;
import android.view.View;
import android.widget.EditText;
import android.widget.ImageView;

public class MainActivity extends AppCompatActivity {

    private static final int CAMERA_REQUEST = 8888;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    public void openWeb(View view) {
        EditText editText = findViewById(R.id.editText);
        Uri uri = Uri.parse(editText.getText().toString());
        Intent intent = new Intent(Intent.ACTION_VIEW, uri);
        startActivity(intent);
    }

    public void openMap(View view) {
        EditText editText = findViewById(R.id.editText2);
        Intent mapIntent = new Intent(Intent.ACTION_VIEW);
        mapIntent.setData(Uri.parse("baidumap://map/geocoder?src=andr.baidu.openAPIdemo&address="+editText.getText()));
        startActivity(mapIntent);
    }

    public void share(View view) {
        EditText editText = findViewById(R.id.editText3);
        Intent sendIntent = new Intent();
        sendIntent.setAction(Intent.ACTION_SEND);
        sendIntent.putExtra(Intent.EXTRA_TEXT, editText.getText().toString());
        sendIntent.setType("text/plain");
        startActivity(Intent.createChooser(sendIntent,"share"));
    }

    public void takePhoto(View view) {
        Intent cameraIntent = new Intent(android.provider.MediaStore.ACTION_IMAGE_CAPTURE);
        startActivityForResult(cameraIntent, CAMERA_REQUEST);
    }
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == CAMERA_REQUEST && resultCode == RESULT_OK) {
            Bitmap photo = (Bitmap) data.getExtras().get("data");
            ImageView imageIV = findViewById(R.id.imageView);
            imageIV.setImageBitmap(photo);
        }
    }
}
```

### lab5	activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <EditText
        android:id="@+id/editText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Please enter website"
        tools:layout_editor_absoluteX="50dp"
        tools:layout_editor_absoluteY="32dp" />

    <Button
        android:id="@+id/button"
        android:layout_width="158dp"
        android:layout_height="wrap_content"
        android:onClick="openWeb"
        android:text="OPEN WEBSITE"
        app:layout_constraintStart_toStartOf="@+id/editText"
        tools:layout_editor_absoluteY="73dp" />


    <EditText
        android:id="@+id/editText2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Please enter location"
        app:layout_constraintStart_toStartOf="@+id/button"
        tools:layout_editor_absoluteY="174dp" />

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:onClick="openMap"
        android:text="OPEN LOCATION"
        app:layout_constraintStart_toStartOf="@+id/editText2"
        tools:layout_editor_absoluteY="213dp" />

    <EditText
        android:id="@+id/editText3"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Please enter message"
        app:layout_constraintStart_toStartOf="@+id/button2"
        tools:layout_editor_absoluteY="315dp" />

    <Button
        android:id="@+id/button3"
        android:layout_width="154dp"
        android:layout_height="wrap_content"
        android:onClick="share"
        android:text="SHARE THIS TEXT"
        app:layout_constraintStart_toStartOf="@+id/editText3"
        tools:layout_editor_absoluteY="362dp" />

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="match_parent"
        android:layout_height="283dp"
        tools:layout_editor_absoluteX="349dp"
        tools:layout_editor_absoluteY="431dp"
        tools:srcCompat="@tools:sample/avatars"
        android:background="@drawable/background" />

    <Button
        android:id="@+id/takephotobtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:onClick="takePhoto"
        android:text="TAKE A PHOTO"
        tools:layout_editor_absoluteX="81dp"
        tools:layout_editor_absoluteY="487dp" />
</LinearLayout>
```

### lab5_rec	MainActivity.java

```java
package com.example.lab5_rec;

import androidx.appcompat.app.AppCompatActivity;
import android.content.Intent;
import android.os.Bundle;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Intent intent = getIntent();
        TextView textView = findViewById(R.id.textView);
        try {
            textView.setText("url:"+intent.getData().toString());
        }catch (Exception e){

        }
    }
}
```

### lab5_rec	AndroidManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.lab5_rec">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Lab5_rec">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="http"
                    android:host="www.baidu.com" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

