# <mark>`copd-group-review`</mark>

| Metadata | Value
| ---- | ----
| specificationVersion | 1.1
| hookVersion | 1.1
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
<mark>`copdGroup`</mark> | REQUIRED | No | *string* | <mark>String value representing the COPD group as recorded at previous COPD review.</mark>
<mark>`mMrcScale_prev`</mark> | REQUIRED | No | *integer*\|*string* | <mark>value representing the mMRC Dyspnoea scale as recorded at previous COPD review.</mark>
<mark>`catScore_prev`</mark> | REQUIRED | No | *integer*\|*string* | <mark>Integer value representing the CAT score as recorded at previous COPD review.</mark>
<mark>`exacerbations_prev`</mark> | REQUIRED | No | *integer*\|*string* | <mark>Integer value representing the number of COPD-related exacerbations as recorded at previous COPD review.</mark>
<mark>`mMrcScale_curr`</mark> | REQUIRED | No | *integer*\|*string* | <mark>Integer value representing the mMRC Dyspnoea scale as recorded at current COPD review.</mark>
<mark>`catScore_curr`</mark> | REQUIRED | No | *integer*\|*string* | <mark>Integer value representing the CAT score as recorded at current COPD review.</mark>
<mark>`exacerbations_curr`</mark> | REQUIRED | No | *integer*\|*string* | <mark>Integer value representing the number of COPD-related exacerbations as recorded at previous COPD review.</mark>
<mark>`isAsthmaPresent`</mark> | REQUIRED | No | *boolean*\|*integer*\|*string* | <mark>Is Asthma Condition resource type present in the patient record?. Either true\|false or 1\|0 or "true"\|"false" or "1"\|"0"</mark>

### Examples


```json
{
    "context" : { 
        "patientId" : "18", 
        "userId" : "Practitioner/123456", 
        "encounterId" : "987654",
        "copdGroup" : "B",
        "mMrcScale_prev" : 3,
        "catScore_prev" : 24,
        "exacerbations_prev" : 0,
        "mMrcScale_curr" : 0,
        "catScore_curr" : 1,
        "exacerbations_curr" : 0,
        "isAsthmaPresent" : true
    }
}
```

## Change Log

Version | Description
---- | ----
1.1 | Tokenised parameters
