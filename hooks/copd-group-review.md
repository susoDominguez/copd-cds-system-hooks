# <mark>`copd-group-review`</mark>

| Metadata | Value
| ---- | ----
| specificationVersion | 1.0
| hookVersion | 1.0
| hookMaturity | [0 - Draft](../../specification/1.0/#hook-maturity-model)

## Workflow

<mark>The practicioner is in the process of reviewing the severity of the airflow limitation on the current patient to adjust medication, if needed. Measurements of current CAT score and mMRC dyspnoea scale have been taken at the current encounter and are sent along with previous measurements and other required COPD-related data such as comorbidities and number of exacerbations recorded since the last COPD review. The CDS service responds with a preliminary linearly ordered set of the four COPD groups and their corresponding drug types, which are customised for the current patient.</mark>

## Context

<mark>The set of resources used to identify the current state of the patient's COPD. These resources identifiers are asthma, COPD group recorded at previous COPD encounter, number of COPD exacerbations recorded at the last COPD encounter as well as exacerbations recorded between this COPD encounter and the previous one, and scores with a status of final for CAT and mMRC measurements recorded at the previous COPD encounter.</mark>

Field | Optionality | Prefetch Token | Type | Description
----- | -------- | ---- | ---- | ----
<mark>`patientId`</mark> | REQUIRED | Yes | *string* | <mark>identifier of current patient</mark>
<mark>`userId`</mark> | OPTIONAL | Yes | *string* | <mark>identifier of current practitioner</mark>
<mark>`encounterId`</mark> | OPTIONAL | Yes | *string* | <mark>identifier of current encounter</mark>
<mark>`prevMeasurements`</mark> | REQUIRED | No | *object* | <mark>FHIR Bundle of Observation and Condition resource types for identifiers related to COPD group, mMRC Dyspnoea scale, CAT score and number of COPD-related exacerbations, all recorded at the immediately previous COPD group review.</mark>
<mark>`currMeasurements`</mark> | REQUIRED | No | *object* | <mark>FHIR Bundle of Observation and Condition resource types for identifiers related to mMRC Dyspnoea scale, CAT score and number of COPD-related exacerbations, all recorded at the current encounter.</mark>
<mark>`comorbidity`</mark> | OPTIONAL | No | *object* | <mark>Asthma Condition resource type.</mark>

### Examples


```json
{
    "context" : { 
        "patientId" : "18", 
        "userId" : "Practitioner/123456", 
        "encounterId" : "987654",
        "prevMeasurements" : { 
            "resourceType" : "Bundle",
            "entry" :[
                {
                    "resource": {
                        "resourceType": "Observation",
                        "id": "cat_score_previous",
                        "status": "final",
                        "valueInteger": 24,
                        "code": {
                            "coding": [
                                {
                                    "system": "https://www.mdcalc.com/copd-assessment-test-cat",
                                    "display": "CAT score previous"
                                }
                            ]
                    },
                    "effectiveDateTime": "2019-05-18T14:16:00-00:00",
                    "referenceRange": [
                        {
                            "low": {
                                "value": 0.0
                            }
                        },
                        {
                            "high": {
                                "value": 35.0
                            }
                        }
                    ]
                }
            },
            {
                "resource" : {
                    "resourceType": "Observation",
                    "id": "mmrc_previous",
                    "status": "final",
                    "valueInteger": 3,
                    "code": {
                        "coding": [
                            {
                                "system": "https://www.mdcalc.com/mmrc-modified-medical-research-council-dyspnea-scale",
                                "display": "mMRC previous"
                            }
                        ]
                    },
                    "effectiveDateTime": "2019-05-18T14:16:00-00:00",
                    "referenceRange": [
                        {
                            "low": {
                                "value": 0.0
                            }
                        },
                        {
                            "high": {
                                "value": 4.0
                            }
                        }
                    ]
                }
            },
            {
                "resource" : {
                    "resourceType": "Observation",
                    "id": "copd_exacerbation_from_last_visit",
                    "status": "final",
                    "valueInteger": 0,
                    "code": {
                        "coding": [
                            {
                                "system": "https://mediately.co/hr/icd/J00-J99/set/J40-J47/cls/J44.1/kroni%C4%8Dna-opstruktivna-plu%C4%87na-bolest-s-akutnom-egzarcerbacijom-nespecificirana?q=j44.1",
                                "code": "J44.1",
                                "display": "COPD exacerbation from last visit"
                            }
                        ]
                    },
                    "effectiveDateTime": "2019-05-18T14:16:00-00:00"
                }
            },
            {
                "resource" : {
                    "resourceType": "Observation",
                    "id": "copd_group_previous",
                    "status": "final",
                    "valueString": "B",
                        "code": {
                            "coding": [
                                {
                                    "system": "https://www.mdcalc.com/global-initiative-obstructive-lung-disease-gold-criteria-copd",
                                    "display": "COPD group previous"
                                }
                            ]
                        },
                    "effectiveDateTime": "2019-05-18T14:16:00-00:00",
                    "referenceRange": [
                        {
                            "text": "A,B,C,D"
                        }
                    ]
                }
            }
            ]
        },
        "currMeasurements" : { 
            "resourceType" : "Bundle",
            "entry" :[
                {
                    "resource": {
                        "resourceType": "Observation",
                        "id": "cat_score",
                        "status": "preliminary",
                        "valueInteger": 1,
                        "code": {
                            "coding": [
                                {
                                    "system": "https://www.mdcalc.com/copd-assessment-test-cat",
                                    "display": "CAT score"
                                }
                            ]
                    },
                    "effectiveDateTime": "2019-11-21T15:44:00-00:00",
                    "referenceRange": [
                        {
                            "low": {
                                "value": 0.0
                            }
                        },
                        {
                            "high": {
                                "value": 35.0
                            }
                        }
                    ]
                }
            },
            {
                "resource" : {
                    "resourceType": "Observation",
                    "id": "mmrc",
                    "status": "preliminary",
                    "valueInteger": 0,
                    "code": {
                        "coding": [
                            {
                                "system": "https://www.mdcalc.com/mmrc-modified-medical-research-council-dyspnea-scale",
                                "display": "mMRC"
                            }
                        ]
                    },
                    "effectiveDateTime": "2019-11-21T15:44:00-00:00",
                    "referenceRange": [
                        {
                            "low": {
                                "value": 0.0
                            }
                        },
                        {
                            "high": {
                                "value": 4.0
                            }
                        }
                    ]
                }
            },
            {
                "resource" : {
                    "resourceType": "Observation",
                    "id": "copd_exacerbation_from_now",
                    "status": "final",
                    "valueInteger": 0,
                    "code": {
                        "coding": [
                            {
                                "system": "https://mediately.co/hr/icd/J00-J99/set/J40-J47/cls/J44.1/kroni%C4%8Dna-opstruktivna-plu%C4%87na-bolest-s-akutnom-egzarcerbacijom-nespecificirana?q=j44.1",
                                "code": "J44.1",
                                "display": "COPD exacerbation from now"
                            }
                        ]
                    },
                    "effectiveDateTime": "2019-11-21T15:44:00-00:00"
                }
            }
            ]
        },
        "comorbidity" : {
            "resource" : 
            {
                "resourceType": "Condition",
                "id": "asthma",
                "code": {
                    "coding": [
                        {
                            "system": "https://mediately.co/hr/icd/J00-J99/set/J40-J47/cls/J45/astma?q=j45",
                            "code": "J45",
                            "display": "Asthma"
                        }
                    ]
                },
                "subject": {
                    "reference": "Patient/38",
                    "display": "Link to patient"
                },
                "recordedDate": "2019-11-21T15:44:00-00:00"
            }
        }
    }
}
```

## Change Log

Version | Description
---- | ----
1.0 | Initial Release
