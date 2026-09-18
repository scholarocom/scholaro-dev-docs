# Setting Up SSO in Scholaro

This page covers the Scholaro side of an SSO connection: claiming your email domains, creating the connection, and switching it on. The [provider guides](../sso.md#set-up-sso-with-your-identity-provider) cover the other side — registering Scholaro in your identity provider.

You will move between the two, because each needs a value from the other. The order below is the one that works.

## Prerequisites

- An **administrator** account in Scholaro. Only administrators can request domains or change connections.
- SSO enabled for your organization. If you do not see **SSO** in your settings menu, contact your Scholaro representative.

---

## 1. Request your email domains

Go to **Settings → SSO → Domains** and enter the email domains your members sign in with — for example `university.edu`. You can enter several at once, separated by commas or on separate lines.

Scholaro reviews each request and verifies that your organization controls the domain before approving it. This is what stops a domain being claimed by anyone else, so it is a manual step and not an instant one.

You can watch the status on the same page:

| Status | What it means |
| --- | --- |
| **Pending** | Submitted, waiting on Scholaro |
| **Approved** | Ready to use for a connection |
| **Rejected** | Not approved — the reason is shown alongside it |

!!! note "Public email providers are not eligible"
    Domains such as `gmail.com` or `outlook.com` cannot be used for SSO. Use your institution's own domain.

---

## 2. Add a connection

Once a domain is approved, go to **Settings → SSO** and choose **Add connection**. Pick the protocol your identity provider uses:

- **OpenID Connect (OIDC)** — Microsoft Entra ID, Okta, and most modern providers. Choose this if you are not sure.
- **SAML 2.0** — Google Workspace, ADFS, Shibboleth, and providers that only offer SAML.

!!! warning "The protocol cannot be changed later"
    It forms part of every URL you will register with your identity provider. To switch, delete the connection and create a new one.

Fill in what you have and save. The fields differ by protocol:

=== "OpenID Connect"

    | Field | Notes |
    | --- | --- |
    | Email domain | Chosen from your approved domains |
    | Authority | Your provider's discovery URL |
    | Client ID | From your provider |
    | Client secret | From your provider |
    | Scopes | Optional — leave blank for the standard set |

=== "SAML 2.0"

    | Field | Notes |
    | --- | --- |
    | Email domain | Chosen from your approved domains |
    | IdP Entity ID | From your provider |
    | IdP metadata URL | Must be reachable from the internet |

If you have not created the application in your identity provider yet, save the connection first anyway — the URLs you need for that step only exist once the connection does.

**Login URL name** is optional. Set it to get a direct sign-in link, such as `https://www.scholaro.com/login/sso/acme`, that you can put on your intranet to send members straight to your identity provider.

A new connection is always created **switched off**, so nothing changes for your users while you configure your provider.

---

## 3. Register Scholaro in your identity provider

Open the connection you just saved. It lists the URLs to give your provider, with the name each one goes by in the common providers, and a copy button on each.

=== "OpenID Connect"

    | URL | Typical field name |
    | --- | --- |
    | Redirect URI | *Redirect URI (Web)* in Entra, *Sign-in redirect URI* in Okta |
    | Post-logout redirect URI | *Sign-out redirect URI* in Okta. In Entra, add it to the **Redirect URIs** list — Entra only returns users to a post-logout address registered there |
    | Front-channel logout URI | *Front-channel logout URL* in Entra |

=== "SAML 2.0"

    | URL | Typical field name |
    | --- | --- |
    | SP Entity ID | *Audience URI (SP Entity ID)*, or *Identifier (Entity ID)* in Entra |
    | ACS URL | *Single sign-on URL*, or *Reply URL* in Entra |
    | SP metadata URL | Scholaro's own metadata — some providers can import this instead of the two values above |

Now follow the guide for your provider: [Microsoft Entra ID](entra-id-setup.md), [Google Workspace](google-workspace-setup.md), [Okta](okta-setup.md), or [other providers](other-idp-setup.md).

Assign the application to the users who should have access before you continue.

---

## 4. Switch it on

Back on the connection's page, use the switch at the top. Confirm when prompted.

!!! warning "This takes effect immediately, for everyone on the domain"
    From that moment, anyone signing in with an address on that domain is sent to your identity provider instead of the Scholaro sign-in page. Make sure your provider is configured and the application is assigned first.

Sign in with a test account from your domain to confirm. If something is wrong, switch the connection off — sign-in returns to normal immediately and your settings are kept.

---

## Changing a connection later

A connection must be **switched off before it can be edited or deleted**. Editing a live connection would change where sign-ins are sent while they are in flight, so Scholaro asks you to take it out of the sign-in path first.

The sequence is always: switch off → make the change → switch back on. Your users fall back to normal Scholaro sign-in in the meantime.

!!! tip "Client secrets expire"
    OIDC client secrets have an expiry date set by your identity provider, and **sign-in stops working when one expires**. Note the date when you create it. To rotate: create the new secret in your provider, then switch the connection off, paste the new value, and switch it back on.

Leaving the client secret field blank when editing keeps the stored one. It is never displayed again after saving.

---

## Troubleshooting

**Sign-in is not redirecting to my provider.** Check the connection is switched on, and that the address you entered is on the connection's domain.

**My provider returns an error before Scholaro is reached.** Usually the redirect URI or ACS URL does not exactly match what is registered. Compare them character by character, including `https://` and any trailing slash.

**I sign in at my provider but Scholaro refuses me.** Scholaro requires the email address in the assertion to be on the connection's domain. If your provider releases the address under a non-standard attribute name, set **Email claim type** on the connection to match.

**I cannot add a connection.** Either the domain is not approved yet, or your organization is not set up to create connections itself — contact your Scholaro representative, who can configure it for you.
