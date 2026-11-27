name: Simple AKS APK

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'
      - run: |
          mkdir -p app/src/main/java/com/aks/callcenter
          cat << 'EOF' > settings.gradle
          rootProject.name = "AKSAutoCallCenter"
          include ':app'
          EOF
          cat << 'EOF' > build.gradle
          buildscript {
              repositories { google(); mavenCentral() }
              dependencies { classpath 'com.android.tools.build:gradle:8.1.4' }
          }
          allprojects {
              repositories { google(); mavenCentral() }
          }
          EOF
          cat << 'EOF' > app/build.gradle
          plugins { id 'com.android.application' }
          android {
              namespace 'com.aks.callcenter'
              compileSdk 33
              defaultConfig {
                  applicationId "com.aks.callcenter"
                  minSdk 24
                  targetSdk 33
                  versionCode 1
                  versionName "1.0"
              }
          }
          dependencies {
              implementation 'androidx.appcompat:appcompat:1.6.1'
          }
          EOF
          cat << 'EOF' > app/src/main/AndroidManifest.xml
          <manifest xmlns:android="http://schemas.android.com/apk/res/android">
              <uses-permission android:name="android.permission.CALL_PHONE" />
              <uses-permission android:name="android.permission.READ_PHONE_STATE" />
              <uses-permission android:name="android.permission.ANSWER_PHONE_CALLS" />
              <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
              <application android:label="AKS Auto Call Center" android:theme="@style/Theme.AppCompat.Light.NoActionBar">
                  <activity android:name=".MainActivity" android:exported="true">
                      <intent-filter>
                          <action android:name="android.intent.action.MAIN" />
                          <category android:name="android.intent.category.LAUNCHER" />
                      </intent-filter>
                  </activity>
              </application>
          </manifest>
          EOF
          cat << 'EOF' > app/src/main/java/com/aks/callcenter/MainActivity.java
          package com.aks.callcenter;
          import android.Manifest;
          import android.content.Intent;
          import android.content.pm.PackageManager;
          import android.net.Uri;
          import android.os.Bundle;
          import android.widget.Button;
          import android.widget.EditText;
          import android.widget.LinearLayout;
          import androidx.appcompat.app.AppCompatActivity;
          import androidx.core.app.ActivityCompat;
          public class MainActivity extends AppCompatActivity {
              @Override
              protected void onCreate(Bundle savedInstanceState) {
                  super.onCreate(savedInstanceState);
                  LinearLayout box = new LinearLayout(this);
                  box.setOrientation(LinearLayout.VERTICAL);
                  box.setPadding(40, 60, 40, 60);
                  EditText phone = new EditText(this);
                  phone.setHint("03001234567");
                  Button callBtn = new Button(this);
                  callBtn.setText("CALL NOW");
                  callBtn.setOnClickListener(v -> {
                      String num = phone.getText().toString().trim();
                      if (!num.isEmpty()) {
                          ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.CALL_PHONE}, 1);
                          if (ActivityCompat.checkSelfPermission(this, Manifest.permission.CALL_PHONE) == PackageManager.PERMISSION_GRANTED) {
                              startActivity(new Intent(Intent.ACTION_CALL, Uri.parse("tel:" + num)));
                          }
                      }
                  });
                  box.addView(phone);
                  box.addView(callBtn);
                  setContentView(box);
              }
          }
          EOF
          gradle assembleDebug --no-daemon
      - uses: actions/upload-artifact@v4
        with:
          name: AKS-Call-App
          path: app/build/outputs/apk/debug/app-debug.apk
