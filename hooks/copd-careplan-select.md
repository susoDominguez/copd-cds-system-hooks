# <mark>`copd-careplan-select`</mark>

| Metadata             | Value                                                     |
| -------------------- | --------------------------------------------------------- |
| specificationVersion | 2.2                                                       |
| hookVersion          | 1.3                                                       |
| hookMaturity         | [0 - Draft](../../specification/1.0/#hook-maturity-model) |

## Workflow

<mark>The `copd-careplan-select` hook is triggered when the pulmonologist has assessed the COPD case of the patient, assigning a GOLD 2017 COPD group (A, B, C, D) as well as selecting one or more suitable COPD treatment pathways (SABA, SAMA, LABA, LAMA, etc.). When all the required patient information has been submitted, the associated CDS service responds with a collection of personalised care plan proposals, based on GOLD 2017 Clinical Guideline (CG), where each proposal contains non-conflictive clinical recommendations that have been selected taking into account the pulmonologist/patient preferences on COPD treatments, as well as other factors such as immunization, lifestyle -e.g., smoking status- and active comorbidities present in the patient's record, in particular cardiovascular and chronic kidney diseases, which could potentially interfere with some COPD treatment.</mark>

## Context

<mark>The context contains the GOLD 20017 COPD group selected by the clinician, along with two collections of COPD drug types, and drug type combinations, one collection includes the treatments proposed by the pulmonologist alongside the patient, and the other includes the treatment preferences as suggested by the COPD CDS system as response to hook `copd-assess` on the active patient. Additionally, there is some lifestyle and demographics information, and comorbidities and immunization status data.</mark>

| Field                                          | Optionality | Prefetch Token | Type     | Description                                                                                                                                                                                                                                                                 |
| ---------------------------------------------- | ----------- | -------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark>`encounterId`</mark>                     | REQUIRED    | No             | _string_ | <mark>identifier of current encounter.</mark>                                                                                                                                                                                                                               |
| <mark>`patientId`</mark>                       | REQUIRED    | No             | _string_ | <mark>identifier of current patient.</mark>                                                                                                                                                                                                                                 |
| <mark>`birthDate`</mark>                       | REQUIRED    | No             | _string_ | <mark>date of birth of patient, used to identify whether Pneumococcal vaccine should be suggested for patients of 65 years of age or older.</mark>                                                                                                                          |
| <mark>`smokingStatus`</mark>                   | REQUIRED    | No             | _object_ | <mark>FHIR Observation which identifies current patient as either a regular smoker or not.</mark>                                                                                                                                                                           |
| <mark>`comorbidities`</mark>                   | OPTIONAL    | No             | _object_ | <mark>FHIR Bundle of Conditions representing CKD or CVD diagnosis which are currently active in the patient's record.</mark>                                                                                                                                                |
| <mark>`immunizationStatus`</mark>              | REQUIRED    | No             | _object_ | <mark>FHIR Bundle of Immunizations denoting whether the patient has taken the annual influenza vaccine, or the pneumococcal vaccine if 65 years of age or older.</mark>                                                                                                     |
| <mark>`copdAssessment`</mark>                  | REQUIRED    | No             | _object_ | <mark>FHIR Bundle composed of Observation and a Medication Bundle, representing the identified GOLD 2017 COPD group, and clinician selected COPD drug types -implemented as Medication resources- being taking into consideration to treat the patient,respectively.</mark> |
| <mark>`suggestedTreatmentsByCdsService`</mark> | OPTIONAL    | No             | _object_ | <mark>FHIR Bundle of Medications as suggested by the COPD CDS service. This object is fed to the conflict resolution engine to provide alternative treatments to the selected ones in case conflicts arise with the selected choices.</mark>                                |

### Example

```json
{
  "hookInstance": "d1577c69-dfbe-44ad-ba6d-3e05e953b2ea",
  "hook": "copd-careplan-select",
  "context": {
    "encounterId": "285064-0",
    "patientId": "1677163",
    "birthDate": "1946-10-03T00:00:00+01:00",
    "smokingStatus": {
      "resourceType": "Observation",
      "id": "smoking_status",
      "status": "final",
      "code": {
        "coding": [
          {
            "system": "http://snomed.info/sct",
            "code": "229819007",
            "display": "Tobacco use and exposure (observable entity)"
          }
        ]
      },
      "valueCodeableConcept": {
        "coding": [
          {
            "system": "http://snomed.info/sct",
            "code": "77176002",
            "display": "Smoker"
          }
        ]
      },
      "referenceRange": [
        {
          "low": {
            "system": "http://snomed.info/sct",
            "code": "8392000",
            "display": "Non-smoker"
          }
        },
        {
          "high": {
            "system": "http://snomed.info/sct",
            "code": "77176002",
            "display": "Smoker"
          }
        }
      ]
    },
    "comorbidities": {
      "resourceType": "Bundle",
      "id": "comorbiditiesBundle",
      "type": "collection",
      "entry": [
        {
          "resource": {
            "resourceType": "Condition",
            "id": "cvd",
            "code": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "49601007",
                  "display": "Disorder of cardiovascular system"
                }
              ]
            },
            "subject": {
              "reference": "Patient/1677163"
            }
          }
        }
      ]
    },
    "immunizationStatus": {
      "resourceType": "Bundle",
      "id": "immunizationStatusBundle",
      "type": "collection",
      "entry": [
        {
          "resource": {
            "resourceType": "Immunization",
            "id": "influenza_immunization",
            "status": "completed",
            "vaccineCode": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "1181000221105",
                  "display": "Vaccine product containing only Influenza virus antigen"
                }
              ]
            },
            "occurrenceDateTime": "2022-09-05T18:00:00+01:00"
          }
        },
        {
          "resource": {
            "resourceType": "Immunization",
            "id": "pneumococcal_immunization",
            "status": "not-done",
            "vaccineCode": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "333598008",
                  "display": "Pneumococcal vaccine"
                }
              ]
            }
          }
        }
      ]
    },
    "copdAssessment": {
      "resourceType": "Bundle",
      "id": "copdAssessmentBundle",
      "type": "collection",
      "entry": [
        {
          "resource": {
            "resourceType": "Observation",
            "id": "copd_group_curr",
            "status": "preliminary",
            "code": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "1097861000000108",
                  "display": "Global Initiative for Chronic Obstructive Lung Disease 2017 group"
                }
              ],
              "text": "COPD group"
            },
            "valueCodeableConcept": {
              "coding": [
                {
                  "system": "http://snomed.info/sct",
                  "code": "1097881000000104",
                  "display": "Global Initiative for Chronic Obstructive Lung Disease 2017 group B"
                }
              ]
            },
            "referenceRange": [
              {
                "low": {
                  "system": "http://snomed.info/sct",
                  "code": "1097871000000101",
                  "display": "Global Initiative for Chronic Obstructive Lung Disease 2017 group A"
                }
              },
              {
                "low": {
                  "system": "http://snomed.info/sct",
                  "code": "1097881000000104",
                  "display": "Global Initiative for Chronic Obstructive Lung Disease 2017 group B"
                }
              },
              {
                "high": {
                  "system": "http://snomed.info/sct",
                  "code": "1097891000000102",
                  "display": "Global Initiative for Chronic Obstructive Lung Disease 2017 group C"
                }
              },
              {
                "high": {
                  "system": "http://snomed.info/sct",
                  "code": "1097901000000101",
                  "display": "Global Initiative for Chronic Obstructive Lung Disease 2017 group D"
                }
              }
            ]
          }
        },
        {
          "resource": {
            "resourceType": "Bundle",
            "id": "selected_treatments",
            "type": "collection",
            "entry": [
              {
                "resource": {
                  "resourceType": "Medication",
                  "id": "DrugTLaba",
                  "code": {
                    "coding": [
                      {
                        "code": "Laba",
                        "display": "administer medication containing LABA"
                      }
                    ]
                  }
                }
              },
              {
                "resource": {
                  "resourceType": "Medication",
                  "id": "DrugTLama",
                  "code": {
                    "coding": [
                      {
                        "system": "http://anonymous.org/data/DrugTLama",
                        "code": "Lama",
                        "display": "administer medication containing a  of LAMA"
                      }
                    ]
                  }
                }
              }
            ]
          }
        }
      ]
    },
    "suggestedTreatmentsByCdsService": {
      "resourceType": "Bundle",
      "id": "suggested_treatments",
      "type": "collection",
      "entry": [
        {
          "resource": {
            "resourceType": "Medication",
            "id": "DrugTLaba",
            "code": {
              "coding": [
                {
                  "code": "Laba",
                  "display": "administer medication containing LABA"
                }
              ]
            }
          }
        },
        {
          "resource": {
            "resourceType": "Medication",
            "id": "DrugTLama",
            "code": {
              "coding": [
                {
                  "system": "http://anonymous.org/data/DrugTLama",
                  "code": "Lama",
                  "display": "administer medication containing a  of LAMA"
                }
              ]
            }
          }
        },
        {
          "resource": {
            "resourceType": "Medication",
            "id": "DrugCatLabaLama",
            "code": {
              "coding": [
                {
                  "system": "http://anonymous.org/data/DrugCatLabaLama",
                  "code": "LabaLama",
                  "display": "administer medication containing a combination of LABA and LAMA"
                }
              ]
            }
          }
        }
      ]
    }
  }
}
```

## Change Log

| Version | Description        |
| ------- | ------------------ |
| 2.1     | tokenised approach |
