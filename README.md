
= terraform-github-team

image:https://img.shields.io/badge/maintained%20by-mineiros.io-%235849a6.svg[Maintained by Mineiros.io, link="https://www.mineiros.io/ref=repo_terraform-github-team"]
image:https://mineiros.semaphoreci.com/badges/terraform-github-team/branches/master.svg?style=shields[Build Status, link="https://mineiros.semaphoreci.com/projects/terraform-github-team"]
image:https://img.shields.io/github/v/tag/mineiros-io/terraform-github-team.svg?label=latest&sort=semver[GitHub tag (latest Semantic Version), link="https://github.com/mineiros-io/terraform-github-team/releases"]
image:https://img.shields.io/badge/tf-%3E%3D0.12.9-blue.svg[Terraform Version]

[.lead]
A Terraform module that acts as a wrapper around the Terraform https://www.terraform.io/docs/providers/github/index.html[GitHub provider]
and offers a more convenient and tested way to provision and manage GitHub Teams following best practices.

[TIP]
====
This Module uses `For, For-Each and Dynamic Nested Blocks` that were introduced in Terraform 0.12.
A common problem in Terraform configurations for versions 0.11 and earlier is dealing with situations where the number
of values or resources is decided by a dynamic expression rather than a fixed count.

You can now dynamically add and remove items from and to Lists without
the necessity to render the whole list of resources again. Terraform will only add and remove the items you want it to.
====

== Table of Contents
toc::[]

== Requirements

* Terraform `~> 0.12.9`
* Terraform GitHub provider `~> 2.3`

== Features
This module uses https://github.com/terraform-providers/terraform-provider-github/releases[Terraform GitHub provider v2.3] that supports the following resources:

* Team
* Nested Team
* Memberships
* Team Repositories

== Usage

[source,hcl]
----
module "team" {
  source = "git::git::git@github.com:mineiros-io/terraform-github-team.git?ref=master"

  name        = "DevOps"
  description = "The DevOps Team"
  privacy     = "secret"

  members = [
    "a-user",
    "b-user"
  ]

  maintainers = [
    "a-maintainer"
  ]

  push_repositories = [
    github_repository.repository.name,
  ]
}

resource "github_repository" "repository" {
  name   = "a-repository"
}
----

[TIP]
====
In a production environment you most likely want to stick to a specific version of this module instead of the master
branch (e.g. ?ref=v0.0.1), because there may be breaking changes between releases.
====

=== Examples

For a complete example please see link:/examples[examples] directory.

=== Tests
This modules ships with a link:Makefile[Makefile] and a link:Dockerfile[Dockerfile] that simplify executing the hooks
and tests for you.

Execute all pre-commit hooks with Docker:
[source,shell script]
----
make docker-run-pre-commit-hooks
----

Execute the Unit Test (deploy und undeploy the example):
[source,shell script]
----
GITHUB_ORGANIZATION=YOUR_GITHUB_ORGANIZATION \
GITHUB_TOKEN=YOUR_GITHUB_TOKEN \
make docker-run-tests
----