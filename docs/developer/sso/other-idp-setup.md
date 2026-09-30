# Other Identity Providers

Scholaro works with any identity provider that supports **OpenID Connect (OIDC)** or **SAML 2.0** — including OneLogin, Ping, ADFS, Shibboleth, and others.

Read [Setting up SSO in Scholaro](self-service-setup.md) first. The URLs below come from your connection's **Setup URLs** page and do not exist until it is saved, so create the application in your provider, save the connection in Scholaro with its details, then come back and enter the URLs. Use the tab that matches the protocol your identity provider uses.

=== "OIDC"

    **1. Register an application**

    In your identity provider, create a new **web / confidential** OIDC application — one that can hold a client secret and uses the authorization-code flow.

    Set the **Redirect URI** (also called the sign-in redirect or callback URL) to the value on your connection's page:

    `https://www.scholaro.com/login/sso/oidc-12/callback`

    Your connection also offers a post-logout redirect URI and a front-channel logout URI. Both are optional; Scholaro does not yet sign users out of your provider.

    **2. Configure scopes**

    Make sure the application can return the **openid**, **profile**, and **email** scopes. Scholaro always also requests **offline_access**, so your provider must accept it or ignore it; if it issues a refresh token, Scholaro can detect within about 15 minutes when a user is disabled at the IdP.

    **3. Enter the details in Scholaro**

    | Field | Notes |
    | --- | --- |
    | Authority | The provider's OIDC issuer, starting `https://`; its discovery document must be at `<issuer>/.well-known/openid-configuration` |
    | Client ID | From the application you registered |
    | Client secret | Stored encrypted, never displayed back |
    | Scopes | Leave blank unless your provider needs something beyond the standard set |
    | Email claim type / Name claim type | Only if your provider uses non-standard claims |

    **4. Add the Redirect URI** from the connection's **Setup URLs** page to your application, then **turn the connection on** and sign in with an address on your domain.

=== "SAML 2.0"

    **1. Create a SAML application**

    In your identity provider, create a new **SAML 2.0** application and enter the service-provider values from your connection's page:

    - **ACS URL** (Assertion Consumer Service / reply URL): `https://www.scholaro.com/login/sso/saml-12/Acs`
    - **SP Entity ID** (audience): `https://www.scholaro.com/login/sso/saml-12/Saml2`
    - **Name ID**: the user's email address (`EMAIL` format).

    Some providers can import Scholaro's own metadata instead — that URL is on the same page.

    !!! note "Scholaro does not sign its authentication requests"
        If your provider requires signed AuthnRequests, contact Scholaro support.

    **2. Confirm the email attribute**

    Ensure the assertion includes the user's email — as the Name ID or as an attribute. If your IdP uses a non-standard attribute name (for example the eduPerson / LDAP OID `urn:oid:0.9.2342.19200300.100.1.3` for email), note which attribute carries the email and name so Scholaro can map them.

    **3. Enter the details in Scholaro**

    | Field | Notes |
    | --- | --- |
    | IdP metadata URL | An `https://` URL reachable from the internet — Scholaro loads your signing certificate from it and cannot accept an uploaded file |
    | IdP Entity ID | The identity provider's issuer / entity ID |
    | Email claim type | Only if your provider uses a non-standard attribute for the email address |
    | Name claim type | Only if your provider uses a non-standard attribute for the name |

    **4. Enter the ACS URL and SP Entity ID** from the connection's **Setup URLs** page in your provider, then **turn the connection on** and sign in with an address on your domain.

!!! note
    The Redirect URI, ACS URL, and SP Entity ID shown above are **examples**. Always use the exact values on your own connection's page — the number in them is unique to it.
