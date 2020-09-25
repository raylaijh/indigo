---
title: "Compliance on infrastructure provisioning: Policy as a Code"
layout: post
date: 2020-09-25 15:37
image: /assets/images/markdown.jpg
headerImage: false
tag:
  - terraform 
  - terraform cloud for business
  - hashicorp
  - sentinel
  - policy as a code
category: blog
---

Recently I had the chance to work on the features of Terraform Cloud for Business and was trying to understand its exclusive uses cases compared to the open source version of Terraform. Sentinel integration is one of them and I do feel that it value adds to an organization by solving a prevalent problem of security compliance of provisioned infrastructure.

### What is Sentinel?

Sentinel is actually a programming language on its own, created with a main purpose of condensing available security policies in a code. By having these policies in a code format, proven software development best practices can be adopted such as version control, automated testing, and automated deployment.The idea is similar to [Infrastructure as a Code][iac] (IaC), just that we consume security policies in a code form. 

The Sentinel language is created to be easy to understand for non-programmers but also programmer friendly at the same time as it also supports programmer constructs like conditionals, loops, functions, etc. Furthermore, it is written to integrate nicely with HashiCorp tools which are written in Go.

More details on Sentinel: https://docs.hashicorp.com/sentinel/concepts/


[iac]: https://raylaijh.github.io/terraform-ansible/


### Policy as Code + Infrastructure as Code

Sentinel integrates with Terraform Cloud natively and does the compliance checks during as part of the `plan` and `apply` process in Terraform. Let's take a quick look at how to set this up on Terraform Cloud and the results.

## Prerequisites 

The steps shown in setting up Sentinel on Terraform Cloud assummes the prerequisites shown below:

* Terraform Cloud access [https://app.terraform.io/signup/account)
* Terraform Cloud for Business license [https://www.hashicorp.com/contact-sales/terraform)
* VCS (Github,Gitlab) with Terraform configuration files
* VCS already setup on Terraform Cloud 
* Sentinel Codes written and hosted on a VCS

More detailed steps are listed on HashiCorp [documentation]

[documentation]: https://learn.hashicorp.com/collections/terraform/policy

## Setting up policy sets on Terraform Cloud for Business (TFC4B)

<center>
<img align="center" src="/assets/images/sentinel_policy_set.png" alt=""> 
  <figcaption>Sample Ansible Tower reference architecture</figcaption>
</center>

With a TFC4B license, you should be able to see under Settings > Policy Sets > Connect a New Policy Set and follow the instructions to link your repository with your Sentinel policy. The repository should contain a `sentinel.hcl` file with the Sentinel code written in it. Once done you, should be able to see the repository appears on screen, as per the image above.

<center>
<img align="center" src="/assets/images/sentinel_policy_set_2.png" alt=""> 
  <figcaption>Sample Ansible Tower reference architecture</figcaption>
</center>

Next, click on the repository created and choose to apply the policy set on the workspace desired. You can to all workspaces or select a couple based on your preference.

## Queue a plan for provisioning

<center>
<img align="center" src="/assets/images/sentinel_policy_set_3.png" alt=""> 
  <figcaption>Sample Ansible Tower reference architecture</figcaption>
</center>
Queue a Terraform Plan as per usual, you should now be able to see an additional row prior to the provisioning with a Policy Check. The results will be based on the Sentinel policies written that was configured to the Policy Set earlier on. In Sentinel, there are basically 3 different enforcement levels to set on the Sentinel policy, mainly `advisory`, `soft-mandatory` and `hard-mandatory`. More details of each enforcement level are described [here]. In this case, the Policy check is failing a `soft-mandatory`, which means it can be overriden by a user with higher authority.


[here]: https://www.terraform.io/docs/cloud/sentinel/enforce.html

<center>
<img align="center" src="/assets/images/sentinel_policy_set_4.png" alt=""> 
  <figcaption>Sample Ansible Tower reference architecture</figcaption>
</center>
You can also read on the Policy check failure logs by clicking the drop down. 


### Conclusion

<center>
<img align="center" src="/assets/images/tfc_sentinel_workflow.png" alt=""> 
</center>


As seen above, enforcement of policies can be done even before the provisioning of resources with the help of Sentinel. Sentinel integrates nicely with Terraform in this way, and with the different tiers of enforcement levels available, organizations can make use of this as part of their existing workflows to ensure good Agile development and also achieve compliance and cost savings at the same time, compared to the usual workflow of enforcing security policies post provisioning. With that in place, the security and compliance team can manage the policies by just updating the repository associated with the policy set, without knowledge of operating Terraform or Terraform Cloud at all, as illustrated on the diagram above.

This is a common cause of concern among organizations as the compliance check process is done usually after provisioning and at which sometimes, a possible breach may have already happened due to the sheer amount of resources provisioned at one time. Secondly, some of these resources may already be running essential workloads and the process of security compliance checking can cause disruption. 


Therefore, with Sentinel perform compliance checks pre resource provisioning could be a good solution to solve compliance issues. This Policy as a Code solution on top of Terraform is pretty easy to set up as well as shown above. 

### Useful links
https://www.hashicorp.com/resources/testing-terraform-sentinel-policies-using-mocks
https://medium.com/hashicorp-engineering/using-new-sentinel-features-in-terraform-cloud-c1ade728cbb
