# Create your own videoStudio with Kaltura
This developer focused guide will provide reference documentation, API recipes, and examples of how to build your own videoStudio solution with Kaltura. 

The videoStudio is defined as a solution that allows a user to record, add text and animation to the video, and share the video in an intuitive easy to use UI/UX. The videoStudio aims to replace expensive production services for short form videos with a quick enterprise grade desktop application. Users will have the ability to craft professional looking videos that are enterprise compliant and on brand. 

# Prerequisites
An admin appToken for accessing the Kaltura API.
see https://developer.kaltura.com/api-docs/VPaaS-API-Getting-Started/Getting-Started-VPaaS-API.html for guides and general reference on using the Kaltura API, including access to Client Libraries in a number of languages. While there are other methods, Kaltura strongly encourages the use of appTokens for authenticating to the API.
This guide assumes you are already familiar with authenticating to the Kaltura API and making calls.

# videoStudio Architecture
Kaltura has many components across different use cases and industries. The architecture and components referenced here are baseline recommendations to build your videoStudio. The baseline of a videoStudio should have the following capabilities at a high level:
* Sourcing the video
* Producing the video
* Managing and Editing the video
* Sharing the video

**List of referenced components and APIs powered by Kaltura**

Components
- Kaltura Editor App
- Kaltura Recorder v2.0
- Kaltura V7 Player
- Kaltura Lottie Animation plugin
- Kaltura Adobe After Affects plugin 

APIs
- Upload Media
- Download Media
- Update Media
- Add Playlist
- Update Playlist
- Media Stitching API 

## Sourcing the video
Your videoStudio should offer 2 primary methods of sourcing their video. To record directly from within the application or to allow direct media uploads from within the application. 

### Kaltura Recorder v2.0

<img width="485" height="266" alt="Screenshot 2026-10-22 at 9 44 06 AM" src="https://github.com/user-attachments/assets/b0561a6a-f0e5-498f-9b94-9243167e1ad7" />

Kaltura offers the Kaltura Recorder v2.0 component that can be embedded within your application. It is a ready to use embeddable recording component with numerous configuration options and runtime settings. 

Git: https://github.com/kaltura/unisphere-recorder/tree/main

### Kaltura Media Upload API

<img width="485" height="266" alt="Screenshot 2026-10-22 at 10 20 14 AM" src="https://github.com/user-attachments/assets/cb95aeb3-81e3-49ab-a3ac-a4e058a4fe3c" />

Kaltura also offers a media upload API. The media upload API allows for multiple media types (video, audio, images) and basic metadata such as entry name and description. 

Developer Guide to Kaltura media upload: https://developer.kaltura.com/workflows/Ingest_and_Upload_Media/Uploading_Media_Files;step=intro


## Producing the Video

### Adobe After Effects Plugin
### Kaltura Lottie Animation Plugin
### Media Stitching API
<img width="485" height="266" alt="Screenshot 2026-10-22 at 1 05 03 PM" src="https://github.com/user-attachments/assets/8bccd9fb-0027-497d-9068-4e61501f4c88" />

Kaltura offers a backend API service to combine (aka stitch) multiple entries into a single entry. The following steps will be required:
1. "Gather list of media assets to stitch" The application needs to obtain and list all Kaltura entries that will be stitched together. The service will require the Kaltura Entry ID object to be listed.
2. "Create a draft entry". This draft palceholder entry will be the new media that is the combined version of all the entries
3. "Run Media Stitching BE service". This BE service will combine all the videos in sequential order 

Media Stitching API and other related BE services can be found here: MediaStitchingAPI.md

## Managing and Editing the Video

### Kaltura Editor App
Kaltura offers the Editor Application component. It can be easily embedded within your videoStudio application. The Kaltura Editor App (aka "KEA") enables end-users to create video clips, Support for cue-points, captions and multi-stream content, Audio graph for detecting silent areas, Trim long videos cutout from the middle, Compatible with desktop and tablet browsers, Easy to use and accessibility minded, Customizable with CSS, Easy to integrate into any web-app using secure iframe API, Create and edit Interactive In-Video Quizzes, Use to set cue points for dynamic ads, Create hotspots overlays that drive action and more.

<img width="485" height="266" alt="keascreenshot" src="https://github.com/user-attachments/assets/a813afa8-2cf0-474f-be42-cf40258892df" />

Git: https://github.com/kaltura-vpaas/kaltura-editor-app-embed

### Kaltura Media Update API
to update the attributes of an existing media entry in Kaltura, you must use the media.update API action. This service allows you to modify properties such as the entry's title, description, tags, access control profiles, or category assignments.

**Core Endpoint & Details**

* HTTP Method: POST
* Endpoint URL: https://www.kaltura.com/api_v3/service/media/action/update
* Behavior: Only the specific properties you provide in the request payload will be modified; all other properties remain unchanged.

**Required Request Parameters**

When constructing the API body, you need to include the following core parameters:
- entryId (string): The unique ID string of the Kaltura media asset you want to update.
- mediaEntry (object): A KalturaMediaEntry object populated with the fields you wish to change. Common fields include:
  - name: The title of the video.
	- description: The description of the video.
	- tags: A comma-separated string of keywords.
	- categories: The categories the entry belongs to.

Developer: https://developer.kaltura.com/console/service/media/action/updateContent

## Sharing the Video
Kaltura offers several methods to share the video created within your videoStudio. A player embed component is the most common and flexible method to share the video. Kaltura also offers an API service to grab the source download file. 
### Kaltura v7 Player Embed

<img width="485" height="266" alt="playlist-up-next-ms" src="https://github.com/user-attachments/assets/a6edfbbb-ac4a-437e-a32d-9b74fd7209f9" />


Knowledge Center: https://knowledge.kaltura.com/help/introduction-to-kalturas-v7-player

Git: https://github.com/kaltura/kaltura-player-js/blob/master/docs/getting-started-using-player-studio.md

### Video asset download
Kaltura offers an API to grab the source video thru a download

Knowledge Center: https://knowledge.kaltura.com/help/how-to-download-a-raw-file

### MP4 Export Flat video

