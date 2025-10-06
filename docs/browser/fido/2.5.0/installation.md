# FIDO2Client SDK for Browser Implementations Installation Guide

The ZSM platform is a modular framework, allowing developers that have installed the Core SDK Package to selctively pick and choose optional, a la carte packages to extend the Core FIDO2 functionality, if desired. 

<a name="core-umfa-package"></a>
## **Core FIDO2 Package**: [`@ideem/zsm-client-sdk`](https://www.npmjs.com/package/@ideem/zsm-client-sdk)
The ZSM Core Package is fully stand-alone, and provides access to the FIDO2 Client out of the box. It is required in order to leverage any of the offered plug-ins, however, and must be installed as part of the installation process for all developers seeking to add ZSM utility to their applications.

**Installation of the Core FIDO2 Package (FIDO2Client)**
> ```
> npm install @ideem/zsm-client-sdk
> ```

<a name="plugins"></a>
## **Plugins**

ZSM Plugins are optional packages that extend the functionality of the Core FIDO2 Package. They can be installed individually as needed/desired, and can be mixed and matched to suit specific use cases. To install, simply locate the desired Plugin from the [ZSM Plugins List](plugins.md), and install it alongside the Core FIDO2 Package. For purposes of this example, we will consider the installation of the Passkeys+ plugin:

**Installation of the Core FIDO2 Package (FIDO2Client) With Passkeys+ Plugin**
> ```
> npm install @ideem/zsm-client-sdk @ideem/plugins.passkeys-plus
> ```

>>> ALL ZSM Plugins have a **`peerDependency`** of the Core `@ideem/zsm-client-sdk` Package defined within their `package.json`. <br>**They CANNOT function without the core package installed as well**!
