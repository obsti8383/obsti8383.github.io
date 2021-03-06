---
layout: page
title: "Okta Code Snippets"
permalink: /okta/
---

Code for managing [Okta](https://www.okta.com){:target="_blank"} IAM environments.

## Powershell Scripts

* Script that fetches all users and writes userLogin and MFA factorType into a CSV - good to get an overview of configured MFA factors: <https://gist.github.com/obsti8383/a5086aac833a14b49b69b7e6afc32811>
 
* Helper Script to iterate all Okta users and find out which have the same attribute contents (here employeeNumber) and list them. Can be used to find out if there are double entries for attributes that should be unique (but not configured as such in Okta): <https://gist.github.com/obsti8383/8433967a0faeeca41dc214a3baad8ed0>

* This generic script can be used to fetch several data sets (e.g. users) from Okta API and write the datasets to a CSV file: <https://gist.github.com/obsti8383/62dd361900f9adddc5ea3bce42676b7c>

## [Test Identity Generator](https://github.com/obsti8383/TestIdentityGenerator)

Tool written in Golang that creates a CSV or JSON file with random test identities.
