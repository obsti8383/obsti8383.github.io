---
layout: page
title: "Info Sec Tools"
permalink: /infosectools/
---
Some tools I participated in or created:

## [Security Without Borders - Hardentools](https://github.com/securitywithoutborders/hardentools)

Hardentools is a tool to disable a number of "features" exposed by Microsoft Windows and primary consumer applications. These features, commonly thought for enterprise customers, are generally useless to regular users and rather pose as dangers as they are very commonly abused by attackers to execute malicious code on a victim's computer. The intent of this tool is to simply reduce the attack surface by disabling the low-hanging fruit. Hardentools is intended for individuals at risk, who might want an extra level of security at the price of some usability. It is not intended for corporate environments.

<!-- ## [Update Checker for Windows 10](https://github.com/obsti8383/UpdateChecker)
Checks for software release states on Windows systems. Enables you to identify popular software that needs to be updated. 

## [Vergrabber](https://github.com/twkrol/vergrabber)
Nice service from twkrol that grabs & publishes version information of selected software packages. It produces the json file that can be further processed programmaticaly (eg. by automation tools).-->

## [Certificate Transparency Explorer](https://github.com/obsti8383/certificateTransparencyExplorer)

Gets list of certificates from certificate transparency logs (currently crt.sh, only non-expired certificates) for a specified list of domains and writes a CSV file (certificates.csv) that gives an overview of all the certificates found.
