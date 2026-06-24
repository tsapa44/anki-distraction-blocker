# Anki Distraction Blocker

Blocks distracting websites on macOS until you've done your daily Anki reviews.

Wake up and the sites you'd otherwise doomscroll are blocked. Open Anki, do your
cards, and they unblock the moment you hit your daily goal. A small lock in your
menu bar shows how close you are (`🔒 12/20`) and flips to `✅` once you're free for
the day.

It's a "do your reviews first" enforcer: the only real way back to your distractions
is to study - with a slow escape hatch for genuine emergencies.

## Features

- **Daily review goal.** Pick how many reviews you owe each day (default 20). Hit it and your sites unblock.
- **Your blocklist.** Block the exact sites that eat your time, and edit the list right from the menu bar.
- **Menu-bar progress.** `🔒 12/20` while you're short, `✅` once you're done, `⏳ 8m` while an emergency unlock counts down.
- **Can't lock yourself out.** If Anki has nothing left for you to study, you're done for the day even under your goal - so an impossible goal can never trap you.
- **Emergency unlock.** A deliberately slow (~15 min) release for real emergencies. Each use is logged so you can see how often you bail.
- **Survives restarts.** It runs as a background service - quitting apps or rebooting won't lift the block.
- **Edits are earned.** You can lower your goal or remove a site only after you've finished the day's reviews. Adding sites is always allowed.

## Requirements

- macOS
- [Anki](https://apps.ankiweb.net/) with the **AnkiConnect** add-on
- A few Terminal commands, once (there's no double-click installer yet)

## Install

**1. Install the AnkiConnect add-on.** In Anki: *Tools → Add-ons → Get Add-ons…*,
paste code **`2055492159`**, then restart Anki. Keep Anki running.

**2. Get the code:**
```bash
git clone https://github.com/tsapa44/anki-distraction-blocker.git
cd anki-distraction-blocker
```

**3. Install the blocker** (asks for your password, since it runs as a background service):
```bash
sudo scripts/install.sh
```

**4. Add the menu-bar app** (no password needed):
```bash
scripts/install-menubar.sh
```
Look for the lock in your menu bar.

**5. Turn off your browser's Secure DNS - don't skip this.** The block works by
editing `/etc/hosts`, which Chrome's and Firefox's "Secure DNS" quietly routes
around. If you skip this, blocked sites will still load:
- **Safari** - nothing to do, it already respects the block.
- **Chrome** - Settings → Privacy and security → Security → turn off **Use secure DNS**.
- **Firefox** - Settings → Privacy & Security → DNS over HTTPS → **Off**.

That's it. Set your goal and blocklist from the menu bar (below).

## Using it

Click the lock in your menu bar:

- **Add site…** - block another site (any time).
- **A site in the list** - stop blocking it (only after today's reviews are done).
- **Change daily quota…** - set your daily goal, 1-999 (also only once you're done; it takes effect tomorrow).
- **Emergency unlock** - lifts the block after ~15 minutes, for real emergencies.

Change a setting and the menu updates instantly; the actual block follows within a
few seconds.

## Heads-up

- **macOS only.**
- **It's a speed bump, not a vault.** It's built to beat a lazy moment, not a
  determined you - you have admin rights and could tear it out. The whole idea is
  that doing your reviews is easier than fighting it.
- **Secure DNS bypasses it** (see step 5). A VPN or an app using hardcoded IPs can too.
- **Your backlog can still grow.** It makes you show up daily; it won't force you to
  clear a giant pile in one sitting.

## Uninstall

```bash
sudo scripts/uninstall.sh
launchctl bootout gui/$(id -u) ~/Library/LaunchAgents/com.ankiblock.menubar.plist 2>/dev/null
rm -f ~/Library/LaunchAgents/com.ankiblock.menubar.plist
rm -rf ~/.ankiblock
```
