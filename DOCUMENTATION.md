# 🖼️ SVG Image Generator API Documentation

A robust and secure image placeholder generation service built by **It Is Unique Official**, perfect for developers and designers needing on-the-fly SVGs with customizable properties.

---

## 🚀 Features Overview

### 🎨 Dynamic SVG Generation

* Customize image dimensions (width & height)
* Add your own text to the SVG image
* Change background and text colors with valid HEX codes
* Set font family from a predefined safe list
* Control font size within secure bounds

### 🧩 Responsive UI

* A clean, real-time preview interface built with Tailwind CSS
* Dynamic URL builder for immediate use in your projects

### 🔐 Security-Focused

* Input validation for all parameters
* Escaping of injected text to prevent XSS attacks
* Strong Content-Security-Policy (CSP) headers
* Fallback SVG for invalid input
* HTTP caching via Cache-Control headers

### 📋 One-Click Copy URLs

* Easy to integrate in HTML `<img>` tags or use directly in web apps

---

## 🏷️ API Endpoint

**Base URL:** `https://img.itisuniqueofficial.com/`

Send parameters as query strings.

### 📌 Example Usage

```html
<img src="https://img.itisuniqueofficial.com/?width=600&height=400&text=Demo&bg=%23FFD700&color=%23000&size=30&font=Verdana,sans-serif" alt="SVG Placeholder" />
```

---

## 📥 Accepted Parameters

| Parameter | Description            | Type    | Constraints                      | Default             |
| --------- | ---------------------- | ------- | -------------------------------- | ------------------- |
| `width`   | Image width in pixels  | Integer | 1–2000                           | `400`               |
| `height`  | Image height in pixels | Integer | 1–2000                           | `300`               |
| `text`    | Custom text on image   | String  | Max 100 characters (URL-encoded) | `{width}x{height}`  |
| `bg`      | Background color (hex) | String  | Must be valid HEX color          | `#ccc`              |
| `color`   | Text color (hex)       | String  | Must be valid HEX color          | `#555`              |
| `size`    | Font size in pixels    | Integer | 8–200                            | `24`                |
| `font`    | Font family            | String  | Must be in safe list             | `Arial, sans-serif` |

---

## 🔤 Safe Font List

Only these fonts are supported:

* Arial, sans-serif
* Helvetica, sans-serif
* Times New Roman, serif
* Courier New, monospace
* Verdana, sans-serif
* Georgia, serif

---

## 🧪 Sample URLs

* **Default SVG:**
  `https://img.itisuniqueofficial.com/`

* **Custom Size:**
  `https://img.itisuniqueofficial.com/?width=800&height=600`

* **Text + Colors:**
  `https://img.itisuniqueofficial.com/?width=500&height=400&text=Hello%20World&bg=%23FF0000&color=%23FFFFFF`

* **Font & Size:**
  `https://img.itisuniqueofficial.com/?width=600&height=300&text=Custom%20Font&font=Helvetica,%20sans-serif&size=36`

---

## 🛡️ Security Architecture

### ✅ Input Validation

All inputs are validated against strict patterns and bounds. Width and height are capped at 2000. Font size is restricted to safe sizes. Colors are verified to be valid hexadecimal.

### 🔐 XSS Protection

The text value is encoded and injected into the SVG safely, escaping any harmful characters that could lead to XSS.

### 🧱 Content Security Policy (CSP)

A strict CSP is enforced via headers:

```
Content-Security-Policy: default-src 'none';
```

This prevents loading of any external scripts, images, or styles.

### 🚫 Error Handling

Invalid or missing parameters return a visually styled fallback SVG with an error message.

### 🕒 Caching Strategy

Responses include the following header:

```
Cache-Control: max-age=3600
```

This enables browsers and CDNs to cache images for up to 1 hour.

---

## 💼 Use Cases

* Placeholder images for wireframes or mockups
* Dynamic image generation for dashboards
* Customizable social media cards (with caution)
* Banner creation tools

---

## 👨‍💻 Developer Tips

* Always URL-encode the `text` parameter.
* Use `%23` instead of `#` in HEX codes when embedding in URLs.
* Use safe fonts to avoid unexpected fallbacks.
* Use `img` or CSS `background-image` for integration.

---

## 🧑‍💼 Maintained By

Built and maintained with ❤️ by [It Is Unique Official](https://itisuniqueofficial.com/)
Visit our [News](https://www.theblazetimes.in/), [Apps](https://apk.itisuniqueofficial.com/), and [Store](https://itisuniqueofficial.gumroad.com/) portals for more projects.
