# Alarm

A simple alarm clock web app that can sync your alarms across devices via your own Solid pod.

Live: https://solid-apps.github.io/alarm/

## Features

- Large clock showing the date, time and seconds, with a chip for the next alarm and how long until it rings.
- Quick alarms: 1 min, 5 min, 10 min, 30 min and 1 hour. These ring once and are removed when you press Stop.
- New alarm: pick hour, minute (in 5-minute steps) and AM/PM, add an optional label (up to 40 characters) and set it. These repeat daily.
- Six synthesized tones (Web Audio, no audio files): Beep, Bell, Chime (default), Gentle, Digital and Siren, with a Preview button.
- While ringing: Snooze (9 minutes), Stop, or Delete.
- Toggle switch to enable or disable each alarm; alarms are listed by time.
- Optional browser notifications when an alarm rings (an Enable prompt is shown if permission hasn't been decided).
- Sign in with Solid through [xlogin](https://unpkg.com/xlogin).
- Web app manifest with icons, so it can be added to a home screen.

Alarms only ring while the page is open in a browser tab; the app checks them every second and again when the tab becomes visible.

## Your data

- **Signed out:** alarms are stored in the browser's `localStorage` under the key `alarms`. Everything works without an account.
- **Signed in with Solid:** alarms are stored on your pod at

  ```
  <your pod origin>/public/alarm/alarms.jsonld
  ```

  The pod origin is taken from your WebID. The document is JSON-LD in the iCal vocabulary: an `ical:Vcalendar` whose components are `ical:Vevent`s (also typed `urn:solid:Alarm`), each with an embedded `ical:Valarm`. Components that are not alarms are kept when the app saves.
- **Private, despite the `/public/` path:** when the document is first created, the app also creates `alarms.jsonld.acl` (only if no ACL exists) granting Read, Write and Control to the owner (`/profile/card.jsonld#me`) and no public access. All pod requests use `xlogin.authFetch`.
- **On first sign-in**, alarms in `localStorage` are copied to the pod only if the pod document is empty. They are not removed from `localStorage`.
- **Sync:** if the pod advertises an `Updates-Via` WebSocket, changes made from another device are picked up live.
- **On sign-out**, the app goes back to `localStorage`.
- Ringing and snooze state is not shared between devices: the ringing flag is stripped on save, and snooze time is not written to the pod.

## Run locally

It is a single static `index.html` with no build step. Serve the folder with any static server, for example:

```
python3 -m http.server
```

or `npm start` (runs `npx serve .`).

## Licence

AGPL-3.0-only (from `package.json`).
