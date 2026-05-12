### Relais Privacy Policy

**Last Updated: May 2026**

Relais is an AI client application that prioritizes **"Data Sovereignty."** We firmly believe that your conversations, configurations, and files belong solely to you. This policy details how we handle your information when you use Relais.

#### 1. Core Philosophy: Local-First

The core architecture of Relais is designed to be **"Local-First."** By default, all your chat history, system logs, model configurations, and the data within the underlying iSH virtual file system are **stored only on your local device.** We do not operate any official cloud servers to collect or analyze your data.

#### 2. Data Collection and Destination

* **Local Models & Data:** When you use local Large Language Models (LLMs) downloaded or imported via Relais, all computation and inference are performed offline on your device (e.g., utilizing Apple Silicon's MLX framework). Your conversation data never leaves your device.
* **Custom Gateways (OpenClaw / Self-hosted Agents):** If you configure an OpenClaw Gateway or other self-hosted agent endpoints, Relais acts as a pure client. Communication data is sent directly to the **server address you configured.** Relais does not intercept, mirror, or perform any telemetry during this process.
* **Third-Party AI Providers:** If you configure API Keys for third-party AI services (such as OpenAI, Anthropic, etc.) in the "Provider List," your conversation content and relevant prompts are sent directly to the respective provider to generate a response. Please refer to the privacy policy of the specific provider for their data handling practices. Your API Keys are stored encrypted only on your local device and your personal iCloud Keychain (if sync is enabled).

#### 3. iCloud and Multi-Device Sync

If you actively enable the **"Cloud Sync"** feature in settings, your configuration files, chat records, and self-hosted settings will be synced via your personal Apple iCloud account. The security and privacy of this data are protected by Apple's iCloud Privacy Policy. The Relais development team cannot access your data within the iCloud container.

#### 4. Use of Permissions

To implement core features, we may request the following permissions:

* **Network Access:** Used to download local model files and connect to your configured APIs or gateways.
* **Local Storage/File Access:** Used to save model weights, database files, the iSH file system, and for importing/exporting configurations.

#### 5. Data Deletion and "Nuclear Reset"

You have absolute control over your data. You can perform a deep and irreversible cleanup through the **"Reset App"** function in the settings. This will completely shred all data, including Gateway configurations, third-party provider data, chat history, downloaded model files, and the underlying iSH file system, restoring the app to its initial "blank" state.

#### 6. Contact Us

If you have any questions about this Privacy Policy or how Relais handles data, or if you need to submit feedback, please contact us via GitHub Issues:
[https://github.com/xy464193/relais/issues](https://www.google.com/search?q=https://github.com/xy464193/relais/issues)