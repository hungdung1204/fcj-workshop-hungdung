---
title: "Workshop"
date: 2026-07-04
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

This chapter presents the workshop for deploying the HaShop e-commerce platform on AWS. The structure follows a clear progression: project context and goals, architecture description, prerequisites, deployment, validation, and clean-up.

**5.1:** [Overview](5.1-Overview/)

Introduces the project background, problem statement, target users, expected outputs, and success criteria.

**5.2:** [Architecture Description](5.2-Architecture/)

Explains the architecture diagram, system layers, request flows, AWS services used, service selection rationale, security/IAM, logging/monitoring, alerts, cost optimization, and clean-up.

**5.3:** [Prerequisite](5.3-Prerequisite/)

Lists the AWS account permissions, regions, and local tools required before deployment.

**5.4:** [Deployment Guide](5.4-Deployment-guide/)

Provides the end-to-end deployment steps for Lambda artifacts, container images, CloudFormation stacks, and the frontend release.

**5.5:** [Test & validation](5.5-Test-validation/)

Validates the deployed frontend, authentication, ECS services, product management, order flow, CloudWatch logs, and WAF metrics.

**5.6:** [Clean-up](5.6-Clean-up/)

Removes the lab resources in the correct order to avoid unnecessary AWS costs.
