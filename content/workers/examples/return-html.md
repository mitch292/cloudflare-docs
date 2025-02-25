---
type: example
summary: Deliver an HTML page from an HTML string directly inside the Worker script.
demo: https://returning-html.workers-sites-examples.workers.dev
languages:
  - JavaScript
  - TypeScript
  - Python
  - Rust
preview:
  - true
playground-link: true
pcx_content_type: example
title: Return small HTML page
weight: 1
layout: example
updated: 2024-01-11
---

{{<tabs labels="js | ts | py | rs">}}
{{<tab label="js" default="true">}}

{{<render file="_return-html-example-js.md">}}

{{</tab>}}
{{<tab label="ts">}}

```ts
export default {
  async fetch(request): Promise<Response> {
    const html = `<!DOCTYPE html>
		<body>
		  <h1>Hello World</h1>
		  <p>This markup was generated by a Cloudflare Worker.</p>
		</body>`;

    return new Response(html, {
      headers: {
        "content-type": "text/html;charset=UTF-8",
      },
    });
  },
} satisfies ExportedHandler;
```

{{</tab>}}
{{<tab label="py">}}

```py
from js import Response, Headers

def on_fetch(request):
    html = """<!DOCTYPE html>
    <body>
      <h1>Hello World</h1>
      <p>This markup was generated by a Cloudflare Worker.</p>
    </body>"""

    headers = Headers.new({"content-type": "text/html;charset=UTF-8"}.items())
    return Response.new(html, headers=headers)
```

{{</tab>}}
{{<tab label="rs">}}

```rs
use worker::*;

#[event(fetch)]
async fn fetch(_req: Request, _env: Env, _ctx: Context) -> Result<Response> {
    let html = r#"<!DOCTYPE html>
		<body>
		  <h1>Hello World</h1>
		  <p>This markup was generated by a Cloudflare Worker.</p>
		</body>
    "#;
    Response::from_html(html)
}
```

{{</tab>}}

{{</tabs>}}
