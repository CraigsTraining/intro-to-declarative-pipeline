# Introduction to Declarative Pipelines - Environment

This document describes how to create a shared CloudBees Jenkins Enterprise Master environment on AWS that the participants will use.

**Note**: You will need an AWS account and knowledge of how to create EC2 instances (https://aws.amazon.com/documentation/ec2/).

## Recommended AWS Instance Types

The following is a rough guildeline to the AWS instance types that work well for for the workshop based on the number of users participating (giving each participant at least one dedicated executor as participants will frequently be building jobs at the same time):

| Users | Executors | Instance Type   | vCPU     | Memory     |  Cost Per Hour |  Cost Per Day |
|-------|-----------|-----------------|----------|------------|----------------|---------------|
| <= 9  | 8         | **t2.xlarge**	  | 4	     | 16	      | $0.188         | $4.512        |
| <- 16 | 16        | **t2.2xlarge**  | 8	     | 32	      | $0.376         | $8.648        |

**Notes**:
  - Recommendation based on creating 2 executors per vCPU
  - Additional validation needs to be performed

