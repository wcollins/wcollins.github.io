# The Best Terraform Feature Yet?


**Optional** attributes for object type constraints is almost here! I've been waiting for this feature to come along for a while. I have tested it extensively in **-alpha**, and I can confidently confirm that it is a **game changer**. This feature is long in the making, being discussed as far back as [this thread](https://github.com/hashicorp/terraform/issues/19898) in **2018**. Today, it is now in [beta](https://github.com/hashicorp/terraform/releases/tag/v1.3.0-beta1), so the official release could be any day now. Let's demonstrate how this is useful and build some common [AWS infrastructure](https://registry.terraform.io/providers/hashicorp/aws/latest).

## Why it is Useful
Before this feature, you could resort to using **tricks** to make arguments in _object variables_ optional. This usually included providing a **null** value for optional parameters and then doing some fancy **lookup** or **conditional** like so:

{{< gist wcollins aa7bdd03595687c78b0b0ac184df2db7 >}}

Now, we can make that individual attribute optional without any _hackery_ involved. The first thing to know here is that when setting **optional(string)** without a _default value_ as shown below, the _default value_ is **null**.

{{< gist wcollins 6e1171e8dfe866030e2f4952a116d30d >}}

While having the _default value_ automatically set to **null** is helpful, it only solves half of the problem. What happens if we have a scenario where we still want to provide a value in the logic, even if one isn't supplied at _runtime_? Then we can set a _default value_ with a second argument like this:

{{< gist wcollins ffc407c5ce54ed3905c74112e97ed8af >}}

## Building some Infrastructure in AWS
This feature comes in handy when building _complex data types_. Let's look at something as simple as building an **AWS VPC**. Your organization _could_ be using [VPC IPAM](https://docs.aws.amazon.com/organizations/latest/userguide/services-that-can-integrate-ipam.html) but may still need the option to pass in _custom_ CIDRs at runtime. Or, other _standard defaults_ may need to be set if not provided at runtime. Take the following example:

{{< gist wcollins e42bff6de9a5c3bed7b7ddb98c6c2264 >}}

If only the _cidr_block_ attribute is provided at runtime, then the _IPAM_ attributes will be nullified. This simplifies our resource configuration as follows:

{{< gist wcollins edfc10c2c905d35d0a2f45f5333652c2 >}}

## Test it Yourself!
Want to start testing? Grab [v1.3.0-beta1](https://github.com/hashicorp/terraform/releases/tag/v1.3.0-beta1) and setup your **versions.tf** like this:

{{< gist wcollins 2ed4c7de1ebe9b991d46ff4b86a49fb4 >}}

## Conclusion
**AWS VPC** is a simple example. This feature really shines when building reusable _infrastructure-as-code_ for [Network Firewall](https://aws.amazon.com/network-firewall/?whats-new-cards.sort-by=item.additionalFields.postDateTime&whats-new-cards.sort-order=desc) or even [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html). Anything that simplifies **something** and reduces or eliminates any **hacks** required to reach a logical outcome is super valuable. Great work finally driving this one home [HashiCorp](https://www.hashicorp.com/).
