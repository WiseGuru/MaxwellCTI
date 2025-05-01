---
{"dg-publish":true,"permalink":"/aws-practitioner/amazon-course/module-2-compute-in-the-cloud/","noteIcon":""}
---

1. Multitenancy
	1. Multiple clients use virtualized instances on the same hardware
2. 
<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/aws-practitioner/amazon-course/aws-definitions/ec-2/#ec-2" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### EC2
- An *Amazon Elastic Compute Cloud* (*EC2*) is a type of virtual server in *AWS*
- *EC2* servers can be specialized with different *Instance Types*
	- General Purpose
		- Applications
		- Gaming
		- Small to medium Databases
	- Compute Optimized
		- High-performance processors
	- Memory Optimized
		- Fast performance for large datasets in memory
	- Accelerated Computing
		- Hardware accelerators, coprocessors, highly-specialized equipment
		- Floating-point number calculations, data pattern matching, etc.
	- Storage Optimized
		- High sequential read-write
		- Large datasets
		- High-Frequency Online Transaction Processing (*OLTP*) systems







</div></div>

3. Purchase options
	1. On-Demand
		1. Better for initial setup
		2. No plans or contracts
	2. Reserved Instances
		1. 1 to 3 year terms
		2. Standard reserved requirements
			1. Know the instance type, region, and size required for your app
			2. You must know:
				1. Instance type and size
				2. Platform description (OS)
				3. Tenancy (Default (shared) or dedicated)
		3. Convertible reserved
			1. More flexible; useful if you're not sure on the specifics you will need (availability zones, instance types, etc.)
	3. EC3 Instance Savings plan
		1. Hourly commitment
		2. Similar savings to Standard Reserved, but don't need to specify your requirements for the servers
	4. Spot instances
		4. Request instances at a discount compared to On-Demand
		5. However, AWS might reclaim instances if another higher-tier service needs it
			1. They give a 2-minute warning
	5. Dedicated hosts
		1. "Someone else's servers"
		2. You reserve specific hardware that only you have access to
4. Scaling
	1. How do you solve scaling on prem?
		1. YOU CAN'T
	2. AWS Auto-scaling allows you to easily add or subtract instances
		1. Elastic Load Balancing allows you to load balance between instances
5. Messaging and Queuing
	1. 