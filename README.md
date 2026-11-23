# MDZ+ Reimagined: play in the browser

**Play:** https://darrenb276.github.io/MDZ-Reimagined-Web/

This is **MDZ+ Reimagined** (2.3.9-mp27c4) in your browser: the same game as the Android APK, the PC build and the [iOS app](https://github.com/DarrenB276/MDZ-Reimagined-iOS), file for file. It plays on the same multiplayer servers.

- Use Chrome, Edge, Firefox or Safari, on PC or phone. Keep the tab in front while you play: browsers pause hidden tabs.
- Your single-player saves stay in this browser. Saves > Back up all makes a .zip you can restore here or in another browser.
- After a new version comes out, reload the page once or twice. The game caches itself for offline play, and a server of the new version refuses the old one ("update the game").

## Multiplayer in the browser

A web page served over https can only open secure (https / wss) connections. Browsers block plain `http://` and `ws://` from it. So, in the browser:

- **Servers tab:** dedicated servers in public mode come with their https tunnel. A server listed by its `http://IP:port` is joined through the tunnel it registered. A server with no tunnel cannot be joined from the browser: use the APK, the PC build or the iOS app.
- **Rooms (P2P):** works.
- **Direct:** type the server's `https://…trycloudflare.com` address, not an IP.
- **Hosting:** use Online (P2P). The browser cannot open a LAN server.

## Why the game files must never be edited

Multiplayer servers run an anti-cheat that compares every player's game files with their own. If any file in `www/` differs from the release by even one byte, every player of this page is kicked or banned.

- `www/` is the released public build, byte for byte. [`www-manifest.json`](www-manifest.json) holds the size and SHA-256 of every file, and the deployment stops if any file differs.
- `.gitattributes` (`* -text`) stops git from changing line endings.

## How the site is built

[`.github/workflows/pages.yml`](.github/workflows/pages.yml) checks `www/` against the manifest, then publishes it to GitHub Pages unchanged.

## Updating to a new game version

1. Replace `www/` with the new public build's `www`, unchanged.
2. Make the manifest: `node tools/check-www.cjs make www www-manifest.json <version>`.
3. Put the version in `version.txt` and push.
