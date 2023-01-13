# Build using EAS

## Change app.json

```js
// app.json
    "android": {
      "package": "in.subrat.sample",
      "versionCode": 1,
      "adaptiveIcon": {
        "foregroundImage": "./assets/adaptive-icon.png",
        "backgroundColor": "#FFFFFF"
      }
    },
```

## Install latest eas cli

```js
npm install -g eas-cli
```

To check weather it is properly installed or not:

```js
npm list -g
```

## Login to your expo account

```js
eas login
```

## Configure the project

```js
eas build:configure
```

It will ask you certain questions.

```js
√ Would you like to automatically create an EAS project for @subrat1977/awesomeproject? ... yes
√ Would you like us to run 'git init' in D:\SubratSir\masai\AwesomeProject for you? ... yes
√ Which platforms would you like to configure for EAS Build? » Android
√ Generated eas.json
```

## Configure profile to build apk

```js
// eas.json
{
  "build": {
    "preview": {
      "android": {
        "buildType": "apk"
      }
    },
    "preview2": {
      "android": {
        "gradleCommand": ":app:assembleRelease"
      }
    },
    "preview3": {
      "developmentClient": true
    },
    "production": {}
  }
}
```

## credential source

If you do not set any option, "credentialsSource" will default to "remote".

```js
// eas.json
{
  "build": {
    "amazon-production": {
      "android": {
        "credentialsSource": "local"
      }
    },
    "google-production": {
      "android": {
        "credentialsSource": "remote"
      }
    }
  }
}
```

## Android app signing credentials

- If you have not yet generated a keystore for your app, you can let EAS CLI take care of that for you by selecting Generate new keystore, and then you are done. The keystore is stored securely on EAS servers.
- If you have previously built your app with expo build:android, you can use the same credentials here.
- If you want to manually generate your keystore, please see the below manual 

## Android credentials guide

```js
keytool -genkey -v -storetype JKS -keyalg RSA -keysize 2048 -validity 10000 -storepass KEYSTORE_PASSWORD -keypass KEY_PASSWORD -alias KEY_ALIAS -keystore release.keystore -dname "CN=in.subrat.sample,OU=,O=,L=,S=,C=US"
```

## Migrate "release.keystore" to PKCS12

```JS
keytool -importkeystore -srckeystore release.keystore -destkeystore release.keystore -deststoretype pkcs12
```

## create credentials.json

```js
{
  "android": {
    "keystore": {
      "keystorePath": "./release.keystore",
      "keystorePassword": "KEYSTORE_PASSWORD",
      "keyAlias": "KEY_ALIAS",
      "keyPassword": "KEY_PASSWORD"
    }
  }
}
```

## Build android platform binary

```js
eas build --platform android // aab file
eas build -p android --profile preview // apk file
```

## Build for both android and ios platform

```js
eas build --platform all
```

[Detailed Official documentation of expo](https://docs.expo.dev/distribution/app-stores/)

[Detailsed official documentation of React Native](https://reactnative.dev/docs/signed-apk-android)




