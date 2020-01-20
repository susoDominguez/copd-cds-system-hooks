# <mark>`copd-group-review`</mark>

| Metadata | Value
| ---- | ----
| specificationVersion | 1.0
| hookVersion | 1.0
| hookMaturity | [0 - Draft](../../specification/1.0/#hook-maturity-model)

## Workflow

<mark>The practicioner is in the process of reviewing the COPD group of the patient. Measurements of current CAT score and mMRC dyspnoea scale have been taken at the current encounter and sent along with previous measurements, and other COPD-related data, for comparison. A choice of COPD-related drug types specific to the patient and their COPD group is also returned by the CDS Service.</mark>

## Context

<mark>The set of resources used to identify the current state of the patient's COPD. These resources identifiers are asthma, COPD group recorded at previous COPD encounter, number of COPD exacerbations recorded at the last COPD encounter as well as exacerbations recorded between this COPD encounter and the previous one, and scores with a status of final for CAT and mMRC measurements recorded at the previous COPD encounter.</mark>

Field | Optionality | Prefetch Token | Type | Description
----- | -------- | ---- | ---- | ----
<mark>`PatientId`</mark> | REQUIRED | Yes | *string* | <mark>identifier of current patient</mark>
<mark>`userId`</mark> | OPTIONAL | Yes | *string* | <mark>identifier of current practitioner</mark>
<mark>`EncounterId`</mark> | OPTIONAL | Yes | *string* | <mark>identifier of current encounter</mark>
<mark>`Measurements`</mark> | REQUIRED | No | *object* | <mark>FHIR Bundle of Observation and Condition resource types for identifiers related to mMRC Dyspnoea scale, CAT score and number of COPD-related exacerbations recorded at previous COPD group review; mMRC and CAT scores MUST have a status of final. Also, it contains the currently recorded COPD group for the patient and the number of COPD-related exacerbations recorded between today and the previous COPD encounter. Additionally, it MAY contain a resource for asthma condition.
</mark>

### Examples

<mark>
```json
"context":{
  "patientId" : "1288992"
}
```

```json
"context":{
  "patientId" : "1288992",
  "encounterId" : "456"
}
```
</mark>

## Change Log

Version | Description
---- | ----
1.0 | Initial Release
