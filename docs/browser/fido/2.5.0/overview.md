# ZSM Client SDK Overview
This document covers the installation and use of the ZSM Client SDK. The ZSM Client SDK offers interfaces into Ideem's cryptographic authentication services for implementing **FIDO2 Authenticator** into your appliction.

<a name="fido2-auth"></a>

## FIDO2 Authentication
If you have an existing relying party infrastructure in place, you can use Ideem's FIDO Authenticator interfaces to perform Attestation (WebAuthn Create) and Assertion (WebAuthn Get) operations.  

#### **The FIDO2Client class exposes these interfaces:**

<a name="webauthn-retrieve"></a>

### webauthnRetrieve()
`webauthnRetrieve()` is used to determine if an end user is already enrolled. If they are enrolled, their ZSM credentials are returned.

---
<a name="webauthn-create"></a>

### webauthnCreate()
`webauthnCreate()` is used to enroll the consumer, which generates their ZSM credentials.  After successful enrollment, the credential ID and Public Key associated with it are returned.  This data can then be stored in association with the consumer ID, and used for future `webauthnGet()` verification operations.

---
<a name="webauthn-get"></a>

### webauthnGet()
`webauthnGet()` is used to verify the consumer against their existing ZSM credential set.  During `webauthnGet()` processing, a challenge from a relying party server is signed using ZSM MPC.  The signed challenge can then be verified using the Public Key stored following the `webauthnCreate()` operation.

---
<a name="fido2-sequence-diagrams"></a>

### FIDO2 Authentication Sequence Diagrams

**Example Enrollment Flow**
After performing strong authentication for a consumer, you cryptographically bind them to their device using `webauthnCreate()`.

<div style="background-color: white; padding: 10px;">
  <img src="./assets/App_First_Use.png" alt="Enrollment" />
</div>

**Example Verification Flow**
On subsequent logins, you perform a silent 2FA for the consumer, using the `webauthnGet()`.

<div style="background-color: white; padding: 10px;">
  <img src="./assets/App_Next_Use.png" alt="Verification" />
</div>

**Example Step Up Verification Flow**
When the consumer initiates a transaction requiring re-verification, e.g., a large money transfer, you can perform a silent verification using `webauthnGet()` before allowing the transaction.

<div style="background-color: white; padding: 10px;">
  <img src="./assets/App_Next_Use_Step_Up.png" alt="Re-Verification" />
</div>

---

<a name="backward-compat"></a>

## API Versioning and Backward Compatibility
Ideem uses semantic versioning of its APIs.  Semantic versioning is an industry standard approach around how API versions are named. It defines a version number in the format of X.Y.Z where X, Y, and Z are the major, minor, and patch versions.

In practicality, Semantic Versioning is implemented on a spectrum of strictness. We take a flexible approach in the following way:

* We only increase the major version for changes of a significant breaking nature. If this version number changes, significant changes must be made by all users of our API.
* We may increase the minor version for minor breaking changes instead of increasing the major version.  We would only take such action when that change has a very limited scope and the impact has been discussed and agreed upon by the impacted parties. A concrete example of this is if we change a special feature used by one or two of our customers in a breaking way in conjunction with agreement from these impacted customers. But the change otherwise does not impact our overall customer base.
* In a strict interpretation, the patch version would only be increased for bug fixes. We may increase our patch version for minor new features or enhancements to existing features so long as these changes do not represent a breaking change.

Ideem will support the current and one prior major version of each API for a 24-month period, including all minor, and patch updates to those major versions.

---

<a name="getting-started"></a>

## Let's Get Started
In the following pages, you will learn how to:

1. Add the ZSM Client SDK to your application project
2. Use the interfaces for FIDO2 Authentication and Universal Multi-Factor Authentication (UMFA).
