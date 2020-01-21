# <mark>`copd-recommendations-suggest`</mark>

| Metadata | Value
| ---- | ----
| specificationVersion | 1.0
| hookVersion | 1.0
| hookMaturity | [0 - Draft](../../specification/1.0/#hook-maturity-model)

## Workflow

<mark>The `copd-recommendations-suggest` hook fires when a practitioner has reviewed the airflow limitation severity of a patient, by identifying one of the 
four COPD groups and selecting one or more drug types that are suitable for the COPD group and the patient at hand. The CDS Service then provides a collection
of clinical recommendation sets where each of which contains non-conflictive care actions, taken into account comorbidities specified in guideline GOLD COPD 2017.</mark>

## Context

<mark></mark>

Field | Optionality | Prefetch Token | Type | Description
----- | -------- | ---- | ---- | ----
<mark>`patient`</mark> | REQUIRED | Yes | *object* | <mark>patient demographics, with a particular interest on patient Id and birthDate</mark>
<mark>`userId`</mark> | OPTIONAL | Yes | *string* | <mark>identifier of current practitioner</mark>
<mark>`encounterId`</mark> | OPTIONAL | Yes | *string* | <mark>identifier of current encounter</mark>

### Examples


```json
{}
```

## Change Log

Version | Description
---- | ----
1.0 | Initial Release
