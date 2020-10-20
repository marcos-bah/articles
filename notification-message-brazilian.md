# Firebase Cloud Messaging para Flutter

[![pub package](https://img.shields.io/pub/v/firebase_messaging.svg)](https://pub.dev/packages/firebase_messaging)

O plugin Flutter usa o [Firebase Cloud Messaging (FCM) API](https://firebase.google.com/docs/cloud-messaging/).

Com esse plugin, sua aplicação em Flutter pode receber e processar notificações push bem como dados de mensagens no Android e IOS. Leia Firebase's [About FCM Messages](https://firebase.google.com/docs/cloud-messaging/concept-options) para aprender mais sobre as diferenças entre mensagens de notificações e mensagens de dados.

Para plugins Flutter para outros produtos Firebase, veja [README.md](https://github.com/FirebaseExtended/flutterfire/blob/master/README.md).

# Usando
Para usar esse plugin, adicione `firebase_messaging` como uma [dependência em seu pubspec.yaml file](https://flutter.io/platform-plugins/).

# Começando

Cheque o diretorio de `exemplo` para uma amostra de aplicação usando o Firebase Cloud Messaging.

# Integração com o Android

Para integrar esse plugin a sua aplicação Android, siga os seguintes passos: 

1. Usando o [Console do Firebase](https://console.firebase.google.com/) adicione um Android app para seu projeto: Siga o assistente, baixe o arquivo gerado `google-services.json` e coloque dentro de `android/app`.

2. Adicione o *classpath* no arquivo `[project]/android/build.gradle`
```
dependencies {
  // Exemplo de um classpath já existente no arquivo
  classpath 'com.android.tools.build:gradle:3.5.3'
  // Adicione o classpath do serviço do google 
  classpath 'com.google.gms:google-services:4.3.2'
}
```

3. Adicione o *apply plugin* no arquivo `[project]/android/app/build.gradle`
```
// Adicione isso na parte inferior do arquivo
apply plugin: 'com.google.gms.google-services'
```

Observe: Se essa parte não for completada você receberá um erro como esse:
```
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


