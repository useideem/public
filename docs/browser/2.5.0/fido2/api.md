# FIDO2Client for Browser Applications API

The FIDO2 module provides a flexible solution for handling multi-factor authentication processes. It allows the configuration to be provided either as a JSON string or via a URL pointing to a configuration file. The authentication steps are executed using dynamically loaded modules.

Note that the following interface definitions presuppose that you have already imported the `FIDO2Client` class from the ZSM Client SDK (`@ideem/zsm-client-sdk`). If you have not yet done so, please refer to the [FIDO2Client Quick-Start Guide](./quick-start.md) documentation for instructions on how to import the ZSM Client SDK.


---

## FIDO2Client API Documentation

### FIDO2Client API Client SDK Methods
-   [`webauthnRetrieve(userIdentifier)`](#webauthn-retrieve)
-   [`webauthnCreate(userIdentifier)`](#webauthnCreate)
-   [`webauthnGet(userIdentifier)`](#webauthnGet)
-   [`webauthnDelete(userIdentifier)`](#webauthnDelete)

---

<a name="webauthn-retrieve"></a>

## FIDO2Client API Client SDK Methods

### `FIDO2Client.webauthnRetrieve(userIdentifier)` <br>(_checks for existing credentials on the device_)

The `webauthnRetrieve` method of the `FIDO2Client` class examines the user's enrollment status for the FIDO2 process on the current device. This allows the application to determine whether the user has already enrolled in the FIDO2 process, allowing the developer to skip the enrollment process, should the user already be enrolled, or perform additional authentication prior to enrollment.

#### Parameters

| Parameter Name   | Data Type | Description                                                                               |
|------------------|-----------|-------------------------------------------------------------------------------------------|
| `userIdentifier` | `String`  | The unique identifier for the user. This is typically the user's email address or a UUID. |

#### Returns

| Parameter Name | Data Type | Description                                                                                                         |
|----------------|-----------|---------------------------------------------------------------------------------------------------------------------|
| `response`     | `Promise` | Returns a promise containing the results of the webauthnRetrieve query. This can include any _one_ of the following: |
|                |           | `credentialID (String)`: The local credential set's credential ID (user HAS enrolled)                               |
|                |           | `false (Boolean)`: Indicating user does not have a local ZSM FIDO2 credential set                                    |
|                |           | `error ( Error )`: An Error occurred while attempting to check the enrollment status                                |

### Usage

**JavaScript**
```javascript
const enrollmentStatus = await FIDO2Client.webauthnRetrieve(userIdentifier);
if(enrollmentStatus instanceof Error) throw(enrollmentStatus);                   // Error Condition
if(enrollmentStatus !== false) {
    console.log(`${user} is enrolled with credential_ID: ${enrollmentStatus}`);  // Credentials Present
} else {
    console.log(`${user} is not enrolled`);                                      // Credentials Absent
}
```
**TypeScript**
```typescript
const enrollmentStatus:string|boolean|Error = await FIDO2Client.webauthnRetrieve(userIdentifier);
if (enrollmentStatus instanceof Error) throw(enrollmentStatus);                  // Error Condition
if (typeof enrollmentStatus === 'string') {
    console.log(`${user} is enrolled with credential_ID: ${enrollmentStatus}`);  // Credentials Present
} else {
    console.log(`${user} is not enrolled`);                                      // Credentials Absent
}
```




<a name="webauthnCreate"></a>

---

### `FIDO2Client.webauthnCreate(userIdentifier)` <br>(_enrolls a user and creates a ZSM credential on the device_)

The `webauthnCreate` method of the `FIDO2Client` class initiates the enrollment process for the FIDO2 module. This process allows the user to enroll in the FIDO2 process, which is required for subsequent authentication operations.

#### Parameters

| Parameter Name   | Data Type | Description                                                                               |
|------------------|-----------|-------------------------------------------------------------------------------------------|
| `userIdentifier` | `String`  | The unique identifier for the user. This is typically the user's email address or a UUID. |

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
const userAuthenticationJWT = await FIDO2Client.webauthnCreate(userIdentifier);
if (userAuthenticationJWT instanceof Error) throw(userAuthenticationJWT);                            // Error Condition
if(userAuthenticationJWT !== false) {
    console.log(`Enrollment of ${userIdentifier} successful! JWT Token: ${userAuthenticationJWT}`);  // Successful Enrollment
} else {
    console.error('Enrollment failed.');                                                             // Enrollment Failed
}
```

**TypeScript**
```typescript
const userAuthenticationJWT:string|boolean|Error = await FIDO2Client.webauthnCreate(userIdentifier);
if (userAuthenticationJWT instanceof Error) throw(userAuthenticationJWT);                            // Error Condition
if (typeof userAuthenticationJWT === 'string') {
    console.log(`Enrollment of ${userIdentifier} successful! JWT Token: ${userAuthenticationJWT}`);  // Successful Enrollment
} else {
    console.error('Enrollment failed.');                                                             // Enrollment Failed
}
``` 





<a name="webauthnGet"></a>

---

### `FIDO2Client.webauthnGet(userIdentifier)`<br>(_validates the user's ZSM credential on the device_)

Performs an authentication operation using the FIDO2 module, using the credential currently stored on the device for the specified user. If the specified user's credential is not present on the device, authentication will fail.


#### Parameters

| Parameter Name   | Data Type | Description                                                                               |
|------------------|-----------|-------------------------------------------------------------------------------------------|
| `userIdentifier` | `String`  | The unique identifier for the user. This is typically the user's email address or a UUID. |

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
const userAuthenticationJWT = await FIDO2Client.webauthnGet(userIdentifier);
if (userAuthenticationJWT instanceof Error) throw(userAuthenticationJWT);                            // Error Condition
console.log(`${userIdentifier} successfully authorized! JWT Token: ${userAuthenticationJWT}`);       // Successful Authorization
```

**TypeScript**
```typescript
const userAuthenticationJWT:string|Error = await FIDO2Client.webauthnGet(userIdentifier);
if (userAuthenticationJWT instanceof Error) throw(userAuthenticationJWT);                            // Error Condition
console.log(`${userIdentifier} successfully authorized! JWT Token: ${userAuthenticationJWT}`);       // Successful Authorization
```





<a name="webauthnDelete"></a>

---

### `FIDO2Client.webauthnDelete(userIdentifier)` <br>(_removes the user's ZSM credential from the device_)

The `webauthnDelete` method of the `FIDO2Client` class removes the specified, enrolled user's credentials from the device the user is actively logged in with. This effectively deletes any credentials associated with the user, preventing future authentication using those credentials, until the user re-enrolls on said device.


#### Parameters

| Parameter Name   | Data Type | Description                                                                               |
|------------------|-----------|-------------------------------------------------------------------------------------------|
| `userIdentifier` | `String`  | The unique identifier for the user. This is typically the user's email address or a UUID. |

#### Returns

| Parameter Name | Data Type | Description                                                                                                |
|----------------|-----------|------------------------------------------------------------------------------------------------------------|
| `response`     | `Promise` | Returns a promise containing the results of the unenrollment. This can include any _one_ of the following: |
|                |           | `true (boolean)` : The user's credentials were successfully removed from the device                        |
|                |           | `false (boolean)`: The user was not enrolled and/or had no valid local credential set                      |
|                |           | `error ( Error )`: Unable to unenroll <userIdentifier>'s identity from this device.                        |

### Usage

**JavaScript**
```javascript
const unenrollResult = await FIDO2Client.webauthnDelete(userIdentifier);
if (unenrollResult instanceof Error) throw(unenrollResult);                // Error Condition
console.log(`${userIdentifier} successfully unenrolled!`);                 // Successful Unenrollment
```

**TypeScript**
```typescript
const unenrollResult:string|Error = await FIDO2Client.webauthnDelete(userIdentifier);
if (unenrollResult instanceof Error) throw(unenrollResult);                // Error Condition
console.log(`${userIdentifier} successfully unenrolled!`);                 // Successful Unenrollment
```

<a name="examples"></a>

---
## Example Code

This example demonstrates the full process of using Ideem’s FIDO2 WebAuthn API for registration, authentication, and how to customize the **Relying Party**. The `WebAuthnClient` can either use the default Relying Party provided by the API or allow you to pass in a custom Relying Party if you want more control over how credential IDs, tokens, or user data are handled.

<a name="default-flow"></a>

---
### Example of Default Flow: Registration and Authentication

In this example, we use the default Relying Party provided by the API:

```javascript
const client = new WebAuthnClient();

// Sign in with username and password
FIDO2Client.signIn('jamiedoe@example.com', 'password123')
.then(() => {
    console.log("Signed in successfully");

    // Start the WebAuthn registration process
    return FIDO2Client.webauthnCreate('jamiedoe@example.com');
})
.then((response) => {
    console.log("WebAuthn registration complete", response);

    // Now authenticate the user using the WebAuthn credential
    return FIDO2Client.webauthnGet('jamiedoe@example.com');
})
.then((data) => {
    console.log("WebAuthn authentication successful", data);
})
.catch((error) => {
    console.error("Error during registration/authentication", error);
});
```

In this flow:
- The user signs in using their email and password.
- If they don’t have a WebAuthn credential, they go through the `webauthnCreate` process to register.
- Once registered, they can authenticate using `webauthnGet` in future logins.

<a name="custom-rp-example"></a>

---
### Custom Relying Party Example

If you need more control over how credentials are stored, retrieved, or verified (for example, if you manage your own storage for `credential_id` and public keys), you can create a custom Relying Party class and pass it into the `WebAuthnClient`. This gives you full control over how the server interactions are managed.

Here’s an example of a custom Relying Party:

```javascript
class CustomRelyingParty {
    constructor(host) {
        this.host = host;
        this.state = new RelyingPartyState();
    }

    login(username, password) {
        const loginPayload = {
            email: username,
            password: password,
        };

        return fetch(`${this.host}/custom-api/auth/login`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(loginPayload),
        })
        .then(response => response.json())
        .then(data => {
            this.state = new RelyingPartyState(data);
            return data;
        });
    }

    startRegistration() {
        const payload = { user_id: this.state.userID };
        return fetch(`${this.host}/custom-api/webauthn/registration/start`, {
            method: 'POST',
            headers: {
                'Authorization': `Bearer ${this.state.token}`,
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(payload),
        })
        .then(response => response.json());
    }

    finishRegistration(credential) {
        const payload = {
            user_id: this.state.userID,
            credential,
        };

        return fetch(`${this.host}/custom-api/webauthn/registration/finish`, {
            method: 'POST',
            headers: {
                'Authorization': `Bearer ${this.state.token}`,
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(payload),
        })
        .then(response => response.json());
    }

    // Implement other methods (startAuthentication, finishAuthentication, etc.) similarly
}

// Use the custom Relying Party in the WebAuthnClient
const customRelyingParty = new CustomRelyingParty("https://your-custom-host.com");
const client = new WebAuthnClient(customRelyingParty);

// Custom sign in flow
FIDO2Client.signIn('janedoe@example.com', 'securePassword!')
    .then(() => {
        console.log("Signed in using custom Relying Party");

        // Start the registration process using the custom Relying Party
        return FIDO2Client.webauthnCreate('janedoe@example.com');
    })
    .then((response) => {
        console.log("Custom WebAuthn registration complete", response);

        // Now authenticate the user using the custom Relying Party
        return FIDO2Client.webauthnGet('janedoe@example.com');
    })
    .then((data) => {
        console.log("Custom WebAuthn authentication successful", data);
    })
    .catch((error) => {
        console.error("Error during registration/authentication", error);
    });
```

<a name="explanations"></a>

---
### Explanation

1. **CustomRelyingParty Class:**
   - This class mimics the behavior of the default `RelyingParty` class, but it uses a custom API endpoint (`/custom-api/...`) for managing authentication and WebAuthn interactions.
   - It maintains the same methods as the default Relying Party, such as `login`, `startRegistration`, `finishRegistration`, and more.

2. **Usage:**
   - You instantiate your custom Relying Party and pass it into the `WebAuthnClient` constructor. This allows the `WebAuthnClient` to interact with your custom API.
   - From here, the WebAuthnClient manages the WebAuthn process, while the custom Relying Party manages the server interactions.

3. **Flexibility:**
   - By implementing a custom Relying Party, you can tailor the behavior to match your own backend, authentication flows, and storage mechanisms.
   - For example, if you store `credential_id` in a database or use different APIs for managing public key credentials, you can adapt the Relying Party accordingly.

4. **Key Considerations:**
   - Ensure that the custom Relying Party's API endpoints handle the WebAuthn protocol correctly, following FIDO2 standards.
   - Security is paramount when handling credentials and tokens, so the custom Relying Party should use secure communication protocols (e.g., HTTPS) and implement proper token management (e.g., JWT expiration handling).


<a name="fido2-summary"></a>

---
### Summary

By default, the `WebAuthnClient` uses a built-in Relying Party to manage credential storage and verification. However, if you need more control or have a custom backend infrastructure, you can pass in your own Relying Party implementation, giving you flexibility to integrate FIDO2 authentication into any system. This modular approach ensures that developers can easily adopt WebAuthn while maintaining full control over server-side processes.
