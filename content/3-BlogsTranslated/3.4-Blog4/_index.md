---
title: "Blog 4"
date: 2026-07-01
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

# Restricting AWS Management Console Access to Trusted Networks with Sign-in Resource-Based Policies and RCPs

In enterprise AWS environments, one of the key responsibilities of a security team is building a strong data perimeter. Service Control Policies (SCPs) are commonly used to control actions inside accounts, but one sensitive entry point is often difficult to protect: the AWS Management Console sign-in page.

If an employee's credentials are leaked, an attacker may still try to access the Console from anywhere on the Internet. AWS Security Blog introduced a stronger perimeter control using Sign-in Resource-Based Policies together with Resource Control Policies (RCPs).

## 1. Why blocking Console sign-in by IP used to be difficult

Previously, teams that wanted employees to sign in only from the corporate network or VPN had to deal with several challenges:

* Dependence on an identity provider such as Okta, Entra ID, or IAM Identity Center.
* SCPs mainly control what principals can do after they are already inside the AWS environment.
* Blocking `aws-signin:SignIn` directly with SCPs can create complex policy conflicts.

As a result, the AWS Console sign-in page could remain exposed to the public Internet, creating risk from brute-force attacks or leaked credentials.

## 2. AWS's new solution

AWS now allows Console sign-in to be protected as a resource.

**Sign-in Resource-based Policies** apply policy directly to the sign-in endpoint. For example, an organization can allow sign-in only when the request comes from approved source IP ranges or a specific VPC.

**Resource Control Policies (RCPs)** are an AWS Organizations policy type. While SCPs control what principals are allowed to do, RCPs control which resources can be accessed, by whom, and from where.

By applying RCPs at the AWS Organizations level, an enterprise can require all Console sign-ins into member accounts to originate from expected networks.

## 3. How the flow works

1. The user opens the AWS Management Console and submits credentials.
2. AWS Sign-in receives the request and extracts network context such as source IP, VPC Endpoint, or Verified Access information.
3. The Organization-level RCP evaluates the sign-in request.
4. The policy checks whether the source network is included in the expected networks.
5. If the network is not trusted, AWS denies the sign-in immediately. If it is trusted, the user continues to MFA and the normal SCP/IAM authorization layers.

## 4. Safe deployment checklist

* Identify all corporate public IP ranges, VPN/proxy ranges, and corporate VPC IDs.
* Keep break-glass accounts as exceptions to prevent accidental lockout.
* Enable RCPs in AWS Organizations from the Management Account.
* Test first in a Sandbox OU before applying the policy across the organization.
* Monitor CloudTrail ConsoleLogin events to tune the policy and avoid blocking legitimate remote users.

## Personal view

RCPs for Sign-in are an important improvement for Zero Trust and data perimeter design. In many environments, teams focus heavily on protecting S3 or DynamoDB but leave the Console entry point broadly reachable from the Internet. Moving network conditions into the sign-in layer makes the outer perimeter much stronger.

## Conclusion

Restricting AWS Management Console access to expected networks with Sign-in Resource-Based Policies and RCPs is a valuable security upgrade for AWS enterprises. It reduces the impact of leaked credentials, blocks access from untrusted networks, and strengthens the organization-level data perimeter.

Reference: <https://aws.amazon.com/vi/blogs/security/restrict-aws-management-console-access-to-expected-networks-with-sign-in-resource-based-policies-and-rcps/>
