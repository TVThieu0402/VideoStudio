# Multi-Clip API Examples for Video Editing with Kaltura

 

## 🎬 Multi-Clip API Examples

 

All examples use the `baseEntry.setResource` API action. The destination entry must already exist (create it first with `media.add` or `baseEntry.add`). All time values are in **milliseconds**.
 

---

 

### ✂️ Example 1: Simple Trim (Single Clip)

 

Extract seconds 10–40 from one entry into a new entry.
   

```
POST /api_v3/service/baseEntry/action/updateContent
{
  "entryId": "1_dest_entry",
  "resource": {
    "objectType": "KalturaOperationResources",
    "resources": [
      {
        "objectType": "KalturaOperationResource",
        "resource": {
          "objectType": "KalturaEntryResource",
          "entryId": "1_source_abc"
        },
        "operationAttributes": [
          {
            "objectType": "KalturaClipAttributes",
            "offset": 10000,
            "duration": 30000
          }
        ]
      }
    ]
  }
}
```

   

**Result:** A 30-second clip starting at the 10-second mark of `1_source_abc`.
 

---

 

### Example 2: Multi-Clip Concatenation (3 Sources, Named Chapters)

 

Combine three clips from different entries into one video, with chapter markers named after each source entry.
   

```
POST /api_v3/service/baseEntry/action/updateContent
{
  "entryId": "1_dest_highlight_reel",
  "resource": {
    "objectType": "KalturaOperationResources",
    "chapterNamePolicy": "byEntryName",
    "resources": [
      {
        "objectType": "KalturaOperationResource",
        "resource": {
          "objectType": "KalturaEntryResource",
          "entryId": "1_lecture_part1"
        },
        "operationAttributes": [
          {
            "objectType": "KalturaClipAttributes",
            "offset": 0,
            "duration": 120000
          }
        ]
      },
      {
        "objectType": "KalturaOperationResource",
        "resource": {
          "objectType": "KalturaEntryResource",
          "entryId": "1_lecture_part2"
        },
        "operationAttributes": [
          {
            "objectType": "KalturaClipAttributes",
            "offset": 30000,
            "duration": 90000
          }
        ]
      },
      {
        "objectType": "KalturaOperationResource",
        "resource": {
          "objectType": "KalturaEntryResource",
          "entryId": "1_lecture_part3"
        },
        "operationAttributes": [
          {
            "objectType": "KalturaClipAttributes",
            "offset": 0,
            "duration": 60000
          }
        ]
      }
    ]
  }
}
```

   

**Result:** A 4.5-minute video (120s + 90s + 60s) with three chapter cue points named after each source entry. Cue points from each source are copied and time-adjusted.
 

`chapterNamePolicy`** values:**
  

| Value | Chapter Name |
| --- | --- |
| byEntryName | Source entry's name |
| byEntryId | Source entry's ID |
| numerical | "Chapter 1", "Chapter 2", … |
  

---

 

### 🖼️ Example 3: Clip with a Logo Overlay (Top-Right Corner)

 

Place a watermark/logo entry in the top-right corner at 20% of frame height, with the main audio kept at full volume and the overlay audio muted.
   

```
POST /api_v3/service/baseEntry/action/updateContent
{
  "entryId": "1_dest_branded",
  "resource": {
    "objectType": "KalturaOperationResources",
    "resources": [
      {
        "objectType": "KalturaOperationResource",
        "resource": {
          "objectType": "KalturaEntryResource",
          "entryId": "1_main_video"
        },
        "operationAttributes": [
          {
            "objectType": "KalturaClipAttributes",
            "offset": 0,
            "duration": 180000,
            "mediaCompositionAttributesArray": [
              {
                "objectType": "KalturaOverlayAttributes",
                "resource": {
                  "objectType": "KalturaEntryResource",
                  "entryId": "1_logo_entry"
                },
                "overlayPlacement": "topRight",
                "overlayShape": "rectangle",
                "overlayScaleAttribute": {
                  "objectType": "KalturaOverlayScaleAttribute",
                  "scalePercentage": {
                    "objectType": "KalturaDimensionsPercentage",
                    "widthPercentage": 0.15,
                    "heightPercentage": 0.15
                  },
                  "scaleBehavior": "fit"
                },
                "marginsPercentage": {
                  "objectType": "KalturaDimensionsPercentage",
                  "widthPercentage": 0.02,
                  "heightPercentage": 0.02
                },
                "audioAttributes": {
                  "objectType": "KalturaAudioAttributes",
                  "volume": 0.0
                }
              }
            ]
          }
        ]
      }
    ]
  }
}
```

   

