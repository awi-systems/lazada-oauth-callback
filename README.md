# Lazada OAuth Redirect Page

This repository hosts a simple OAuth redirect page for **Lazada Open Platform authorization**, deployed using **GitHub Pages**.

The page captures the authorization `code` returned by Lazada and forwards it to **Microsoft Power Automate** for token exchange, storage, and API integration.

---

## 🔐 Purpose

This project is designed to:

- Receive the Lazada OAuth authorization code
- Forward the code to a Power Automate HTTP trigger
- Support internal tools for:
  - Sales reporting
  - Order synchronization
  - Inventory monitoring
  - API-based data integrations

This redirect page **does not store access tokens** and performs no backend processing.

---

## 🌐 Live Redirect URL

```
https://awi-systems.github.io/lazada-oauth-redirect/lazada_callback.html
```

Use this URL as the **Callback / Redirect URL** when creating a Lazada **Seller In-house App**.

---

## 🔄 OAuth Flow Overview

```
Seller authorizes Lazada App
          ↓
Lazada redirects to callback page
          ↓
Authorization code captured (code)
          ↓
Code sent to Power Automate
          ↓
Power Automate exchanges code for tokens
          ↓
Tokens stored and used for Lazada APIs
```

---

## 📄 How It Works

1. Lazada redirects the browser to the callback page with query parameters:
   ```
   ?code=AUTH_CODE&state=STATE
   ```

2. JavaScript extracts:
   - `code` (authorization code)
   - `state` (optional)

3. The data is sent via HTTP `POST` to a **Power Automate trigger URL**.

4. A success message is displayed to the user.

---

## 🧩 Power Automate Integration

The callback page sends the following JSON payload:

```json
{
  "platform": "lazada",
  "code": "AUTHORIZATION_CODE",
  "state": "STATE_VALUE"
}
```

Your Power Automate flow should:
- Receive the authorization code
- Call Lazada `/auth/token/create`
- Store:
  - `access_token`
  - `refresh_token`
  - `expires_in`
  - `seller_id`
- Handle token refresh logic

---

## ⚙️ Configuration

Inside `lazada_callback.html`, update the following value:

```js
const POWER_AUTOMATE_URL = "YOUR_POWER_AUTOMATE_TRIGGER_URL";
```

This must be a **valid HTTPS Power Automate HTTP trigger**.

---

## 🔒 Security Notes

- No API keys or secrets are stored in this repository
- OAuth tokens are never exposed in the browser
- HTTPS is enforced via GitHub Pages
- Intended for **Seller In-house / internal use only**

---

## 🛠️ Technologies Used

- HTML
- JavaScript
- GitHub Pages
- Microsoft Power Automate
- Lazada Open Platform (OAuth)

---

## 📌 Intended Use

- Lazada Seller In-house Apps
- Internal reporting tools
- Inventory and sales data automation
- ERP and analytics integrations

This project is **not intended for public SaaS distribution**.

---

## 📄 License

Internal use only.  
© AWI Systems
