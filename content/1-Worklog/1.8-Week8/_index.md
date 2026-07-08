---
title: "Week 8 Worklog"
date: 2026-06-05
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---



### Week 8 Objectives:
* Learn about Elastic Load Balancing and Auto Scaling Group.
* Understand how to deploy a scalable application.
* Understand the roles of Launch Template, Target Group, and Health Check.
* Practice deploying an application with Auto Scaling Group.
* Understand high availability model across multiple Availability Zones.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 6 | - Learn about Elastic Load Balancing overview <br> - Understand the role of Application Load Balancer <br> - Note how ALB distributes requests to multiple EC2 Instances <br> - Identify ALB's position in the architecture diagram | 05/06/2026 | 05/06/2026 | <https://000006.awsstudygroup.com/> <br><https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i> |
| 2 | - Learn about Target Group and Health Check <br> - Configure or simulate a Target Group for EC2 <br> - Check healthy/unhealthy status <br> - Note how health checks help detect faulty instances | 08/06/2026 | 08/06/2026 | <https://000006.awsstudygroup.com/> |
| 3 | - Learn about Launch Template <br> - Understand AMI, instance type, key pair, and security group in a launch template <br> - Note the launch template's role when creating an Auto Scaling Group | 09/06/2026 | 09/06/2026 | <https://000006.awsstudygroup.com/> |
| 4 | - Learn about Auto Scaling Group <br> - Understand desired capacity, minimum capacity, and maximum capacity <br> - Note how ASG automatically adds or removes EC2 Instances <br> - Practice or simulate Auto Scaling configuration | 10/06/2026 | 10/06/2026 | <https://000006.awsstudygroup.com/> |
| 5 | - Complete architecture diagram with ALB and Auto Scaling Group <br> - Show the flow User → ALB → Target Group → EC2 Auto Scaling Group <br> - Note benefits and costs when deploying Multi-AZ <br> - Clean up resources after completing the lab | 11/06/2026 | 11/06/2026 | <https://000006.awsstudygroup.com/> |


### Week 8 Achievements:
**Overview:**

This week I understood how to deploy a scalable and highly available system using Application Load Balancer and Auto Scaling Group. I know how to describe the request flow from users to multiple EC2 Instances through ALB.

**Learned theory:**

* Elastic Load Balancing.
* Application Load Balancer.
* Target Group and Health Check.
* Launch Template.
* Auto Scaling Group.
* Multi-AZ and high availability.

**Hands-on labs:**

* Create or simulate a Target Group.
* Configure Health Check.
* Learn about Launch Template.
* Configure Auto Scaling Group.
* Draw the User → ALB → Target Group → EC2 Auto Scaling Group diagram.
* Clean up resources after completing the lab.
