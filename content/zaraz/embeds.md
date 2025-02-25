---
pcx_content_type: get-started
title: Embeds
weight: 7
---

# Embeds

Embeds are tools for incorporating external content, like social media posts, directly onto webpages, enhancing user engagement without compromising site performance and security.

Cloudflare Zaraz introduces server-side rendering for embeds, avoiding third-party JavaScript to improve security, privacy, and page speed. This method processes content on the server side, removing the need for direct communication between the user's browser and third-party servers.

To add an Embed to Your Website:

1. Log in to the [Cloudflare dashboard](https://dash.cloudflare.com/login), and select your account and domain.
2. Go to **Zaraz** > **Tools Configuration**.
3. Click "add new tool" and activate the desired tools on your Cloudflare Zaraz dashboard.
4. Add a placeholder in your HTML, specifying the necessary attributes. For a generic embed, the snippet looks like this:
```html
<componentName-embedName attribute="value"></componentName-embedName>
```
Replace `componentName`, `embedName` and `attribute="value"` with the specific Managed Component requirements. Zaraz automatically detects placeholders and replaces them with the content in a secure and efficient way.

## Examples

### X (Twitter) embed

```html
<twitter-post tweet-id="12345"></twitter-post>
```
Replace `tweet-id` with the actual tweet ID for the content you wish to embed.

### Instagram embed

```html
<instagram-post post-url="https://www.instagram.com/p/ABC/" captions="true"></instagram-post>
```

Replace `post-url` with the actual URL for the content you wish to embed. To include posts captions set captions attribute to `true`.
