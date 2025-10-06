# FIDO2Client for Browser Applications Quick Start

The following steps will help you get started with the FIDO2Client SDK. Additional documentation can be found in the [FIDO2Client API Documentation, here](api.md).

<a name="core-sdk-installation"></a>

## Core SDK Installation

```shell
  # core 
  npm i @ideem/zsm-client-sdk 
``` 

<a name="optional-plugin-installation"></a>

## (Optional) Plugin Installation

```shell
  # optional: Passkeys+ plugin
  npm i @ideem/plugins.passkeys-plus
```

>>> ALL ZSM Plugins have a **`peerDependency`** of the Core `@ideem/zsm-client-sdk` Package defined within their `package.json`. <br>**They CANNOT function without the core package installed as well**!

<a name="importing-the-packages"></a>

## Importing the Package(s)
You can import the package(s) in a variety of ways, depending on your project setup. Below are some common examples.

<a name="with-a-bundler"></a>

-   ### Using the FIDO2Client with a Bundler (Webpack, Rollup, Vite, Parcel, etc.)
    If you are using a modern JavaScript framework (React, Vue, Angular, etc.), or your application is a SPA ("single-page app"), you will almost assuredly be using a module bundler. In this case, you can import the SDK and any plugins directly in your JavaScript files.

    #### _**Core Only** - Importing ONLY the FIDO2Client Package_
    ```js
    import { FIDO2Client } from  '@ideem/zsm-client-sdk';        // Import the Core Package
    ```

    #### _**Core With Plugins** - Importing the FIDO2Client with Plugin(s):_
    ```js
    import { FIDO2Client } from  '@ideem/zsm-client-sdk';        // Import the Core Package
    import PasskeysPlus    from '@ideem/plugins.passkeys-plus';  // Import the Passkeys+ Plugin
    // ... plus any additional plugins desired
    ```

<a name="without-a-bundler"></a>

-   ### Using the FIDO2Client WITHOUT a Bundler (VanillaJS implementation)
    Because the JavaScript implementation in most browsers does not yet fully support ES Modules, you'll need to explain to the browser where to find your modules. 
    
    This is traditionally done via the use of an **`importmap`** `<script>` tag. This can be done in one of two ways:

    #### _**Option A (RECOMMENDED)** — Using the Included Loader Shim:_
    The VanillaJSLoader shim is included in the ZSM Core Package, and will automatically detect the installed plugins and register the import map for you.

    ```html
    <!-- Place this BEFORE any <script type="module"> that imports your SDK/plugins -->
    
    <script type="module" src="./node_modules/@ideem/zsm-client-sdk/shims/VanillaJSLoader.js"></script>

    <script type="module">
        import { FIDO2Client } from '@ideem/zsm-client-sdk';
        import PasskeysPlus from '@ideem/plugins.passkeys-plus'; 
    </script>
    ``` 

    #### _**Option B** — Declaring an Import Map Manually:_
    Alternatively, you can declare an import map manually in your HTML if you would prefer less overhead/more granular control.

    ```html
    <script type="importmap">
        {
            "imports": {
                "@ideem/zsm-client-sdk"         : "/node_modules/@ideem/zsm-client-sdk/ZSMClientSDK.js",
                "@ideem/zsm-client-sdk/"        : "/node_modules/@ideem/zsm-client-sdk/",
                "@ideem/plugins.passkeys-plus"  : "/node_modules/@ideem/plugins.passkeys-plus/PasskeysPlus.js",
                "@ideem/plugins.passkeys-plus/" : "/node_modules/@ideem/plugins.passkeys-plus/"
                // ... with additional declarations for each plugin being used
            }
        }
    </script>
    <script type="module"> 
        import  '@ideem/plugins.passkeys-plus'; 
        import { FIDO2Client } from  '@ideem/zsm-client-sdk'; 
    </script>
    ```

>>>> Because the ZSM Modules are designed to communicate with one another without requiring explicit registration, an import map is **REQUIRED** for VanillaJS usage.

## Set up the ZSM Configuration File 
Within your application, create a `.json` file or JSON object to store the ZSM application configuration. It should contain the following properties:
```
{ 
    "application_id"           : "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
    "host_url"                 : "<URL provided by Ideem>",
    "application_environment"  : "TEST"
}
```
These values are provided by Ideem and are unique to your application, although testing versions are pre-populated in the Sample Apps specific to the language you are using.

### Configuration Properties

| Property Name             | Data Typ       | Description                                                                 | Default Value                                             |
|---------------------------|----------------|-----------------------------------------------------------------------------|-----------------------------------------------------------|
| `application_id`          | string         | The application ID to be used during the FIDO2 process                       | provided by Ideem; test value available in sample config  |
| `host_url`                | Boolean        | The URL to your region-specific ZSM server                                  | provided by Ideem; demo server specified in sample config |
| `application_environment` | string         | The application environment to be used during the FIDO2 process              | `TEST`                                                    |

---

<a name="basic-usage"></a>

## Basic Usage
Once your modules are loaded, you can simply initialize them for use:

```js
    const zsmClient = new FIDO2Client({ /* config */ });

    // ...additional application logic here...

    zsmClient.webauthnRetrieve(username);
```

