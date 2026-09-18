# Okta SSO Setup

This guide explains how to connect **Okta** to Scholaro using OpenID Connect (OIDC).

Before you start, create the connection in Scholaro — see [Setting up SSO in Scholaro](self-service-setup.md). That is where the Redirect URI below comes from, and it does not exist until the connection is saved.

## Prerequisites

- Administrator access to your Okta org (the Admin Console, e.g. `https://<your-org>-admin.okta.com`).
- The **Redirect URI** from your connection's page in Scholaro. It looks like:

    `https://www.scholaro.com/login/sso/<your-connection>/callback`

!!! note
    Use the exact Redirect URI Scholaro gave you — the example above is illustrative.

---

## 1. Create an OIDC app integration

In the Okta Admin Console, go to **Applications -> Applications** and click **Create App Integration**.

- **Sign-in method**: *OIDC - OpenID Connect*
- **Application type**: *Web Application*

![Create a new app integration](../images/sso/okta-create-app.png)

Click **Next**.

---

## 2. Configure the app

On the **New Web App Integration** screen:

- **App integration name**: `Scholaro SSO` (or any name your team will recognize).
- **Grant type**: leave **Authorization Code** selected.
- **Sign-in redirect URIs**: paste the Redirect URI from your connection's page.
- **Sign-out redirect URIs**: paste the Post-logout redirect URI from the same page. Optional.

![Web app integration settings](../images/sso/okta-app-settings.png)

Under **Assignments**, choose who can use the app — for example *Allow everyone in your organization to access*, or limit it to specific groups.

Click **Save**.

---

## 3. Copy the client credentials

On the application's **General** tab, copy:

- **Client ID**
- **Client secret**

![Client ID and client secret](../images/sso/okta-client-credentials.png)

!!! warning
    Send the client secret to Scholaro through a secure channel.

---

## 4. Confirm your issuer URL

Your Okta **issuer** is your org URL, for example `https://<your-org>.okta.com`. You can confirm it by opening `https://<your-org>.okta.com/.well-known/openid-configuration` and reading the `issuer` value.

!!! note
    Scholaro requests the standard OpenID Connect scopes (`openid`, `profile`, `email`, and `offline_access`), which Okta grants to OIDC apps by default — there is nothing extra to configure.

---

## 5. Enter your details in Scholaro

Open your connection under **Settings -> SSO** and fill in:

| Field | Value |
| --- | --- |
| Authority | `https://<your-org>.okta.com` |
| Client ID | Application -> General tab |
| Client secret | Application -> General tab |
| Scopes | Leave blank |

Scholaro stores the client secret encrypted and never displays it back. Save, then assign your users to the application in Okta.

---

## 6. Switch it on and test

Use the switch at the top of your connection's page, then sign in with an address in your domain. You should be redirected to Okta and returned to Scholaro signed in.

!!! tip
    If sign-in fails, check that the Sign-in redirect URI in Okta matches the one on your connection's page exactly, and that your test user is assigned to the app.

If anything is wrong, switch the connection off. Sign-in returns to normal immediately.
