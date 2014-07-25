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

##### Notificaciones desde el servidor
Implemento PushPlugin https://github.com/phonegap-build/PushPlugin

Tener en cuenta al instalarlo de comprobar que el AndroidManifest este de acuerdo a lo que marca el plugin

Para que funcione correctamente, se realizo una modificacion en GCMIntentService.java:

en la linea 67, se modifica
```groovy
if ( PushPlugin.isInForeground() ){
```
por
```groovy
if ( PushPlugin.isInForeground() && PushPlugin.isActive() ){
```

en index.html esta el codigo javascript para manejar el registro y manejo de la notificacion

```javascript
document.addEventListener('deviceready', function () {
    pushNotification = window.plugins.pushNotification;
    pushNotification.register(
        successHandler,
        errorHandler,
        {
            "senderID":"Project Number",
            "ecb":"onNotification" //metodo que se llama una vez que android retorna el token del dispositivo, 
            //o cuando llega una notificacion (Si se encuentra en primer plano, si no se ejecuta cuando se accede a la app a travez de la notificacion del status bar)
        });
})
```
el Project Number, se genera al registrar el proyecto en android, https://console.developers.google.com/project
Ahi, tambien hay que obtener el Api Key, en la seccion de APIS & AUTH -> Credentials, Public API access


El onNotification va a ser el encargado de registrarce en el servidor y recibir el mensaje

A travez del evento que llega, va a tener los dos casos importantes, 'registered' y 'message':

###### registered

Este evento recibe el token que relaciona la app con el dispositivo, debe enviarselo al backend para que este lo guarde y pueda enviarle los mensajes.

###### message

Este evento recibe el mensaje en si, segun cuando llego el evento y como se estaba ejecutando la app.

1.  si la app se encuentra activo, procesa el mensaje inmediatamente

2.  Si la app esta corriendo pero no esta activa, llega una notificacion en la barra de estado y se ejecuta una vez que el usuario accede al la misma

3.  Si la app estaba detenida, igual caso anterior

A pesar de que los dos ultimos casos son iguales en como se accede, en el javascript es posible diferenciar uno del otro con la variable e.coldstart

```javascript
function onNotification(e) {
    $("#resultado").append('<li>EVENT -> RECEIVED:' + e.event + '</li>');

    switch( e.event )
    {
    case 'registered':
        if ( e.regid.length > 0 )
        {
            // e.regid ->token de registro del dispositivo, enviar al servidor
        }
    break;

    case 'message':
        //Notificacion del servidor
        if ( e.foreground )
        {
            //Notificacion mientras la app esta en primer plano
        }
        else
        {  
            //la aplicacion no estaba en primer plano
            if ( e.coldstart )
            {
                //la aplicacion estaba cerrada, se ejecuto al abrir la app por la notificacion
            }
            else
            {
                //la app se estaba ejecutando, pero en segundo plano
            }
        }
    break;

    case 'error':
        $("#resultado").append('<li>ERROR -> MSG:' + e.msg + '</li>');
    break;

    default:
        $("#resultado").append('<li>EVENT -> Unknown, an event was received and we do not know what it is</li>');
    break;
  }
}
```