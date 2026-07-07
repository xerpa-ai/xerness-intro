# Security Policy

The Xerness team takes security seriously. Thank you for helping keep Xerness and its users safe.

## Reporting a vulnerability

**Please do not report security vulnerabilities through public GitHub issues, discussions, or pull requests.**

Instead, report them privately through either of the following:

- **GitHub Security Advisories** — use the [**Report a vulnerability**](https://github.com/xerpa-ai/xerness-intro/security/advisories/new) button on this repository's *Security* tab (preferred).
- **Direct contact** — reach the maintainers via [xagt.ai](https://xagt.ai) or [@XAgent_official](https://x.com/XAgent_official).

Please include:

- A description of the vulnerability and its potential impact.
- Steps to reproduce, or a proof of concept.
- Any relevant versions, configuration, or environment details.

## What to expect

- **Acknowledgement** within 3 business days.
- **An assessment and remediation plan** communicated as we investigate.
- **Coordinated disclosure** — we will work with you on timing and will credit you (if you wish) once a fix is available.

## Scope

This repository is the public documentation for Xerness. If your report concerns the Xerness engine, adapters, or a downstream deployment, note that in your report and we will route it to the right team.

## Handling secrets

Xerness is designed so that **no credentials or user code leave your control**: workflows, standards, and memory live in your own Git repository, and LLM calls use your own API keys. If you believe a credential, key, or private endpoint has been exposed in this repository or in Xerness output, report it privately using the channels above.
