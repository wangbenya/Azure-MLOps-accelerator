---
sort: 5
---
# MLOps Maturity Model

**Source**: [Machine Learning Operations maturity model](https://docs.microsoft.com/en-us/azure/architecture/example-scenario/mlops/mlops-maturity-model)

The purpose of this maturity model is to help clarify the Machine Learning Operations (MLOps) principles and practices. The maturity model shows the continuous improvement in the creation and operation of a production level machine learning application environment. You can use it as a metric for establishing the progressive requirements needed to measure the maturity of a machine learning production environment and its associated processes.

## Maturity model

The MLOps maturity model helps clarify the Development Operations (DevOps) principles and practices necessary to run a successful MLOps environment. It's intended to identify gaps in an existing organization's attempt to implement such an environment. It's also a way to show you how to grow your MLOps capability in increments rather than overwhelm you with the requirements of a fully mature environment. Use it as a guide to:

* Estimate the scope of the work for new engagements.

* Establish realistic success criteria.

* Identify deliverables you'll hand over at the conclusion of the engagement.

As with most maturity models, the MLOps maturity model qualitatively assesses people/culture, processes/structures, and objects/technology. As the maturity level increases, the probability increases that incidents or errors will lead to improvements in the quality of the development and production processes.

The MLOps maturity model encompasses five levels of technical capability:

| Level | Description | Highlights | Technology |
| ----- | ----------- | ---------- | ---------- |
| 0 | [No MLOps](#level-0-no-mlops) | <ul><li>Difficult to manage full machine learning model lifecycle</li><li>The teams are disparate and releases are painful</li><li>Most systems exist as "black boxes," little feedback during/post deployment</li></ul> | <ul><li>Manual builds and deployments</li><li>Manual testing of model and application</li><li>No centralized tracking of model performance</li><li>Training of model is manual</ul> |
| 1 | [DevOps but no MLOps](#level-1-devops-no-mlops) | <ul><li>Releases are less painful than No MLOps, but rely on Data Team for every new model</li><li>Still limited feedback on how well a model performs in production</li><li>Difficult to trace/reproduce results</li></ul> | <ul><li>Automated builds</li><li>Automated tests for application code</li></ul> |
| 2 | [Automated Training](#level-2-automated-training) | <ul><li>Training environment is fully managed and traceable</li><li>Easy to reproduce model</li><li>Releases are manual, but low friction</li></ul> | <ul><li>Automated model training</li><li>Centralized tracking of model training performance</li><li>Model management</li></ul> |
| 3 | [Automated Model Deployment](#level-3-automated-model-deployment) | <ul><li>Releases are low friction and automatic</li><li>Full traceability from deployment back to original data</li><li>Entire environment managed: train > test > production </li></ul>| <ul><li>Integrated A/B testing of model performance for deployment</li><li>Automated tests for all code</li><li>Centralized training of model training performance</li></ul> |
| 4 | [Full MLOps Automated Operations](#level-4-full-mlops-automated-retraining) | <ul><li>Full system automated and easily monitored</li><li>Production systems are providing information on how to improve and, in some cases, automatically improve with new models<li>Approaching a zero-downtime system </li></ul>| <ul><li>Automated model training and testing</li><li>Verbose, centralized metrics from deployed model</li></ul> |

The tables that follow identify the detailed characteristics for that level of process maturity. The model will continue to evolve. This version was last updated in January 2020.

## Level 0: No MLOps

| People | Model Creation | Model Release | Application Integration |
| ------ | -------------- | ------------- | ----------------------- |
| <ul><li>Data scientists: siloed, not in regular communications with the larger team</li><li>Data engineers (_if exists_): siloed, not in regular communications with the larger team</li><li>Software engineers: siloed, receive model remotely from the other team members</li></ul> | <ul><li>Data gathered manually</li><li>Compute is likely not managed</li><li>Experiments aren't predictably tracked</li><li>End result may be a single model file manually handed off with inputs/outputs</li></ul> | <ul><li>Manual process</li><li>Scoring script may be manually created well after experiments, not version controlled</li><li>Release handled by data scientist or data engineer alone</li></ul> | <ul><li>Heavily reliant on data scientist expertise to implement</li><li>Manual releases each time</li></ul> |

## Level 1: DevOps no MLOps

| People | Model Creation | Model Release | Application Integration |
| ------ | -------------- | ------------- | ----------------------- |
| <ul><li>Data scientists: siloed, not in regular communications with the larger team</li><li>Data engineers (if exists): siloed, not in regular communication with the larger team</li><li>Software engineers: siloed, receive model remotely from the other team members</li></ul> | <ul><li>Data pipeline gathers data automatically</li><li>Compute is or isn't managed</li><li>Experiments aren't predictably tracked</li><li>End result may be a single model file manually handed off with inputs/outputs</li></ul> | <ul><li>Manual process</li><li>Scoring script may be manually created well after experiments, likely version controlled</li><li>Is handed off to software engineers</li></ul> | <ul><li>Basic integration tests exist for the model</li><li>Heavily reliant on data scientist expertise to implement model<li>Releases automated</li><li>Application code has unit tests</li></ul> |

## Level 2: Automated Training

| People | Model Creation | Model Release | Application Integration |
| ------ | -------------- | ------------- | ----------------------- |
| <ul><li>Data scientists: Working directly with data engineers to convert experimentation code into repeatable scripts/jobs</li><li>Data engineers: Working with data scientists</li><li>Software engineers: siloed, receive model remotely from the other team members</li></ul> | <ul><li>Data pipeline gathers data automatically</li><li>Compute managed</li><li>Experiment results tracked<li>Both training code and resulting models are version controlled</li></ul> | <ul><li>Manual release</li><li>Scoring script is version controlled with tests</li><li>Release managed by Software engineering team</li></ul> | <ul><li>Basic integration tests exist for the model</li><li>Heavily reliant on data scientist expertise to implement model<li>Application code has unit tests</li></ul> |

## Level 3: Automated Model Deployment

| People | Model Creation | Model Release | Application Integration |
| ------ | -------------- | ------------- | ----------------------- |
| <ul><li>Data scientists: Working directly with data engineers to convert experimentation code into repeatable scripts/jobs</li><li>Data engineers: Working with data scientists and software engineers to manage inputs/outputs</li><li>Software engineers: Working with data engineers to automate model integration into application code</li></ul> | <ul><li>Data pipeline gathers data automatically</li><li>Compute managed</li><li>Experiment results tracked<li>Both training code and resulting models are version controlled</li></ul> | <ul><li>Automatic release<li>Scoring script is version controlled with tests</li><li>Release managed by continuous delivery (CI/CD) pipeline</li></ul> | <ul><li>Unit and integration tests for each model release</li><li>Less reliant on data scientist expertise to implement model</li><li>Application code has unit/integration tests</li></ul> |

## Level 4: Full MLOps Automated Retraining

| People | Model Creation | Model Release | Application Integration |
| ------ | -------------- | ------------- | ----------------------- |
| <ul><li>Data scientists: Working directly with data engineers to convert experimentation code into repeatable scripts/jobs. Working with software engineers to identify markers for data engineers</li><li>Data engineers: Working with data scientists and software engineers to manage inputs/outputs<li>Software engineers: Working with data engineers to automate model integration into application code. Implementing post-deployment metrics gathering</li></ul> | <ul><li>Data pipeline gathers data automatically</li><li>Retraining triggered automatically based on production metrics<li>Compute managed</li><li>Experiment results tracked<li>Both training code and resulting models are version controlled</li></ul> | <ul><li>Automatic Release<li>Scoring Script is version controlled with tests</li><li>Release managed by continuous integration and CI/CD pipeline</li></ul> | <ul><li>Unit and Integration tests for each model release</li><li>Less reliant on data scientist expertise to implement model</li><li>Application code has unit/integration tests</li></ul> |
