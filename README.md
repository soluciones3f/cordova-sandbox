cordova-sandbox
===============

A simple cordova sandbox to play a little, work a little 

Modificacion en el AndroidManifest:

<code>&lt;intent-filter&gt;
    &lt;action android:name=&quot;android.intent.action.SEND&quot; /&gt;
    &lt;action android:name=&quot;android.intent.action.SEND_MULTIPLE&quot; /&gt;
    &lt;category android:name=&quot;android.intent.category.DEFAULT&quot; /&gt;
    &lt;data android:mimeType=&quot;application/pdf&quot; /&gt;
&lt;/intent-filter&gt;</code>