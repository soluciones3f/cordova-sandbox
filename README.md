cordova-sandbox
===============

A simple cordova sandbox to play a little, work a little 

Modificacion en el AndroidManifest:

<intent-filter>
    <action android:name="android.intent.action.SEND" />
    <action android:name="android.intent.action.SEND_MULTIPLE" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:mimeType="application/pdf" />
</intent-filter>