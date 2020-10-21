# Firebase Cloud Messaging para Flutter

[Guide in English](https://github.com/FirebaseExtended/flutterfire/blob/master/packages/firebase_messaging/README.md)  

[![pub package](https://img.shields.io/pub/v/firebase_messaging.svg)](https://pub.dev/packages/firebase_messaging)

O plugin Flutter usa o [Firebase Cloud Messaging (FCM) API](https://firebase.google.com/docs/cloud-messaging/).

Com esse plugin, sua aplicação em Flutter pode receber e processar notificações push bem como dados de mensagens no Android e IOS. Leia sobre [Firebase FCM Messages](https://firebase.google.com/docs/cloud-messaging/concept-options) para aprender mais sobre as diferenças entre as mensagens de notificações e as mensagens de dados.

Para plugins Flutter para outras finalidades no Firebase, veja [README.md](https://github.com/FirebaseExtended/flutterfire/blob/master/README.md).

# Usando
Para usar esse plugin, adicione `firebase_messaging` como uma [dependência em seu pubspec.yaml file](https://flutter.io/platform-plugins/).

# Começando

Cheque o diretorio de [`exemplo`](https://github.com/FirebaseExtended/flutterfire/tree/master/packages/firebase_messaging/example) para uma amostra de aplicação usando o Firebase Cloud Messaging.

# Integração para Android

Para integrar esse plugin a sua aplicação Android, siga os seguintes passos: 

1. Usando o [Console do Firebase](https://console.firebase.google.com/) adicione um Android app para seu projeto: Siga o assistente, baixe o arquivo gerado `google-services.json` e coloque dentro de `android/app`.

2. Adicione o *classpath* no arquivo `[projeto]/android/build.gradle`
```
dependencies {
  // Exemplo de um classpath já existente no arquivo
  classpath 'com.android.tools.build:gradle:3.5.3'
  // Adicione o classpath do serviço do google 
  classpath 'com.google.gms:google-services:4.3.2'
}
```

3. Adicione o *apply plugin* no arquivo `[projeto]/android/app/build.gradle`
```
// Adicione isso na parte inferior do arquivo
apply plugin: 'com.google.gms.google-services'
```

Observe: Se essa parte não for completada você receberá um erro como esse:
```v
java.lang.IllegalStateException:
Default FirebaseApp is not initialized in this process [package name].
Make sure to call FirebaseApp.initializeApp(Context) first.
```

Observe: Quando você for debugar no Android, use um dispositivo ou emulador AVD com Google Play services. De outro modo você não estará habilitado para ser autenticado.

4. (opcional, mas recomendado) Se quiser ser notificado em sua aplicação (via `onResume` and `onLaunch`, veja abaixo) quando o usuário clicar na notificação o sistema abrirá o seguinte `intent-filter` dentro da tag `<activity>`de seu arquivo `android/app/src/main/AndroidManifest.xml`:
  ```xml
  <intent-filter>
      <action android:name="FLUTTER_NOTIFICATION_CLICK" />
      <category android:name="android.intent.category.DEFAULT" />
  </intent-filter>
  ```

### Lidar com mensagens em segundo plano opcionalmente

>Os tratamentos de mensagem em segundo plano devem ser rápidos, pois tarefas longas podem não ser permitidas pelo sistema Android. Veja mais em [Limites de Execução em Segundo Plano](https://developer.android.com/about/versions/oreo/background).

Por padrão mensagens em segundo plano não estão habilitadas, para habilitar:

1. Adicione a dependência `com.google.firebase:firebase-messaging` no nível de sua aplicação, no arquivo `build.gradle` que geralmente se encontra em `<nome-aplicacao>/android/app/build.gradle`.

   ```gradle
   dependencies {
     // ...
   
     implementation 'com.google.firebase:firebase-messaging:<ultima-versao>'
   }
   ```
   
   Observe: você pode encontrar a última versão do plugin [aqui ("Cloud Messaging")](https://firebase.google.com/support/release-notes/android#latest_sdk_versions).

1. Adicione / crie a classe `Application.java` em sua aplicação no mesmo diretorio que `MainActivity.java`. Geralmente se encontra em `<nome-aplicacao>/android/app/src/main/java/<caminho-organicao-nome>/`.

   ```java
   package io.flutter.plugins.firebasemessagingexample;
   
   import io.flutter.app.FlutterApplication;
   import io.flutter.plugin.common.PluginRegistry;
   import io.flutter.plugin.common.PluginRegistry.PluginRegistrantCallback;
   import io.flutter.plugins.GeneratedPluginRegistrant;
   import io.flutter.plugins.firebasemessaging.FlutterFirebaseMessagingService;
   
   public class Application extends FlutterApplication implements PluginRegistrantCallback {
     @Override
     public void onCreate() {
       super.onCreate();
       FlutterFirebaseMessagingService.setPluginRegistrant(this);
     }
   
     @Override
     public void registerWith(PluginRegistry registry) {
       GeneratedPluginRegistrant.registerWith(registry);
     }
   }
   ```

1. Em `Application.java`, tenha certeza de mudar `package io.flutter.plugins.firebasemessagingexample;` para seu pacote identificador. Seu pacote identificador deve ser algo como `com.domain.myapplication`.

   ```java
   package com.domain.myapplication;
   ```

1. Coloque o nome da da propriedade da aplicação em `AndroidManifest.xml`. Geralmente ele se encontra em `<nome-aplicacao>/android/app/src/main/`.

   ```xml
   <application android:name=".Application" ...>
   ```

1. Defina uma função **TOP-LEVEL** ou **STATIC** para lidar com mensagens em plano de fundo.

   ```dart
   Future<dynamic> myBackgroundMessageHandler(Map<String, dynamic> message) async {
     if (message.containsKey('data')) {
       // Handle data message
       final dynamic data = message['data'];
     }
   
     if (message.containsKey('notification')) {
       // Handle notification message
       final dynamic notification = message['notification'];
     }
   
     // Or do other work.
   }
   ```

   Observe: Por protocolo `data` e `notification` são campos definidos pelo [RemoteMessage](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/RemoteMessage). 

1. Coloque `onBackgroundMessage` para ser manipulado quando chamar `configure`.

   ```dart
   _firebaseMessaging.configure(
         onMessage: (Map<String, dynamic> message) async {
           print("onMessage: $message");
           _showItemDialog(message);
         },
         onBackgroundMessage: myBackgroundMessageHandler,
         onLaunch: (Map<String, dynamic> message) async {
           print("onLaunch: $message");
           _navigateToItemDetail(message);
         },
         onResume: (Map<String, dynamic> message) async {
           print("onResume: $message");
           _navigateToItemDetail(message);
         },
       );
   ```

   Observe: `configure` deve ser chamado antecipadamente no ciclo de vida de sua aplicação, que então, possa ser lido para receber mensagens o mais cedo possível. Veja o [exemplo de aplicação](https://github.com/FirebaseExtended/flutterfire/tree/master/packages/firebase_messaging/example) para uma demostração.
   
   
