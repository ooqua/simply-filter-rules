# simply-filter-rules

A regular-expression filter configuration for [Simply Filter SMS](https://github.com/adibendahan/SimplyFilterSMS-iOS) on iOS.

The ruleset automatically identifies almost everything that isn't human while making sure personal texts remain in your Primary Inbox.

---

## How Message Routing Works

Apple's `IdentityLookup` framework does not permit third-party extensions to delete incoming messages. Instead, incoming SMS, MMS, and RCS messages from numbers not saved in your contacts are evaluated upon receipt and routed into dedicated system folders.

### Folder Examples

* Fake toll texts, fake delivery texts, weird letters, and other scam texts go to **Junk**.
* Sales, deals, new clothes, and cart reminders go to **Promotions**.
* Receipts, tracking, and delivery texts go to **Transactions**.
* Texts from people you know, codes, and bank alerts stay in your **Primary Inbox**.

**Pretty self-explanatory.**

## Requirements

* An iPhone running iOS 16.6 or later.
* [Simply Filter SMS](https://apps.apple.com/us/app/simply-filter-sms/id1603222959) installed from the App Store.

## System Configuration

Apple requires third-party SMS filters to be explicitly granted permission in system settings.

1. Open **Settings** on your iPhone.
2. Go to:
   * **iOS 18 and later:** Settings → Apps → Messages → Unknown & Spam
   * **iOS 17 and earlier:** Settings → Messages → Unknown & Spam
3. Make sure **Screen Unknown Senders** (or **Filter Unknown Senders**) is turned **ON**.
4. Under **Text Message Filter** (or **SMS Filtering**), select **Simply Filter SMS** and tap **Enable** when asked.

## Installation

### 1. Download and Import

Download the [**simply.sfsfilters**](https://github.com/ooqua/simply-filter-rules/releases/latest/download/simply.sfsfilters) file.

* **Safari:** Tap **Download** when asked.
* **Files:** Save the file in the Files app.

After downloading, import the file into Simply Filter SMS.

* **Safari:** Tap the **Share** button and select **Simply Filter SMS**.
* **Files:** Open Simply Filter SMS → **Filter Tools** → **Import Filters** → select `simply.sfsfilters`.

## Upgrading to New Releases

**Simply Filter SMS** automatically skips duplicate rules when importing. If an existing rule is changed, the app will not replace the old rule and will add the updated rule to the list.

So to remove the old rules:

1. Open **Simply Filter SMS**.
2. Go to your custom rules.
3. Tap and Slide to delete existing rules.
4. Import the updated [**simply.sfsfilters**](https://github.com/ooqua/simply-filter-rules/releases/latest/download/simply.sfsfilters) file.


## Required In-App Settings

Turn **Automatic Filtering (AI)** OFF and **Smart Filters (All)** OFF. The rules handle the filtering, including @ senders, unsafe websites, and weird letters.

---

## Operating System Architecture & Constraints

### 1. The Contacts Exemption

Messages from people saved in your Contacts bypass third-party filtering and go to the **Primary Inbox**.

### 2. Folder Behavior

* **Junk:** Once a conversation goes to Junk, future messages from that sender will stay there.
* **Deleting the conversation:** If you delete the conversation, a new message from the sender will be checked again as a new conversation.
* **Promotions, Transactions, and Primary Inbox:** These folders are based on the most recent message.

**Important:** Do not reply to scam or smishing messages. If you reply, future messages from that number will bypass the filter.

### 3. SMS, MMS, and RCS vs. iMessage

* **SMS, MMS, and RCS:** These messages can be checked by third-party filters.
* **iMessage:** Blue-bubble messages cannot be checked by third-party filters. Use **Report Junk** for unwanted iMessages.

### 4. On-Device Filtering

The ruleset runs on your device. Messages are checked on your device and are not sent to an external server.

---

## Troubleshooting & Common Issues

### A Message Was Not Filtered

If the app says a message should be Junk but it went to your Primary Inbox, check:

* **Blue bubble:** The message is an iMessage and cannot be checked by the filter.
* **Saved contact:** Messages from contacts bypass filtering.
* **You replied:** Future messages from that sender bypass filtering.
* **Filter settings:** Make sure **Screen Unknown Senders** and **Simply Filter SMS** are turned ON under Settings → Apps → Messages → Unknown & Spam.

If none of these apply, turn the filter OFF, wait a few seconds, then turn **Simply Filter SMS** back ON. You can also close Messages or restart your iPhone.

### A Bank Alert Was Sent to Junk

Bank alerts with **“Reply YES or NO”** should go to your Primary Inbox. Alerts that only tell you to call an unknown number might be flagged as scams. Always check bank alerts through your bank's official app or the phone number on the back of your card.

### A Spam Message Reached the Primary Inbox

Messages like **“Hi, is this David?”** might not have enough information to identify them as scams. The filter waits for things like links, payment requests, or other scam patterns.

### A Real Delivery Message Was Misclassified

Messages from USPS, UPS, FedEx, and DHL using their official websites are allowed through. Delivery messages using unknown tracking websites might still be flagged if they look like scams.

---

## Reporting Issues & Contributing

### Legitimate Message Was Blocked

If a real message went to Junk or Promotions:

1. Copy the message.
2. Remove personal information like names, phone numbers, addresses, and account details.
3. Open a GitHub issue with the message, the folder it went to, and who sent it.

### Scam Message Was Not Blocked

If a scam message reached your Primary Inbox:

1. Copy the message.
2. Make sure it has something like a link, phone number, fake brand, or payment request.
3. Open a GitHub issue with the message and the scam link or phone number.

> **Note:** Do not report simple messages like **“Hey, how are you?”** unless they show clear signs of a scam. Blocking every simple greeting would also block messages from real people.

---

## Disclaimer & License

Please know that these rules will not catch every scam out there. Be careful with unknown messages; don't click links or share personal information unless you know the message is absolutely safe. This project uses the [MIT License](LICENSE).

