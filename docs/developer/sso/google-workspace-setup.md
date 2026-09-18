# Google Workspace SSO Setup

This guide explains how to connect **Google Workspace** to Scholaro using a custom SAML application — the standard way to set up SSO for a third-party app in the Google Admin console.

Before you start, create the connection in Scholaro — see [Setting up SSO in Scholaro](self-service-setup.md). That is where the ACS URL and SP Entity ID below come from, and they do not exist until the connection is saved.

## Prerequisites

- A **super administrator** account for your Google Workspace domain ([admin.google.com](https://admin.google.com)).
- The **ACS URL** and **SP Entity ID** from your connection's page in Scholaro. They look like:

    - ACS URL: `https://www.scholaro.com/login/sso/<your-connection>/Acs`
    - SP Entity ID: `https://www.scholaro.com/login/sso/<your-connection>/Saml2`

!!! note
    Use the exact ACS URL and Entity ID Scholaro gave you — the examples above are illustrative.

---

## 1. Create a custom SAML app

In the Google Admin console, go to **Apps -> Web and mobile apps**, click **Add app**, then **Add custom SAML app**.

- **App name**: `Scholaro`
- (Optional) upload an app icon.

![Add custom SAML app - App details](../images/sso/google-app-details.png)

Click **Continue**.

---

## 2. Copy Google's identity provider details

On the **Google Identity Provider details** step, **download the IdP metadata** (recommended) and note the values Scholaro will need:

- **SSO URL**
- **Entity ID**
- **Certificate** (included in the metadata download)

![Google Identity Provider details](../images/sso/google-idp-details.png)

Click **Continue**.

---

## 3. Enter Scholaro's service-provider details

On the **Service provider details** step, enter the values from your connection's page:

- **ACS URL**: the ACS URL from Scholaro.
- **Entity ID**: the SP Entity ID from Scholaro.
- **Name ID format**: `EMAIL`.
- **Name ID**: *Basic Information > Primary email*.

![Service provider details](../images/sso/google-service-provider-details.png)

Click **Continue**.

---

## 4. Map attributes (optional)

On the **Attributes** step you can map Google directory fields to the claims Scholaro reads, for example:

- *Primary email* -> `email`
- *First name* / *Last name* -> `name`

The user's email is already carried by the Name ID, so attribute mapping is usually optional. Click **Finish**.

---

## 5. Turn the app on

A new app is **OFF for everyone** by default. Open the app, click **User access**, and turn it **ON** for the organizational units (or groups) whose members should use Scholaro SSO. Save.

![Service status set to ON for everyone](../images/sso/google-user-access.png)

!!! note
    Changes to user access can take a few minutes to propagate in Google Workspace.

---

## 6. Enter your details in Scholaro

Open your connection under **Settings -> SSO** and fill in:

| Field | Value |
| --- | --- |
| IdP Entity ID | Google Identity Provider details (step 2) |
| IdP metadata URL | The metadata URL from step 2 |

!!! note "Scholaro reads your metadata over the internet"
    Enter a metadata **URL**, not the downloaded XML. Scholaro re-reads it periodically, which is how it picks up your signing certificate when Google rotates it. If you only have the XML file, host it somewhere Scholaro can reach or contact your representative.

---

## 7. Switch it on and test

Use the switch at the top of your connection's page, then sign in with an address in your domain. You should be redirected to Google and returned to Scholaro signed in.

!!! tip
    If sign-in fails, confirm the app is turned **ON** for the test user's organizational unit, and that the ACS URL and Entity ID in Google match your connection's page exactly.

If anything is wrong, switch the connection off. Sign-in returns to normal immediately.
