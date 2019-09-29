---
title: "YubiKey on Fedora 27"
date: 2018-01-26T21:00:59-08:00
draft: false
---

If you use a yubikey for [two-factor authentication](https://en.wikipedia.org/wiki/Multi-factor_authentication), you may be surprised to find it not working on Fedora 27. After installing, I could not use [FIDO U2F](https://www.yubico.com/solutions/fido-u2f/) in the browser when logging into an account. I then reproduced the issue using yubico's own [U2F demo page](https://demo.yubico.com/u2f?tab=register) using both Firefox and Vivaldi browsers. I found nothing unusual in the systemd journal. At this point, I googled "yubikey neo fedora broken" and found a recent [GitHub issue](https://github.com/Yubico/yubioath-desktop/issues/192) filed against the yubico authenticator application. Apparently, the "PC/SC Smart Card Daemon Activation Socket" needs to be enabled for the YubiKey to work, but it is not enabled or running (listening) by default in Fedora 27. Thanks to the [effort of a yubico engineer](https://github.com/Yubico/yubioath-desktop/issues/192#issuecomment-334407086), I found the root cause and fix. In short:

```
# Check whether the socket is running (listening)
systemctl status pcscd.socket
# Enable listening on the socket (to persist across boots)
systemctl enable pcscd.socket
# Start listening
systemctl start pcscd.socket
```
