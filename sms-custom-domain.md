---
title: Using a Custom Domain
version: 3.0.0+
source_path: sms-custom-domain.md
github_url: https://github.com/devopssmartech/Intrack-doc/blob/main/sms-custom-domain.md
github_raw_url: https://raw.githubusercontent.com/devopssmartech/Intrack-doc/main/sms-custom-domain.md
---
# Using a Custom Domain

In digital marketing, short and visually appealing SMS links are essential. Intrack automatically shortens any link included in your SMS campaigns, reducing message length while enabling click tracking.

**Example:** <br/>
- Original link: <br/>
`https://www.intrack.ir/search/category-mobile-phone/`
- Shortened link: <br/>
`intk.ir/xyz`

To strengthen your branding, you can use a custom domain for your links. For instance, if your custom domain is app.com, the shortened link would appear as:
`link.app.com/xyz`

This ensures your SMS campaigns are both user-friendly and brand-consistent. To avoid affecting SEO, credibility, or domain authority, it’s recommended to use a dedicated domain or subdomain rather than your main website domain. Choose a short and memorable domain to leave a lasting impression on your audience.

## Steps to Create a Custom Domain/Subdomain
### 1. Create a Subdomain
   Select a personalized subdomain for your custom links. Examples include:
   - `click.app.com`
   - `link.app.com`

### 2. Notify the Intrack Customer Success Team
   If your DNS is managed via ArvanCloud, inform the Intrack team first. Due to security requirements, the team must register your custom domain with ArvanCloud before you can proceed.
   Send a request using your corporate email to the **Customer Success team**.

### 3. Configure the Subdomain in Your CDN
   This setup works at the DNS level, redirecting requests to Intrack without additional processing.

#### 3.1. ArvanCloud
   - Open your DNS settings in ArvanCloud.
   - Create a new CNAME record:
     - **Type**: CNAME
     - **Name**: your subdomain (e.g., link)
     - **TTL**: 30 minutes
     - **Use Cloud Service**: Enabled
     - **Value/Target**: intk.ir
     - **Host Header**: your subdomain (e.g., link.app.com)

   The Host Header ensures the destination server knows which domain/subdomain the request is for, allowing it to respond correctly.

#### **3.2 Cloudflare**
   - Open your DNS settings in Cloudflare.
   - Create a new CNAME record:
     - **Type**: CNAME
     - **Name**: your subdomain (e.g., link)
     - **Target**: intk.ir
     - **TTL**: 30 minutes
     - **Proxy Status**: DNS only

### 4. Add the custom domain in Intrack
   Go to:
   - Profile Drop Down → General Setting → Custom Domain
   - Add your domain and save.

![Link Custom Domain](../../static/img/custom-domain/sms-custom-domain.png)

## How Custom Domain Links Work
Once the CNAME setup is complete, all SMS links in your Intrack campaigns can use your custom domain instead of the default `intk.ir/xyz`.

**Example:**
- `link.app.com/xyz` replaced by `intk.ir/xyz`

## Important Notes
- Ensure caching or extra security features in your CDN do not interfere with redirects.
- After configuration, verify your new record using tools like [Google Dig Toolbox](https://toolbox.googleapps.com/apps/dig) to confirm accuracy.
