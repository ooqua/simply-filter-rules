![simply-filter-rules](https://github.com/ooqua/simply-filter-rules/releases/download/v2.0.0/simply-filter-rules.png)

Simply Filter Rules sorts incoming unknown SMS, MMS, and RCS messages into Apple's four native categories (Primary, Transactions, Promotions, and Junk) so scams and spam get filtered out while personal texts, codes, and real alerts stay where you'll see them.

Available directly inside Simply Filter SMS 4.0+ as a featured Community List, with automatic updates.

---

## Key Features

* Scams get caught whether they have a link, a phone number, an email address, or just a "Reply Y" prompt.
* Real texts come first. A real bank alert, 2FA code, or appointment reminder should never end up in Junk. If a message is unclear it stays in Primary, and if it's only borderline it goes to Promotions.
* It also catches the sneaky stuff: lookalike letters from other alphabets, dotted or spaced-out website names, a brand name with the wrong website, and `@` handle senders.
* It's light and fast, and it's been tested with very long and oddly formatted messages.
* Everything runs on your phone. No server, no data collection.
* It updates on its own. As a Community List, a new release replaces the old rules automatically, with no re-importing and no duplicates.

---

## Requirements

* iPhone running **iOS 16.6** or later.
* [Simply Filter SMS](https://apps.apple.com/us/app/simply-filter-sms/id1603222959) (v4.0+) installed from the App Store.

---

## Setup

### 1. Enable System Permissions
1. Open **Settings** on your iPhone.
2. Go to **Apps → Messages → Unknown & Spam** (or **Messages → Unknown & Spam** on iOS 17 and earlier).
3. Turn **Screen Unknown Senders** **ON**.
4. Under **Text Message Filter**, select **Simply Filter SMS** and tap **Enable**.

### 2. Turn On The List
1. Open **Simply Filter SMS**.
2. Under **Your Lists**, tap **Community Lists**.
3. Under **Featured**, turn on **Simply Filter Rules**.

That's it. Nothing to download or import.

---

## Where Messages Go

| Folder | What Goes Here | Examples |
| :--- | :--- | :--- |
| **Primary** | Personal texts, codes, and anything unclear. | People you know, 2FA codes, bank fraud checks ("Reply YES or NO"), emergency and civic alerts, official election and government notices. |
| **Transactions** | Automated account and order records. | Shipping updates, order confirmations, appointment reminders, receipts. |
| **Promotions** | Marketing, sales, and borderline texts. | Store discounts, coupon codes, flash sales, loyalty alerts, political fundraising and campaign blasts, "wrong number" openers. |
| **Junk** | Scams, phishing, and spam. | Fake bank alerts, toll and delivery fee threats, IRS and jury-duty scares, prize and gift-card giveaways, crypto pitches, fake job offers, tech-support scares, reply-trap texts, and political-looking messages that are really payment or donation scams. |

Political texts are handled by what they do, not by who sends them. A campaign asking for donations is marketing, so it goes to Promotions. A real notice about voting or an election stays in Primary. A fake "final notice" that pushes you to pay through a strange link is a scam and goes to Junk. Texts that are just a person talking to you stay in Primary like any other conversation.

---

## How It Was Tested

Over the life of this project I've tested well over 100,000 messages, a mix of real-looking legit texts (bank alerts, deliveries, promos, personal texts) and scams. For the current rules, a tracked set of more than 5,000 unique messages (about 2,750 legit and 2,400 scams) is re-run on every change, plus all-caps, extra-spaces, and tacked-on-link versions of them, which is over 25,000 checks. Only 4 of the legit ones ended up in Junk, and I looked at each one. On a fresh set the rules had never seen, none of about 580 legit texts went to Junk, and about 96% of one 160-scam set was caught.

A harder set of 262 scams was caught about 70% of the time. It's full of short lures with no link, like "Hi, is this David?", which are kept out of Junk on purpose so real people don't get hidden. Normal link-based scams do better than that. Nothing here is a guarantee, and a brand new scam with no link, or one with an ordinary-looking link, can still get through.

The whole list uses about 15 MB of memory and checks a normal message in a few milliseconds. Anything that could cause heavy backtracking is capped. It runs fine on an iPhone 12.

---

## iOS Rules & Limits

* **Contacts always pass.** Anyone saved in your Contacts goes straight to Primary. Apple bypasses third-party filtering for known contacts.
* **Messages are sorted, not deleted.** Apple's `IdentityLookup` framework doesn't let third-party extensions delete texts, only route them into folders.
* **iMessage (blue bubbles) can't be filtered.** Only SMS, MMS, and RCS can be checked by third-party apps. Use **Report Junk** for unwanted iMessages. This is an Apple limit, not something specific to this list.
* **Don't reply to spam.** Replying tells iOS you know the sender, and future messages from that number will bypass the filter.
* **Junk sticks; everything else goes by the latest message.** Once a conversation lands in Junk it stays there until you delete it. That's why the rules lean away from Junk when a message is unclear. Promotions, Transactions, and Primary are decided by the most recent message.
* **Notifications for Promotions and Transactions are off by default.** With unknown-sender screening on, iOS hides notifications for most screened messages, so a delivery or rideshare text that lands in Transactions may not buzz you. On newer iOS versions (iOS 26 and later, in supported countries) you can change this under Settings → Apps → Messages → Unknown Senders → Allow Notifications. Pick which types you want (Time Sensitive, Personal, Transactions, Promotions).

---

## Known Limits

No text filter is perfect, and this one is built to fail on the safe side. A few things to expect:

* Short openers like "Hi, is this David?" and "Hi Mom, this is my new number" stay in Primary. They have no link, payment ask, or other signal, and real people send them too. Once a payment, link, or callback shows up, the message is judged on that.
* A bank alert that only says "if this wasn't you, call 1-8xx-..." stays in Primary even though scammers copy that format, because real banks use it. Always check with your bank's app or the number on your card.
* Something in Primary isn't automatically safe. New scam wording and fresh domains will get through until the rules catch up.
* The rules are written for U.S. messages and phrasing. Other countries will see weaker results.
* The rules only see the sender and the message text. They can't check whether a sender is spoofed.

---

## Troubleshooting

**A message wasn't filtered.** Check whether it's a blue-bubble iMessage, from a saved contact, or from a sender you've replied to. All three bypass the filter. Also confirm Screen Unknown Senders and Simply Filter SMS are both on under Settings → Apps → Messages → Unknown & Spam. If none of that's it, turn the filter off, wait a few seconds, and turn it back on, or restart your phone.
**A bank alert went to Junk.** Real "Reply YES or NO" alerts should land in Primary. Alerts that tell you to call an unknown number or follow an unfamiliar link can get flagged. Verify through your bank's official app. If it was a real alert, please report it (see below).
**A spam message reached Primary.** Some messages don't carry enough on their own yet. The filter waits for a link, a payment request, or another clear signal.
**A delivery message was misclassified.** USPS, UPS, FedEx, and DHL messages using their official sites go through. Unfamiliar tracking links can still get flagged if they look like scams.

---

## Reporting Issues & Contributing

Reports are public on GitHub, so please remove anything personal first: names, phone numbers, account or card digits, and any link with your details in it. Replace them with placeholders like `[NAME]` or `555-0100`. The wording and the sender style matter, not who you are.

* **A real message got blocked:** open an issue with the message (cleaned up) and which folder it landed in.
* **A scam got through:** open an issue with the message and what made it a scam, such as the link, callback number, or prompt.

Updates are published as tagged releases with notes on what changed. Every change is re-run against the full test set before release, so please include the exact message text when you report a miss.

Simple messages like "Hey, how are you?" aren't worth reporting unless they show clear scam signs.

---

## License

This project is open-source under the [MIT License](LICENSE). No filter catches everything. Be careful with unknown messages, and don't click links or share personal information unless you're sure a message is safe.