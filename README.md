# Policy Set for Testing the Sentinel HTTP Import and Parameters
This repository contains a policy set that can be used to test the Sentinel [HTTP import](https://docs.hashicorp.com/sentinel/imports/http) and [policy parameters](https://docs.hashicorp.com/sentinel/language/parameters) that were added in the Sentinel 0.13.0 runtime.

## Files
There are currently two example policies in this directory:
* [check-external-http-api.sentinel](./check-external-http-api.sentinel)
* [use-latest-module-versions.sentinel](./use-latest-module-versions.sentinel)

The first policy simply uses the HTTP import to call an external API, https://yesno.wtf/api that randomly returns "yes" or "no" (but sometimes returns "maybe"). It also uses the recently added [case statement](https://docs.hashicorp.com/sentinel/language/spec/#case-statements) that provides a selection control mechanism to conditionally execute different logic based on the value of an argument.

The second policy uses the HTTP import to call the Terraform Registry [List Modules API](https://www.terraform.io/docs/registry/api.html#list-modules) against a Terraform Cloud or Terraform Enterprise server in order to determine the most recent version of each module in the [Private Module Registry](https://www.terraform.io/docs/cloud/registry/index.html)(PMR) of an organization on that server.

There is also a policy set configuration file, [sentinel.hcl](./sentinel.hcl) that lists the policies and sets their enforcement levels.

## Use of Parameters in use-latest-module-versions.sentinel
The [use-latest-module-versions.sentinel](./use-latest-module-versions.sentinel) policy uses four parameters:
* `public_registry` indicates whether the public Terraform registry is being used.  This is `false` by default, but could be set to `true`.
* `address` gives the address of the Terraform Cloud or Terraform Enterprise server.  It defaults to `app.terraform.io` which is the address of the multi-tenant Terraform Cloud server that HashiCorp runs. You must specify a value for this if using a Terraform Enterprise server.
* `organization` gives the name of an [organization](https://www.terraform.io/docs/cloud/users-teams-organizations/organizations.html) on the Terraform Cloud or Terraform Enterprise server specified by `address`. You must always specify a valid organization.
* `token` gives a valid Terraform Cloud API token which can be a user, team, or organization token. See the [API tokens](https://www.terraform.io/docs/cloud/users-teams-organizations/api-tokens.html) document for more information.

## Using the Policy Set in Your Own Organzation
To use this policy set in your own organization on a Terraform Cloud or Terraform Enterprise server, follow these steps:
1. Register the policy as described [here](https://www.terraform.io/docs/cloud/sentinel/manage-policies.html#managing-policy-sets).
1. Edit the registered policy sets to specify values for the `organization` and `token` parameters making sure you pick an organization that actually has some modules in its PMR and that the token you give is a valid API token with permission in that organization.
1. If using a Terraform Enterprise server, also specify a value for the `address` parameter, using a value like "tfe.example.com".
1. Save the policy set.
1. Add a workspace to the policy set that uses Terraform code that references modules in the PMR in the organization you specified.
1. Queue a plan against that workspace in the Terraform Cloud UI.

You could also configure the policy set to use the public Terraform registry by adding the `address` parameter and setting it to `registry.terraform.io` and adding the `public_registry` parameter and setting it to `true`. In this case, you should set `organization` to the organization who published the modules you wish to test.  This is described [here](https://www.terraform.io/docs/registry/api.html#namespace) in the documentation for the Terraform Registry HTTP API.
