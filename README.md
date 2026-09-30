# CrossClip

Take a screenshot on Android, press **Cmd+V** on your Mac. Nothing else in
between.

No cloud, no account, no manual send. The phone sends the screenshot straight to
a listener on your Mac over your local Wi-Fi, and it lands on the system
clipboard. Nothing leaves your network.

- Universal build — native on Apple silicon and Intel
- Requires macOS 12 Monterey or later
- Android 8.0 or later

---

## Install (Mac)

Paste this into Terminal. It has to be `curl` rather than a browser download —
browsers attach a quarantine flag that makes macOS refuse to open apps that
aren't from the App Store.

```bash
cd ~/Downloads && \
curl -fLO https://github.com/tarunkumarreddy299/crossclip/releases/download/v2.1/CrossClip-mac-2.1.zip && \
unzip -oq CrossClip-mac-2.1.zip -d CrossClip-mac && \
rm -rf /Applications/CrossClip.app && \
cp -R CrossClip-mac/CrossClip.app /Applications/ && \
open /Applications/CrossClip.app
```

Verify what you downloaded, if you like:

```bash
shasum -a 256 ~/Downloads/CrossClip-mac-2.1.zip
# 3a4c649da22971c6ebcd364c2160e347697d0f572af4fe4b06a1c4388e6a959a
```

A welcome dialog confirms it started. CrossClip then lives in the **menu bar** as
a small clipboard icon near the clock — there is no Dock icon and no window.

> If you downloaded the zip in a browser instead and macOS says the app cannot be
> verified, clear the flag and open it again:
> `xattr -d com.apple.quarantine /Applications/CrossClip.app`

## Install (Android)

Download `CrossClip-2.1.apk` from the release and open it. You will need to allow
installing apps from whatever you downloaded it with.

## Pair them

1. Mac: click the menu bar icon → **Connect**.
2. Mac: click the icon → **Pair a phone…** to show a QR code.
3. Phone: open CrossClip → **Scan pairing code** → point at the QR.
4. Phone: tap the big power button. It turns green and reads *Connected*.

Take a screenshot. Press Cmd+V on the Mac.

Both devices must be on the same Wi-Fi.

## Keeping a copy of every screenshot

Off by default. Tick **Save copies to CrossClip Share** in the menu bar and every
screenshot that lands on the clipboard is also written to
`~/Downloads/CrossClip Share`. The folder is created the first time you enable
it. Copies are the original bytes as the phone sent them, and an existing file is
never overwritten — a repeated name becomes `… 2.png`, `… 3.png`.

## Updating

CrossClip does not check for updates. Re-run the install command above; it
replaces the app in place. If the listener is already running, use this instead,
because replacing the app alone leaves the old background copy running:

```bash
cd ~/Downloads && \
curl -fLO https://github.com/tarunkumarreddy299/crossclip/releases/download/v2.1/CrossClip-mac-2.1.zip && \
unzip -oq CrossClip-mac-2.1.zip -d CrossClip-mac && \
osascript -e 'quit app "CrossClip"' 2>/dev/null; \
rm -rf /Applications/CrossClip.app && \
cp -R CrossClip-mac/CrossClip.app /Applications/ && \
/Applications/CrossClip.app/Contents/MacOS/CrossClip --connect && \
open /Applications/CrossClip.app
```

On Android, download the new APK and open it — it installs as an update and your
pairing survives.

## Notes

- The listener keeps running after you quit the menu bar app. It stops only when
  you click **Stop**, and stays stopped across restarts until you Connect again.
- The pairing QR contains your Mac's secret key. Anyone who scans it can put
  images on your clipboard, so don't post it in a group chat.
- Each Mac generates its own key on first run, so several people can run
  CrossClip on the same network without their phones crossing over.
- Traffic is authenticated but not encrypted. It never leaves your local
  network, but treat it as you would anything else on a shared Wi-Fi.
- The app is ad-hoc signed and not notarised, which is why the install uses
  `curl`. Nothing is collected, and there is no network access beyond your LAN.
- Log, if something misbehaves: `~/Library/Logs/CrossClip/listener.log`

## Troubleshooting

**"Could not reach…" on the phone.** Almost always the phone is on mobile data
rather than Wi-Fi, or the Mac's address changed. Check Wi-Fi first, then
Settings → **Find my Mac** in the app. If your network blocks mDNS, set the
Mac's IP by hand under Settings → Advanced.

**Nothing in the menu bar.** The icon may be pushed out of sight by a crowded
menu bar — hold Cmd and drag icons to rearrange.
