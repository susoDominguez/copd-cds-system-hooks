# <mark>`copd-recommendations-suggest`</mark>

| Metadata | Value
| ---- | ----
| specificationVersion | 1.0
| hookVersion | 1.0
| hookMaturity | [0 - Draft](../../specification/1.0/#hook-maturity-model)

## Workflow

<mark>The `copd-recommendations-suggest` hook fires when a practitioner has reviewed the airflow limitation severity of a patient, by identifying one of the 
four COPD groups and selecting one or more drug types that are suitable for the COPD group and the patient at hand. The CDS Service then provides a collection
of personalised guidelines based on GOLD COPD 2017 where each of which contains non-conflictive clinical recommendations taking into account practitioner/patient choices -COPD group and medication-, immunization, lifestyle -smoker status- and comorbidities potentially affecting COPD treatment.</mark>

## Context

<mark></mark>

Field | Optionality | Prefetch Token | Type | Description
----- | -------- | ---- | ---- | ----
<mark>`patientID`</mark> | REQUIRED | Yes | *object* | <mark>identifier of current patient</mark>
<mark>`userId`</mark> | OPTIONAL | Yes | *string* | <mark>identifier of current practitioner</mark>
<mark>`encounterId`</mark> | OPTIONAL | Yes | *string* | <mark>identifier of current encounter</mark>
<mark>`birthDate`</mark> | REQUIRED | No | *string* | <mark>date of birth of patient, used to identify whether Pneumococcal vaccine should be administered on patients of sixty-five years of age or older</mark>
<mark>`currentSmoker`</mark> | REQUIRED | No | *boolean* | <mark>identifies current patient as a regular smoker or otherwise</mark>
<mark>`comorbidities`</mark> | OPTIONAL | No | *object* | <mark>FHIR Bundle with conditions related to history of cardiovascular and renal disease</mark>
<mark>`immunization`</mark> | REQUIRED | No | *object* | <mark>FHIR Bundle for immunization resources related to Influenza and Pneumococcal vaccines. Status is either completed or not-completed</mark>
<mark>`copdGroup`</mark> | REQUIRED | No | *object* | <mark>FHIR Bundle containing reviewed COPD group and selected potential treatment(s)</mark>

### Examples


```json
{
    "context" : { 
        "patientId" : "18", 
        "userId" : "Practitioner/123456", 
        "encounterId" : "987654",
        "birthDate" : "1954-01-01T00:00:00-00:00",
        "currentSmoker" : true,
        "comorbidities" : { 
            "resourceType" : "Bundle",
            "entry" :[
            {
                "resource" : {
                    "resourceType": "Condition",
                    "id": "nephrotic",
                    "code": {
                      "coding": [
                        {
                          "system": "https://mediately.co/hr/icd",
                          "code": "???",
                          "display": "Nephrotic"
                        }
                      ]
                    },
                    "subject": {
                      "reference": "Patient/38",
                      "display": "Link to patient"
                    },
                    "recordedDate": "2019-11-21T16:06:00-00:00"
                  }        
            },
            {
                "resource" : {
                    "resourceType": "Condition",
                    "id": "cardiovascular_disease",
                    "code": {
                      "coding": [
                        {
                          "system": "https://mediately.co/hr/icd",
                          "code": "???",
                          "display": "Cardiovascular disease"
                        }
                      ]
                    },
                    "subject": {
                      "reference": "Patient/38",
                      "display": "Link to patient"
                    },
                    "recordedDate": "2019-11-21T16:06:00-00:00"
                  }
            }
            ]
        },
        "immunization" : { 
            "resourceType" : "Bundle",
            "entry" :[
            {
                "resource" : {
                    "resourceType": "Immunization",
                    "id": "influenza",
                    "status": "completed",
                    "vaccineCode": {
                        "coding": [
                            {
                            "system": "https://mediately.co/rs/atcs/J07BB/vakcine-protiv-influence",
                            "code": "J07BB",
                            "display": "Seasonal flu vaccine"
                            }
                        ]
                    },
                    "patient": {
                        "reference": "Patient/38",
                    "display": "Link to patient"
                    },
                    "occurrenceDateTime": "2019-11-21T16:06:00-00:00"
                }
            },
            {
                "resource" : {
                    "resourceType": "Immunization",
                    "id": "influenza",
                    "status": "completed",
                    "vaccineCode": {
                      "coding": [
                        {
                          "system": "https://mediately.co/rs/atcs/J07BB/vakcine-protiv-influence",
                          "code": "J07BB",
                          "display": "Seasonal flu vaccine"
                        }
                      ]
                    },
                    "patient": {
                      "reference": "Patient/38",
                      "display": "Link to patient"
                    },
                    "occurrenceDateTime": "2019-11-21T16:06:00-00:00"
                  }
            }
            ]
        },
        "copdGroup" : {
            "resourceType" : "Bundle",
            "entry" :[
                {
                    "resource" : {
                        "resourceType": "Observation",
                        "id": "copd_group",
                        "status": "preliminary",
                        "valueString": "B",
                        "code": {
                          "coding": [
                            {
                              "system": "https://www.mdcalc.com/global-initiative-obstructive-lung-disease-gold-criteria-copd",
                              "code": "???",
                              "display": "Suggested COPD group"
                            }
                          ]
                        },
                        "effectiveDateTime": "2019-11-21T16:06:00-00:00",
                        "referenceRange": [
                          {
                            "text": "A,B,C,D"
                          }
                        ]
                      }
                },
                {
                "resource" : {
                    "resourceType": "Medication",
                    "id": "suggested_treatments",
                    "code": {
                      "coding": [
                        {
                          "code": "Laba",
                          "display": "LABA"
                        },
                        {
                          "code": "Lama",
                          "display": "LAMA"
                        },
                        {
                          "code": "LabaLama",
                          "display": "LABA + LAMA"
                        }
                      ],
                      "text": "Suggested tretments by DSS"
                    }
                  }
                }
            ]
        }
    }
}
```

## Change Log

Version | Description
---- | ----
1.0 | Initial Release
