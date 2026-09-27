# simply-filter-rules

A filter configuration for the [Simply Filter SMS](https://github.com/adibendahan/SimplyFilterSMS-iOS) app on iOS. It filters phishing links, delivery scams, toll citations, and vishing callbacks while keeping legitimate 2FA codes and bank verification alerts functional.

## How Filtering Works

In Simply Filter SMS, "Blocked" does not mean messages are thrown away into Junk. It means the message is **blocked from cluttering your Primary Inbox** and routed into its appropriate iOS folder:

- **Primary Inbox (Allowed / Unmatched)**: Personal messages from friends, legitimate 2FA verification codes, bank fraud alerts (YES/NO replies), and your custom whitelist entries.
- **Transactions**: Legitimate order receipts, shipping notifications, package tracking, and food delivery arrivals.
- **Promotions**: Retail discount codes, sales reminders, and marketing campaigns.
- **Junk**: Malicious smishing, package fee scams, toll road fraud, vishing callbacks, and unsolicited spam.

## Prerequisites

1. Install **[Simply Filter SMS](https://apps.apple.com/us/app/simply-filter-sms/id1603222959)** from the App Store.
2. Enable it in iOS: Open your iPhone **Settings** > **Messages** > **Unknown & Spam**, and toggle on **Simply Filter SMS**.

## Installation

### Fastest Method (Direct iPhone Download)

1. Tap to download: **[simply.sfsfilters](https://github.com/ooqua/simply-filter-rules/releases/download/v1.0.1/simply.sfsfilters)**
2. Tap **Download** when prompted by Safari or Chrome.
3. Tap the **Downloads icon** (down arrow in Safari's address bar) and select `simply.sfsfilters`.
4. Tap the **Share** button (box with upward arrow) and select **Simply Filter SMS** from the app list.

> **Note:** If tapping the link displays text on your screen instead of downloading, press and hold (long-press) the link and select **Download Linked File**.

### Option 2: Download on PC and transfer to iPhone

1. Download `simply.sfsfilters` to your computer.
2. Transfer the file to your iPhone using one of the following methods:
   - **LocalSend**: Open LocalSend on your PC and iPhone. Send `simply.sfsfilters` and save it to the Files app.
   - **AirDrop**: If using macOS, share `simply.sfsfilters` directly to your device.
   - **USB Cable**: Transfer via Finder (macOS) or iTunes File Sharing (Windows) directly into the Files directory.
   - **Cloud Storage**: Upload to iCloud Drive or Google Drive, then open it in the Files app.
3. Open Simply Filter SMS.
4. Open the menu and navigate to **Filter Tools > Import Filters**.
5. Select `simply.sfsfilters`.

## App Settings

To prevent false positives and let the custom rules run cleanly, configure these settings in Simply Filter SMS:

1. **Automatic Filtering (AI)**: Set to **OFF**.
   - Disabling AI filtering ensures only your explicit regex rules make decisions, preventing the on-device model from unpredictably blocking legitimate messages.
2. **Smart Filters**: Enable **only** the toggle for **Block Email Senders** (blocks text messages sent from email addresses).
   - Keep all other Smart Filters (like links, unknown senders, or emojis) turned off, as this ruleset already handles malicious patterns directly.

## Configuration

Before or after importing, you can edit the whitelist placeholders at the top of `simply.sfsfilters` in any text editor or inside the Simply Filter SMS app:

| Placeholder | Purpose | Example |
| :--- | :--- | :--- |
| `REPLACE_WITH_YOUR_NAME` | Whitelists your personal name or a specific keyword | `Alex` |
| `REPLACE_WITH_YOUR_HOSPITAL_OR_CLINIC` | Whitelists your primary clinic, doctor, or dentist | `Evergreen Medical Group` |
| `REPLACE_WITH_YOUR_EMPLOYER` | Whitelists company interview updates | `Five Guys` |

> **Note:** Do not use square brackets `[ ]` inside regex fields, as regex parses brackets as character classes.

## Filter Coverage

- **Anti-Evasion & Obfuscation Defense**: Traps zero-width invisible spaces (`\u200B`), soft hyphens, mixed Latin-Cyrillic homoglyphs, and Punycode (`xn--`) domain spoofing.
- **Brand Spoofing & Phishing URLs**: Flags lookalike domains (`usps-tracking.*`, `chase-verify.*`), deceptive subdomains (`chase.com-auth.*`), IP address hosts, and abused TLDs (`.top`, `.xyz`, `.icu`, `.cfd`, `.sbs`, `.zip`, etc.). Generic `.app` and `.us` domains are excluded from blanket bans to prevent breaking legitimate services like Cash App or Zoom.
- **Fake Invoices & Callbacks**: Catches tech support and billing scams (Geek Squad, Norton, PayPal) that instruct the recipient to call phone numbers to dispute or cancel charges.
- **Tolls & DMV Citations**: Flags toll enforcement spam (SunPass, FasTrak, E-ZPass, E-PASS) and fake DMV license suspension notices.
- **Task & Job Scams**: Catches recruitment scams (hotel reviews, app ratings, merchant optimization, weekly/monthly remote wages) and requests to move conversations to Telegram or WhatsApp.
- **Two-Factor Authentication**: Specifically targets social engineering attacks where someone demands that you send them a 6-digit code. Automated login codes containing disclaimers like "never share this code" are left alone.
- **Bank Fraud Verification**: Does not block standard shortcode alerts asking for an interactive "YES" or "NO" reply.
- **Political Spam**: Catches manipulative campaign blasts (`Save America`, `Grassroots`, `FEC deadline`, `PAC match`).