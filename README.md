Here is your **Image URL Generator** project overview again, rewritten for clarity, formatting, and easy copying or sharing:

---

# **Image URL Generator**

A web-based tool for generating custom SVG placeholder images using a **Cloudflare Worker**.

---

## 🌟 Features

* **Dynamic SVG Generation**
  Customizable width, height, text, background/text colors, font, and font size.

* **Responsive UI**
  A clean, Tailwind CSS-styled `index.html` for real-time image preview and URL creation.

* **Security-Focused**
  Input validation, XSS prevention, and CSP headers (`Content-Security-Policy`).

* **Copyable URLs**
  One-click copy to use in `<img>` tags or web projects.

* **Favicon**
  Uses: [It Is Unique Official favicon](https://www.itisuniqueofficial.com/favicon.ico)

* **Powered by It Is Unique Official**
  Built and maintained with ❤️ by [It Is Unique Official](https://www.itisuniqueofficial.com)

---

## 📁 Project Structure

* `index.html` – Home page with form UI, preview, and copy button.
* `worker.js` – Cloudflare Worker backend that returns dynamic SVG.
* Favicon – Linked from `https://www.itisuniqueofficial.com/favicon.ico`

---

## ⚙️ Setup

### Prerequisites

* A modern browser (Chrome, Firefox, Safari, etc.)
* Cloudflare account with **Workers** enabled
* Web server or local environment (e.g., **VS Code Live Server**)

### Installation

```bash
git clone <repository-url>
cd image-url-generator
```

### Serve the Home Page

* Use a local server (like Live Server) or host `index.html`.
* Opening `file://index.html` may restrict clipboard access in browsers.

### Deploy the Worker

1. Copy `worker.js` into Cloudflare Workers.
2. Deploy at a route like:
   `https://img.itisuniqueofficial.workers.dev/`

---

## 🧠 `worker.js` Code

```javascript
export default {
  async fetch(request) {
    try {
      const { searchParams } = new URL(request.url);
      const w = Math.min(Math.max(parseInt(searchParams.get("width") || "400", 10), 1), 2000);
      const h = Math.min(Math.max(parseInt(searchParams.get("height") || "300", 10), 1), 2000);
      const text = decodeURIComponent(searchParams.get("text") || `${w}x${h}`).slice(0, 100);
      const bg = validateColor(searchParams.get("bg") || "#ccc");
      const color = validateColor(searchParams.get("color") || "#555");
      const size = Math.min(Math.max(parseInt(searchParams.get("size") || "24", 10), 8), 200);
      const font = sanitizeFont(searchParams.get("font") || "Arial, sans-serif");

      const svg = `
        <svg xmlns="http://www.w3.org/2000/svg" width="${w}" height="${h}">
          <rect width="100%" height="100%" fill="${bg}"/>
          <text x="50%" y="50%" dominant-baseline="middle" text-anchor="middle"
                fill="${color}" font-family="${font}" font-size="${size}">
            ${escapeXML(text)}
          </text>
        </svg>`.trim();

      const headers = {
        "Content-Type": "image/svg+xml",
        "Cache-Control": "public, max-age=3600",
        "Content-Security-Policy": "default-src 'none'",
      };
      return new Response(svg, { headers });

    } catch (error) {
      return new Response(
        `<svg xmlns="http://www.w3.org/2000/svg" width="400" height="300">
          <rect width="100%" height="100%" fill="#ccc"/>
          <text x="50%" y="50%" dominant-baseline="middle" text-anchor="middle" fill="#555" font-family="Arial, sans-serif" font-size="24">
            Error: Invalid parameters
          </text>
        </svg>`,
        {
          status: 400,
          headers: { "Content-Type": "image/svg+xml" },
        }
      );
    }
  },
};

function validateColor(color) {
  const hexPattern = /^#([0-9A-Fa-f]{3}){1,2}$/;
  return hexPattern.test(color) ? color : "#ccc";
}

function sanitizeFont(font) {
  const allowedFonts = [
    "Arial, sans-serif",
    "Helvetica, sans-serif",
    "Times New Roman, serif",
    "Courier New, monospace",
    "Verdana, sans-serif",
    "Georgia, serif",
  ];
  return allowedFonts.includes(font) ? font : "Arial, sans-serif";
}

function escapeXML(str) {
  return str.replace(/[&<>"']/g, (char) => ({
    '&': '&amp;',
    '<': '&lt;',
    '>': '&gt;',
    '"': '&quot;',
    "'": '&apos;',
  }[char]));
}
```

---

## 🔧 Parameters Accepted by Worker

| Parameter | Description            | Constraints                 | Default            |
| --------- | ---------------------- | --------------------------- | ------------------ |
| `width`   | Image width (px)       | 1–2000                      | 400                |
| `height`  | Image height (px)      | 1–2000                      | 300                |
| `text`    | Text on image          | Max 100 chars (URL-encoded) | `{width}x{height}` |
| `bg`      | Background color (hex) | Valid hex code              | `#ccc`             |
| `color`   | Text color (hex)       | Valid hex code              | `#555`             |
| `size`    | Font size (px)         | 8–200                       | 24                 |
| `font`    | Font family            | Predefined safe list        | Arial, sans-serif  |

---

## 🔗 Example URLs

* Default:
  `https://img.itisuniqueofficial.workers.dev/`

* Custom Size:
  `https://img.itisuniqueofficial.workers.dev/?width=800&height=600`

* Custom Text + Colors:
  `https://img.itisuniqueofficial.workers.dev/?width=500&height=400&text=Hello%20World&bg=#FF0000&color=#FFFFFF`

* Font + Size:
  `https://img.itisuniqueofficial.workers.dev/?width=600&height=300&text=Custom%20Font&font=Helvetica,%20sans-serif&size=36`

---

## 🛡️ Security Features

* **Validation** – Ensures dimensions, colors, and fonts are safe.
* **XSS Protection** – Escapes all text injected into SVGs.
* **CSP** – `Content-Security-Policy: default-src 'none'`.
* **Error Handling** – Invalid inputs return a fallback error SVG.
* **Caching** – Cache-Control header for 1 hour (`max-age=3600`).

---

## ⚠️ Limitations

* SVG favicons may not be supported by older browsers.
* Clipboard may not work over `file://` paths.
* JavaScript required for full functionality.

---

## 🤝 Contributing

1. Fork the repo
2. Create your branch:
   `git checkout -b feature/your-feature`
3. Commit your changes
4. Push and create a Pull Request

---

## 📜 License

Licensed under the **MIT License**. See the `LICENSE` file for details.

---

## 🙌 Acknowledgments

* Built by [It Is Unique Official](https://www.itisuniqueofficial.com)
* Powered by **Cloudflare Workers** & **Tailwind CSS**
* Favicon from It Is Unique Official

---

## 📬 Contact

For help or inquiries:
Email or open an issue on the GitHub repository.

---

Would you like me to generate the `index.html` file with the UI now?
