# Setting Up SSO in Scholaro

This page covers the Scholaro side of an SSO connection: claiming your email domains, creating the connection, and turning it on. The [provider guides](../sso.md#set-up-sso-with-your-identity-provider) cover the other side — registering Scholaro in your identity provider.

You will move between the two, because each needs a value from the other: Scholaro needs your provider's details to save a connection, and your provider needs the URLs Scholaro generates once it is saved. The order below is the one that works.

## Prerequisites

- An **administrator** account in Scholaro. The **Settings** menu, including **SSO**, is shown only to administrators, and only they can request domains or change connections.
- SSO enabled for your organization. If you do not see **SSO** in your settings menu, contact your Scholaro representative.

---

## 1. Request your email domains

Go to **Settings → SSO → Domains** and enter the email domains your members sign in with — for example `university.edu`. You can enter several at once, separated by commas, spaces or semicolons, or one per line.

Each domain matches exactly. A subdomain such as `mail.university.edu` is a separate domain and needs its own request and connection.

Scholaro reviews each request and verifies that your organization controls the domain before approving it. This is what stops a domain being claimed by anyone else, so it is a manual step and not an instant one.

You can watch the status on the same page:

| Status | What it means |
| --- | --- |
| **Pending review** | Submitted, waiting on Scholaro |
| **Approved** | Ready to use for a connection |
| **Rejected** | Not approved — any note from Scholaro is shown in the Notes column |

!!! note "Public email providers are not eligible"
    Domains such as `gmail.com` or `outlook.com` cannot be used for SSO. Use your institution's own domain.

---

## 2. Create the application in your identity provider

Scholaro will not save a connection without your provider's details, so start there. Create the application following the [guide for your provider](../sso.md#set-up-sso-with-your-identity-provider), copy the values listed for Scholaro, and leave the redirect or ACS URL blank for now — or enter a temporary value if your provider insists. You will fill it in at step 4.

---

## 3. Add a connection

Once a domain is approved, go to **Settings → SSO** and choose **Add connection**. Pick the protocol your identity provider uses:

- **OpenID Connect (OIDC)** — Microsoft Entra ID, Okta, and most modern providers. Choose this if you are not sure.
- **SAML 2.0** — Google Workspace, ADFS, Shibboleth, and providers that only offer SAML.

!!! warning "The protocol cannot be changed later"
    It forms part of every URL you will register with your identity provider. To switch, delete the connection and create a new one.

Fill in the details from your provider and save. The fields differ by protocol:

=== "OpenID Connect"

    | Field | Notes |
    | --- | --- |
    | Email domain | Chosen from your approved domains |
    | Authority | Required. Your provider's **issuer** URL, starting `https://`. Scholaro appends `/.well-known/openid-configuration` itself, so do not paste the full discovery document URL |
    | Client ID | Required. From your provider |
    | Client secret | Required. From your provider. Stored encrypted and never displayed again |
    | Scopes | Optional — leave blank for `openid profile email`. Scholaro always adds `openid` and `offline_access` |
    | Email claim type / Name claim type | Optional — only if your provider sends the email or name under a non-standard claim |

=== "SAML 2.0"

    | Field | Notes |
    | --- | --- |
    | Email domain | Chosen from your approved domains |
    | IdP Entity ID | Required. From your provider |
    | IdP metadata URL | Required. An `https://` URL reachable from the internet. Scholaro cannot accept an uploaded metadata file or certificate |
    | Email claim type / Name claim type | Optional — only if your provider sends the email or name under a non-standard attribute |

**Login URL name** is optional. Set it to get a direct sign-in link, such as `https://www.scholaro.com/login/sso/acme`, that you can put on your intranet to send members straight to your identity provider. It must be 3–63 characters of lowercase letters, numbers and single hyphens, and the link works only while the connection is on.

A new connection is always created **turned off**, so nothing changes for your users while you configure your provider.

---

## 4. Give Scholaro's URLs to your identity provider

Open **Setup URLs** on the connection you just saved. It lists the URLs to give your provider, with the name each one goes by in the common providers, and a copy button on each.

=== "OpenID Connect"

    | URL | Typical field name |
    | --- | --- |
    | Redirect URI | *Redirect URI (Web)* in Entra, *Sign-in redirect URI* in Okta |
    | Post-logout redirect URI | Optional. *Sign-out redirect URI* in Okta, or add it to the **Redirect URIs** list in Entra |
    | Front-channel logout URI | Optional. *Front-channel logout URL* in Entra |

    !!! note "Single sign-out is not active yet"
        Signing out of Scholaro does not currently sign users out of your identity provider. Registering the two logout URIs now is harmless and saves a change later.

=== "SAML 2.0"

    | URL | Typical field name |
    | --- | --- |
    | SP Entity ID | *Audience URI (SP Entity ID)*, or *Identifier (Entity ID)* in Entra |
    | ACS URL | *Single sign-on URL*, or *Reply URL* in Entra |
    | SP metadata URL | Scholaro's own metadata — some providers can import this instead of the two values above |

For either protocol, if you set a **Login URL name**, the page also shows a **Login launch URL** — the direct sign-in link to put on your intranet. Your provider does not need it.

Paste each into the application you created in step 2, replacing any temporary value. The guides for [Microsoft Entra ID](entra-id-setup.md), [Google Workspace](google-workspace-setup.md), [Okta](okta-setup.md), and [other providers](other-idp-setup.md) show where each one goes.

Assign the application to the users who should have access before you continue.

---

## 5. Turn it on

Back on the connection's page, use the **Turn on** button at the top. Confirm when prompted.

!!! warning "This takes effect immediately, for everyone on the domain"
    From that moment, anyone signing in with an address on that domain is sent to your identity provider instead of the Scholaro sign-in page. Make sure your provider is configured and the application is assigned first.

Sign in with a test account from your domain to confirm. If something is wrong, turn the connection off — sign-in returns to normal immediately and your settings are kept. People already signed in stay signed in.

---

## Changing a connection later

A connection must be **turned off before it can be edited or deleted**. Editing a live connection would change where sign-ins are sent while they are in flight, so Scholaro asks you to take it out of the sign-in path first.

The sequence is always: turn off → make the change → turn back on. Deleting also asks you to type the domain to confirm. Your users fall back to normal Scholaro sign-in in the meantime.

!!! tip "Client secrets expire"
    OIDC client secrets have an expiry date set by your identity provider, and **sign-in stops working when one expires**. Note the date when you create it. To rotate: create the new secret in your provider, then turn the connection off, paste the new value, and turn it back on.

Leaving the client secret field blank when editing keeps the stored one. It is never displayed again after saving.

---

## Troubleshooting

**Sign-in is not redirecting to my provider.** Check the connection is turned on, and that the address you entered is exactly on the connection's domain — subdomains do not match.

**OIDC sign-in fails before reaching my provider.** Check the Authority is the issuer (for example `https://login.microsoftonline.com/<tenant-id>/v2.0`), not the `/.well-known/openid-configuration` address.

**My provider returns an error before Scholaro is reached.** Usually the redirect URI or ACS URL does not exactly match what is registered. Compare them character by character, including `https://` and any trailing slash.

**I sign in at my provider but Scholaro refuses me.** Scholaro requires the email address in the assertion to be on the connection's domain. If your provider releases the address under a non-standard attribute name, set **Email claim type** on the connection to match.

**I cannot add a connection.** Either the domain is not approved yet, or your organization is not set up to create connections itself — contact your Scholaro representative, who can configure it for you.
