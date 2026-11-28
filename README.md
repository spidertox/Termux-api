# Intelli Termux:API

[![Build & Publish Signed Intelli Termux:API Release](https://github.com/spidertox/Termux-api/actions/workflows/release.yml/badge.svg)](https://github.com/spidertox/Termux-api/actions/workflows/release.yml)

Companion Android API app (`ai.intelli.api`, `sharedUserId="ai.intelli"`) for [Intelli](https://github.com/spidertox/Intelli) (not for official Termux). Exposes Android hardware and system APIs to CLI scripts and agents.

When developing or packaging, note that this APK must be signed with the same release certificate (`RELEASE_KEYSTORE_BASE64`) as Intelli (`ai.intelli`) so both apps share the `ai.intelli` UID and permission boundaries.

## Installation

Latest base version: `v0.53.0`.

Download the signed release APK (`intelli-termux-api-v*.apk`) from [GitHub Releases](https://github.com/spidertox/Termux-api/releases) or [GitHub Actions](https://github.com/spidertox/Termux-api/actions/workflows/release.yml).

> **Note on package ID:** This fork uses `ai.intelli.api` (socket `ai.intelli.api://listen`, receiver `ai.intelli.api/.TermuxApiReceiver`) with `sharedUserId="ai.intelli"` so it does not collide with an official F-Droid/Play `com.termux.api` install. Intelli's `TermuxRelocation` rewrites `com.termux.api` in upstream `termux-api` binaries/scripts to `ai.intelli.api`.

Signature keys differ from official Termux builds; always install matching-signed builds of Intelli (`ai.intelli`) and **Intelli Termux:API**.

## License

Released under the [GPLv3 license](http://www.gnu.org/licenses/gpl-3.0.en.html).

## How API calls are made through the termux-api helper binary

The [termux-api](https://github.com/termux/termux-api-package/blob/master/termux-api.c)
client binary generates two Linux abstract-namespace UNIX sockets and forwards them to the receiver (`ai.intelli.api.TermuxApiReceiver`) or socket listener (`ai.intelli.api://listen`):

```
/system/bin/am broadcast ${BROADCAST_RECEIVER} --es socket_input ${INPUT_SOCKET} --es socket_output ${OUTPUT_SOCKET}
```

The two sockets are used to forward stdin from `termux-api` to the relevant API
class and output from the API class to the stdout of `termux-api`.

## Client scripts

Client scripts which processes command line arguments before calling the
`termux-api` helper binary are available in the [termux-api package](https://github.com/termux/termux-api-package).

## Ideas

- Wifi network search and connect.
- Add extra permissions to the app to (un)install apps, stop processes etc.
