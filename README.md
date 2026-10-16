# noise lab

Noise Lab v0.1 by zkSI. Hide one SI request among decoys, pad every request to a fixed size, give them random timing, then commit and audit the batch. Everything runs in your browser.

Try it: https://zksi.io/lab/

Status: v0.1 · local only. The lab runs today in your browser; the network comes next.

## What it does

- You write a request. The lab hides it among 1 to 19 decoys (default 9).
- Every request goes into a JSON envelope `{v,c,p,z}` and is padded to one of five size classes: 256, 512, 1024, 2048 or 4096 bytes, exact to the byte.
- Arrival times are random, with exponential gaps over about one minute.
- Commit: SHA-256 over the sorted decoy hashes.
- Audit: opens about 1% of the decoys (at least one), picked from `hash(commit + beacon)`, and checks them against the commit. The beacon stands in for a future block hash.
- Two views: what the SI provider sees (a shuffled list; guess which one is yours, then reveal) and what the network sees (only time and size).
- Download `batch.json`: what a client would send, with no marker for the real request.

## Limits of v0.1

- Nothing is sent to any SI model or provider. Nothing leaves your browser, so the lab does not protect a real request yet. It shows how the mechanism works.
- No miners, gateways, mixnet, stake, payouts or contracts. Those come next.
- Decoys come from fixed templates (about 25 families). A trained detector could likely tell many of them apart from real requests.
- The audit only checks hashes against the commit. The beacon and the timing are simulated.

## No network

`lab.js` makes no network requests after the page loads: no `fetch`, no analytics, no third-party scripts. The page also sets a Content-Security-Policy with `connect-src 'none'`, so the browser blocks any attempt to connect anywhere.

To check it yourself: open DevTools, go to the Network tab, use the lab and watch. After the page and its fonts load, nothing else appears.

## Run locally

```
python3 -m http.server 8000
```

Then open http://localhost:8000. Opening `index.html` straight from disk may block the script because of the CSP, so use a local server.

## Files

```
index.html   page and styles
lab.js       all the logic: envelope, padding, decoys, timing, commit, audit
assets/      zkSI logo (SVG)
fonts/       Inter, JetBrains Mono and IBM Plex Mono, self-hosted
```

The same files are served at https://zksi.io/lab/.

## License

Code: MIT, see [LICENSE](LICENSE).
Fonts: SIL Open Font License 1.1, see `fonts/OFL-Inter.txt`, `fonts/OFL-JetBrainsMono.txt` and `fonts/OFL-IBMPlexMono.txt`.
The zkSI name and logo are not covered by the MIT license.

## Links

Site https://zksi.io · Docs https://zksi.io/docs/ · X https://x.com/0x_zkSI · Telegram https://t.me/zkSI0x · Email contact@zksi.io

No one from zkSI will ever ask you for keys, seed phrases or money. There is no token.
