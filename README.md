# Server Soft Launcher

A Windows desktop launcher for Minecraft: Java Edition, currently under private development as part of Server Soft, a Minecraft server-management application.

**This is a public project-information repository, not a source-code repository.** It provides an overview for Minecraft AppID review. Application source code, installers, executables, authentication data, and development logs are not published here. There is no public launcher release or download at this time.

## Purpose

The planned launcher will let users create separate Java Edition profiles and launch a client suited to a server they manage in Server Soft. Each profile is intended to have its own game directory so that mods, settings, and saved worlds remain separate. Independent profiles without a linked server are also planned.

Vanilla support is planned first. Support for Fabric, Forge, and NeoForge, compatible mod configurations, and optional server connection settings is planned for later stages. These launcher features are not yet available.

## Current development status

The following reflects development testing, not a released product:

| Component | Status |
| --- | --- |
| Microsoft OAuth 2.0 with PKCE | Implemented; sign-in and token exchange tested |
| Xbox Live authentication | Successfully tested in the development application |
| XSTS authorization | Successfully tested in the development application |
| Minecraft Services authentication | Implemented; currently returns HTTP 403 during testing |
| Java Edition entitlement and profile verification | Implemented and tested with mocks; not yet verified against live Minecraft Services |
| Encrypted credential storage and local sign-out | Implemented and covered by automated tests; end-to-end live Minecraft account persistence is not yet verified |
| Profile management, game installation, and game launch | Planned; not yet implemented |

Launcher development is currently on hold pending AppID review and Minecraft Services access verification. An HTTP 403 response alone does not establish its exact cause, and this project does not claim that API access has been approved.

## Authentication and API access

Server Soft Launcher uses its own registered Microsoft application ID. It does not reuse another launcher's application ID, tokens, account files, or user data.

The implemented authentication flow uses the system browser and OAuth 2.0 authorization code flow with PKCE, followed by Xbox Live and XSTS authentication. The application does not ask users to enter their Microsoft account password into an application-owned form.

Minecraft API access is requested to complete legitimate Minecraft Services authentication, verify Java Edition access, and retrieve the authenticated user's Java Edition profile. Future game launching will require valid authentication and Java Edition access; no authentication or license-check bypass is planned.

## Security and user-data protection

- Authentication tokens are handled in the Electron main process and are not exposed to the renderer through the account-status API.
- Persisted credentials are encrypted using Electron `safeStorage` on Windows. Plaintext credential storage is not an alternative when encryption is unavailable.
- Account passwords, tokens, authorization codes, and raw authentication responses are not written to application diagnostics.
- Authentication callbacks validate state, expiry, and single-use handling. Cancelled or superseded authentication attempts cannot save a late result.
- Local sign-out removes only this application's stored Minecraft credentials. It does not sign the user out of Microsoft globally or alter another launcher's account data.
- Minecraft entitlement, parental-control, account-restriction, and other safety checks are not intentionally bypassed.
- Existing Server Soft servers, worlds, backups, and unrelated account data remain separate from launcher credentials.

## Planned mod handling

Server mods will not be copied indiscriminately to a client. Planned synchronization will distinguish server-only, client-only, required-on-both-sides, and unknown compatibility; check dependencies and game/loader versions; and present changes for confirmation. Manually added client mods and existing server files will not be silently removed or overwritten. Distribution and licensing requirements must be respected.

## Review and availability

This README is the public project overview for the AppID application. No Minecraft game files, third-party mods, source archive, or runnable build are distributed in this repository. Providing this overview does not guarantee application approval.

## Disclaimer

Server Soft Launcher is an independent project. It is not an official Minecraft product and is not approved by or associated with Mojang Studios or Microsoft. Minecraft is a trademark of its respective owners.
