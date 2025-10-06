# 5.1 - FIDO2Client Plugin: Passkeys+ API

The Passkeys+ Plugin provides a seamless way to integrate enhanced passkey support to your existing FIDO2Client implementation.

<a name="5-1-passkeys-plus"></a>

## 5.1.1 - FIDO2Client Passkeys+ API Documentation

#### FIDO2Client Passkeys+ API Client SDK Methods
-   [`webauthnCreate(userIdentifier[, usePasskeys[, pkpOnly]])`](#webauthnCreate)
-   [`webauthnGet(userIdentifier[, usePasskeys[, pkpOnly]])`](#webauthnGet)

---

<a name="5-1-1-umfa-client-passkeys-api-client-sdk-methods"></a>

## 5.1.1 - FIDO2Client Passkeys+ API Client SDK Methods
<a name="webauthnCreate"></a>

---

### `FIDO2Client.webauthnCreate(userIdentifier[, usePasskeys[, pkpOnly]])` <br>(_enrolls a user and creates a ZSM credential on the device_)

The Passkeys+-extended `webauthnCreate` method of the `FIDO2Client` class integrates passkey support into the enrollment process, allowing users to register their devices for passwordless authentication while still maintaining the added security benefits provided by the ZSM credential framework.

#### Parameters

| Parameter Name   | Data Type | Description                                                                               |
|------------------|-----------|-------------------------------------------------------------------------------------------|
| `userIdentifier` | `String`  | The unique identifier for the user. This is typically the user's email address or a UUID. |
| `usePasskeys`    | `Boolean` | Indicates whether to use passkeys for enrollment. If true, the enrollment process will leverage passkey technology. |
| `pkpOnly`        | `Boolean` | Indicates whether to enroll ONLY a passkey; used when a ZSM credential is already present, and the user wishes to add a passkey. |

#### Returns

| Parameter Name | Data Type | Description                                                                                                         |
|----------------|-----------|---------------------------------------------------------------------------------------------------------------------|
| `response`     | `Promise` | Returns a promise containing the results of the webauthnRetrieve query. This can include any _one_ of the following: |
|                |           | `credential (Object) `: The credential object containing the user's authentication information*.                     |
|                |           | `false (Boolean)`     : Indicating user is already enrolled and has a valid local credential set                    |
|                |           | `error ( Error )`     : An Error occurred while attempting to enroll the user                                       |

_(to be stored for use in verification later. Note this is a JSON object, and may need to be `JSON.stringify`'d depending on your storage mechanism.)_

### Usage

**JavaScript**
```javascript
const userAuthenticationJWT = await FIDO2Client.webauthnCreate(userIdentifier, true);
if (userAuthenticationJWT instanceof Error) throw(userAuthenticationJWT);                            // Error Condition
if(userAuthenticationJWT !== false) {
    console.log(`Enrollment of ${userIdentifier} successful! JWT Token: ${userAuthenticationJWT}`);  // Successful Enrollment
} else {
    console.error('Enrollment failed.');                                                             // Enrollment Failed
}
```

**TypeScript**
```typescript
const userAuthenticationJWT:string|boolean|Error = await FIDO2Client.webauthnCreate(userIdentifier, true);
if (userAuthenticationJWT instanceof Error) throw(userAuthenticationJWT);                            // Error Condition
if (typeof userAuthenticationJWT === 'string') {
    console.log(`Enrollment of ${userIdentifier} successful! JWT Token: ${userAuthenticationJWT}`);  // Successful Enrollment
} else {
    console.error('Enrollment failed.');                                                             // Enrollment Failed
}
``` 





<a name="webauthnGet"></a>

---

### `FIDO2Client.webauthnGet(userIdentifier[, usePasskeys])`<br>(_validates the user's ZSM credential on the device_)

The Passkeys+-extended `webauthnGet` method of the `FIDO2Client` class facilitates a user's passkey authentication while simultaneously validating the user's ZSM credential on the device.

#### Parameters

| Parameter Name   | Data Type | Description                                                                               |
|------------------|-----------|-------------------------------------------------------------------------------------------|
| `userIdentifier` | `String`  | The unique identifier for the user. This is typically the user's email address or a UUID. |
| `usePasskeys`    | `Boolean` | Indicates whether to use passkeys for authentication. If true, the authentication process will leverage passkey technology. |

#### Returns

| Parameter Name | Data Type | Description                                                                                                           |
|----------------|-----------|-----------------------------------------------------------------------------------------------------------------------|
| `response`     | `Promise` | Returns a promise containing the results of the webauthnRetrieve query. This can include any _one_ of the following:   |
|                |           | `credential (Object) `: The credential object containing the user's authentication information*.                      |
|                |           | `error ( Error )`: "${userIdentifier} is not enrolled." (if the user is not enrolled)                                 |
|                |           | `error ( Error )`: An Error occurred while attempting to enroll the user                                              |

_(to be stored for use in verification later. Note this is a JSON object, and may need to be `JSON.stringify`'d depending on your storage mechanism.)_


### Usage

**JavaScript**
```javascript
const userAuthenticationJWT = await FIDO2Client.webauthnGet(userIdentifier, true);
if (userAuthenticationJWT instanceof Error) throw(userAuthenticationJWT);                            // Error Condition
console.log(`${userIdentifier} successfully authorized! JWT Token: ${userAuthenticationJWT}`);       // Successful Authorization
```

**TypeScript**
```typescript
const userAuthenticationJWT:string|Error = await FIDO2Client.webauthnGet(userIdentifier, true);
if (userAuthenticationJWT instanceof Error) throw(userAuthenticationJWT);                            // Error Condition
console.log(`${userIdentifier} successfully authorized! JWT Token: ${userAuthenticationJWT}`);       // Successful Authorization    
```
