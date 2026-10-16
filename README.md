<div align="center">

<img width="100%" alt="header" src="https://capsule-render.vercel.app/api?type=waving&height=210&text=ATF%20Verification%20Bot&fontAlign=50&fontAlignY=36&fontSize=56&desc=Auto%20Verification%20%7C%20Captcha%20Solver%20%7C%20TON%20Address%20%7C%20Multi-Account&descAlign=50&descAlignY=58"/>

<img alt="typing" src="https://readme-typing-svg.demolab.com?font=Inter&size=18&duration=3000&pause=650&center=true&vCenter=true&width=900&lines=Auto+Verification+ATF+%7C+Above+500+ATF+Threshold;Built-in+Captcha+Solver+%7C+No+External+Service;TON+Wallet+Address+Converter+%7C+Raw+to+Friendly;Per-Account+Device+ID+Cached;Proxy+Support+%7C+Multi-Account"/>

<p>
  <img alt="ruby" src="https://img.shields.io/badge/Ruby-3.2+-CC342D?logo=ruby&logoColor=white"/>
  <img alt="platform" src="https://img.shields.io/badge/Platform-ATF%20Miners-111111"/>
  <img alt="multi-account" src="https://img.shields.io/badge/Multi--Account-Supported-111111"/>
  <img alt="proxy" src="https://img.shields.io/badge/Proxy-Supported-111111"/>
  <img alt="author" src="https://img.shields.io/badge/by-Yuurisandesu-111111"/>
</p>

<p>
  <b>ATF Verification Bot</b> automates account sign-in and identity verification using initData from <code>data.txt</code>.<br/>
  The bot retrieves account details, checks verification and entry challenge states, processes identity reviews until approval, maintains persistent device IDs, and supports multi-account automation with proxy rotation and automatic countdown cycles.<br/>
  Built and distributed by <b>Yuurisandesu</b>.
</p>

</div>

<hr/>

<p align="center">
  <img src="image/image.png" alt="preview" width="100%"/>
</p>

<hr/>

---

## Table of Contents

- [Requirements](#requirements)
- [Install Ruby](#install-ruby)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Bot](#running-the-bot)
- [Features](#features)
- [File Structure](#file-structure)
- [Disclaimer](#disclaimer)

---

## Requirements

- Ruby `3.2+` (for the downloader only)
- Git

---

## Install Ruby

Ruby is only needed to run the downloader script. Skip this section if you download the binary manually.

### Linux

```bash
sudo apt update && sudo apt install -y ruby git
```

### macOS

```bash
brew install ruby
```

### Windows

Download and run the installer from https://rubyinstaller.org. Check **Add Ruby executables to your PATH** during installation.

### Termux (Android)

```bash
pkg update && pkg install ruby git
```

---

## Installation

**Clone the repository:**

```bash
git clone https://github.com/Yuurisan-N1/Atf-Verification.git
cd Atf-Verification
```

**Download the binary:**

```bash
ruby bot.rb
```

The script shows a numbered menu:

```
1. ATF Linux ARM64
2. ATF Linux AMD64
3. Windows (PowerShell / CMD)
```

Enter the number for your platform. The binary downloads with a live progress bar and is set to executable automatically on Linux.

Or download manually from the Releases page:
https://github.com/Yuurisan-N1/Atf-Verification/releases/latest

| File | Platform |
|---|---|
| `atf-linux-amd64` | Linux x86_64 |
| `atf-linux-arm64` | Linux ARM64 |
| `atf.exe` | Windows x86_64 |

**Linux after manual download:**

```bash
chmod +x atf-linux-amd64
```

---

## Configuration

### 1. Accounts (data.txt)

Fill `data.txt` with Telegram WebApp `initData` for each account, one per line:

```
user=%7B%22id%22...&hash=abc123
user=%7B%22id%22...&hash=def456
```

> `initData` can be obtained from the browser DevTools when opening ATF Miners on Telegram Web.

### 2. Proxy (proxy.txt)

Fill `proxy.txt` with proxies, one per line (optional, leave empty to run without proxy):

```
host:port
host:port:user:pass
http://user:pass@host:port
```

Proxies are assigned to accounts by index in round-robin order.

### 3. Bot Settings (config.json)

`sleep_seconds` controls how many seconds the bot waits between cycles. If `config.json` is missing, the bot falls back to a default of `3600` seconds.


---

## Features

### InitData Sign In

Every account signs in with the initData line read from `data.txt`, so no browser session is needed. The login answer carries the wallet address, the mined balance and the Telegram handle of the account, and the session token handed back by the server is reused for every following call of that account.

### Verification State Read

The verification state of the account is read from the login answer, so the bot knows whether the account is flagged for an identity review or already cleared before it sends any other request.

### Entry Challenge Check

The entry challenge of the account is requested from the server and the public challenge state is read next to it, so the bot always knows whether a browser challenge is armed for that account before the review runs. When the challenge is armed it is answered on the spot, and when it is not the review runs straight away.

### Identity Review Pass

The identity review of a flagged account is walked in four steps. An exchange account is registered for the account first, then the exchange user id that comes back is written to the local registry, then that user id is sent to the review endpoint as the identity reference of the account, and finally the review answer is polled until the server answers approved. No video, no photo and no document is involved in the pass, and the account leaves the cycle cleared for payout.

### Multi Account

All accounts in `data.txt` are processed one by one, and each account keeps its own persistent device id, so a rerun is treated as the same device by the server.

### Auto Countdown

After every account has been swept the bot shows a live countdown line and starts the next cycle once it runs out, driven by `sleep_seconds` from the config.

### Proxy Support

Proxies are loaded from `proxy.txt`, one per line, and each account takes the next line in turn. Credentials are never printed, the log only shows a masked form, and an empty file runs the bot direct.

### Auto Countdown
After all accounts complete a cycle, the bot displays a live `HH:MM:SS` countdown until the next cycle starts.

---

## File Structure

```text
Atf-Verification/
├── atf-linux-amd64      # Linux x86_64 binary
├── atf-linux-arm64      # Linux ARM64 binary
├── atf.exe              # Windows binary
├── bot.rb               # Interactive downloader script (Ruby)
├── config.json          # Sleep duration between cycles
├── data.txt             # Account initData, one per line
├── proxy.txt            # Proxy list, one per line (optional)
├── .data.log            # Per-account device ID cache (auto-generated)
├── LICENSE              # License file
└── utils/
    └── banner.rb        # Banner using yuurisan module
```

---

## Disclaimer

This tool is built for educational and technical exploration purposes. Use it wisely and at your own responsibility.

---

<div align="center">
<img width="100%" alt="footer" src="https://capsule-render.vercel.app/api?type=waving&height=120&section=footer"/>
</div>