<a name="asynchronous-usage"></a>

## Asynchronous Usage
In some cases - especially those in which the SDK is lazy-loaded or JIT ("just-in-time") loaded - you may need to wait for the SDK to be ready before using it (it takes, on average, about 200ms to initialize). In those cases, it can be helpful to asynchronously wait for the SDK to be ready. 

This can be done in any one of several ways:

<a name="using-the-finished-promise-async-await"></a>

-   ### Using the `finished()` Promise (`async`/`await`)
    After initialization is completed, a `finished()` promise is available on the client instance. If you are using the conventional `async`/`await` syntax, you can simply `await` this promise:

    ```js
        const zsmClient = new FIDO2Client({ /* config */ }); 
        await zsmClient.finished();
        // ...additional logic here...
    ```

    ...the `finished()` method is also a "thennable", if you prefer the more traditional `Promise`-driven syntax (or wish to leverage its group handler methods, like `Promise.all()`/`Promise.race()`/`Promise.any()`/etc.):

    ```js
        const zsmClient = new FIDO2Client({ /* config */ }); 
        zsmClient.finished().then(() => {
            // ...additional logic here...
        });
    ```

    ...in either usage, the `.catch()` and `.finally()` handlers can be used as usual:

    ```js
        const zsmClient = new FIDO2Client({ /* config */ }); 
        zsmClient.finished().then(() => {
            // ...additional logic here...
        }).catch((error) => {
            // Handle errors here
        }).finally(() => {
            // Cleanup or final actions here
        });
    ```

<a name="passing-a-callback-function-to-finished"></a>

-   ### Passing a callback function to `finished()` (`finished(callbackFn [, ...args])`)
    If you prefer to use a callback-based approach, you can pass a callback function INTO the `finished` method, which is called when the SDK is fully initialized (if provided).

    ```js
        const zsmClient = new FIDO2Client({ /* config */ }); 
        zsmClient.finished(() => {
            // ...additional logic here...
        });
    ```

    ...or...


    ```js
        const zsmClient = new FIDO2Client({ /* config */ }); 
        zsmClient.finished(callbackFnName, arg1, arg2, ...);
    ```

<a name="using-an-event-listener-zsmclientready"></a>

-   ### Using an event listener (`ZSMClientReady`)
    Finally, if you prefer to use an event-driven approach, you can listen for the `ZSMClientReady` event, which is dispatched when the SDK is fully initialized.

    ```js
        const zsmClient = new FIDO2Client({ /* config */ }); 
        zsmClient.addEventListener('ZSMClientReady', () => {
            // ...additional logic here...
        });
    ```

<a name="putting-it-all-together"></a>

## Putting It All Together
Let's take a look at what the complete initialization process looks like:

### Complete Example when using a bundler (e.g. Webpack, Rollup)

**JS File (`app.js`/`index.js`/etc,)**
```js
    // Import the necessary modules
    import { FIDO2Client } from  '@ideem/zsm-client-sdk';        // Import the Core Package
    import PasskeysPlus from '@ideem/plugins.passkeys-plus';    // (Optionally) Import any additional plugins

    // Create a new instance of the client
    const zsmClient = new FIDO2Client({ /* config */ }); 

    // Perform additional application logic (if loading the client on page/view load)
    // [...]
    // - or -
    // Optionally, wait for the client to be ready (if lazy-loading or using a just-in-time or immediate invocation)
    await zsmClient.finished();

    // Now you can use the client
    zsmClient.webauthnRetrieve(username);
```

### Complete Example when NOT using a bundler (VanillaJS implementation)

**HTML File (`index.html`/`app.html`/etc.)**
```html
    <!DOCTYPE html>
    <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>FIDO2Client Example</title>

            <!-- Include the VanillaJSLoader (or manually define your importmap) -->
            <script type="module" src="./node_modules/@ideem/zsm-client-sdk/shims/VanillaJSLoader.js"></script>

            <script type="module">
                // Import the necessary modules
                import { FIDO2Client } from  '@ideem/zsm-client-sdk';        // Import the Core Package
                import PasskeysPlus from '@ideem/plugins.passkeys-plus';    // (Optionally) Import any additional plugins

                // Create a new instance of the client
                const zsmClient = new FIDO2Client({ /* config */ }); 

                // Perform additional application logic (if loading the client on page/view load)
                // [...]
                // - or -
                // Optionally, wait for the client to be ready (if lazy-loading or using a just-in-time or immediate invocation)
                await zsmClient.finished();

                // Now you can use the client
                zsmClient.webauthnRetrieve(username);
            </script>
        </head>
        <body>
        </body>
    </html>
```

>>> If you are using a legacy framework that does NOT support modern JavaScript features (like ES modules), a polyfill is included in the Core Package for your convenience. Simply add `<script src="./node_modules/@ideem/zsm-client-sdk/shims/es-module-shims.js"></script>` before any other `<script>` declarations to your HTML file. Note that the `VanillaJSLoader.js` shim will automatically load this polyfill for you, if the environment requires it.

# Using the FIDO2Client
Please refer to the [API Reference](api.md) for detailed information on available methods and their usage.
