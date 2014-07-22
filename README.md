cordova-sandbox
===============

A simple cordova sandbox to play a little, work a little 

para agregar el boton de compartir cuando se abre un pdf en android,
Modificacion en el AndroidManifest (Agregar dentro del activity):

```xml
<intent-filter>
    <action android:name="android.intent.action.SEND" />
    <action android:name="android.intent.action.SEND_MULTIPLE" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:mimeType="application/pdf" />
</intent-filter>
```