# ROAD2H-hooks
Documentation for a pair of CDS hooks to trigger notifications for remote decision support on the management of patients with Chronic Obstructive Pulmonary Disease (COPD) (and possibly with Cardiovascular Disease (CVD) and/or Chronic Kidney Disease (CKD) comorbidities). The CDS services provided -as part of the COPD CDS system- are the assessment of the COPD severity in the patient and a personalised COPD treatment planning that takes into account potential conflicts with CVD and CKD comorbidities.

The CDS hooks habe been developed as part of the ROAD2H project.

````
http://www.road2h.org/

`````
## hooks folder
The folder contains the pair of hook specifications, with examples, to trigger CDS services for COPD management. The hook with ID 'copd-assess' provides patient information on previous and current COPD symptoms, and is associated with the COPD severity assessment CDS service. The hook with ID 'copd-careplan-select' provides patient information on active COPD treatments, the current pulmonologist-verified COPD severity assessment as well as on the status of current vaccinations and active comorbidities. This hook is associated with the COPD treatment planning CDS service.

## cards folder
Contains CDS card responses to the examples displayed in the CDS hook definitions.

## FHIR ForecastEffect folder
This folder contains the schema for the new FHIR type, ForecastEffect, that describes the expected effects of a care action on the overall health of a patient if a particular clinical treatment or service is administered. The FHIR forecastEffect permits mapping clinical suggestions in the form of Transition-based Medical Recommendations (TMRs) to FHIR types. FHIR does not include resources to describe potential outcomes.

## ROAD2H eval useCases folder 
This folder contains the use cases applied to the evaluation of the ROAD2H COPD CDS system. Additionally, there is an initial Dummy patient that is based on the running examples used in folders hooks and cards. Imagines with captions have also been added to this dummy example to demonstrate the graphical output of the cards on the EHR system of the pulmonologist.
