# ClankChat privacy

ClankChat collects no data. There are no accounts, no analytics, no tracking and no ads. In the App Store's terms:
Data Not Collected.

## What stays on your Mac

ClankChat for Mac runs the pi coding agent on your Mac and keeps everything it needs there: conversations, prompts,
answers, tool output, folder paths, the list of paired devices and the log (`~/.clankchat/data`,
`~/Library/Logs/ClankChat/daemon.log`). Pairing codes and device tokens are stored only as hashes.

Your iPhone talks to your Mac directly over your own Tailscale network. We run no server in that path and never see
your conversations. The iPhone keeps its pairing (the Mac's address and a device token) and a push key in its
Keychain. Face ID for the app lock is handled by iOS; ClankChat only gets "unlocked" or "not unlocked".

pi itself talks to the model providers you set up in pi, under their terms. ClankChat adds nothing to that.

## What the push gateway and Apple see

Notifications and Live Activity updates on a locked iPhone need Apple's push service (APNs). Your Mac sends each
push to our push gateway (a Cloudflare Worker), which signs it and passes it to Apple. Per push the gateway and Apple
see:

- the push token Apple gave your iPhone (for the notification or the Live Activity),
- your Mac's IP address (the gateway) and the time of the push,
- for a notification: its kind (done, failed, needs you), a generic text ("An agent finished.") and an encrypted
  blob with the real title and text. Only your iPhone has the key to open it. The blob names its key by your
  iPhone's random device id.
- for a Live Activity: neutral fields only: state, step count, the kind of tool running (for example "edit" or
  "bash"), timestamps, and for a question its random id and the number of options. Never a prompt, a title, a
  folder path, a tool title or any other agent text. Conversation and Mac ids are random ids.

The gateway stores nothing. It logs no payloads, tokens or IP addresses. When Apple rejects a push it writes one
line with the push type and Apple's reason (for example "Unregistered"); Workers Logs are off, so the line is not
stored. Its rate limits count pushes for one minute per hashed push token and per hashed IP address of the sender.
The IP address is used only for that counter: it is never stored or logged. Cloudflare handles the network traffic as
the gateway's host.

## Contact

Questions: use the support link on ClankChat's App Store page.
