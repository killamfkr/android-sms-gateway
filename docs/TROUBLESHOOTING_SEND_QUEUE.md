# Send queue stops after ~30 messages

## Symptoms

- Local (or cloud) mode sends fine for a while, then new messages stay **Pending**
  or fail with `RESULT_ERROR_LIMIT_EXCEEDED` in logs.
- Often happens around **30** messages when sending quickly with no delay between
  API calls.

## Causes

1. **Android SMS queue limit** — The OS only buffers a limited number of outgoing
   SMS submissions (commonly ~30). The gateway must wait for `SENT` callbacks
   before submitting more when you send in bursts.
2. **App send limit** — Settings → Messages → **Limits** may be set to e.g.
   **30** messages per **30 minutes**. When the limit is hit, sending pauses until
   the window resets (check app logs for `Send limit reached`).
3. **Random delay** — Settings → **Delay between messages** adds spacing; set min/max
   to `0` only if you accept modem queue throttling.

## What to do

1. Open **Settings → Messages → Limits**: set period to **Disabled** or raise the
   count if you configured a cap intentionally.
2. Add a small delay between messages (e.g. min `3`, max `10` seconds) for large
   batches.
3. Check **Logs** (local `/logs` API) for `RESULT_ERROR_LIMIT_EXCEEDED` or
   `Send limit reached`.
4. Use builds from vNext with send-queue throttling (waits for in-flight `Processed`
   recipients to clear before new submits).

## Carrier / OEM limits

Some manufacturers apply additional caps. If problems persist after the above,
spread traffic across SIMs/devices or use a commercial SMS provider.
