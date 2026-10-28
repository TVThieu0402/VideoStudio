
# Kaltura Express Recorder 2.0

A browser-based recorder that captures video, audio and screen shares, lets the user
review the result, and uploads it to Kaltura — with no plugins, extensions or installs:
everything runs in the page.

**What it records**

- Camera, microphone, or both — with device selection, and audio-only capture when
  there is no camera
- A screen, window or browser tab, including the shared surface's system audio where
  the browser offers it — delivered either as **one video** (the screen, with the
  microphone mixed in) or as **two** (screen and camera, uploaded as linked Kaltura
  entries so the viewer controls the layout)
- Pause and resume mid-recording, with a duration that excludes the time spent paused

**While setting up and recording**

- A teleprompter that auto-scrolls the script at a chosen reading speed
- Background blur and virtual backgrounds
- Framing, lighting and microphone-level guidance, and a warning when the input clips
- Selectable orientation, resolution, frame rate, format and codec, each narrowed to
  what the active device and browser can actually deliver
- A mirrored preview, which is never baked into the recording

**Afterwards**

- Replay before deciding what to keep
- Download locally, and/or accept the recording — which always hands it to the host
  page, and additionally uploads it to Kaltura (as a new entry, or as replacement
  media on an existing one) when the host asked for that

The recorder is **served via the [Unisphere](https://unisphere.kaltura.com/) platform**,
which delivers and versions it as the `recorder` runtime (widget
`unisphere.widget.recorder`). A host page mounts that runtime and configures it through
the settings below; Unisphere handles loading and updating the recorder itself, so an
embedding page never pins a build of its own.

---

## Recorder API



### Settings

Passed as the runtime's `settings` object and validated against
`recorderRuntimeSettingsSchema` on creation. Validation failures reject the embed, so
the five required properties must be present.

| Property | Type | Required | Description |
| --- | --- | --- | --- |
| `ks` | `string` | ✅ | Kaltura session. Used for uploads and reported with analytics. **Read once, when the recorder mounts**: `updateSettings` cannot replace it (see [Methods](#methods)), so give the recorder a KS that stays valid until its last upload has finished. |
| `serviceUrl` | `string` | ✅ | Kaltura API base URL. |
| `playerUrl` | `string` | ✅ | Player URL used for playback after capture. |
| `partnerId` | `number` | ✅ | Kaltura partner ID. |
| `uiConfId` | `number` | ✅ | Player uiConf ID. |
| `schemaVersion` | `'1'` | — | Settings schema version. Only `'1'` is accepted. |
| `conversionProfileId` | `number` | — | Transcoding profile for the uploaded entry. Omit to let the backend apply the partner's default — the parameter is then left off the request entirely. |
| `entryName` | `string` | — | Name for the created entry. Defaults to a generated name based on capture type and timestamp. |
| `videoConstraints` | `{ frameRate?, height?, width? }` | — | Capture-profile overrides, e.g. for low-bandwidth contexts. `frameRate` is the frame rate used when the user has no saved choice and `recorderConfig.frameRate` is not set (default **30**). `width` and `height` apply **only while `recorderConfig.hideVideoSettings` is `true` with no `recorderConfig.resolution`**: the largest resolution that fits them is used. Otherwise the resolution control decides, and they are ignored. |
| `allowAudio` | `boolean` | — | Defaults to `true`. `false` hides microphone capture from the UI. |
| `allowVideo` | `boolean` | — | Defaults to `true`. `false` hides camera capture from the UI. |
| `allowScreenShare` | `boolean` | — | Defaults to `true`. `false` hides screen sharing from the UI. |
| `allowSettingsMenu` | `boolean` | — | Defaults to `true`. `false` removes the toolbar's 3-dots button and the **whole settings menu** behind it — Assistance, Mirror preview, orientation, frame rate, format & codec, Audio settings. |
| `showUploadUI` | `boolean` | — | Defaults to `true`. `false` suppresses the recorder's upload UI — progress and the done screen — but **does not disable uploading**: the upload still runs, silently, and the host is expected to present its own progress. |
| `maxRecordingTime` | `number` | — | Recording limit in **seconds**. On reaching it the recording stops itself down the normal path, exactly as pressing Stop would. Time spent paused does not count toward it. |
| `translations` | `Record<string, string>` | — | Overrides for individual i18n keys. |
| `browserNotSupportedText` | `string` | — | Replaces the message shown when the browser cannot record at all. |
| `analytics` | `{ analyticsServiceUrl, hostAppName?, hostAppVersion? }` | — | Product analytics. **Omit the whole block to send no events.** `ks` and `partnerId` come from the settings above rather than being repeated, so beacons always report the session the recorder is using. |
| `scriptsNonce` | `string` | — | CSP nonce applied to scripts the recorder injects. |
| `mediapipeAssetBase` | `string` | — | Base URL the MediaPipe segmentation WASM/model files behind the background blur and replace are fetched from. Defaults to the Kaltura-hosted `recorder-assets` path. Set it to serve the files yourself and drop that external request. The same files ship in `node_modules/@mediapipe/selfie_segmentation`. |
| `recorderConfig` | `RecorderConfig` | — | **Every host-settable value.** The root carries only infrastructure and the `allow*` gates; anything a host can choose lives here. See below. |

#### `analytics`

| Property | Type | Required | Description |
| --- | --- | --- | --- |
| `analyticsServiceUrl` | `string` | ✅ | Analytics host, with or without a scheme (`analytics.kaltura.com` or `https://analytics.kaltura.com`). No path — the endpoint is appended. |
| `hostAppName` | `number` | — | `KalturaApplication` enum value of the embedding application, reported as `hostingKalturaApplication` so recorder usage can be attributed to it. |
| `hostAppVersion` | `string` | — | Version of the embedding application. |

#### `recorderConfig`

The whole host-settable surface. If a host can choose it, it is here; the root of the
settings object carries only infrastructure (`ks`, URLs, ids) and the `allow*`
capability gates.

Each value resolves as **host value → the user's persisted `localStorage` value → the
recorder's default**, so a host value wins on every load, including over a choice the
user made in an earlier session.

A bad entry is *ignored rather than rejected*: an unrecognised value, a value of the
wrong type (`framingAssistance: "yes"`), and a key not listed below. Each logs a `[recorder] recorderConfig.<key> …`
console warning. The rest of the object still applies, and the bad key falls through to
the next tier, so the user's own preference survives and the recorder still loads. The
settings schema deliberately does not validate `recorderConfig`, because a schema error
would stop the recorder from mounting at all. Absent values (`undefined`, `null`) are
skipped silently.

Providing a value has one of three effects on the settings menu:

| When provided | Configs |
| --- | --- |
| **Forced, and its control is removed from the UI** | `orientation`; and `resolution`, `frameRate`, `videoFormat` while `hideVideoSettings` is `true` |
| **Pre-filled, control stays user-editable** | `prompterText`, `teleprompterOpen`, `framingAssistance`, `lightingAssistance`, `audioLevelFeedback`, `echoCancellation`, `noiseSuppression`, `autoGainControl`; and `resolution`, `frameRate`, `videoFormat` while `hideVideoSettings` is not `true` |
| **Never in the menu at all** | `saveMode`, `entryId`, `backgroundCategoryId`, `allowBuiltInBackgrounds`, `allowBackgroundUpload`, `prompterTextLocked`, `hideVideoSettings` |

| Property | Type | Description |
| --- | --- | --- |
| `prompterText` | `string` | Initial teleprompter script, clamped to the recorder's character limit and editable in-app afterwards. Has **no persisted tier** — the script is deliberately never written to storage, so it is the host's value or empty. A non-blank script also **opens the teleprompter on load**, overriding `teleprompterOpen`. A script longer than the limit is clamped **with a console warning**. Stays user-editable unless `prompterTextLocked` is `true`. |
| `prompterTextLocked` | `boolean` | Whether the host's `prompterText` is **read-only** in the recorder. Default `false`. `true` stops the user typing, pasting, cutting, deleting or dropping text into the script, and shows a **lock icon** on the panel whose tooltip says the script is provided by the organization. The reading controls still work: play/pause, speed, font size, contrast, restart, scrolling and moving the panel. With **no non-blank `prompterText`** the lock is ignored with a console warning and the user can type a script as usual. Read on every load, with **no persisted tier**. |
| `teleprompterOpen` | `boolean` | Whether the teleprompter panel is **open when the recorder loads**. Default `false`. A non-blank `prompterText` opens the panel regardless, so `false` does **not** keep it closed when a script is supplied. The user can still close it, and reopen it, from the toolbar's teleprompter button. Read once on load and has **no persisted tier**, so the panel never reopens from an earlier session. A panel opened this way does not take keyboard focus. Has no effect when `allowTeleprompter` is `false`. |
| `orientation` | `'portrait' \| 'landscape'` | Capture orientation, default `landscape`. **Providing a valid one hides the orientation control** from the settings menu entirely — not shown-disabled, absent. An unrecognised value (`"diagonal"`, `"Portrait"`) is ignored with a warning and leaves the control in place. **Not affected by `hideVideoSettings`.** |
| `hideVideoSettings` | `boolean` | Removes the **resolution, frame rate and format controls** from the settings menu and records with a fixed profile. Default `false`. While `true`: resolution is `resolution`, else the largest one fitting the root `videoConstraints` width and height, else **FHD**; frame rate is `frameRate`, else `videoConstraints.frameRate`, else **30**; format is `videoFormat`, else the recorder's default **WebM** format. The user's saved choices are **not applied, and not deleted**: they return once the host stops hiding the controls. A camera that cannot deliver the resolution or frame rate gets the highest one below it (the lowest supported frame rate when none is below); a browser that cannot record the format (Safari records no WebM) gets its best supported one. **None of these shows the user a message**; a format substitution logs a console warning. Orientation is not affected. Has **no persisted tier**. With `allowSettingsMenu: false` the controls are gone already, but this still fixes the profile and sets the saved choices aside. |
| `resolution` | `'4K' \| '2K' \| 'FHD' \| 'HD' \| 'SD'` | Capture resolution (3840×2160, 2560×1440, 1920×1080, 1280×720, 640×480; portrait swaps them). **Pre-fills the resolution control, which stays editable**, over the user's saved choice. Forced instead while `hideVideoSettings` is `true`. A resolution the camera cannot deliver falls back silently. |
| `frameRate` | `24 \| 25 \| 30 \| 50 \| 60` | Capture frame rate, as a number. **Pre-fills the frame rate control, which stays editable**, over the user's saved choice. Forced instead while `hideVideoSettings` is `true`. A string such as `"30"` is ignored with a warning. |
| `videoFormat` | `'webm-vp9' \| 'webm-av1' \| 'webm-h264' \| 'mp4-h264' \| 'mp4-h265' \| 'mp4-vp9' \| 'mp4-av1'` | Recording container and codec. **Pre-fills the format control, which stays editable**, over the user's saved choice. Forced instead while `hideVideoSettings` is `true`. A format this browser cannot record is passed over **with a console warning, not a user message**: the user's saved choice applies if the control is visible, the default format if hidden. There is no `webm-h265`. |
| `framingAssistance` | `boolean` | Positioning-guide overlay. Default off. |
| `lightingAssistance` | `boolean` | Lighting indicator. Default off. |
| `audioLevelFeedback` | `boolean` | Microphone level meter. Default off. |
| `echoCancellation` | `boolean` | Echo cancellation on the captured microphone. Default on. Ignored where the browser cannot honour the constraint. |
| `noiseSuppression` | `boolean` | Noise suppression on the captured microphone. Default off. Ignored where the browser cannot honour the constraint. |
| `autoGainControl` | `boolean` | Automatic gain control on the captured microphone. Default on. Ignored where the browser cannot honour the constraint. |
| `saveMode` | `'kaltura' \| 'non_kaltura'` | Whether "Use this" **also** uploads to Kaltura. Defaults to `kaltura`; an unrecognised value falls back to it **and warns**, while omitting it is silent. Does **not** affect which buttons the review screen shows. See below. |
| `entryId` | `string` | An existing entry to **replace the media on** instead of creating a new one. Consulted only when `saveMode` is `kaltura` — under `non_kaltura` nothing is uploaded, so there is no entry to update. The recorder never deletes this entry, including when an upload against it is cancelled. |
| `backgroundCategoryId` | `number \| string` | A Kaltura **category whose image entries replace the six built-in backgrounds** in the camera menu's Background tab. A number or a string of digits (`40693452` or `"40693452"`); anything else is ignored with a console warning and the tab behaves as if it were not set. On load the recorder lists the category's **READY image entries** with the session `ks`, **oldest first**, so the order they were added to the category is the order shown; at most **50**, with a console warning when the category holds more. Each is drawn through the thumbnail API at up to **1280px** on its longest edge, and named by its entry name (or "Background image N" when it has none). While the list loads, the image area shows a loading status; the blurs, and recording, are usable at once. A category that **cannot be used** — not found, not readable with the `ks`, an API error, or with no ready images — leaves **only None, Light blur and Strong blur**, plus uploads when allowed, with a console warning: the built-ins are **never** offered in its place. The `ks` must be able to list the category's entries (category entitlement and privacy context). Has **no persisted tier**. Has no effect when `allowBackgroundEffects` is `false`. |
| `allowBuiltInBackgrounds` | `boolean` | Whether the camera menu's Background tab offers the **six built-in background images**. Default `true`. `false` removes them, leaving None, Light blur, Strong blur and the user's own uploads (uploading stays available unless `allowBackgroundUpload` is `false`). Has **no further effect while `backgroundCategoryId` is set**, which replaces the built-ins anyway. Has **no persisted tier**. Has no effect when `allowBackgroundEffects` is `false`, which removes the whole Background tab. |
| `allowBackgroundUpload` | `boolean` | Whether the user may **upload their own background images** in the camera menu's Background tab. Default `true`. `false` removes the Upload tile **and the images uploaded in earlier sessions**, which stay in the browser's storage and return in an embed that allows uploads. With no built-ins and no category either, the tab **stays**, offering None, Light blur and Strong blur. Has **no persisted tier**. Has no effect when `allowBackgroundEffects` is `false`. |

**A saved background that is no longer offered** — a built-in while they are hidden or
replaced by a category, an upload while uploads are off, or a category image the
category no longer holds — is **not applied, and the user's saved choice is cleared**,
so the camera starts with no effect. A saved category image is kept while the category
is loading or fails to load, so a passing API error does not cost the user their
choice.

> **Not host-settable.** Only the properties above can be set inline; anything else a
> host passes inside `recorderConfig` is ignored with a console warning.
>
> - **Mirror preview** is the user's own view preference, not a deployment setting. Its
>   in-app control remains (default **on**, persisted); a host simply cannot pre-set it.

#### `saveMode`

**Both review-screen buttons are always shown, in both modes** — visibility is not
mode-dependent. `saveMode` decides only what "Use this" does *in addition to* handing
the recording to the host, which it always does:

| Mode | "Use this" does |
| --- | --- |
| `kaltura` | Emits `onUseThisSelected`, **then** uploads to Kaltura. |
| `non_kaltura` | Emits `onUseThisSelected` and makes **no network call at all**. |

The hand-off event is published **before** the upload begins and independently of its
outcome, so a failed Kaltura upload still leaves the host holding the recording. That
ordering is also why the event carries no Kaltura entry ID — it does not exist yet.

Either action leaves the other available, so a recording can be downloaded and then
accepted. Downloads are named `rec-<YYYYMMDD-HHMM>` from the recording's completion
time in local time. A screen share with the camera on downloads **two files** in one
click, `rec-<YYYYMMDD-HHMM>-screen` and `rec-<YYYYMMDD-HHMM>-camera`; every other
recording downloads one, under the plain name. Download is purely local — it emits no
host event.

### Methods

Declared on the `RecorderRuntime` type.

| Method | Signature | Description |
| --- | --- | --- |
| `startRecording` | `(): void` | Begins a session: enters the countdown, then captures. |
| `stopRecording` | `(): void` | Stops the current session. Called during the countdown it cancels instead, emitting no completion. |

Inherited from `UnisphereRuntimeBaseType`: `mountVisual`, `unmountVisual`,
`updateSettings`, and the readonly `widgetName` / `runtimeName` identifiers.

> **`updateSettings` cannot refresh the session.** It re-renders the mounted recorder
> with the new object, but the Kaltura client that uploads, and the analytics sender,
> are created once on mount from the first `ks`, `serviceUrl` and `partnerId`, and
> keep them. A host cannot hand an expiring recorder a new KS this way; size the KS's
> lifetime to the whole session instead. Values in `recorderConfig` that pre-fill a
> control are likewise read once, on mount.

> **Not in the published type.** The runtime class also implements `saveCopy()`,
> `upload()`, `cancelUpload()` and `destroy()`, but `RecorderRuntime` does not declare
> them — a host typed against it cannot call them without a cast. Either the type
> should be extended or they should be treated as internal.

### Events

Subscribe through the consumer each property exposes. All are readonly.

| Event | Payload | Description |
| --- | --- | --- |
| `onRecordingStarted` | `void` | A session started — fired as the countdown begins, not when capture does. |
| `onRecordingComplete` | `{ cameraBlob: Blob \| null, screenBlob: Blob \| null, durationMs: number }` | Capture finished. `screenBlob` is null unless the session was a screen share; `cameraBlob` is null when the camera was off. In a screen share **all the audio is on `screenBlob`** — the microphone mixed with any shared tab or system audio — and `cameraBlob` has **no audio track**. `durationMs` excludes any time spent paused. |
| `onOrientationChanged` | `{ orientation: 'portrait' \| 'landscape' }` | The user picked a different capture orientation. Fires **on selection, before the camera is re-acquired**, so a host can resize its container in step. If the camera then rejects the new orientation, the recorder puts it back on the previous one and **fires again with that previous orientation** — so always size to the latest event rather than to the selection. Also fires once on mount, before any selection, if the *initial* orientation came from the user's own persisted preference (a returning visitor whose last choice was `portrait`) — the one case a host cannot otherwise learn it, having no way to read `localStorage` itself. It does **not** fire on mount for the `landscape` default or for a host-supplied `recorderConfig.orientation`, since the host already knows both of those without being told. |
| `onUseThisSelected` | `{ cameraBlob: Blob \| null, screenBlob: Blob \| null, fileName: string, durationMs: number, mimeType: string, size: number }` | The user accepted a recording with "Use this". **Fires in both save modes**, and **before** any Kaltura upload begins — so under `saveMode: 'kaltura'` a failed upload still leaves the host holding the media. `fileName` is the name the recorder would give the file, `rec-<YYYYMMDD-HHMM>`; `durationMs` matches what `onRecordingComplete` reported, so a host need not correlate the two events; `mimeType` and `size` describe the blobs (`size` is their combined bytes). Carries **no Kaltura entry ID** — it does not exist when this fires; watch `onUploadComplete` for that. |
| `onUploadComplete` | `{ entryId: string, source: 'camera' \| 'screen', role: 'primary' \| 'secondary' }` | A Kaltura entry's media uploaded **successfully**. Fires **once per entry**, as each one lands — so a screen share with the camera on fires **twice**: the screen (`role: 'primary'`, the parent entry, carrying the audio) then the camera (`role: 'secondary'`, its silent child). Read `role` rather than the order. Every other recording fires once, as `primary`. `entryId` is the host's own `recorderConfig.entryId` when one was supplied *and* this is the primary; otherwise an entry the recorder created — note a screen + camera upload against a host-supplied entry still creates a **new** secondary entry for the camera, so the two events carry different ids. **Never fires** for an upload that failed or was cancelled, nor at all under `saveMode: 'non_kaltura'`, which uploads nothing. Fires regardless of `showUploadUI`. |

### State

| Property | Type | Description |
| --- | --- | --- |
| `isRecording` | `TinyDataStoreConsumer<boolean>` | `true` from `startRecording()` until the session ends or is cancelled. |

> A cancelled session — Stop pressed during the countdown — resets `isRecording` to
> `false` but emits **no** public event. Hosts that need to distinguish "cancelled"
> from "completed" should watch `isRecording` rather than wait for
> `onRecordingComplete`, which never arrives in that case.

### Leaving the page

The recorder asks the browser to show its "Leave site?" prompt **only while
recording**, paused included. It does **not** cover the countdown, the review screen
(a recording not yet accepted or downloaded), or a **Kaltura upload in progress**:
closing or navigating away then drops the upload with no warning.

A host that wants the prompt during uploads adds it itself, from "Use this" until the
last entry lands. A screen share with the camera on uploads two entries; every other
recording uploads one:

```js
let pending = 0;
const guard = (e) => { e.preventDefault(); e.returnValue = ''; };

runtime.onUseThisSelected.on(({ cameraBlob, screenBlob }) => {
  // Under saveMode 'non_kaltura' nothing uploads; skip this there.
  pending = cameraBlob && screenBlob ? 2 : 1;
  window.addEventListener('beforeunload', guard);
});
runtime.onUploadComplete.on(() => {
  if (--pending <= 0) window.removeEventListener('beforeunload', guard);
});
// A failed or cancelled upload emits no public event, so also drop the guard
// when the next recording starts.
runtime.onRecordingStarted.on(() => window.removeEventListener('beforeunload', guard));
```

A host that frames the recorder does the same with the `useThisSelected`,
`uploadComplete` and `recordingStarted` messages below. The blobs it counts arrive
only with `?parentOrigin=`.

### Embedded in an iframe

A host that frames the recorder has no runtime object to subscribe to, so **every
public event** also crosses the boundary as a `postMessage`:

```js
window.addEventListener('message', (e) => {
  const { messageType, payload } = e.data ?? {};
  if (messageType === 'orientationChanged') {
    const { orientation } = payload; // a plain object, 'portrait' | 'landscape'
  }
  if (messageType === 'uploadComplete') {
    const { entryId, source, role } = JSON.parse(payload); // a JSON string
  }
  if (messageType === 'useThisSelected') {
    const { fileName, durationMs, mimeType, size } = JSON.parse(payload);
    const { cameraBlob, screenBlob } = e.data; // only with ?parentOrigin=
  }
});
```

| Message | Payload | Mirrors |
| --- | --- | --- |
| `recordingStarted` | none | `onRecordingStarted` |
| `recordingComplete` | `'{"durationMs":…}'` — a **JSON string**; the media rides alongside as `cameraBlob` / `screenBlob` (see below) | `onRecordingComplete` |
| `useThisSelected` | `'{"fileName":"…","durationMs":…,"mimeType":"…","size":…}'` — a **JSON string**; the media rides alongside as `cameraBlob` / `screenBlob` (see below) | `onUseThisSelected` |
| `orientationChanged` | `{ orientation }` — a plain **object**, the one exception | `onOrientationChanged` |
| `uploadComplete` | `'{"entryId":"…","source":"…","role":"…"}'` — a **JSON string**, parsed with `JSON.parse` | `onUploadComplete` |

Object payloads are JSON strings, parsed with `JSON.parse`. `orientationChanged` is
the exception and stays a plain object, read as `payload.orientation`.

**The recorded media is only sent to a pinned origin.** `JSON.stringify` cannot carry
a `Blob`, so `recordingComplete` and `useThisSelected` put the blobs on the message
itself, next to `payload`, as `cameraBlob` and `screenBlob` (`null` for a stream that
was not recorded). They are attached **only when the host framed the recorder with
`?parentOrigin=`** — a recording of the user's camera and screen is never broadcast to
`*`. Without it both messages still arrive, carrying their metadata, but with no blob
fields at all.

Each message is posted from the same call site as the runtime event it mirrors, so
the two channels cannot disagree — `uploadComplete` fires **once per entry** (twice
for a screen share with the camera on: screen as `primary`, then camera as `secondary`), never for a failed or cancelled
upload, and not at all under `saveMode: 'non_kaltura'`; `useThisSelected` fires in
both save modes, before any upload. A session cancelled during the countdown sends
`recordingStarted` and nothing after it, as the runtime events do.

Messages are addressed to the origin the host declared when framing the recorder
(`?parentOrigin=https://its.host`, the same parameter the loader page uses to validate
inbound messages), falling back to `*` when that is absent. Nothing is posted when the
recorder is not framed. 
