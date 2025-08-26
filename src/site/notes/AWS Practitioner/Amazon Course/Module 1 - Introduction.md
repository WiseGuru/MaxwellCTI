---
{"dg-publish":true,"permalink":"/aws-practitioner/amazon-course/module-1-introduction/","updated":"2024-02-16T14:04:41.000-08:00"}
---

1. Fundamental cloud-compute model
	1. Client-server model
		1. A client makes a request, and the server responds to the request
			1. In AWS, a server would be a *[[AWS Practitioner/Amazon Course/AWS Definitions/EC2\|EC2]]* (Amazon Elastic Compute Cloud)
	2. Key concept to AWS
		1. Only pay for what you use
			1. *Elastic* means *flexible*; if you need more compute power, it can spin up. When you don't, it spins down.
2. 
<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/aws-practitioner/amazon-course/aws-definitions/cloud-computing/#cloud-computing" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



#### Cloud Computing
- The *on-demand delivery* of [[AWS Practitioner/Amazon Course/AWS Definitions/IT resources\|IT resources]] *over the Internet* with *pay-as-you-go* pricing
	1. [[AWS Practitioner/Amazon Course/AWS Definitions/IT resources\|IT resources]] are fundamental, broadly similar tools used by different companies
	2. *Over the internet* means it can be served to any device with *Internet* connectivity
	3. *Pay-as-you-go* means you pay for resources based on *need* and *use*, not simply having them, available
- Offers flexibility of being able to *request* and *release* resources as needed, only paying for *what's in use*
- Significantly reduces *CapEx* by increasing *OpEx*
	- Reduces the impact of under- or over-estimating required resources
- By *sharing* hardware resources, *individuals and companies with reduced need* can pay for less and have lower *idle resources* than self-hosted solutions
- *Resiliency* and *global reach* of cloud servers allows for rapid access across the world


</div></div>

<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/aws-practitioner/amazon-course/aws-definitions/cloud-computing/#deployment-models" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">



### Deployment Models
1. Cloud-based
	1. All parts of the application are run in the cloud
	2. These applications could be designed and built for cloud use, or migrated from existing infrastructure
2. On-prem (private cloud)
	1. Applications are deployed using on-prem hardware
	2. Applications can be run in virtualized environments or on dedicated servers
3. Hybrid
	1. Cloud-based resources are connected to on-prem infrastructure
	2. This can be useful to meet compliance or regulations, while offloading non-regulated or other resources to the public cloud








</div></div>
