# Glossary

## API Consumers

> API consumers are third-parties that are authorized by either a vendor or customer organization to have access to
> selected ZSM APIs for purposes of performing trusted third-party functions and operations. ZSM provides optional
> integrations for selected trusted third parties such.

## Consumer

> Consumers are the end-users of an application which contains the ZSM Client SDK. They are the customers of a ZSM
> customer organization.

## CredentialID

> WebAuthn identifier for a credential, synonymous with the identifier used for an enrollment. CredentialID is returned
> during the WebAuthn create() call and is used by the Relying Party server to store and find the Public Key needed to
> verify signed challenges during WebAuthn get() calls.

## Customer Client Application

> A customer organization’s client application (iOS, Android, or Web) that has incorporated the ZSM Cryptographic Module
> using the ZSM Client SDK

## Customer Host Server (aka Relying Party Server)

> A customer organization’s server that validates challenges signed by ZSM for consumer enrollments and verifications.

## Customer Organization

> ZSM customer that integrates the ZSM Client SDK into their application(s) and uses the ZSM Management APIs and Web
> Administration Console to manage their use of the service.

## Enrollment

> Enrollment is a term used to describe the unique combination of installation, consumer, application, and application
> environment. There is one enrollment for each unique combination of these four entities. Keysets are associated with
> an enrollment.

## Instance ID (deprecated)

> A unique identifier that identifies a specific instance of an application on a specific installation of the ZSM client
> on a device. This identifier is unique per application such that two applications installed on the same installation
> on a device have different instance identifiers. And, a specific application installed on different devices will each
> have
> their own unique instance identifiers.

## Organization

> Three types of organizations are defined in ZSM: API consumers, customers, and vendors.

## Server Client APIs

> REST APIs used to facilitate transactions between the ZSM Server and Client for consumer registration, key set
> generation, signing, and signature verification.

## Server Management APIs

> REST APIs used to manage the ZSM system, including organization management, application management, installation
> management, and consumer management.

## User

> Individual associated with a Customer who has registered an account with the ZSM service.

## Vendor

> This vendor organization is the organization that provides the ZSM system to customers. 
