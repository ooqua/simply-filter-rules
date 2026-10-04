# simply-filter-rules

A hardened, regular-expression filter configuration for the [Simply Filter SMS](https://github.com/adibendahan/SimplyFilterSMS-iOS) app on iOS.

The ruleset automatically identifies and separates smishing links, package delivery lures, toll citations, and marketing clutter, while ensuring authentic 2FA codes, interactive bank fraud verifications, and personal texts remain in your Primary Inbox.

---

## How Message Routing Works

Apple's `IdentityLookup` framework does not permit third-party extensions to delete incoming messages. Instead, incoming SMS, MMS, and RCS messages from numbers not saved in your contacts are evaluated upon receipt and routed into dedicated system folders:

| Folder | Intended Traffic | Examples |
| :--- | :--- | :--- |
| **Primary Inbox** | Unmatched safe traffic | Conversations from real people, login 2FA passcodes, bank fraud alerts requiring an interactive `YES`/`NO` response. |
| **Transactions** | Automated utility alerts | Order receipts, tracking numbers, courier dispatches, airline boarding updates, rideshare and food delivery driver arrivals. |
| **Promotions** | Commercial bulk messaging | Discount codes, flash sales, apparel drops, storefront access codes, cart abandonments, political fundraising blasts. |
| **Junk** | Malicious & deceptive traffic | Toll citations, delivery address-update lures, fake bank callback notices, extortion, zero-width obfuscation, carrier gateway spam. |

---

## Requirements

* An iPhone running iOS 16.6 or later.
* [Simply Filter SMS](https://apps.apple.com/us/app/simply-filter-sms/id1603222959) installed from the App Store.

---

## System Configuration

Apple requires third-party SMS filters to be explicitly granted permission in system settings:

1. Open iPhone **Settings**.
2. Navigate to:
   * **iOS 18 and later:** **Apps** > **Messages** > **Unknown & Spam (or Unknown Senders)**
   * **iOS 17 and earlier:** **Messages** > **Unknown & Spam**
3. Ensure **Screen Unknown Senders (or Filter Unknown Senders)** is toggled **ON**.
4. Under the **Text Message Filter** (or **SMS Filtering**) section, select **Simply Filter SMS** and tap **Enable** when prompted.

---

## Installation

### Method 1: Direct Safari Download (Recommended)

1. Download the compiled ruleset file: [**simply.sfsfilters**](https://github.com/ooqua/simply-filter-rules/releases/latest/download/simply.sfsfilters)
2. Tap **Download** when prompted by Safari.
3. Open Safari's **Downloads** manager (the circle icon in the address bar) and select `simply.sfsfilters`.
4. Tap the iOS **Share** button (the square with an upward arrow) and select **Simply Filter SMS** from the share sheet.

> **Note:** If tapping the link displays raw JSON text instead of initiating a download, long-press the link in Safari and select **Download Linked File**.

### Method 2: Manual Import via Files

1. Download [**simply.sfsfilters**](https://github.com/ooqua/simply-filter-rules/releases/latest/download/simply.sfsfilters) and save it anywhere in your local **Files** app.
2. Open **Simply Filter SMS**.
3. Go to **Filter Tools** > **Import Filters** and select `simply.sfsfilters`.

---

## Upgrading to New Releases

*Simply Filter SMS* automatically skips exact duplicate rules when importing. However, if an existing rule was updated or modified in a newer release, the app does not replace the old rule—it appends the updated version alongside it, leaving obsolete rules active in your list.

To keep your ruleset clean and ensure outdated patterns are removed:

1. Open **Simply Filter SMS**.
2. Navigate to your custom rules.
3. Slide and delete existing rules (or use a bulk clear/delete option if available in your app version).
4. Import the updated `simply.sfsfilters` file.

---

## Required In-App Settings

To prevent local machine learning models or built-in heuristics from overriding your regular expressions, configure the app settings as follows:

| Setting | Recommended State | Technical Reason |
| :--- | :--- | :--- |
| **Automatic Filtering (AI)** | **OFF** | Prevents Apple's local natural-language heuristics from overriding deterministic regex rules. |
| **Smart Filters (All)** | **OFF** | The ruleset handles gateway senders (`@`), suspicious TLDs, and Unicode evasion natively. Running duplicate Smart Filters adds redundant overhead. |

---

## Filter Coverage & Threat Scope

The configuration targets three primary categories of incoming SMS traffic:

### 1. Fraud & Financial Exploitation (Junk)
* **Road Toll Smishing:** Impersonations of SunPass, E-ZPass, FasTrak, TxTag, Illinois Tollway, RiverLink, and The Toll Roads claiming unpaid tolls, citations, or registration holds.
* **Postal Delivery Phishing:** Spoofed USPS, UPS, FedEx, and DHL alerts alleging packages are held at warehouses due to incomplete address details or unpaid customs fees.
* **Vishing Callbacks:** Fake subscription renewals and security alerts (Geek Squad, Norton, McAfee, PayPal) demanding the recipient call a fraudulent call center.
* **Executive & Emergency Impersonation:** Boss-in-a-meeting gift card requests, stranded relative passport emergencies, and hitman/cartel extortion schemes.
* **Credential & Wallet Theft:** Fake 2FA harvesting ("send me the 6-digit code to verify you are real"), compromised crypto wallet alerts (MetaMask, Phantom, Ledger), and iCloud Find My phishing.

### 2. Commercial Outreach & Spam (Promotions)
* **Retail Marketing:** Promotional discount codes, BOGO alerts, and flash sales.
* **Hype Commerce:** Limited-edition streetwear drops, storefront access codes, and early access raffles.
* **Telemarketing & Lead Gen:** Solar panel incentives, cash home-buying offers, and autodialer consent disclosures.
* **Political Outreach:** Campaign fundraising solicitations, matching donation deadlines, PAC surveys, and WinRed/ActBlue links.

### 3. Technical Adversarial Evasion (Junk)
* **Intra-Word Unicode Insertion:** Detection of zero-width spaces (`\u200B`), soft hyphens (`\u00AD`), word joiners (`\u2060`), and byte-order marks (`\uFEFF`) hidden between alphanumeric characters to evade token matching.
* **Homoglyph Blending:** Detection of words mixing Latin with Cyrillic or Greek alphabets (e.g., `Αррlе`).
* **Obfuscated Infrastructure:** Punycode domains (`xn--`), raw IPv4 URLs, search-engine open redirects (`google.com/url?q=...`), and high-abuse top-level domains (`.top`, `.xyz`, `.shop`, `.site`, `.online`, `.cloud`, `.vip`).

---

## Operating System Architecture & Constraints

Understanding how Apple's `IdentityLookup` framework works prevents false assumptions about filter behavior:

### 1. The Contacts Exemption
Messages sent from contacts saved in your native iOS **Contacts** app are completely immune to filtering. Apple's baseband subsystem delivers contact messages straight to the Primary Inbox without running them through third-party filter extensions. A friend or family member will never be caught by these rules.

### 2. Folder Hierarchy & Stateless Evaluation
The filter evaluates incoming messages **statelessly, text by text**, following iOS folder hierarchy rules:
* **Junk is a One-Way Quarantine:** If any incoming text triggers a scam, smishing, or malicious rule, the conversation is locked in the **Junk** folder. No subsequent message from that number can move the thread back to your Inbox, Promotions, or Transactions.
* **Dynamic Non-Junk Routing:** Between your **Primary Inbox**, **Transactions**, and **Promotions**, the thread dynamically moves to whichever folder matches the *most recent* incoming text (e.g., a follow-up clean message can move a thread from Promotions back into your Primary Inbox).
* **The Replying Rule (Permanent Whitelist):** The moment you send an outbound reply (even typing *"STOP"* or *"Wrong number"*), iOS treats the sender as an active contact. Apple automatically exempts that number from third-party filtering, and all future messages will go straight to your Primary Inbox. Never reply to suspected smishing messages.

```
[Inbound SMS from Unknown Sender]
                │
                ▼
  [Simply Filter SMS Evaluation]
                │
      ┌─────────┴─────────┐
      │                   │
      ▼                   ▼
[Matches Scam]      [Clean Message]
      │                   │
      ▼                   ▼
 [Junk / Promo]    [Primary Inbox]
                          │
                  ┌───────┴───────┐
                  │               │
                  ▼               ▼
              [Ignored]       [Replied]
                  │               │
                  ▼               ▼
             Future texts    WHITELISTED
            still filtered   BY APPLE OS
                             (All future
                            texts bypass
                             filtering)
```

### 3. SMS/MMS/RCS vs. iMessage Scope
Filter extensions can only inspect cellular SMS, MMS, and RCS messages (green bubbles). Apple does not allow third-party code to intercept or inspect end-to-end encrypted iMessages (blue bubbles). Unsolicited iMessages must be reported using Apple's native *Report Junk* interface.

### 4. Local On-Device Execution
All matching runs locally on your device via the iOS regular expression engine. No text messages, phone numbers, metadata, or telemetry are ever sent to external servers.

---

## Troubleshooting & Common Edge Cases

### Safari displays raw text instead of downloading
Safari occasionally attempts to preview `.sfsfilters` files as plaintext. Long-press the download link and select **Download Linked File** from the context menu to force a file download.

### A message tests as "Junk" in the app, but wasn't filtered in Messages
If you pasted the text into **Filter Tools > Test Your Filters** and the app confirmed it should be Junk, but the real message landed in your Primary Inbox, check these iOS operating system overrides:
1. **Was the bubble Blue?** Blue bubbles are end-to-end encrypted **iMessages**. Apple does not permit third-party extensions to inspect iMessages; only green cellular bubbles (SMS/MMS/RCS) can be filtered. Report blue bubbles using Apple's native *Report Junk* button.
2. **Is the sender in your Contacts?** Senders saved in your iOS Contacts app automatically bypass all filtering rules.
3. **Did you previously reply to this number?** If you ever sent an outbound reply (even *"STOP"* or *"Who is this?"*), iOS permanently whitelisted the thread.
4. **Is the extension enabled in Settings?** Confirm that **Screen Unknown Senders** is toggled on under **Settings > Apps > Messages > Unknown & Spam**.

**If none of the above apply (iOS Filter Daemon Stalled):**  
Occasionally, iOS fails to wake up background filter extensions. To kickstart the process:
* Go to **Settings > Apps > Messages > Unknown & Spam**, set the filter to **None**, wait 5 seconds, and re-select **Simply Filter SMS**.
* Force-close the **Messages** app (swipe up from the App Switcher) or restart your iPhone. This forces iOS to re-register the background filtering extension.

### An interactive bank alert was routed to Junk
Legitimate card fraud alerts that offer interactive verification (*"Reply YES or NO"*) will route to your Primary Inbox. However, if an automated alert lacks a reply prompt and strictly orders you to call an unknown number, it mimics callback smishing (vishing) and may be flagged. Always verify unfamiliar bank notices through your financial institution's official app or the phone number printed on the back of your card.

### A conversational spam message slipped into the Primary Inbox
If an opening message contains no links, no suspicious top-level domains, and no commercial/fraud keywords (*"Hi, is this David?"*), it is an unclassified conversational hook. Because standard greetings share the exact lexical structure of legitimate messages from tradespeople, doctors, and couriers, blocking them via static regex would cause catastrophic false positives on everyday human communication. The filter intercepts the attack the moment a payload (URL, payment request, or task lure) is introduced.

### A legitimate delivery text was misclassified
Legitimate carrier notifications from USPS, UPS, FedEx, and DHL linking to official apex domains (`usps.com`, `ups.com`, `fedex.com`, `dhl.com`) are protected by domain lookaheads. However, third-party logistics aggregators linking to unknown tracking domains that combine delivery exception phrasing with non-carrier links may match the delivery smishing rule.

---

## Reporting Issues & Contributing

### 1. Legitimate Message Wrongly Blocked (False Positive)
If a real personal message, delivery update, or bank alert was routed to Junk or Promotions:
1. Copy the raw message text.
2. Redact personal information (names, personal phone numbers, account balances, addresses).
3. Open an issue on GitHub with the **redacted message**, the **folder it was routed to**, and **who sent it** (e.g., local clinic, utility company).

### 2. Scam or Spam Slipped Through (Missed Scam)
If an unsolicited smishing text, fake toll citation, or marketing blast reached your Primary Inbox:
1. Copy the raw message text.
2. Ensure the text contains an **actionable payload** (a link, suspicious phone number, brand impersonation, or payment demand).
3. Open an issue on GitHub with the **message body** and the **unredacted scam link or phone number** so a new pattern can be added.

> > **Note on Conversational Texts:** Do not report generic "wrong number" greetings (*"Hi, how are you?"*) that contain no links or scam keywords. They cannot be blocked via regex without blocking real people, and as explained in [Folder Hierarchy & Stateless Evaluation](#2-folder-hierarchy--stateless-evaluation), replying to them permanently whitelists the scammer in iOS. Simply delete the message.

### Pull Requests & Rule Submissions
Pull requests updating regular expressions must preserve bounded quantifiers (`[\s\S]{0,N}`) and include verification against both false positives and evasive variations.

---

## Disclaimer & License

This configuration is distributed under the [MIT License](LICENSE).

This ruleset is an independent community project and is not affiliated with, endorsed by, or associated with Apple Inc., Simply Filter SMS, the United States Postal Service, or any financial institution. Message filtering operates on deterministic pattern matching and cannot guarantee the interception of 100% of malicious outreach. Users remain responsible for verifying the authenticity of any unsolicited communications before interacting with links or sharing sensitive information.