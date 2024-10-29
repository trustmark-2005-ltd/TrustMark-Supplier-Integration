# TrustMark Supplier Integration

## Introduction

The TrustMark Supplier.IntegrationApi (SIAPI) allows consumers (Energy Suppliers) to query Projects and Measures that exist within the TrustMark Retrofit Platform under ECO4 and GB Insulation Scheme for purposes of verifying data correctness to improve downstream Ofgem processes.

This allows suppliers to be able to check that the data they hold about a project / measure is consistent with that lodged with TrustMark before notifying Ofgem of the measure. This new service does not return a match / mismatch against data attributes, it returns the values provided to TrustMark by the party lodging the measure.

Notes:
* This may be subject to change due to Ofgem requirements.
* General project and lodgement concepts resource [Retrofit Integration API](https://github.com/trustmark-2005-ltd/TrustMark-Retrofit-Integration).
* Meassure details can be found in the [Data Dictionary](https://www.trustmark.org.uk/tradespeople/data-warehouse) and items relating to PAS2035 defects and eligibility can be found in the [Structured Retrofit Data Model](https://www.trustmark.org.uk/tradespeople/data-warehouse).

## API

### Access

A Data Sharing Agreement will need to be in place before live access can can be provided.
You will require an `x-api-key` which will be provided by TrustMark during onboarding.


### Service Fees

Payable by the party signing the Data Sharing Agreement.

| Service                  | Fee (+VAT)               | Notes                                                            |
| ------------------------ | ------------------------ | ---------------------------------------------------------------- |
| Setup                    | £1500                    |                                                                  |
| Annual Service Charge    | £4750                    |  Annual increase capped at CPI +3%                               |


### Response Codes

TrustMark uses [HTTP response status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) to indicate the success or failure of your API requests.

#### 2xx

Success status codes confirm that your request worked as expected.

#### 400 Bad Request

Almost likely that a missing or malformed request has been provided, often due to incorrect data-types being provided. Check your request with the documentation.

### RetrofitProject

> POST /api/RetrofitProject

Provides Project data for the project reference provided which can be accessed at any point during the lifetime of a Project created in the TrustMark Retrofit.

| Field                             | Information                              |
| --------------------------------- | ---------------------------------------- |
| projectReference                  | The project reference of the Project for this Lodgement to be associated with e.g. P1000 |
| projectType                       | ECO4 / GBIS |
| address                           |      |
| propertyType                      |      |
| createdDate                       |      |
| isComplete                        | true / false     |
| completionDate                    |      |
| startSAPScore                     |      |
| finalSAPScore                     |      |
| status                            | Draft / inProgress / Complete      |
| retrofitAssessmentId              |      |
| completionCertificateNumber       |  e.g. P1000-C    |
| premisesTenure                    |      |
| projectDefects [] | Array of new defects that have been identified |
| - defectType | Condensation, Cracks / Pointing Defects, Leaks, Mould Growth, Rising Damp, Rubble in Cavity, Structural Defect, Wet Rot / Dry Rot, Other  |
| - repairCost |  |
| - toBeAddressedInProject | True / False |
| measuresEvaluated [] | |
| - improvementOptionEvaluationId | Can be used to track which measures evaluation was used (there can be more than one) |
| - measureEligibilityStatus | Eligible, Access issues, Alternative funding, Conservation area, Customer declined, Inhabitation of protected species, Insufficient funding, Listed building, Property suitability, Unlawful |
| - workTypeCode | Value from Data Dictionary Work Type sheet |
| - measureType  |  |
| - eco4Name     |  |
| - improvementOptionEvaluationMeasureId  | Can be used to track which measures evaluataion was installed |
| selectedMeasures [] | Which measures have been selected for inclusion in a plan|
| - measureEligibilityStatus | |
| - measureType  | |
| - eco4Name     | |
| - workTypeCode | |
| - improvementOptionEvaluationMeasureId  | Can be used to track which measures evaluataion was installed |
| totalRepairCost | |
| actualRepairCost | Actual cost of repairs as lodged |
| floorAreaM2 | e.g. 121.4 |
| buildingServicesGas | Y / N |
| isSuccess | true / false |
| message | Project Reference P1000|

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
    "postcode": "RG21 4EB"
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
          "uprn": "10008507326"
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


| Field                             | Information                              |
| --------------------------------- | ---------------------------------------- |
| umr                               | Unique Measure Reference e.g. P4306NGJM |
| projectType                       | ECO4 |
| measureCategory                   |      |
| standard                          |      |
| handoverDate                      |      |
| workCarriedOutByTMLN              | TMLN of the business who installed the measure     |
| registeredBusinessName            | Name of the business who installed the measure     |
| measureType                       |      |
| productModel                      |      |
| guaranteeName                     |      |
| PercentageMeasureInstalled        | Has 100% of measure been installed yes/no        |
| operativeCertificationReference   | Where any other certification data is provided  |
| innovationMeasureNumber           | Value from Innovation Measures sheet in Data Dictionary  |
| retrofitProjectReference          | e.g. P4306      |
| isComplete                        |      |
| status                            |      |
| Defects [] | Array of defects |
| - toBeAddressedInProject | true / false |
| - repairCost |  |
| - defectType |  |
| totalRepairCost | |
| actualRepairCost | Actual cost of repairs as lodged |
| certificateNumber   | Certificate associated with this measure e.g. P4306-1 |
| lodgementId | |
| improvementOptionEvaluationMeasureId | Links to porject record |
| eco4Name |  e.g 'CWI_0.040' Value from the Data Dictionary WorkType sheet |
| workTypeCode | e.g. DW-101 Value from the Data Dictionary WorkType sheet |


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
        "guaranteeName": "Some 25 Year Guarantee",
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

### RetrofitProjectUPRNStatus

> POST /api/RetrofitProjectUPRNStatus

Checks for existing projects under the GBIS against the project owner TMLN (the Retrofit Coordinator who created it) and UPRN provided.

Returns a code to indicate the existence of any existing projects against the UPRN and TMLN. The code will be one of the following:
* OK - nothing found
* UPRN_EXISTS_FIRST - The TMLN has created a GBIS project at this property
* UPRN_EXISTS_THIRD - Another GBIS project exists at this property
* UPRN_EXISTS_FIRST_AND_THIRD - The TMLN and another party have created GBIS projects at this property

| Field                      | Information                              |
| -------------------------- | ---------------------------------------- |
| ownerTMLN                  | The TMLN to check                        |
| uprn                       | The UPRN to check                        |

#### Request

```json
{
  "ownerTMLN": "string",
  "uprn": "string"
}
```

##### Example Request

```json
{
  "ownerTMLN": "1399311",
  "uprn": " 10008507326"
}
```

#### Response

```json
{
  "uprn": "string",
  "checkedTMLN": "string",
  "checkedFundType": "string",
  "code": "string"
}
```

##### Example Response

```json
{
  "uprn": "10008507326",
  "checkedTMLN": "1399311",
  "checkedFundType": "GBIS",
  "code": "UPRN_EXISTS_FIRST_AND_THIRD"
}
```

## Amendments

During the course of a Project and Lodgement lifetime, it is possible that the data may need to be amended. This can be done by the Retrofit Coordinator who created the Project or Lodgement using the TrustMark Retrofit Platform.

As the Energy Supplier, you can receive a notification of any amendments made to a Project or Measure that Ofgem have confirmed your involvement in by suppling the Measure Reference Number (MRN) for the Measure by (Unique Measure Reference) UMR.

TrustMark offer a service to notify you of these amendedments on request in the form of a webhook or email.

### Webhook

TrustMark will push real-time notifications to your webhook URL when an amendment is made to a Project or Measure that you are involved in.

Request to use this service with TrustMark by providing a webhook URL and HTTP header key and secret value, who will then configure the service to send notifications to your URL.

#### Example Webhook Payload

```json
{
  "EventType": "Amendment",
  "EventDate": "2024-08-24T10:35:49.8947069Z",
  "NotificationId": "175a3808-7384-4886-ae84-9dd2d7be2f3a",
  "Environment": "uat",
  "EnergySupplier": "SupplierABC",
  "EventData": {
    "ProjectReference": "P1000",
    "RcTMLNumber": "100000",
    "RcName": "TrustMark RC",
    "ProjectChanges": null,
    "MeasuresAmended": [
      {
        "Umr": "P1000NY66",
        "Mrn": "ABC001",
        "Changes": [
          {
            "Field": "InstalledDate",
            "OldValue": "04/Aug/2022",
            "NewValue": "05/Aug/2022"
          },
          {
            "Field": "HandoverDate",
            "OldValue": "04/Aug/2022",
            "NewValue": "05/Aug/2022"
          }
        ]
      }
    ]
  },
  "DeduplicationId": "feee0323-a742-470d-beed-a8c65690cde8"
}
```

#### Event Type

Check the `eventType` field to determine the type of event that has occurred.

| Event Type | Description |
| ---------- | ----------- |
| Amendment  | An amendment has been made to a Project or Measure that you are involved in |

#### Environment

Check the `environment` field to determine the environment the event occurred in. This will be either `uat`, `sandbox` or `live`


### Email

TrustMark will send you an email notification when an amendment is made to a Project or Measure that you are involved in.

Request to use this service with TrustMark by providing an email address to send notifications to.

#### Example Email Notification

```text
Subject: TrustMark Retrofit Platform - Amendment Notification - 100000 [ P1000NY66 ] DDID feee0323-a742-470d-beed-a8c65690cde8

Body:

{
  "EventType": "Amendment",
  "EventDate": "2024-08-24T10:35:49.8947069Z",
  "NotificationId": "175a3808-7384-4886-ae84-9dd2d7be2f3a",
  "Environment": "uat",
  "EnergySupplier": "SupplierABC",
  "EventData": {
    "ProjectReference": "P1000",
    "RcTMLNumber": "100000",
    "RcName": "TrustMark RC",
    "ProjectChanges": null,
    "MeasuresAmended": [
      {
        "Umr": "P1000NY66",
        "Mrn": "ABC001",
        "Changes": [
          {
            "Field": "InstalledDate",
            "OldValue": "04/Aug/2022",
            "NewValue": "05/Aug/2022"
          },
          {
            "Field": "HandoverDate",
            "OldValue": "04/Aug/2022",
            "NewValue": "05/Aug/2022"
          }
        ]
      }
    ]
  },
  "DeduplicationId": "feee0323-a742-470d-beed-a8c65690cde8"
}
```

### Field List

The `projectChanges` and `measuresAmended` fields will contain an array of changes made to the Project or Measure.

#### Project Changes

| Field     | Description |
| --------- | ----------- |
| SAPScore  | The SAP Score which also indicates the POST RdSAP file has changed |

#### Measure Changes

| Field         | Description |
| ------------- | ----------- |
| HandoverDate  | The date the measure was handed over |
| IsInnovationMeasure | Is the measure an Innovation Measure |
| InnovationMeasureProduct | The product of the Innovation Measure |
| InnovationMeasureNumber | The number of the Innovation Measure |
| InstalledDate | The date the measure was installed |
| ProductManufacturer  | The manufacturer of the product installed |
| ProductModel  | The model of the product installed |
| ProductVersion  | The version of the product installed |
| MeasureType   | The type of measure installed |
| WorkCarriedOutByTMLN | The TMLN of the business who installed the measure |
| RegisteredBusinessName | The name of the business who installed the measure |
| WorkTypeCode  | The Work Type Code (DW) of the measure |
| UMR | A new measure has been added |

