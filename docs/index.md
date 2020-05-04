# Overview

This workshop is designed to help you get familiar with AWS Security services and learn how to use them to identify and remediate threats in your environment. You'll be working with services such as Amazon GuardDuty (threat detection), Amazon Inspector (vulnerability & behavior analysis), AWS Security Hub (centralized security view). You will learn how to use these services to investigate threats during and after an attack, set up a notification and response pipeline, and add additional protections to improve the security posture of your environment.

* **Level**: Intermediate
* **Duration**: 2 - 3 hours
* **<a href="https://www.nist.gov/cyberframework/online-learning/components-framework" target="_blank">CSF Functions</a>**: Detect, Respond, Recover
* **<a href="https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf" target="_blank">CAF Components</a>**: Detective, Responsive

## Scenario

Your company is new to the cloud and has recently performed a lift-and-shift of your infrastructure for piloting purposes.  You are a systems administrator and have been tasked with security monitoring within your AWS environment.  As part of that maintenance you are also responsible for responding to any security event in your environment.

## Architecture

For this Workshop you will have a simple setup with a single instance setup in the us-west-2 region. As this was a “lift-and-shift” migration for piloting, you have yet to build redundancy into your application, so you have a single public-facing web server that is accessed through an internet gateway and retrieves static content from an S3 bucket.

![Architecture](./images/diagram-basic-arch-v2.png "Workload Architecture")

<!--## Presentation deck
[Workshop Presentation Deck](./threat-detect-workshop-presentation.pdf)-->

## Region
Please use the **us-west-2 (Oregon)** region for this workshop.

## Modules

This workshop is broken up into the four modules below:

1. [Environment Build and Configuration](./01-environment-setup.md)
2. [Attack Simulation](./02-attack-simulation.md)
3. [Detection and Remediation](./03-detection-and-remediation.md)
4. [Review and Discussion](./04-review-and-discussion.md)
