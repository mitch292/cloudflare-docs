---
type: example
summary: Redirect a response based on the country code in the header of a visitor.
goal:
  - Localization
operation:
  - Redirect
product:
  - Snippets
preview:
  - true
pcx_content_type: example
title: Country code redirect
layout: example
---

```js
export default {
  async fetch(request) {
    /**
     * A map of the URLs to redirect to
     * @param {Object} countryMap
     */
    const countryMap = {  // Replace the country codes and target URLs with ones that apply to your case.
      US: "https://example.com/us",
      EU: "https://example.com/eu",
    };

    // Use the cf object to obtain the country of the request
    // more on the cf object: https://developers.cloudflare.com/workers/runtime-apis/request#incomingrequestcfproperties
    const country = request.cf.country;

    // If country is not null and is defined in the country map above, redirect.
    if (country != null && country in countryMap) {
      const url = countryMap[country];
      // Remove this logging statement from your final output.
      console.log(`Based on ${country}-based request, your user would go to ${url}.`);
      return Response.redirect(url);

    // If request country not in map, return another page.
    } else {
      return fetch("https://example.com", request);
    }
  },
};
```