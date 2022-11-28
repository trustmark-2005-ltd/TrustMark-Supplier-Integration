# TrustMark Supplier Integration

## Introduction

The TrustMark Supplier.IntegrationApi (SIAPI) allows consumers to query Projects and Measures that exist within the TrustMark Retrofit Platform under ECO4 and ECO+ for purposes of verifying data correctness to improve downstream Ofgem processes.

## API

### Access

You will require an `x-api-key` which will be provided by TrustMark during onboarding.

### Response Codes

TrustMark uses [HTTP response status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) to indicate the success or failure of your API requests.

#### 2xx

Success status codes confirm that your request worked as expected.

#### 400 Bad Request

Almost likely that a missing or malformed request has been provided, often due to incorrect data-types being provided. Check your request with the documentation.

### RetrofitProject

> POST /api/RetrofitProject

Provides Project data for the project reference provided which can be accessed at any point during the lifetime of a Project created in the TrustMark Retrofit.

#### Request

Accepts an array of project references and postcode pairs. Each pair must be a correct match as lodged in order to return a success result; otherwise expect a detailed reason back.

```json
[
  {
    "projectReference": "string",
    "postcode": "string"
  }
]
```

##### Example Request

```json
[
  {
    "projectReference": "P4306",
    "postcode": "RG21 4EB"
  },
  {
    "projectReference": "P4307",
    "postcode": "NG4 6OP"
  }
]
```

#### Response

Your response data will be split into successful and failed responses with each `successfulResponses` and `failedResponses` providing a collection for your review.

```json
{
    "successfulResponses": [],
    "failedResponses": []
}
```

##### Example Response

```json
{
  "successfulResponses": [
    {
      "model": {
        "projectReference": "P4306",
        "projectType": "ECO4",
        "address": {
          "address1": "The Square, Arena Business Centres,",
          "address2": "Basing View",
          "town": "Basingstoke",
          "postcode": "RG21 4EB",
          "uprn": "100050092133"
        },
        "propertyType": "House",
        "createdDate": "2022-11-27T22:56:22.958+00:00",
        "isComplete": true,
        "completionDate": "2022-11-27T22:56:52.997+00:00",
        "startSAPScore": 53,
        "finalSAPScore": 53,
        "status": "Complete",
        "retrofitAssessmentId": "ass-25ef3b2c-9bf8-49e5-bb25-a85a906a5fb4",
        "completionCertificateNumber": "P4306-C",
        "premisesTenure": "Owner Occupied",
        "defects": [
          {
            "toBeAddressedInProject": null,
            "repairCost": 0,
            "defectType": "Leaks"
          },
          {
            "toBeAddressedInProject": false,
            "repairCost": 10,
            "defectType": "Leaks"
          },
          {
            "toBeAddressedInProject": false,
            "repairCost": 50,
            "defectType": "Condensation"
          }
        ],
        "measuresEvaluated": [
          {
            "improvementOptionEvaluationId": "ioe-13479890-7eea-4c9c-9286-5e4ca43213b6",
            "measureEligibilityStatus": "Eligible",
            "measureType": "Cavity wall insulation (0.040)",
            "eco4Name": "CWI_0.040",
            "workTypeCode": "DW-101",
            "improvementOptionEvaluationMeasureId": "274096be-2755-4d4b-87cb-0480b47ffc4b"
          },
          {
            "improvementOptionEvaluationId": "ioe-13479890-7eea-4c9c-9286-5e4ca43213b6",
            "measureEligibilityStatus": "Customer declined",
            "measureType": "TRV",
            "eco4Name": "TRV",
            "workTypeCode": "DW-159",
            "improvementOptionEvaluationMeasureId": "7ff6ef81-7a17-4f65-9303-0c2c7181de05"
          },
          {
            "improvementOptionEvaluationId": "ioe-13479890-7eea-4c9c-9286-5e4ca43213b6",
            "measureEligibilityStatus": "Conservation area",
            "measureType": "Solar PV",
            "eco4Name": "Solar_PV",
            "workTypeCode": "DW-231",
            "improvementOptionEvaluationMeasureId": "b9223893-9dfc-4cdf-aa34-fd72eec92834"
          },
          {
            "improvementOptionEvaluationId": "ioe-13479890-7eea-4c9c-9286-5e4ca43213b6",
            "measureEligibilityStatus": "Insufficient funding",
            "measureType": "[GSHP] First time central heating (FTCH)",
            "eco4Name": "B_First_Time_CH",
            "workTypeCode": "DW-299",
            "improvementOptionEvaluationMeasureId": "5fd07ff6-7291-4edd-8498-0ba359ccd644"
          },
          {
            "improvementOptionEvaluationId": "ioe-13479890-7eea-4c9c-9286-5e4ca43213b6",
            "measureEligibilityStatus": "Eligible",
            "measureType": "Pitched roof insulation",
            "eco4Name": "PRI",
            "workTypeCode": "DW-253",
            "improvementOptionEvaluationMeasureId": "11820f89-502c-4e07-91de-b36224aa9726"
          },
          {
            "improvementOptionEvaluationId": "ioe-13479890-7eea-4c9c-9286-5e4ca43213b6",
            "measureEligibilityStatus": "Property suitability",
            "measureType": "Solid wall - External Insulation",
            "eco4Name": "EWI_solid",
            "workTypeCode": "DW-126",
            "improvementOptionEvaluationMeasureId": "eb8a6567-af28-4eb3-beff-b9c375d4f5ae"
          }
        ],
        "selectedMeasures": [
          {
            "measureEligibilityStatus": "Eligible",
            "measureType": "Cavity wall insulation (0.040)",
            "eco4Name": "CWI_0.040",
            "workTypeCode": "DW-101",
            "improvementOptionEvaluationMeasureId": "274096be-2755-4d4b-87cb-0480b47ffc4b"
          }
        ],
        "totalRepairCost": 0,
        "actualRepairCost": 80,
        "floorAreaM2": 63,
        "buildingServicesGas": "Y",
        "startRdSAPFile": null,
        "endRdSAPFile": null
      },
      "isSuccess": true,
      "message": "Project Reference P4306"
    }
  ],
  "failedResponses": [
    {
      "status": "ValidationFailed",
      "reasons": [
        {
          "field": "ProjectReference",
          "reason": "Invalid project type for P4307"
        }
      ]
    }
  ]
}

```