**Key overlay parameters:**
  

| Parameter | Description | Range / Values |
| --- | --- | --- |
| overlayPlacement | Position on frame | topLeft, topCenter, topRight, centerLeft, center, centerRight, bottomLeft, bottomCenter, bottomRight |
| overlayShape | Clip shape | rectangle, circle |
| scalePercentage.widthPercentage | Overlay width as fraction of frame | 0.0–0.9 |
| scalePercentage.heightPercentage | Overlay height as fraction of frame | 0.0–0.9 |
| marginsPercentage.widthPercentage | Horizontal margin from edge | 0.0–0.9 |
| marginsPercentage.heightPercentage | Vertical margin from edge | 0.0–0.9 |
| scaleBehavior | How overlay is scaled | fit, crop |
| audioAttributes.volume | Overlay audio volume | 0.0 (mute) – 1.0 (full) |
  

> **⚠️ Warning**
> 
> ***Constraint:** *`scalePercentage + marginsPercentage`* must not exceed *`1.0`* on either axis.*

 

> **📝 Note**
> 
> ***Note:** When any clip uses *`KalturaOverlayAttributes`*, cue points are **not** copied to the output entry.*

 

---

 

### 👤 Example 4: Circular Profile-Picture Overlay (Center-Bottom)

 

Overlay a circular speaker thumbnail in the bottom-center, scaled to 25% of frame width.
   

```
{
  "objectType": "KalturaOverlayAttributes",
  "resource": {
    "objectType": "KalturaEntryResource",
    "entryId": "1_speaker_headshot"
  },
  "overlayPlacement": "bottomCenter",
  "overlayShape": "circle",
  "overlayScaleAttribute": {
    "objectType": "KalturaOverlayScaleAttribute",
    "scalePercentage": {
      "objectType": "KalturaDimensionsPercentage",
      "widthPercentage": 0.25,
      "heightPercentage": 0.25
    },
    "scaleBehavior": "crop"
  },
  "marginsPercentage": {
    "objectType": "KalturaDimensionsPercentage",
    "widthPercentage": 0.0,
    "heightPercentage": 0.05
  },
  "audioAttributes": {
    "objectType": "KalturaAudioAttributes",
    "volume": 0.0
  }
}
```

   

---

 

### Example 5: Background Replacement (Chroma Key / Green Screen)

 

Replace the green-screen background of a clip with a background video entry. Scale the foreground subject to 80% and position it centered.
   

```
POST /api_v3/service/baseEntry/action/updateContent
{
  "entryId": "1_dest_composited",
  "resource": {
    "objectType": "KalturaOperationResources",
    "resources": [
      {
        "objectType": "KalturaOperationResource",
        "resource": {
          "objectType": "KalturaEntryResource",
          "entryId": "1_greenscreen_presenter"
        },
        "operationAttributes": [
          {
            "objectType": "KalturaClipAttributes",
            "offset": 0,
            "duration": 60000,
            "mediaCompositionAttributesArray": [
              {
                "objectType": "KalturaReplaceBackgroundAttributes",
                "resource": {
                  "objectType": "KalturaEntryResource",
                  "entryId": "1_background_video"
                },
                "foregroundScalePercentage": 0.8,
                "foregroundPositionPercentage": {
                  "objectType": "KalturaPosition",
                  "x": 0.5,
                  "y": 0.5
                },
                "audioAttributes": {
                  "objectType": "KalturaAudioAttributes",
                  "volume": 0.3
                }
              }
            ]
          }
        ]
      }
    ]
  }
}
```

   

`KalturaReplaceBackgroundAttributes`** parameters:**
  

