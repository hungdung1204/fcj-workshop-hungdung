---
title: "Blog 3"
date: 2026-07-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Reducing EC2 Waste with Automated Right-Sizing Using Compute Optimizer, CloudWatch, and Automation

## 1. EC2 cost is a common concern

When operating AWS infrastructure, EC2 cost is often one of the easiest areas to overspend. Teams may choose instance sizes that are larger than necessary, reuse old configurations, or forget to review capacity after workload changes. As a result, many instances run with more resources than they need.

![EC2 Right-Sizing Automation](/images/3-BlogsTranslated/blog3/image1.png)

## 2. Why waste happens

Common causes include:

* Overprovisioning to be safe.
* Lack of regular review.
* Manual processes where engineers must inspect metrics and decide changes one by one.

The result is unnecessary cost, inefficient resource usage, and wasted engineering time.

## 3. Automated right-sizing

The goal of automated right-sizing is to detect overprovisioned or underutilized instances and recommend or apply safe changes.

The proposed solution combines:

* **AWS Compute Optimizer:** analyzes CPU, memory, network, and disk I/O to recommend instance changes.
* **CloudWatch:** collects metrics, creates dashboards, and raises alarms.
* **Automation:** uses Lambda, SSM Automation, Systems Manager Run Command, or Step Functions to apply changes.
* **Governance:** requires approval before production changes and uses tagging plus audit trails.

## 4. Proposed workflow

1. Compute Optimizer collects 14-30 days of data and returns recommendations per instance.
2. A filter keeps high-confidence recommendations and excludes stateful, database, or critical instances based on tags.
3. The suggested instance type is tested in staging using AMIs or Launch Templates.
4. Production changes are applied through rolling updates or a controlled stop/modify/start process during a maintenance window.
5. CloudWatch monitors latency, error rate, CPU, and memory, then triggers rollback if thresholds are exceeded.

## 5. Quick implementation guide

* Enable Compute Optimizer for the account or organization.
* Wait at least 14 days for reliable data.
* Export recommendations to S3 or fetch them through API/SDK.
* Build a Lambda function or script to filter recommendations daily.
* Create an approval workflow through Slack or pull requests.
* Apply approved changes through SSM Automation or rolling updates.
* Configure CloudWatch dashboards and rollback alarms.

## 6. Deployment checklist

* Enable Compute Optimizer.
* Export recommendations automatically.
* Tag instances that must not be changed with `do-not-rightsize`.
* Filter recommendations by confidence, environment, and criticality.
* Test changes in staging.
* Store audit trails with CloudTrail and S3 logs.

## 7. When to apply this approach

This approach works well for stateless web servers, API servers, worker nodes, batch workers, Auto Scaling Group fleets, and workloads that can restart quickly.

It requires caution for production databases on EC2, workloads with local state, instances with hardware-bound licensing, and applications with strict boot or driver requirements.

## 8. Key lessons

Start with non-production workloads, standardize tagging and inventory, always prepare rollback, and combine automation with approval for important workloads. Right-sizing also works best when combined with Savings Plans and Reserved Instances for long-term cost optimization.

## 9. Example filtering policy

Only consider recommendations when confidence is at least 70%, the instance is not tagged as `prod-db`, uptime is long enough, median CPU is below 30%, and median memory is below 40% over 14 days.

Non-production changes can be auto-approved, while production changes should create a pull request or Slack notification and wait for manual approval.

## 10. Conclusion

Automated right-sizing is an effective way to reduce EC2 cost without sacrificing stability. By combining Compute Optimizer, CloudWatch, and safe automation, organizations can reduce waste, maintain performance, and make optimization repeatable and auditable.
