# React Native using Expo and Typescript

This repository is both a

* [React Native](https://facebook.github.io/react-native/) demo app using [Expo](https://expo.io) and written in [TypeScript](http://www.typescriptlang.org); and it also
* contains type definition file for the [APIs provided by Expo](https://docs.expo.io/versions/latest/sdk/index.html).

There is now [`@types/expo`](https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types/expo) package available in the Definitetly Typed project, and most of the work done in this project has been merged into that package.

The app is published on Expo: [Expo and TypeScript on Expo](https://expo.io/@janaagaard75/expo-and-typescript). It is not published to the app stores, but you can run it on a device, if install the free [Expo client](https://expo.io/tools#client).

![Screen shot](https://github.com/janaagaard75/expo-and-typescript/raw/master/screen-shot.png)

The code is orignally based on [expo-ts-example](https://github.com/dalcib/expo-ts-example) and the type definitions are based on [pierre-H's type definition file](https://gist.github.com/pierre-H/eef9a9225fb1c5a0f81180a8b0fbb2c2).

## Running the App

You can either run the app using [Expo XDE](https://expo.io/tools#xde) or using the command line. Personally I find that using the XDE is the easiest solution, since it handles a lot of the setup for you, but if you're prefer using the command line here are the commands to do so.

Start the local server. This will give you a QR code that you can scan using the Expo Client app on your mobile divice.

```shell
yarn start
```

If you're on a Mac and have Xcode installed, you can run the app using the iOS Simulator. The first time you have to install the Expo Client on the simulator. Use the same command to update the client.

```shell
yarn exp install:ios
```

Once the client is installed you can launch the simulator and open the project with this command. The server has to be kept running, so open up a second terminal.

```shell
yarn ios
```

## Type Definitions for Expo

The ultimate goal is to get the definitions merged into to [expo/expo-sdk](https://github.com/expo/expo-sdk), or if that's is not possible, then as a secondary option to create the `@types/expo` package through the [DefinitelyTyped project](https://github.com/DefinitelyTyped/DefinitelyTyped). I do however believe that the quality of the type defintions have to be better before any of these paths are persued.

These are the APIs provided by Expo, with a checkmark for the ones, that have been tested in this repo. 23 out of 51 done.

* [x] Accelerometer
* [ ] AdMob
* [x] Amplitude
* [x] AppLoading (No test screen)
* [ ] Art (Deprecated in favor of Svg)
* [x] Asset
* [x] Audio
* [ ] AuthSession
* [ ] AV
* [ ] BarCodeScanner
* [x] BlurView
* [ ] Branch
* [x] Brightness
* [x] Camera
* [x] Constants
* [ ] Contacts
* [ ] DocumentPicker
* [ ] ErrorRecovery
* [x] Facebook
* [ ] FacebookAds
* [ ] FileSystem
* [x] Fingerprint
* [x] Font
* [ ] GestureHandler
* [ ] GLView
* [ ] Google
* [x] Gyroscope
* [ ] ImagePicker
* [ ] IntentLauncherAndroid
* [ ] KeepAwake
* [x] LinearGradient
* [ ] Location
* [ ] Lottie
* [ ] Magnetometer
* [x] MapView
* [ ] Notifications
* [ ] Payments
* [ ] Pedometer
* [x] Permissions
* [x] registerRootComponent
* [x] ScreenOrientation
* [ ] SecureStore
* [x] Segment (No test screen)
* [ ] Speech
* [ ] SQLite
* [x] Svg
* [ ] takeSnapshotAsync
* [x] Util
* [x] Vector Icons
* [x] Video (No test screen)
* [ ] WebBrowse

## Setting up Expo and React Native with TypeScript

Here is how you set up an Expo app to be able to code in TypeScript instead of JavaScript. Debugging of TypeScript files works, but I am not sure is hot reloading (aka module replacement) works as well as it does with a non-transpiled solution. The Expo client has some caching, so subsequent reloads are faster than the initial load.

### TypeScript

Add TypeScript and the helpers library, `tslib`, to the project. It's optional to use the --exact switch. I just prefer micro managing the version of the packages that I'm using. You can, of course, also use `npm` instead of `yarn`.

```shell
yarn add --dev --exact TypeScript react-native-typescript-transformer
yarn add --exact tslib
```

Configure TypeScript by putting a `tsconfig.json` file in the root of your project. You probably don't need all of these settings. **TODO: Boil the configuration down to the required settings.**

```json
{
  "compilerOptions": {
    "allowSyntheticDefaultImports": true,
    "experimentalDecorators": true,
    "forceConsistentCasingInFileNames": true,
    "importHelpers": true,
    "jsx": "react-native",
    "lib": [
      "dom",
      "es2015",
      "es2016",
      "es2017"
    ],
    "module": "es2015",
    "moduleResolution": "node",
    "noEmitHelpers": true,
    "noImplicitReturns": true,
    "noUnusedLocals": true,
    "outDir": "build/dist",
    "sourceMap": true,
    "strict": true,
    "target": "es2017"
  },
  "exclude": [
    "build",
    "node_modules"
  ],
  "types": [
    "typePatches"
  ]
}
```

### React Native TypeScript Transformer

Add the React Native TypeScript Transformer package.

```shell
yarn add --dev --exact react-native-typescript-transformer
```

Configure Expo to use the transformer for `ts` and `tsx` files by adding the following lines to `app.json` under `expo/packagerOpts`. [The final app.json should look somewhat like this](https://raw.githubusercontent.com/janaagaard75/expo-and-typescript/master/app.json).

```json
"sourceExts": [
  "ts",
  "tsx"
],
"transformer": "node_modules/react-native-typescript-transformer/index.js"
```

I don't know why it is necessary to append `/index.js` to the URL, but it has been so since version 19 of Expo.

[The final `app.json`](https://github.com/janaagaard75/expo-and-typescript/blob/master/app.json)

### App Component in TypeScript

Create a `src` folder, move `App.js` to that folder, and rename the file to `App.tsx`. Since TypeScript has a syntax that is so similar to JavaScript it's not necessary to make any modifications to App.tsx to make it valid TypeScript.

Create a new `App.js` in the root of the project, and insert the following lines. Expo will still be looking for App.js in the root of the project, so we simply tell it to use the new `src/App.tsx.

```javascript
import App from './src/App'

export default App
```

### Add Type Definitions for React

Add type definitions for React and React Native.

```shell
yarn add --dev --exact @types/react @types/react-native
```

### Add Type Definitions for Expo

Besides what React Native already has, the Expo SDK comes with a lot of additional APIs for your app. Unfortunately there aren't any type definitions for these APIs, and that makes it difficult to use them correctly in TypeScript. I have started on creating these type definitions, but bare in mind that they still lack a lot of testing.

Create a file `expo.d.ts` in the `src` folder and copy the content of this [`expo.d.ts`](https://raw.githubusercontent.com/janaagaard75/expo-sdk-with-type-definitions/master/expo.d.ts) to it.

I'm working on both improving these type definitions, and I'm trying to make it easier to add and update them, but they are currently in a very early stage.

## Similar Projects

* [expo-ts-example](https://github.com/dalcib/expo-ts-example)
* [TypeScript-React-Native-Starter](https://github.com/Microsoft/TypeScript-React-Native-Starter)
* [react-native-typescript-starter](https://github.com/cbrevik/react-native-typescript-starter)
