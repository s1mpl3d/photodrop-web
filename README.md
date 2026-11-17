# PhotoDrop

Send photos, files and text between your phone and computer by scanning a QR code. No app, no account, no upload to a server.

Live: https://photodrop-web.vercel.app

## How it works

1. Open the site on your computer. It shows a QR code.
2. Scan it with your phone's camera. The two devices connect directly over WebRTC.
3. Check that both screens show the same 4-digit code.
4. Send photos either way. Received photos can be saved straight to Photos on iPhone.

## Security

- **End-to-end encrypted.** Every message is encrypted with AES-256-GCM. The key is created on the computer and travels only in the QR link's `#fragment`, which browsers never send to servers.
- **Bound messages.** Each chunk is authenticated with its sender role, transfer id and index, so chunks can't be reflected, reordered, swapped, replayed or dropped without detection.
- **One peer per session.** Only the first device to connect is accepted.
- **Metadata removal.** By default, GPS location, camera details and other EXIF/XMP data are stripped from JPEG and PNG without re-encoding (orientation is kept). HEIC/WebP are re-encoded.
- **No third-party scripts at runtime.** PeerJS and qrcode-generator are vendored (see `vendor/SHA256SUMS`), and a strict Content Security Policy is set in `vercel.json`.

The PeerJS public broker is used only to exchange connection info. It never sees the key or your files. Connections that can't go direct are relayed through PeerJS's TURN server, which only sees encrypted data.

## Run locally

It's a static site. Serve the folder with any web server, for example:

```
python3 -m http.server 8000
```

Camera scanning and the clipboard need HTTPS or `localhost`.

## License

BSD 3-Clause. Bundled libraries keep their own licenses: PeerJS (MIT), qrcode-generator (MIT).