| Parameter | Description | Range |
| --- | --- | --- |
| resource | Background entry/image | KalturaEntryResource, KalturaAssetResource, KalturaDocumentImagesResource |
| backgroundColorCode | Solid color background (alternative to resource) | Hex string, e.g. "0xFF0000" |
| foregroundScalePercentage | Scale of the foreground subject | 0.0–5.0 |
| foregroundPositionPercentage.x | Horizontal position (0=left, 1=right) | 0.0–1.0 |
| foregroundPositionPercentage.y | Vertical position (0=top, 1=bottom) | 0.0–1.0 |
| audioAttributes.volume | Background audio volume | 0.0–1.0 |
  

**Solid color background alternative** (no background entry needed):
   

```
{
  "objectType": "KalturaReplaceBackgroundAttributes",
  "backgroundColorCode": "0x1A1A2E",
  "foregroundScalePercentage": 1.0,
  "foregroundPositionPercentage": {
    "objectType": "KalturaPosition",
    "x": 0.5,
    "y": 0.5
  }
}
```

   

---

 

### 🆖 Example 6: Clip with Burned-In Captions

 

Render a caption asset's text directly into the video pixels (hard subtitles).
   

```
POST /api_v3/service/baseEntry/action/updateContent
{
  "entryId": "1_dest_with_subs",
  "resource": {
    "objectType": "KalturaOperationResources",
    "resources": [
      {
        "objectType": "KalturaOperationResource",
        "resource": {
          "objectType": "KalturaEntryResource",
          "entryId": "1_source_video"
        },
        "operationAttributes": [
          {
            "objectType": "KalturaClipAttributes",
            "offset": 0,
            "duration": 300000,
            "captionAttributes": [
              {
                "objectType": "KalturaRenderCaptionAttributes",
                "captionAssetId": "1_caption_asset_en"
              }
            ]
          }
        ]
      }
    ]
  }
}
```

   

> **⚠️ Warning**
> 
> ***Constraint:** Only one *`KalturaRenderCaptionAttributes`* is allowed per clip.*

 

---

 

### ➕ Example 7: Full Complex Request

 

Three clips: the first has a logo overlay, the second has background replacement, the third is a plain trim. Chapters are numbered.
   

```
POST /api_v3/service/baseEntry/action/updateContent
{
  "entryId": "1_dest_final",
  "resource": {
    "objectType": "KalturaOperationResources",
    "chapterNamePolicy": "numerical",
    "resources": [
      {
        "objectType": "KalturaOperationResource",
        "resource": {
          "objectType": "KalturaEntryResource",
          "entryId": "1_intro_video"
        },
        "operationAttributes": [
          {
            "objectType": "KalturaClipAttributes",
            "offset": 5000,
            "duration": 60000,
            "cropAlignment": 50,
            "mediaCompositionAttributesArray": [
              {
                "objectType": "KalturaOverlayAttributes",
                "resource": {
                  "objectType": "KalturaEntryResource",
                  "entryId": "1_logo"
                },
                "overlayPlacement": "topRight",
                "overlayShape": "rectangle",
                "overlayScaleAttribute": {
                  "objectType": "KalturaOverlayScaleAttribute",
                  "scalePercentage": {
                    "objectType": "KalturaDimensionsPercentage",
                    "widthPercentage": 0.12,
                    "heightPercentage": 0.12
                  },
                  "scaleBehavior": "fit"
                },
                "marginsPercentage": {
                  "objectType": "KalturaDimensionsPercentage",
                  "widthPercentage": 0.02,
                  "heightPercentage": 0.02
                },
                "audioAttributes": {
                  "objectType": "KalturaAudioAttributes",
                  "volume": 0.0
                }
              }
            ]
          }
        ]
      },
      {
        "objectType": "KalturaOperationResource",
        "resource": {
          "objectType": "KalturaEntryResource",
          "entryId": "1_greenscreen_segment"
        },
        "operationAttributes": [
          {
            "objectType": "KalturaClipAttributes",
            "offset": 0,
            "duration": 90000,
            "mediaCompositionAttributesArray": [
              {
                "objectType": "KalturaReplaceBackgroundAttributes",
                "resource": {
                  "objectType": "KalturaEntryResource",
                  "entryId": "1_office_background"
                },
                "foregroundScalePercentage": 0.9,
                "foregroundPositionPercentage": {
                  "objectType": "KalturaPosition",
                  "x": 0.5,
                  "y": 0.5
                },
                "audioAttributes": {
                  "objectType": "KalturaAudioAttributes",
                  "volume": 0.2
                }
              }
            ]
          }
        ]
      },
      {
        "objectType": "KalturaOperationResource",
        "resource": {
          "objectType": "KalturaEntryResource",
          "entryId": "1_outro_video"
        },
        "operationAttributes": [
          {
            "objectType": "KalturaClipAttributes",
            "offset": 120000,
            "duration": 30000,
            "cropAlignment": 50
          }
        ]
      }
    ]
  }
}
```

   

