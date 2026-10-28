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



|                               |                                                      |
|-------------------------------|------------------------------------------------------|
| Kaltura Express Recorder v2.0 | Recording web component with numerous configurations |
| Kaltura Media API             | Media API for uploading content                      |

### Producing
In Progress :construction:

### Managing and Editing
In Progress :construction:
* [Kaltura Editor Application](https://github.com/TVThieu0402/VideoStudio/blob/9ace4367b2732aad08e4579048639998355b57f3/ExpressRecorder2.0.md): Kaltura Express Recorder v2.0 Component

### Sharing
In Progress :construction:

### Enriching
In Progress :construction:
