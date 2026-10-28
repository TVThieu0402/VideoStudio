# Managing Scripts

It is recommended to store Scripts and other Data objects as a Kaltura Data Entry.

The Kaltura Data Entry object (KalturaDataEntry) is a specialized entry subclass in the Kaltura API schema (derived from KalturaBaseEntry with type DATA [value 6]) used to store data, documents, or structured information items within the Kaltura platform.


Developer Guide on Kaltura Base Entry: https://developer.kaltura.com/api-docs/General_Objects/Objects/KalturaBaseEntry

Data Entry Object Example
``` {
  "accessControlId": 0,
  "adminTags": "",
  "application": "0",
  "applicationVersion": "",
  "blockAutoTranscript": true,
  "capabilities": "",
  "categories": "",
  "categoriesIds": "",
  "conversionProfileId": 0,
  "createdAt": 0,
  "creatorId": "",
  "description": "",
  "displayInSearch": -2,
  "downloadUrl": "",
  "endDate": 0,
  "entitledUsersEdit": "",
  "entitledUsersPublish": "",
  "entitledUsersView": "",
  "groupId": 0,
  "id": "",
  "licenseType": -1,
  "moderationCount": 0,
  "moderationStatus": 1,
  "multiLingual_description": [
    {
      "language": "",
      "value": ""
    }
  ],
  "multiLingual_name": [
    {
      "language": "",
      "value": ""
    }
  ],
  "multiLingual_tags": [
    {
      "language": "",
      "value": ""
    }
  ],
  "name": "",
  "objectType": "KalturaBaseEntry",
  "operationAttributes": [
    {
      "objectType": "KalturaClipAttributes"
    }
  ],
  "parentEntryId": "",
  "partnerData": "",
  "partnerId": 0,
  "partnerSortValue": 0,
  "rank": 0,
  "redirectEntryId": "",
  "referenceId": "",
  "replacedEntryId": "",
  "replacementStatus": "0",
  "replacingEntryId": "",
  "rootEntryId": "",
  "searchText": "",
  "startDate": 0,
  "status": "-2",
  "tags": "",
  "templateEntryId": "",
  "thumbnailUrl": "",
  "totalRank": 0,
  "type": "-1",
  "updatedAt": 0,
  "userId": "",
  "version": 0,
  "votes": 0,
  "dataContent": "",
  "retrieveDataContentByGet": true
} ```

## Recommended Actions

### Add Data Entry
https://developer.kaltura.com/api-docs/service/data/action/delete

### Delete Data Entry
https://developer.kaltura.com/api-docs/service/data/action/delete

### Update Data Entry
https://developer.kaltura.com/api-docs/service/data/action/list

### List Data Entry
https://developer.kaltura.com/api-docs/service/data/action/list