### Measure

> /api/Measure

Provides measure data for the UMR provided which can be accessed at any point during the lifetime of a Lodgement created in the TrustMark Retrofit.

#### Request

Accepts an array of umr and postcode pairs. Each pair must be a correct match as lodged in order to return a success result; otherwise expect a detailed reason back.

```json
[
  {
    "umr": "string",
    "postcode": "string"
  }
]
```

##### Example Request

```json
[
  {
    "umr": "P4306NGJM",
    "postcode": "RG21 4EB"
  },
  {
    "umr": "P4307PMWJ",
    "postcode": "NG4 6OP"
  }
]
```

#### Response

Your response data will be split into successful and failed responses with each `successfulResponses` and `failedResponses` providing a collection for your review.

```json
{
    "successfulResponses": [],
    "failedResponses": []
}
```

##### Example Response

```json
{
  "successfulResponses": [
    {
      "model": {
        "umr": "P4306NGJM",
        "projectType": "ECO4",
        "measureCategory": "Cavity Wall Insulation",
        "standard": "PAS 2030:2019",
        "handoverDate": "2022-04-27T22:01:57.353+00:00",
        "workCarriedOutByTMLN": "2337207",
        "registeredBusinessName": "TrustMark_All_Trades_Test_Only",
        "measureType": "Cavity wall insulation (0.040)",
        "productModel": "CWI",
        "guaranteeName": "CIGA 25 Year Guarantee",
        "percentageMeasureInstalled": "Yes",
        "operativeCertificationReference": null,
        "innovationMeasureNumber": "009",
        "retrofitProjectReference": "P4306",
        "isComplete": true,
        "status": "Complete",
        "completionCertificateNumber": "P4306-C",
        "defects": [
          {
            "toBeAddressedInProject": null,
            "repairCost": 0,
            "defectType": "Leaks"
          },
          {
            "toBeAddressedInProject": false,
            "repairCost": 10,
            "defectType": "Leaks"
          },
          {
            "toBeAddressedInProject": false,
            "repairCost": 50,
            "defectType": "Condensation"
          }
        ],
        "totalRepairCost": 0,
        "actualRepairCost": 80,
        "certificateNumber": "P4306-1",
        "lodgementId": "lod-7a835737-0e4f-40be-b97d-36b20455633d",
        "improvementOptionEvaluationMeasureId": "274096be-2755-4d4b-87cb-0480b47ffc4b",
        "eco4Name": "CWI_0.040",
        "workTypeCode": "DW-101"
      },
      "isSuccess": true,
      "message": null
    }
  ],
  "failedResponses": [
    {
      "status": "Not Found",
      "reasons": [
        {
          "field": "UMR",
          "reason": "P4307PMWJ"
        }
      ]
    }
  ]
}
```

## Postman Examples

An example collection is available to import, copy this link `https://api.postman.com/collections/3727645-1b5173a2-3a40-40f5-8ed9-b30941c5af4c?access_key=PMAT-01GJYRWDJ6WEW3WP0WR7SSC6EZ` and use the 'Import From Link' feature in Postman. Once imported, update the variables in the API Collection with your own x-api-key as provided by TrustMark.

If you don't have Postman installed, you can get a copy here https://www.postman.com
