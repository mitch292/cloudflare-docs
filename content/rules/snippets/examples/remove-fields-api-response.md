---
type: example
summary: If origin responds with `JSON`, parse the response and delete fields to return a modified response.
goal:
  - Other
operation:
  - Response modification
product:
  - Snippets
pcx_content_type: example
title: Remove fields from API response
layout: example
---

```js
export default {
    async fetch(request) {
        // Send original request to the origin
        const response = await fetch(request);
        // Check if origin responded with JSON
        try {
            // Parse API response as JSON
            var api_response = response.json();
            // Specify the fields you want to delete. For example, to delete "botManagement" array from parsed JSON:
            delete api_response.botManagement;
            // Serve modified API response
            return Response.json(api_response);
        }
        // On failure, serve unmodified origin's response
        catch (err) {
            return response;
        }
    },
};
```