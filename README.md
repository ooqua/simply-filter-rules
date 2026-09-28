# simply-filter-rules

A comprehensive, hardened filter configuration for the [Simply Filter SMS](https://github.com/adibendahan/SimplyFilterSMS-iOS) app on iOS. It filters smishing links, delivery scams, toll citations, and marketing blasts while keeping legitimate 2FA codes, bank verification alerts, and personal texts functional.

## How Filtering Works

In iOS SMS filtering, messages are not deleted. Instead, they are kept out of your Primary Inbox and routed into their designated iOS folders:

- **Primary Inbox (Unmatched)**: Personal messages from friends, authentic two-factor authentication codes, and interactive bank verification alerts (YES/NO replies).
- **Transactions**: Legitimate order receipts, shipping updates, package tracking, rideshare/delivery driver arrivals, and flight alerts.
- **Promotions**: Retail discount codes, brand drops, abandoned cart reminders, and political campaign blasts.
- **Junk**: Malicious smishing, package fee scams, toll road fraud, vishing callbacks, extortion, and unsolicited spam.

## Prerequisites

1. Install **[Simply Filter SMS](https://apps.apple.com/us/app/simply-filter-sms/id1603222959)** from the App Store.
2. Enable it in iOS: Open iPhone **Settings** > **Messages** > **Unknown & Spam**, and toggle on **Simply Filter SMS**.

## Installation

### Direct iPhone Download (Recommended)

1. Tap to download: **[simply.sfsfilters](https://github.com/ooqua/simply-filter-rules/releases/download/v1.0.2/simply.sfsfilters)**
2. Tap **Download** when prompted by Safari.
3. Tap the **Downloads icon** in Safari's address bar and select `simply.sfsfilters`.
4. Tap the **Share** button (box with upward arrow) and select **Simply Filter SMS** from the app list.

> **Note:** If tapping the link displays raw text instead of downloading, long-press the link and select **Download Linked File**.

### Alternative: Import via Files or Computer

1. Download `simply.sfsfilters` and save it to your iPhone's **Files** app (via AirDrop, iCloud Drive, or cable).
2. Open Simply Filter SMS.
3. Navigate to **Filter Tools > Import Filters** and select `simply.sfsfilters`.

## App Settings

Configure these settings inside Simply Filter SMS to let the custom rules run cleanly:

1. **Automatic Filtering (AI)**: Set to **OFF**.
   - Prevents the on-device model from unpredictably overriding your explicit regex rules.
2. **Smart Filters**: Enable **Block Email Senders** only.
   - Keep all other Smart Filters (like links, unknown senders, or emojis) turned off, as this ruleset already handles malicious patterns directly.

## Filter Coverage

The ruleset is designed to catch virtually any unsolicited text that does not come from a real human:

- **Scams & Fraud**: Package delivery fee traps, highway tolls, DMV citations, fake court fines, bank account alerts, account recovery link theft, fake checks, and extortion threats.
- **Spam & Marketing**: Retail discount codes, hype streetwear drops, locked storefront passwords, solar telemarketing, and political campaign blasts.
- **Technical Evasion**: Zero-width invisible spaces, Punycode domains, raw IP hosts, and suspicious domain extensions.

Legitimate personal conversations, authentic two-factor authentication login codes, and interactive bank verification alerts are left untouched in your Primary Inbox.