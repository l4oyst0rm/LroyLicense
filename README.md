![header](https://github.com/l4oyst0rm/LroyLicense/blob/main/assets/image_2025-06-25_233408199.png)

---
<p align="center">
  <h3 align="center">
    🗣️ Discord: https://discord.gg/vQCjCPJa5Z  📚  Docs: Coming Soon
  </h3>
  <h3 align="center">
    🏢 Full Version: https://discord.gg/vQCjCPJa5Z
  </h3>
</p>

---

> [!WARNING]
> This GitHub repo contains only example code and system information for LroyLicense, not the full code or functionality. For the complete system, check out our full version at [Discord Ticket](https://discord.gg/vQCjCPJa5Z) or contact us on [Discord](https://discord.gg/vQCjCPJa5Z). We provide no support for the example code and are not liable for any issues arising from its use.

* [🚀 Quick Start](#-quick-start)
  * [🔥 Installation](#-installation)
  * [🔥 Manual Installation](#-manual-installation)
* [🕵️ What is LroyLicense?](#-what-is-lroylicense)
* [✨ Features](#-features)
* [🔑 License Verification Examples](#-license-verification-examples)
  * [Java Example](#java-example)
  * [JavaScript Example](#javascript-example)
  * [Pseudocode Example](#pseudocode-example)
* [🔐 Developer Notes](#-developer-notes)
* [👨‍💻 About the Creator](#-about-the-creator)
* [🐞 Reporting Bugs](#-reporting-bugs)
* [🤝 Contributing](#-contributing)
* [🙌 Acknowledgements](#-acknowledgements)
* [📬 Contact](#-contact)
* [📜 License](#-license)

## 🚀 Quick Start

### 🔥 Installation

Get your license server up in **two steps**:

1. **Install Dependencies**:
   ```bash
   npm install express express-rate-limit discord.js node-fetch@2
   ```

2. **Start the Server**:
   ```bash
   node run.js --trace-warnings
   ```

Your server is live! Check [Configuration](#-developer-notes) for setup details.

### 🔥 Manual Installation

For hands-on setup:

1. Clone the repo:
   ```bash
   git clone https://github.com/L4oySt0rm/LroyLicense.git
   ```
2. Navigate to the folder:
   ```bash
   cd LroyLicense
   ```
3. Install dependencies:
   ```bash
   npm install express express-rate-limit discord.js node-fetch@2
   ```
4. Configure `config.js` (see [Developer Notes](#-developer-notes)).
5. Run the server:
   ```bash
   node run.js --trace-warnings
   ```

---

## 🕵️ What is LroyLicense?

**LroyLicense System** is a lightweight, secure, and affordable ($10) license server built by L4oySt0rm to protect your software. It’s designed for developers who need a fast, reliable way to manage licenses with minimal hassle. With Node.js support, a Discord bot, and a web dashboard, it’s perfect for small apps or large projects. The system is optimized to run on low-spec servers (1GB RAM, 50% CPU) and includes robust security to keep your licenses safe.

---

## ✨ Features

LroyLicense is packed with tools to make license management a breeze:

- **Node.js Support**: Easy integration with Node.js apps.
- **Discord Bot**: Manage licenses directly in Discord.
- **Web Dashboard**: Secure interface for license monitoring.
- **Anti-Phishing & Anti-Bot**: Blocks unauthorized license checks.
- **Free Data Storage**: No extra costs for license data.
- **Rate Limiting**: Protects your server from spam.
- **DDoS Protection**: Built-in defenses against attacks.
- **Easy Setup**: Up and running in minutes.
- **Clear Console Logs**: Detailed, colorful logs for debugging.
- **Low Resource Usage**: Runs smoothly on minimal hardware.
- **Free 24/7 Support**: Get help anytime via Discord.

> [!TIP]
> Host with **WardenHosting** for free setup and fast support. Perfect for LroyLicense!

---

## 🔑 License Verification Examples

Integrate LroyLicense with these example snippets for **Java**, **JavaScript**, and **Pseudocode**. These are demos, not the full system—get the complete version at [Discord Ticket](https://discord.gg/vQCjCPJa5Z).

### Java Example
Uses `java.net.http` and `org.json` for HTTP requests and JSON parsing.

```java
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.net.URI;
import java.time.Duration;
import org.json.JSONObject;

public class LicenseVerifier {
    private static final int MAX_RETRIES = 3;
    private static final Duration TIMEOUT = Duration.ofSeconds(5);

    public static boolean validateLicense(String licenseKey) {
        String product = "your-product";
        String apiKey = "your-api-key";
        int attempts = 0;

        while (attempts < MAX_RETRIES) {
            attempts++;
            try {
                HttpClient client = HttpClient.newBuilder()
                        .connectTimeout(TIMEOUT)
                        .build();
                HttpRequest request = HttpRequest.newBuilder()
                        .uri(URI.create("https://your-license-server.com/" + product + "/" + licenseKey + "?key=" + apiKey))
                        .header("x-api-key", apiKey)
                        .timeout(TIMEOUT)
                        .GET()
                        .build();

                HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());

                if (response.statusCode() == 200) {
                    JSONObject license = new JSONObject(response.body());
                    String status = license.optString("status", "");
                    if ("active".equals(status) && "your-product".equalsIgnoreCase(license.optString("productName", ""))) {
                        logSuccess(licenseKey, license);
                        return true;
                    } else {
                        logError("Invalid license: " + status);
                    }
                } else {
                    logError("Server error: HTTP " + response.statusCode());
                }
            } catch (Exception e) {
                if (attempts == MAX_RETRIES) {
                    logError("Server unreachable after " + MAX_RETRIES + " tries: " + e.getMessage());
                    return false;
                }
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException ie) {
                    Thread.currentThread().interrupt();
                }
            }
        }
        return false;
    }

    private static void logSuccess(String licenseKey, JSONObject license) {
        System.out.println("✅ License valid!");
        System.out.println("Key: " + obfuscateKey(licenseKey));
        System.out.println("Product: " + license.optString("productName"));
        System.out.println("Expires: " + license.optString("expired"));
    }

    private static void logError(String message) {
        System.err.println("❌ ERROR: " + message);
        System.err.println("Contact support: https://support.your-site.com");
    }

    private static String obfuscateKey(String key) {
        if (key == null || key.length() <= 4) return key;
        return key.substring(0, 4) + "*".repeat(key.length() - 4);
    }

    public static void main(String[] args) {
        boolean isValid = validateLicense("sample-key-1234");
        System.out.println("License check: " + (isValid ? "Valid" : "Invalid"));
    }
}
```

**Usage**:
- Requires `org.json:json` and Java 11+.
- Replace `your-product`, `your-api-key`, and `https://your-license-server.com`.

### JavaScript Example
Uses `node-fetch` in Node.js for HTTP requests.

```javascript
const fetch = require('node-fetch');

class LicenseVerifier {
    static MAX_RETRIES = 3;
    static TIMEOUT = 5000;

    static async validateLicense(licenseKey) {
        const product = 'your-product';
        const apiKey = 'your-api-key';
        let attempts = 0;

        while (attempts < this.MAX_RETRIES) {
            attempts++;
            try {
                const controller = new AbortController();
                const timeoutId = setTimeout(() => controller.abort(), this.TIMEOUT);

                const response = await fetch(
                    `https://your-license-server.com/${product}/${licenseKey}?key=${apiKey}`,
                    {
                        method: 'GET',
                        headers: { 'x-api-key': apiKey },
                        signal: controller.signal
                    }
                );

                clearTimeout(timeoutId);

                if (response.ok) {
                    const license = await response.json();
                    if (license.status === 'active' && license.productName.toLowerCase() === 'your-product') {
                        this.logSuccess(licenseKey, license);
                        return true;
                    } else {
                        this.logError(`Invalid license: ${license.status}`);
                    }
                } else {
                    this.logError(`Server error: HTTP ${response.status}`);
                }
            } catch (error) {
                if (attempts === this.MAX_RETRIES) {
                    this.logError(`Server unreachable after ${this.MAX_RETRIES} tries: ${error.message}`);
                    return false;
                }
                await new Promise(resolve => setTimeout(resolve, 1000));
            }
        }
        return false;
    }

    static logSuccess(licenseKey, license) {
        console.log('✅ License valid!');
        console.log(`Key: ${this.obfuscateKey(licenseKey)}`);
        console.log(`Product: ${license.productName}`);
        console.log(`Expires: ${license.expired}`);
    }

    static logError(message) {
        console.error(`❌ ERROR: ${message}`);
        console.error('Contact support: https://support.your-site.com');
    }

    static obfuscateKey(key) {
        if (!key || key.length <= 4) return key;
        return key.slice(0, 4) + '*'.repeat(key.length - 4);
    }
}

(async () => {
    const isValid = await LicenseVerifier.validateLicense('sample-key-1234');
    console.log(`License check: ${isValid ? 'Valid' : 'Invalid'}`);
})();
```

**Usage**:
- Install `node-fetch` (`npm install node-fetch@2`).
- Update `your-product`, `your-api-key`, and server URL.
- Runs in Node.js.

### Pseudocode Example
A language-agnostic guide for license verification.

```
FUNCTION validateLicense(licenseKey)
    SET product = "your-product"
    SET apiKey = "your-api-key"
    SET maxRetries = 3
    SET timeout = 5 seconds
    SET attempts = 0

    WHILE attempts < maxRetries
        INCREMENT attempts
        TRY
            SEND GET request to "https://your-license-server.com/" + product + "/" + licenseKey + "?key=" + apiKey
                WITH header "x-api-key" = apiKey
                WITH timeout = timeout

            IF response.statusCode = 200
                PARSE response as JSON into license
                IF license.status = "active" AND license.productName = "your-product"
                    LOG "✅ License valid!"
                    LOG "Key: " + obfuscateKey(licenseKey)
                    LOG "Product: " + license.productName
                    LOG "Expires: " + license.expired
                    RETURN true
                ELSE
                    LOG "❌ ERROR: Invalid license: " + license.status
                ENDIF
            ELSE
                LOG "❌ ERROR: Server error: HTTP " + response.statusCode
            ENDIF
        CATCH error
            IF attempts = maxRetries
                LOG "❌ ERROR: Server unreachable after " + maxRetries + " tries: " + error.message
                RETURN false
            ENDIF
            WAIT 1 second
        ENDTRY
    ENDWHILE
    RETURN false
ENDFUNCTION

FUNCTION obfuscateKey(key)
    IF key IS NULL OR key.length <= 4
        RETURN key
    ENDIF
    RETURN key[0:4] + "*" repeated (key.length - 4) times
ENDFUNCTION
```

**Usage**:
- Implement in any language.
- Replace placeholders with real values.

---

## 🔐 Developer Notes

- **Dependencies**:
  - Server: `express`, `express-rate-limit`, `discord.js`, `node-fetch@2`.
  - Java client: `org.json:json`, Java 11+.
  - JavaScript client: `node-fetch@2`.
  - Pseudocode: No dependencies.
- **Security**: Store API keys in `config.js` or environment variables, not hardcoded.
- **Configuration**: Create `config.js`:
  ```javascript
  module.exports = {
    product: 'your-product',
    apiKey: 'your-api-key',
    serverUrl: 'https://your-license-server.com',
    discordBotToken: 'your-discord-bot-token',
    supportLink: 'https://support.your-site.com'
  };
  ```
- **Customization**: Adjust server URLs, product names, and logs to fit your project.
- **Support**: Update `https://support.your-site.com` and Discord links.

---

## 👨‍💻 About the Creator

- **L4oySt0rm**: The brains behind LroyLicense, handling design and optimization. Connect on [Discord](https://discord.gg/vQCjCPJa5Z) for support or collabs!
- **Open to Collaborators**: Skilled in Node.js, Discord bots, or web dev? Join us to level up LroyLicense!

---

## 🐞 Reporting Bugs

Hit a bug? Open an issue on our [GitHub repo](https://github.com/L4oySt0rm/LroyLicense). We’ll tackle it fast!

---

## 🤝 Contributing

Want to make LroyLicense better? Here’s how to contribute:

1. Fork the repo.
2. Create a branch: `git checkout -b feature/awesome-update`.
3. Commit changes: `git commit -m "Added awesome update"`.
4. Push: `git push origin feature/awesome-update`.
5. Open a Pull Request.

---

## 🙌 Acknowledgements

Big thanks to L4oySt0rm for building and open-sourcing this project, and to the community for testing and feedback!

---

## 📬 Contact

- **L4oySt0rm**: Reach out on [Discord](https://discord.gg/vQCjCPJa5Z) for support or collabs.
- **Support**: Join our [Discord server](https://discord.gg/vQCjCPJa5Z) for 24/7 help.

---

## 📜 License

LroyLicense System is licensed under the [MIT License](LICENSE).

---

**Ready to protect your software?** Get the full LroyLicense System at [Discord Ticket](https://discord.gg/vQCjCPJa5Z) and secure your licenses with ease! 🚀