**Result:** A 3-minute video (60s + 90s + 30s) with:
 

- Chapter 1 — intro with logo watermark
- Chapter 2 — green-screen segment composited onto office background
- Chapter 3 — plain trim of the outro
 

---

 

### Example 8: Overlay with Its Own Background Replacement

 

An overlay resource can itself have a `KalturaReplaceBackgroundAttributes` applied to it (nested composition). This removes the green screen from the overlay before placing it on the main video.
   

```
{
  "objectType": "KalturaOverlayAttributes",
  "resource": {
    "objectType": "KalturaEntryResource",
    "entryId": "1_greenscreen_pip"
  },
  "overlayPlacement": "bottomRight",
  "overlayShape": "circle",
  "overlayScaleAttribute": {
    "objectType": "KalturaOverlayScaleAttribute",
    "scalePercentage": {
      "objectType": "KalturaDimensionsPercentage",
      "widthPercentage": 0.2,
      "heightPercentage": 0.2
    },
    "scaleBehavior": "crop"
  },
  "marginsPercentage": {
    "objectType": "KalturaDimensionsPercentage",
    "widthPercentage": 0.02,
    "heightPercentage": 0.02
  },
  "resourceMediaCompositionAttributesArray": [
    {
      "objectType": "KalturaReplaceBackgroundAttributes",
      "backgroundColorCode": "0x00FF00",
      "foregroundScalePercentage": 1.0,
      "foregroundPositionPercentage": {
        "objectType": "KalturaPosition",
        "x": 0.5,
        "y": 0.5
      }
    }
  ],
  "audioAttributes": {
    "objectType": "KalturaAudioAttributes",
    "volume": 0.0
  }
}
```

   

> **⚠️ Warning**
> 
> ***Constraint:** *`resourceMediaCompositionAttributesArray`* on an overlay accepts at most **1** entry, and it cannot itself be a *`KalturaOverlayAttributes`* (no recursive overlays).*

 

---

 

### ⚠️ Validation Errors Reference

  

| Error | Cause |
| --- | --- |
| MISSING_MANDATORY_PARAMETER: duration | A KalturaClipAttributes is missing duration |
| RESOURCES_COUNT_EXCEEDED_MAX_ALLOWED_COUNT | More than 5 resources in the request |
| CLIPS_DURATIONS_EXCEEDED_MAX_ALLOWED_DURATION | Total clip duration exceeds 5 hours |
| MULTIPLE_PARAMETER_NOT_SUPPORTED: dimensionsAttributes | More than one dimensionsAttributes entry |
| MULTIPLE_PARAMETER_NOT_SUPPORTED: renderCaptionAttributes | More than one burn-in caption per clip |
| PARAMETER_VALUE_OUT_OF_RANGE: cropAlignment | cropAlignment outside 0–100 |
| PARAMETER_VALUE_OUT_OF_RANGE: volume | volume outside 0.0–1.0 |
| PARAMETER_VALUE_OUT_OF_RANGE: heightPercentage | heightPercentage outside 0.0–0.9 |
| PARAMETER_VALUE_OUT_OF_RANGE: widthPercentage | widthPercentage outside 0.0–0.9 |
| PARAMETER_VALUE_OUT_OF_RANGE: foregroundScalePercentage | foregroundScalePercentage outside 0.0–5.0 |
| INVALID_FIELD_VALUE: backgroundColorCode | Color code not in 0xRRGGBB format |
| RESOURCE_MEDIA_COMPOSITION_COUNT_EXCEEDED_MAX_ALLOWED_COUNT | More than 1 entry in resourceMediaCompositionAttributesArray |
