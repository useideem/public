# Ideem FIDO2Client for Browser Applications Introduction

**Confidential** - *All materials are intended to be used for informational purposes only and are the sole intellectual property of Ideem, Inc.*

For questions and feedback, contact <a href="mailto:support@useideem.com">support@useideem.com</a>.

## Introduction
Ideem provides this client software development kit (ZSM Client SDK) for your use in integrating **FIDO2 Authentication** into your application.

Here is a high level view of the interfaces exposed by the ZSM Client SDK:

<div>
  <img src="./assets/ideem_client_sdk_overview2.png" alt="Ideem Client SDK" />
</div>

<a name="the-ideem-system"></a>

## The Ideem System
The ZSM Client SDK works along with other Ideem system components to perform the available operations.  Those components are:

<a name="ideem-servers"></a>

### Ideem Servers
* **ZSM Server**: The Multi-Party Cryptographic Server contains the ZSM Crypto Module and participates with the Client SDK in all MPC-based operations.
* **Authentication Server**: The Authentication Server (sometimes referred to as the "Relying Party" Server) is responsible for managing consumer profiles, integrating with authentication services, and providing WebAuthn relying party capabilities.  The Authentication Server is used as part of Ideem's Universal Multi-Factor Authentication (UMFA) product, but is **NOT** used by the FIDO2 Authentication product.

<a name="ideem-core-components"></a>

### Ideem Core Components
* **ZSM Crypto Module**: The ZSM Crypto Module provides the underlying MPC-based cryptography at the core of Ideem’s ZSM and is [FIPS 140-3 certified](https://csrc.nist.gov/projects/cryptographic-module-validation-program/certificate/4982).  

<a name="ideem-self-service-utilities"></a>

### Ideem Self-Service Utilities
* **Admin Console**: The Admin Console provides our customers application management, consumer management, usage monitoring, and troubleshooting capabilities.

<a name="ideem-client-packages"></a>

### Ideem Client Packages
* **ZSM FIDO2Client SDK**: The FIDO2Client SDK contains the ZSM Crypto Module and provides programmatic access to Ideem’s interfaces for FIDO2 authentication.
* **FIDO2Client Plugins**: The FIDO2Client SDK can be extended with plugins to provide additional functionality. See [FIDO2Client Available Plugins](./plugins.md) for more information.

Here is a high level view of how these system components work together:

<div style="background-color: white; padding: 10px;">
  <img src="./assets/ideem_system_overview.png" alt="Ideem System Overview" />
</div>

