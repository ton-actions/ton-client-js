# JavaScript Free TON SDK

In this repository you can find the source code of Javascript SDK for Web, Node.js and React Native platforms.

**Have a question? Get quick help in our channel:**

[![Chat on Telegram](https://img.shields.io/badge/chat-on%20telegram-9cf.svg)](https://t.me/ton_sdk) 

To get a deeper understanding dive into our [SDK guides](https://docs.ton.dev/86757ecb2/p/783f9d-about-sdk) where you can find extensive explanations and descriptions of each step of DApp development on Free TON.

This SDK is distributed via npm packages:
- [@tonclient/core](https://www.npmjs.com/package/@tonclient/core) – common binding independent from JavaScript platform you use.
- [@tonclient/lib-node](https://www.npmjs.com/package/@tonclient/lib-node) – bridge to NodeJs including NodeJs binary addon.
- [@tonclient/lib-web](https://www.npmjs.com/package/@tonclient/lib-web) – bridge to browser including WASM module.
- [@tonclient/lib-react-native](https://www.npmjs.com/package/@tonclient/lib-react-native) – bridge to mobile react-native platform including static libraries for iOS and Android.

You can find their source code in this repository.
 
# Installation

## Install core package

```shell script
npm i --save @tonclient/core
```

## Install bridge package (depends on target JS platform)

The bridge package will download precompiled binaries from TON Labs cloud storage.
If you want to rebuild binary from sources see [build binaries](#build binaries) section. 

NodeJs:
```shell script
npm i --save @tonclient/lib-node
```

Web:
```shell script
npm i --save @tonclient/lib-web
```

React Native:
```shell script
npm i --save @tonclient/lib-react-native
```


# Setup library

You must initialize the library before the first use. The best place to do it is an 
initialization code of your application.

You need to attach the chosen binary module to the `TonClient` class.

NodeJs:
```ts
const {TonClient} = require("@tonclient/core");
const {libNode} = require("@tonclient/lib-node");

// Application initialization

TonClient.useBinaryLibrary(libNode)
```
  
Web:
```ts
import {TonClient} from "@tonclient/core";
import {libWeb} from "@tonclient/lib-web";

// Application initialization

TonClient.useBinaryLibrary(libWeb);
```

By default the library loads wasm module from relative URL `/tonclient.wasm`.

You can specify alternative URL if you want to place (or rename) wasm module.
```ts
import {TonClient} from "@tonclient/core";
import {libWeb, libWebSetup} from "@tonclient/lib-web";

// Application initialization
libWebSetup({
    binaryURL: "/assets/tonclient_1_2_3.wasm",
});

TonClient.useBinaryLibrary(libWeb);
```

React Native:
```ts
import {TonClient} from "@tonclient/core";
import {libReactNative} from "@tonclient/lib-react-native";

// Application initialization

TonClient.useBinaryLibrary(libReactNative);
```
  
# Use library

All library functions are incorporated into `TonClient` class. Each client module is represented as a 
property of the `TonClient` object.

To start use library you must create an instance of the `TonClient` class:
```ts
const client = new TonClient();
const keys = await client.crypto.generate_random_sign_keys();
```

You can pass a configuration object in `TonClient` constructor:
```ts
const client = new TonClient({
    network: { 
        server_address: 'net.ton.dev' 
    } 
});
```

You can find reference guide to `TonClient` here: [TON-SDK API Documentation](https://github.com/tonlabs/TON-SDK/blob/master/docs/modules.md)

# Build bridge binaries

You can build binaries from sources.

If you install a bridge package from the `npmjs` you can build it with the following commands (e.g. for nodejs):
```shell script
cd node_modules/@tonclient/lib-node/build
cargo run
```

# Build binaries

If you checkout this repository you can build binaries for all bridges.

```shell script
cd packages/lib-node/build
cargo run
cd ../../../lib-web/build
cargo run
cd ../../../lib-react-native/android/build
cargo run
cd ../../ios/build
cargo run
```

Also the archives will be created to be published on the TON Labs cloud storage. Archives will be placed into the following folders:
- `packages/lib-node/publish`
- `packages/lib-web/publish`
- `packages/lib-react-native/ios/publish`
- `packages/lib-react-native/android/publish`

# Run tests

This suite has test packages:
- `tests` – common test package containing all unit tests for a library.
- `tests-node` – tests runner on node js.
- `tests-web` – tests runner on web browser.
- `tests-react-native` – tests runner on react native platform.

## Preparation to run tests

```shell script
cd packages/core
npm i
npx tsc
cd packages/tests
npm i
npx tsc
```

## Run tests on node js

```shell script
cd packages/tests-node
npm i
node run
```

## Run tests on web browser

```shell script
cd packages/tests-web
npm i
node run
```

## Run tests on react native platform

```shell script
cd packages/tests-react-native
npm i
node run ios
node run android
```

## To control where your tests will run use this environments
```shell script
TON_USE_SE=true TON_NETWORK_ADDRESS=http://localhost node run
```

# Download precompiled binaries

Instead of building library yourself, you can download the __latest__ precompiled binaries from 
TON Labs SDK Binaries Store.

Binary              | Target           | Major | Download links
------------------- | ---------------- | ----- | --------------
Node.js AddOn       | Win32            | 1     | [`tonclient.node`](https://binaries.tonlabs.io/tonclient_1_nodejs_addon_win32.gz)
&nbsp;              | macOS            | 1     | [`tonclient.node`](https://binaries.tonlabs.io/tonclient_1_nodejs_addon_darwin.gz)
&nbsp;              | Linux            | 1     | [`tonclient.node`](https://binaries.tonlabs.io/tonclient_1_nodejs_addon_linux.gz)
React Native Module | Android x86_64   | 1     | [`libtonclient.so`](https://binaries.tonlabs.io/tonclient_1_react_native_x86_64-linux-android.gz)
&nbsp;              | Android i686     | 1     | [`libtonclient.so`](https://binaries.tonlabs.io/tonclient_1_react_native_i686-linux-android.gz)
&nbsp;              | Android armv7    | 1     | [`libtonclient.so`](https://binaries.tonlabs.io/tonclient_1_react_native_armv7-linux-android.gz)
&nbsp;              | Android aarch64  | 1     | [`libtonclient.so`](https://binaries.tonlabs.io/tonclient_1_react_native_aarch64-linux-android.gz)
&nbsp;              | iOS              | 1     | [`libtonclient.a`](https://binaries.tonlabs.io/tonclient_1_react_native_ios.gz)
WASM Module         | Browser          | 1     | [`tonclient.wasm`](https://binaries.tonlabs.io/tonclient_1_wasm.gz), [`index.js`](https://binaries.tonlabs.io/tonclient_1_wasm_js.gz)

_Downloaded archive is gzipped file_

---
Copyright 2018-2020 TON DEV SOLUTIONS LTD.
