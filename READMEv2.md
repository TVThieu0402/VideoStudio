# Create your own videoStudio with Kaltura
This developer focused guide will provide reference documentation, API recipes, and examples of how to build your own videoStudio solution with Kaltura.

The videoStudio is defined as a solution that allows a user to record, add text and animation to the video, and share the video in an intuitive easy to use UI/UX. The videoStudio aims to replace expensive production services for short form videos with a quick enterprise grade desktop application. Users will have the ability to craft professional looking videos that are enterprise compliant and on brand.

## Content

- [Pre-requisites](#Pre-requisites)
- [videoStudio Architecture](#videoStudio-Architecture)
- [Building Blocks](#Kaltura-Building-Blocks)
  - [Templates](#Templates)
  - [Sourcing](#Sourcing)
  - [Producing](#Producing)
  - [Managing and Editing](#Managing-and-Editing)
  - [Sharing](#Sharing)
  - [Enriching](#Enriching)

## Pre-requisites
An admin appToken for accessing the Kaltura API. see https://developer.kaltura.com/api-docs/VPaaS-API-Getting-Started/Getting-Started-VPaaS-API.html for guides and general reference on using the Kaltura API, including access to Client Libraries in a number of languages. While there are other methods, Kaltura strongly encourages the use of appTokens for authenticating to the API. This guide assumes you are already familiar with authenticating to the Kaltura API and making calls.

## videoStudio Architecture
Kaltura has many embeddable components, API endpoints, integrations used across different use cases and industries. The architecture and tech stack referenced here are baseline recommendations to build your videoStudio. The baseline of a videoStudio should have the following capabilities at a high level:

Templates for your video
Sourcing the video
Producing the video
Managing and Editing the video
Sharing the video
Enrich your video

## Kaltura Building Blocks
### Templates
In Progress :construction:

### Sourcing 
Your videoStudio should offer 2 primary methods of sourcing their video. To record directly from within the application or to allow direct media uploads from within the application.


<table class="tg"><thead>
  <tr>
    <th class="tg-0pky"></th>
    <th class="tg-0pky"></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-6a0s"><a href="https://github.com/TVThieu0402/VideoStudio/blob/9ace4367b2732aad08e4579048639998355b57f3/ExpressRecorder2.0.md" target="_blank" rel="noopener noreferrer">Kaltura Express Recorder v2.0</a></td>
    <td class="tg-6a0s">Recording web component with numerous configurations</td>
  </tr>
  <tr>
    <td class="tg-6a0s"><a href="https://developer.kaltura.com/workflows/Ingest_and_Upload_Media/Uploading_Media_Files;step=intro" target="_blank" rel="noopener noreferrer">Kaltura Media API</a></td>
    <td class="tg-6a0s">Media API for uploading content</td>
  </tr>
</tbody>
</table>

### Producing
In Progress :construction:

### Managing and Editing
In Progress :construction:
<table class="tg"><thead>
  <tr>
    <th class="tg-0pky"></th>
    <th class="tg-0pky"></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-6a0s"><a href="https://github.com/kaltura-vpaas/kaltura-editor-app-embed" target="_blank" rel="noopener noreferrer">Kaltura Editor Application</a></td>
    <td class="tg-6a0s">Tool to allow users to clip and trim content. Also supports adding hotspots and quizzing</td>
  </tr>
</tbody>
</table>

### Sharing
In Progress :construction:
<table class="tg"><thead>
  <tr>
    <th class="tg-0pky"></th>
    <th class="tg-0pky"></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-6a0s"><a href="https://developer.kaltura.com/player/web/getting-started-web" target="_blank" rel="noopener noreferrer">Kaltura V7 Player</a></td>
    <td class="tg-6a0s">Modern HTML5 media player </td>
  </tr>
  <tr>
    <td class="tg-0lax"><a href="https://knowledge.kaltura.com/help/how-to-download-a-raw-file" target="_blank" rel="noopener noreferrer">Kaltura Media Download</a></td>
    <td class="tg-0lax"><span style="font-weight:400;font-style:normal"> API endpoint to download source video</span></td>
  </tr>
  <tr>
    <td class="tg-0lax"></td>
    <td class="tg-0lax"></td>
  </tr>
</tbody>
</table>

### Enriching
In Progress :construction:
