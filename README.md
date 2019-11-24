# Policy Set for Testing the Sentinel HTTP Import and Parameters
This repository contains a policy set that can be used to test the Sentinel [HTTP import](https://docs.hashicorp.com/sentinel/imports/http) and [policy parameters](https://docs.hashicorp.com/sentinel/language/parameters) that were added in the Sentinel 0.13.0 runtime.

## Files
There are currently two example policies in this directory:
* [check-external-http-api.sentinel](./check-external-http-api.sentinel)
* [use-latest-module-versions.sentinel](./use-latest-module-versions.sentinel)

There is also a policy set configuration file, [sentinel.hcl](./sentinel.hcl) that lists the policies and sets their enforcement levels.

## Using the Policy Set
To use this policy set on a Terraform Cloud or Terraform Enterprise server, follow these steps:
1. Register the policy as described [here](https://www.terraform.io/docs/cloud/sentinel/manage-policies.html#managing-policy-sets).
1. Edit the registered policy sets to specify values for the `organization` and `token` parameters making sure you pick an organization that actually has some modules in its PMR and that the token you give is a valid API token with permission in that organization.
1. If using a Terraform Enterprise server, also specify a value for the `address` parameter, using a value like "tfe.example.com".
1. Save the policy set.
1. Add a workspace to the policy set that uses Terraform code that references modules in the PMR in the organization you specified.
1. Queue a plan against that workspace in the Terraform Cloud UI.


