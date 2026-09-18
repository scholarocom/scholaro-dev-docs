# Single Sign-On (SSO)

Single Sign-On (SSO) lets your users sign in to Scholaro through your organization's identity provider (IdP) using their existing work credentials, instead of a separate Scholaro email and password.

Scholaro supports both **OpenID Connect (OIDC)** and **SAML 2.0**, so any standards-compliant identity provider can be connected.

## How Scholaro SSO works

- Your users go to the Scholaro sign-in page and enter their work email address.
- Scholaro routes them to your organization's identity provider based on their **email domain** (for example, `@university.edu`).
- They authenticate with your IdP and are returned to Scholaro, already signed in.
- On a user's first sign-in, Scholaro **creates their account automatically** (just-in-time provisioning). There is no need to pre-create users in Scholaro.

!!! warning "Your IdP controls access to Scholaro"
    When SSO is enabled for your domain, anyone your identity provider can authenticate for that domain can sign in to Scholaro. If your IdP is compromised, unauthorized parties could gain access to your Scholaro account. Scholaro only trusts your IdP to assert identities **within your verified email domain**.

## Enabling SSO

You set SSO up yourself, under **Settings → SSO** in Scholaro. The high-level process is:

1. **Request your email domains.** Scholaro verifies that your organization controls each domain before approving it. This is the one step that waits on us.
2. **Add a connection.** Choose OIDC or SAML and enter the details from your identity provider.
3. **Register Scholaro in your IdP.** Your connection's page lists the exact URLs to use. See the provider-specific guide below.
4. **Switch it on** and sign in with a test account from your domain.

[Full walkthrough of the Scholaro side →](sso/self-service-setup.md)

!!! note
    The Redirect URI and ACS URL shown in these guides are **examples**. Always use the exact URLs shown on your own connection's page — they contain an identifier unique to it.

!!! info "Prefer us to do it?"
    Scholaro can still configure the connection on your behalf. Ask your representative, and you need only approve the domain and register the application in your identity provider.

## Set up SSO with your identity provider

Start with [Setting up SSO in Scholaro](sso/self-service-setup.md), then follow the guide for your provider:

- [Microsoft Entra ID](sso/entra-id-setup.md) — Microsoft 365 / Azure AD, via OIDC.
- [Google Workspace](sso/google-workspace-setup.md) — via a custom SAML app.
- [Okta](sso/okta-setup.md) — via OIDC.
- [Other identity providers](sso/other-idp-setup.md) — any OIDC or SAML 2.0 provider (OneLogin, Ping, ADFS, Shibboleth, and more).

## Supported features

- **OpenID Connect (OIDC) and SAML 2.0** identity providers.
- **Email-domain routing** — users are sent to the right IdP automatically based on their email address.
- **Just-in-time provisioning** — Scholaro accounts are created on first sign-in.
- **Automatic deprovisioning** — when you disable a user at your IdP, their Scholaro access is revoked within the re-authentication interval.
- **Custom attribute mapping** — if your IdP sends email or name under non-standard attribute names, you can map them on the connection.
- **Self-service setup** — configure, test, and switch connections on and off yourself, without waiting on a Scholaro release.

## Limitations

- **Domains must be approved by Scholaro.** Claiming a domain decides where every sign-in on it is routed, so we verify that you control it first. Everything after that is in your hands.
- **One identity provider per email domain.**
- **One protocol per connection.** OIDC or SAML is fixed when the connection is created, because it forms part of the URLs you register with your provider.
- **SCIM is not supported.** User deprovisioning takes effect at the next re-authentication rather than instantly through directory sync.
