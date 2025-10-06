# ZSM Client SDK for Browser UMFA Applications
# API Reference

The Universal Multi-Factor Authentication (UMFA) module provides a flexible solution for handling multi-factor authentication processes. It allows the configuration to be provided either as a JSON string or via a URL pointing to a configuration file. The authentication steps are executed using dynamically loaded modules.

Note that the following interface definitions presuppose that you have already imported the `UMFAClient` class from the ZSM Client SDK (`@ideem/zsm-client-sdk`). If you have not yet done so, please refer to the [UMFAClient Quick-Start Guide](./quick-start.md) documentation for instructions on how to import the ZSM Client SDK.


---

## UMFAClient API Documentation

### UMFAClient API Client SDK Methods
-   [`checkEnrollment(userIdentifier)`](#check-enrollment)
-   [`enroll(userIdentifier)`](#enroll)
-   [`authenticate(userIdentifier)`](#authenticate)
-   [`unenroll(userIdentifier)`](#unenroll)

### UMFAClient Server Methods
-   [Out-of-Band Token Validation](#oob)

---

<a name="check-enrollment"></a>

## UMFAClient API Client SDK Methods

### `UMFAClient.checkEnrollment(userIdentifier)` <br>(_checks for existing credentials on the device_)

The `checkEnrollment` method of the `UMFAClient` class examines the user's enrollment status for the Universal Multi-Factor Authentication (UMFA) process on the current device. This allows the application to determine whether the user has already enrolled in the UMFA process, allowing the developer to skip the enrollment process, should the user already be enrolled, or perform additional authentication prior to enrollment.

#### Parameters

| Parameter Name   | Data Type | Description                                                                               |
|------------------|-----------|-------------------------------------------------------------------------------------------|
| `userIdentifier` | `String`  | The unique identifier for the user. This is typically the user's email address or a UUID. |

#### Returns

| Parameter Name | Data Type | Description                                                                                                         |
|----------------|-----------|---------------------------------------------------------------------------------------------------------------------|
| `response`     | `Promise` | Returns a promise containing the results of the checkEnrollment query. This can include any _one_ of the following: |
|                |           | `credentialID (String)`: The local credential set's credential ID (user HAS enrolled)                               |
|                |           | `false (Boolean)`: Indicating user does not have a local ZSM UMFA credential set                                    |
|                |           | `error ( Error )`: An Error occurred while attempting to check the enrollment status                                |

### Usage

**JavaScript**
```javascript
const enrollmentStatus = await umfaClient.checkEnrollment(userIdentifier);
if(enrollmentStatus instanceof Error) throw(enrollmentStatus);                   // Error Condition
if(enrollmentStatus !== false) {
    console.log(`${user} is enrolled with credential_ID: ${enrollmentStatus}`);  // Credentials Present
} else {
    console.log(`${user} is not enrolled`);                                      // Credentials Absent
}
```
**TypeScript**
```typescript
const enrollmentStatus:string|boolean|Error = await umfaClient.checkEnrollment(userIdentifier);
if (enrollmentStatus instanceof Error) throw(enrollmentStatus);                  // Error Condition
if (typeof enrollmentStatus === 'string') {
    console.log(`${user} is enrolled with credential_ID: ${enrollmentStatus}`);  // Credentials Present
} else {
    console.log(`${user} is not enrolled`);                                      // Credentials Absent
}
```




<a name="enroll"></a>

---

### `UMFAClient.enroll(userIdentifier)` <br>(_enrolls a user and creates a ZSM credential on the device_)

The `enroll` method of the `UMFAClient` class initiates the enrollment process for the Universal Multi-Factor Authentication (UMFA) module. This process allows the user to enroll in the UMFA process, which is required for subsequent authentication operations.

#### Parameters

| Parameter Name   | Data Type | Description                                                                               |
|------------------|-----------|-------------------------------------------------------------------------------------------|
| `userIdentifier` | `String`  | The unique identifier for the user. This is typically the user's email address or a UUID. |

#### Returns

| Parameter Name | Data Type | Description                                                                                                         |
|----------------|-----------|---------------------------------------------------------------------------------------------------------------------|
| `response`     | `Promise` | Returns a promise containing the results of the checkEnrollment query. This can include any _one_ of the following: |
|                |           | `credential (Object) `: The credential object containing the user's authentication information*.                     |
|                |           | `false (Boolean)`     : Indicating user is already enrolled and has a valid local credential set                    |
|                |           | `error ( Error )`     : An Error occurred while attempting to enroll the user                                       |

_(to be stored for use in verification later. Note this is a JSON object, and may need to be `JSON.stringify`'d depending on your storage mechanism.)_

### Usage

**JavaScript**
```javascript
const userAuthenticationJWT = await UMFAClient.enroll(userIdentifier);
if (userAuthenticationJWT instanceof Error) throw(userAuthenticationJWT);                            // Error Condition
if(userAuthenticationJWT !== false) {
    console.log(`Enrollment of ${userIdentifier} successful! JWT Token: ${userAuthenticationJWT}`);  // Successful Enrollment
} else {
    console.error('Enrollment failed.');                                                             // Enrollment Failed
}
```

**TypeScript**
```typescript
const userAuthenticationJWT:string|boolean|Error = await UMFAClient.enroll(userIdentifier);
if (userAuthenticationJWT instanceof Error) throw(userAuthenticationJWT);                            // Error Condition
if (typeof userAuthenticationJWT === 'string') {
    console.log(`Enrollment of ${userIdentifier} successful! JWT Token: ${userAuthenticationJWT}`);  // Successful Enrollment
} else {
    console.error('Enrollment failed.');                                                             // Enrollment Failed
}
``` 





<a name="authenticate"></a>

---

### `UMFAClient.authenticate(userIdentifier)`<br>(_validates the user's ZSM credential on the device_)

Performs an authentication operation using the Universal Multi-Factor Authentication (UMFA) module, using the credential currently stored on the device for the specified user. If the specified user's credential is not present on the device, authentication will fail.


#### Parameters

| Parameter Name   | Data Type | Description                                                                               |
|------------------|-----------|-------------------------------------------------------------------------------------------|
| `userIdentifier` | `String`  | The unique identifier for the user. This is typically the user's email address or a UUID. |

#### Returns

| Parameter Name | Data Type | Description                                                                                                           |
|----------------|-----------|-----------------------------------------------------------------------------------------------------------------------|
| `response`     | `Promise` | Returns a promise containing the results of the checkEnrollment query. This can include any _one_ of the following:   |
|                |           | `credential (Object) `: The credential object containing the user's authentication information*.                      |
|                |           | `error ( Error )`: "${userIdentifier} is not enrolled." (if the user is not enrolled)                                 |
|                |           | `error ( Error )`: An Error occurred while attempting to enroll the user                                              |

_(to be stored for use in verification later. Note this is a JSON object, and may need to be `JSON.stringify`'d depending on your storage mechanism.)_


### Usage

**JavaScript**
```javascript
const userAuthenticationJWT = await UMFAClient.authenticate(userIdentifier);
if (userAuthenticationJWT instanceof Error) throw(userAuthenticationJWT);                            // Error Condition
console.log(`${userIdentifier} successfully authorized! JWT Token: ${userAuthenticationJWT}`);       // Successful Authorization
```

**TypeScript**
```typescript
const userAuthenticationJWT:string|Error = await UMFAClient.authenticate(userIdentifier);
if (userAuthenticationJWT instanceof Error) throw(userAuthenticationJWT);                            // Error Condition
console.log(`${userIdentifier} successfully authorized! JWT Token: ${userAuthenticationJWT}`);       // Successful Authorization
```





<a name="unenroll"></a>

---

### `UMFAClient.unenroll(userIdentifier)` <br>(_removes the user's ZSM credential from the device_)

The `unenroll` method of the `UMFAClient` class removes the specified, enrolled user's credentials from the device the user is actively logged in with. This effectively deletes any credentials associated with the user, preventing future authentication using those credentials, until the user re-enrolls on said device.


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
const unenrollResult = await UMFAClient.unenroll(userIdentifier);
if (unenrollResult instanceof Error) throw(unenrollResult);                // Error Condition
console.log(`${userIdentifier} successfully unenrolled!`);                 // Successful Unenrollment
```

**TypeScript**
```typescript
const unenrollResult:string|Error = await UMFAClient.unenroll(userIdentifier);
if (unenrollResult instanceof Error) throw(unenrollResult);                // Error Condition
console.log(`${userIdentifier} successfully unenrolled!`);                 // Successful Unenrollment
```

<a name="oob"></a>

---

## Out-of-Band Token Validation

For out-of-band validation of the JWT returned by enroll and authenticate, use the `/api/umfa/validate-token` endpoint.

### HTTP Method
`POST`
### URL
```bash
$ZSM_AUTHENTICATOR_HOST/api/umfa/validate-token
```

### Request Headers
| Header          | Value                                         | Description                           |
|-----------------|-----------------------------------------------|---------------------------------------|
| `Content-Type`  | `application/json`                            | Indicates the payload format          |
| `Authorization` | `Bearer XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX` | Authorizes the API (API Key expected) |

### Request Body
| Field            | Type   | Required | Description                                                                                                                           |
|------------------|--------|----------|---------------------------------------------------------------------------------------------------------------------------------------|
| `application_id` | string | Yes      | The unique identifier for the request's (server-to-server) application, parsed as a UUID.                                             |
| `user_id`        | string | Yes      | The unique identifier for the user.                                                                                                   |
| `token`          | string | Yes      | The token received from a previous UMFA authentication operation.                                                                     |
| `token_type`     | string | No       | An optional token type specifying the type of validation. Can be "credential" to validate a get() PublicKeyCredential.                |
| `trace_id`       | string | No       | An optional identifier for the validate-token operation. If a `trace_id` is not provided, then a random `trace_id` will be generated. |

### Successful Response (HTTP 200)
```json
{
  "user_id": "janedoe@gmail.com",
  "trace_id": "7a626fe9-ce25-4b87-8eb2-b12a7ee20143"
}
```
| Field      | Type   | Description                                            |
|------------|--------|--------------------------------------------------------|
| `user_id`  | string | The unique identifier for the user that was validated. |
| `trace_id` | string | The trace identifier for the validate-token operation. |

### Error Responses
```json
{
  "status": 400,
  "trace_id": "7a626fe9-ce25-4b87-8eb2-b12a7ee20143",
  "message": "Validate token failed with: MFA login JWT was invalid: Invalid JWT: There is no user_id claim"
}
```
| Field      | Type    | Description                                          |
|------------|---------|------------------------------------------------------|
| `status`   | integer | The HTTP response code.                              |
| `trace_id` | string  | The trace identifier for the verification operation. |
| `message`  | string  | Additional information about the validation failure. |

| Status Code                 | Description                 | Example Response                                                                                                         |
|-----------------------------|-----------------------------|--------------------------------------------------------------------------------------------------------------------------|
| `400 Bad Request`           | Incorrectly formed request  | { ... "message": "No data provided." }                                                                                   |
| `401 Unauthorized`          | Invalid or expired token    | { ... "message": "Validate token failed with: MFA login JWT was invalid: Invalid JWT: There is no webauthn_time claim" } |
| `500 Internal Server Error` | Server encountered an issue | { ... "message": "Server encountered an internal error" }                                                                |

### Example cURL Commands
#### HTTP Success (200) JWT Validation
```shell
$ curl -s - X POST -H "Content-Type: application/json"
-H "Authorization: Bearer XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
-d '{"application_id": "bf468b21-308f-49d2-9031-83556e0781d2", 
"user_id": "janedoe@gmail.com", "token": "eyJ0eX ... Nk9uWg"}' 
$ZSM_AUTHENTICATOR_HOST/api/umfa/validate-token | jq
{
  "user_id": "c7d7d44b-385e-4e83-bdd5-37e4fb3c8b7d",
  "trace_id": "7a626fe9-ce25-4b87-8eb2-b12a7ee20143"
}
```
#### HTTP Success (200) Credential Validation
```shell
$ curl -s - X POST -H "Content-Type: application/json"
-H "Authorization: Bearer XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
-d '{"application_id": "bf468b21-308f-49d2-9031-83556e0781d2", 
"user_id": "janedoe@gmail.com", 
"token":
{
  "id":"",
  "rawId":"rF2kHiKUQCO0d0Y4Wek9kA",
  "response":
  {
    "clientDataJSON":"eyJ0eXBlIjoid2ViYXV0aG4uZ2V0IiwiY2hhbGxlbmdlIjoiNWtMb2tjOXpkZzhuU2RFT2hzV1o5MUwzZTNGdjlqbERlRU9KcF93SnRkayIsIm9yaWdpbiI6Imh0dHBzOi8venNtLmFwcCJ9","authenticatorData":"ZxcNEwlh6TBKTTC5FMSFPTOboOZWzGeOSiYY7rm67WUFAAAAAQ","signature":"UOOoSBAwemLSPvqLVG2MDw41cqKLcHEUp4LAFGuvrVPsR1GWBYTWtmpqG_Jtjn-DXq5tGGAE58SYvu7uvcw7oHqquoNG4VEdm4Tz7UNe5kdSoc3RFpEGGDLCIz28iKXaBPbv3jdHi4xGoCIJKIIeHyh0-g7LUb4ZjYFIZHyXds7cdH9ozXRt5ERWUVvH1axDnPDKpntGQXG8FC4VXd0Rc01-4bBklNSGHOVgbO-Rpm8HgeFj3J4uOZDJ0xP7pnIkwOo5Uw_0ZO9xI66S8NQEtVzXVUKXs98f38LpLiLGEPlWtr_RIdf9xgsmHx-oVRJxC37gzV2ydSGKJaV6bNsXOw"},
    "type":"public-key"
  }
},
"token-type": "credential"}' 
$ZSM_AUTHENTICATOR_HOST/api/umfa/validate-token | jq
{
  "user_id": "c7d7d44b-385e-4e83-bdd5-37e4fb3c8b7d",
  "trace_id": "7a626fe9-ce25-4b87-8eb2-b12a7ee20143"
}
```
#### Bad Request (400)
```shell
$ curl -s - X POST -H "Content-Type: application/json"
-H "Authorization: Bearer XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
-d '{"application_id": "bf468b21-308f-49d2-9031-83556e0781d2", 
"user_id": "janedoe@gmail.com"}' 
$ZSM_AUTHENTICATOR_HOST/api/umfa/validate-token | jq
{
  "status": 400,
  "trace_id": "7a626fe9-ce25-4b87-8eb2-b12a7ee20143",
  "message": "Invalid data provided" 
}
```
#### Unauthorized (401)
```shell
$ curl -s - X POST -H "Content-Type: application/json"
-H "Authorization: Bearer XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
-d '{"application_id": "bf468b21-308f-49d2-9031-83556e0781d2", 
"user_id": "janedoe@gmail.com", "token": "eyJ0eX ... Nk9uWg"}' 
$ZSM_AUTHENTICATOR_HOST/api/umfa/validate-token | jq
{
  "status": 401,
  "trace_id": "7a626fe9-ce25-4b87-8eb2-b12a7ee20143",
  "message": "Validate token failed with: MFA login JWT was invalid: Invalid JWT: Invalid claim: The token has expired: 2024-12-10 18:41:03.0 +00:00:00" 
}
```

### Validate Token Failures

The server's token validation can fail for two general reasons:

- HTTP 400 Response: The request was malformed (i.e., `token` was not included)
- HTTP 401 Response: The token was invalid, which could have various causes
  - Token's `user_id` did not match the supplied `user_id`
  - Token has expired (the token's "exp" claim has passed)
  - Token was missing required claims ("iss", "sub", "iat", "exp", "user_id", "webauthn_time")
  - Token was not signed by the expected ZSM server's certificate
  - Token was not a valid PublicKeyCredential when supplied `"token_type" = "credential"`

### Decoded JWT

Below, we illustrate an example of a decoded UMFA JWT (Header and Payload). 
The validation performs standard JWT validation like signature, liveness, 
and structure. The validation also ensures that the payload claim `user_id`
matches that of the supplied `user_id`.

```json
{
  "typ": "JWT",
  "alg": "RS256",
  "iss": "Ideem::Authenticator"
}
```

```json
{
  "sub": "UMFA_login",
  "iss": "Ideem::Authenticator",
  "aud": [
    "Ideem::Authenticator",
    "Ideem::ZSM",
    "Ideem::ZSM_CLI"
  ],
  "iat": 1729280408,
  "exp": 1729366808,
  "jti": "042142c1-40c8-4d9b-bf5e-1fa84e0f3f03",
  "user_id": "c7d7d44b-385e-4e83-bdd5-37e4fb3c8b7d",
  "userpw_time": "2024-10-18T19:40:07.053364351+00:00",
  "webauthn_time": "2024-10-18T19:40:08.053364351+00:00"
}
```
