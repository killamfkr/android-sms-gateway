# RCS (Rich Communication Services)

SMS Gateway sends and receives traffic through the **SMS/MMS** stack only. It does **not**
implement RCS (Google Chat features, blue bubbles, RCS read receipts, etc.).

## Sending

API requests (`textMessage`, `mmsMessage`, `dataMessage`) are delivered as **SMS**, **MMS**, or
**binary SMS**. There is no supported way for a third-party Android app to send RCS chat messages:
Google's RCS transport is not exposed through a public SDK, and the hidden Google Messages
`EXTERNAL_MESSAGING_API` is restricted to an OEM allowlist.

Recipients who normally chat with you over RCS will still get your API messages as **SMS/MMS**
(green bubble / carrier MMS), not as RCS.

## Receiving and webhooks

RCS inbound messages **do not** trigger `SMS_RECEIVED` / WAP push broadcasts or the Telephony
provider entries this app uses. If a sender uses RCS, your device may show the message in Google
Messages, but **no `sms:received` / `mms:received` webhook** will fire from SMS Gateway.

### Workaround

On the **phone that runs SMS Gateway** (for inbound webhooks) and on any test handsets where you
need classic SMS/MMS behavior:

1. Open **Google Messages** (or your carrier messaging app).
2. Turn off **RCS chats** / **Chat features** (exact menu varies by OEM).
3. Reboot the device and restart SMS Gateway.

After RCS is disabled, compatible messages arrive as SMS/MMS and are handled normally.

## Related

- [Official FAQ: RCS support](https://docs.sms-gate.app/faq/general/#does-the-app-support-rcs-messaging)
- [MMS sending notes](./MMS.md)
- GitHub discussion: [issue #295](https://github.com/capcom6/android-sms-gateway/issues/295),
  [issue #450](https://github.com/capcom6/android-sms-gateway/issues/450)
