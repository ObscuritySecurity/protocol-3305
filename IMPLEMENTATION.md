
# Protocol 3305: Implementation Guide

This document provides technical guidance and recommended best practices for implementing the articles of Protocol 3305. These are not exhaustive rules but rather a starting point for building compliant, people-first software.

## Pillar I: Architectural Foundation

### Art. 1: Privacy by Design
- **Data Minimization:** Before implementing any feature, ask: "What is the absolute minimum amount of data required for this to function?" Avoid collecting data for "future use."
- **Ephemeral Data:** Data that is not essential for long-term functionality should be ephemeral, existing only for the duration of a session and never written to disk.
- **Decentralization:** Where possible, prefer peer-to-peer or federated architectures over centralized, server-based models.

### Art. 2: Security by Default
- **Secure Configuration:** Your application should ship with the most secure settings enabled. For example, if you offer optional E2EE, it should be on by default.
- **Opt-In for Less Secure Features:** Any feature that might weaken the security posture (e.g., generating unencrypted exports) must be an explicit, well-warned opt-in.
- **Strong Cryptography:** Use modern, audited cryptographic libraries (e.g., Libsodium, Signal Protocol). Avoid implementing your own cryptography.

### Art. 3: Zero Trust
- **Authenticate Everything:** Every request between services, or between a client and a server, must be authenticated and authorized, even within a "trusted" network.
- **Principle of Least Privilege:** Services and user sessions should only have access to the specific resources they absolutely need. A user's token should not grant access to another user's data.
- **Short-Lived Tokens:** Use short-lived access tokens (e.g., JWTs) that require frequent re-authentication for sensitive operations.

## Pillar II: Data Sovereignty

### Art. 4: Zero Knowledge
- **Client-Side Encryption:** All encryption and decryption of user-generated content must occur on the client's device.
- **Key Management:** The user's private keys must **never** be sent to or stored on your servers. Implement key derivation from a user-provided password (e.g., using Argon2) or have the user manage their keys directly.
- **E2EE (End-to-End Encryption):** For communication apps, all messages, calls, and file transfers must be end-to-end encrypted. For storage apps, all data must be encrypted at rest with keys only the user holds.

### Art. 5: Zero Personal Data Collection
- **Anonymous Registration:** If possible, do not require an email or phone number for registration. Use cryptographically generated identities.
- **No Analytics SDKs:** Do not include third-party analytics SDKs (e.g., Google Analytics, Firebase Analytics, Mixpanel) in your client-side applications.
- **Server-Side Anonymization:** If you require operational metrics (e.g., total number of active users), ensure they are aggregated and completely anonymous, with no link to any individual.

### Art. 6: Zero Activity Logs
- **Disable Access Logs:** Configure your servers to not keep web server access logs (which contain IP addresses).
- **In-Memory Logging:** If logging is absolutely necessary for debugging, it should be done in-memory, be non-persistent, and contain no PII. It should be disabled in production environments.
- **No Metadata Trails:** Be mindful of metadata. For example, file storage services should not store plaintext filenames if they can be descriptive. Encrypt metadata where possible.

## Pillar III: Integrity and Guarantees

### Art.7: Open Source
- **Public Repository:** The source code for all client-facing applications and a significant portion of the backend logic must be available in a public repository (e.g., on GitHub, GitLab).
- **Reproducible Builds:** For advanced compliance, provide a way for users to verify that the distributed application was built from the public source code.
- **License:** Use a recognized open-source license (e.g., MIT, GPLv3, Apache 2.0).

### Art. 8: Zero Non-Essential Permissions
- **Audit Permissions:** On mobile and desktop, regularly audit the permissions your application requests. For each permission, justify its necessity for a core feature.
- **Graceful Degradation:** If a user denies a permission, the app should continue to function, perhaps with reduced capabilities. For example, a messaging app denied contact access should still allow users to add contacts manually.
- **Transparency:** Clearly explain why a permission is needed at the moment it is requested.
