---
title: Custom Shortener
version: 3.0.0+
source_path: custom-shortener.md
github_url: https://github.com/devopssmartech/Intrack-doc/blob/main/custom-shortener.md
github_raw_url: https://raw.githubusercontent.com/devopssmartech/Intrack-doc/main/custom-shortener.md
---
# Custom Shortener

By default, Intrack shortens every link in the **Messages** section of a channel or campaign with its own shortener. Shortened links start with `intk.ir/`.

To use your own branded domain instead (for example `link.yourdomain.com`), activate a **custom shortener** by completing the steps below.

:::tip Continuity
Until setup is fully complete, campaigns and journeys keep using the default `intk.ir/` shortener with no interruption.
:::

## Prerequisites

- Access to your domain’s **DNS** or **CDN** panel (to create `A` and `TXT` records)
- A chosen subdomain prefix (commonly `link`, or any other prefix you prefer)
- Contact the **Intrack Customer Success (CS)** team to get the custom shortener **IP addresses**

## Steps

### 1. Get the Intrack IP addresses

Contact the **Intrack Customer Success** team and ask for the current **A record IP addresses** for the custom shortener.

Do not hardcode or reuse old IPs from screenshots or previous setups. Always confirm the latest values with CS.

### 2. Create an A record

In your DNS/CDN panel, create a new **A** record:

| Field | Value |
| --- | --- |
| **Type** | `A` |
| **Name / Host** | Your prefix (for example `link`) |
| **Value / Points to** | The IP addresses provided by Intrack CS |

You do **not** need to create a separate subdomain first. The Name/Host field alone creates the subdomain (for example `link` → `link.yourdomain.com`).

If CS provides multiple IPs, create one A record per IP under the same Name/Host.

### 3. Wait for DNS propagation

After saving the record, wait until DNS changes have fully propagated before continuing.

### 4. Register a TXT record for SSL (with Intrack)

Contact Intrack CS (Telegram, Bale, or any channel you already use with them) so they can register a **TXT** record for a **DNS-01** challenge on the same subdomain.

This step:

- Confirms you own the subdomain
- Lets Intrack issue and later renew the SSL certificate automatically
- Removes the need to upload certificates manually

### 5. SSL issuance

After the TXT record is in place, Intrack issues and activates SSL for your subdomain (for example `link.yourdomain.com`).

### 6. Final activation

When setup is complete, links generated in campaigns and journeys use your custom domain instead of `intk.ir/`.

**Example**

- Default: `intk.ir/xyz`
- Custom: `link.yourdomain.com/xyz`

## FAQ

### Does this affect my main domain or its SSL?

No. Setup only adds a new subdomain that points to Intrack and gets its **own** SSL certificate. Your main domain and its existing SSL remain unchanged.

### What is the SSL certificate issued through the TXT record?

It is only for the shortener subdomain (for example `link.yourdomain.com`). It is not your website’s primary SSL, and that certificate is never modified.

## Important notes

- Prefer a dedicated subdomain (for example `link.yourdomain.com`) rather than your primary website hostname.
- Keep DNS caching and extra CDN security features from interfering with resolution and certificate validation.
