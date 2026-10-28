# Create your own videoStudio with Kaltura
This developer focused guide will provide reference documentation, API recipes, and examples of how to build your own videoStudio solution with Kaltura.

The videoStudio is defined as a solution that allows a user to record, add text and animation to the video, and share the video in an intuitive easy to use UI/UX. The videoStudio aims to replace expensive production services for short form videos with a quick enterprise grade desktop application. Users will have the ability to craft professional looking videos that are enterprise compliant and on brand.

## Content

- [Pre-requisites](#Pre-requisites)
  - [Installation with GHC](#installation-with-ghc)
    - [Pre-requisites](#pre-requisites)
    - [Compilation](#compilation)
  - [Installation with MicroHs](#installation-with-microhs)
- [Examples](#examples)
- [Related projects](#related-projects)
  - [Games and applications](#games-and-applications)
  - [Use in industry](#use-in-industry)
  - [Backends](#backends)
  - [Testing](#testing)
  - [Other projects](#other-projects)
- [Documentation](#documentation)
  - [API documentation and tutorials](#api-documentation-and-tutorials)
  - [Publications](#publications)
- [Contributions](#contributions)
  - [Discussions, issues and pull requests](#discussions-issues-and-pull-requests)
  - [Structure and internals](#structure-and-internals)
  - [Style](#style)
  - [Version control](#version-control)
  - [Versioning model](#versioning-model)
- [History](#history)

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
