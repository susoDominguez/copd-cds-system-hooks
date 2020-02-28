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
<mark>`copdGroup_prev`</mark> | OPTIONAL | No | *Observation* | <mark>resource with 'final' status representing a patient's COPD group as recorded at previous COPD-related encounter. Omission of this resource suggests  patient has just been newly diagnosed with COPD at this encounter.</mark>
<mark>`mMrcScale_prev`</mark> | OPTIONAL | No | *Observation* | <mark>resource with 'final' status representing the mMRC Dyspnoea scale as recorded at previous COPD-related encounter. Omission of this resource suggests  patient has just been newly diagnosed with COPD at this encounter.</mark>
<mark>`catScore_prev`</mark> | OPTIONAL | No | *Observation* | <mark>resource with 'final' status representing the CAT score as recorded at previous COPD-related encounter. Omission of this resource suggests patient has just been newly diagnosed with COPD at this encounter.</mark>
<mark>`exacerbations_prev`</mark> | OPTIONAL | No | *integer*\|*string* | <mark>value representing the number of COPD-related exacerbations as recorded at previous COPD-related encounter. Omission of this resource suggests patient has just been newly diagnosed with COPD at this encounter.</mark>
<mark>`mMrcScale_curr`</mark> | REQUIRED | No | *Observation* | <mark>resource representing a patient's mMRC Dyspnoea scale as recorded at current encounter.</mark>
<mark>`catScore_curr`</mark> | REQUIRED | No | *Observation* | <mark>resource  representing a patient's CAT score as recorded at current encounter.</mark>
<mark>`exacerbations_curr`</mark> | REQUIRED | No | *integer*\|*string* | <mark>value representing the number of COPD-related exacerbations recorded between this and the previous COPD-related encounter.</mark>
<mark>`asthmaCondition`</mark> | OPTIONAL | No | *Condition* | <mark>resource denoting the presence of Asthma in patient's record.</mark>

### Examples


```json
{
    "context" : { 
        "patientId" : "18", 
        "userId" : "Practitioner/123456", 
        "encounterId" : "987654",
        "copdGroup_prev" : {
			  "resourceType": "Observation",
			  "id": "copd_group_previous",
			  "status": "final",
			  "code": {
				"coding": [
				  {
					"system": "https://www.mdcalc.com/global-initiative-obstructive-lung-disease-gold-criteria-copd",
					"code": "???",
					"display": "Suggested COPD group"
				  }
				]
			  },
			  "effectiveDateTime": "2016-05-18T13:17:00+02:00",
			  "valueString" : "C"
		 },
        "mMrcScale_prev" : {
			"resourceType": "Observation",
			  "id": "mMRC_previous",
			  "status": "final",
			  "code": {
				"coding": [
				  {
					"system": "https://www.mdcalc.com/mmrc-modified-medical-research-council-dyspnea-scale",
					"code": "???",
					"display": "mMrc (Modified Medical Research Council)"
				  }
				]
			  },
			  "effectiveDateTime": "2016-05-18T13:17:00+02:00",
			  "valueInteger" : "3",
			  "referenceRange": [
				{
				  "low": {
					"value": "0"
				  }
				},
				{
				  "high": {
					"value": "4"
				  }
				}
			  ]
		  },
        "catScore_prev" : {
			  "resourceType": "Observation",
			  "id": "cat_score_previous",
			  "status": "final",
			  "code": {
				"coding": [
				  {
					"system": "https://www.mdcalc.com/copd-assessment-test-cat",
					"code": "???",
					"display": "Cat score"
				  }
				]
			  },
			  "effectiveDateTime": "2017-08-26T13:17:00+02:00",
			  "valueInteger" : "15",
			  "referenceRange": [
				{
				  "low": {
					"value": "0"
				  }
				},
				{
				  "high": {
					"value": "35"
				  }
				}
			  ]
		    },
        "exacerbations_prev" : {
			  "resourceType": "Observation",
			  "id": "copd_exacerbation",
			  "status": "final",
			  "code": {
				"coding": [
				  {
					"code": "J44.1",
					"display": "Number of COPD exacerbation (J44.1) in the last 12 moths."
				  }
				]
			  },
			  "effectiveDateTime": "2018-11-06T14:47:00+02:00",
			  "valueInteger" : "0"
		   },
        "mMrcScale_curr" : {
			  "resourceType": "Observation",
			  "id": "mMRC",
			  "status": "preliminary",
			  "code": {
				"coding": [
				  {
					"system": "https://www.mdcalc.com/mmrc-modified-medical-research-council-dyspnea-scale",
					"code": "???",
					"display": "mMrc (Modified Medical Research Council)"
				  }
				]
			  },
			  "effectiveDateTime": "2019-11-06T14:47:00+02:00",
			  "valueInteger" : "2",
			  "referenceRange": [
				{
				  "low": {
					"value": "0"
				  }
				},
				{
				  "high": {
					"value": "4"
				  }
				}
			  ]
		    },
        "catScore_curr" : {
			  "resourceType": "Observation",
			  "id": "cat_score",
			  "status": "preliminary",
			  "code": {
				"coding": [
				  {
					"system": "https://www.mdcalc.com/copd-assessment-test-cat",
					"code": "???",
					"display": "Cat score"
				  }
				]
			  },
			  "effectiveDateTime": "2019-11-06T14:47:00+02:00",
			  "valueInteger" : "10",
			  "referenceRange": [
				{
				  "low": {
					"value": "0"
				  }
				},
				{
				  "high": {
					"value": "35"
				  }
				}
			  ]
		   },
        "exacerbations_curr" : {
			  "resourceType": "Observation",
			  "id": "copd_exacerbation",
			  "status": "preliminary",
			  "code": {
				"coding": [
				  {
					"code": "J44.1",
					"display": "Number of COPD exacerbation (J44.1) in the last 12 moths."
				  }
				]
			  },
			  "effectiveDateTime": "2019-11-06T14:47:00+02:00",
			  "valueInteger" : "0"
		   },
        "asthmaCondition" : {
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
				"reference": "Patient/1677163",
				"display": "Link to patient"
			  },
			  "recordedDate": "2016-05-18T13:17:00+02:00"
		  }
    }
}
```

## Change Log

Version | Description
---- | ----
1.2 | Tokenised parameters
