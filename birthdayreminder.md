---
layout: page
title: "Birthday Reminder Tool"
permalink: /birthdayreminder/
---
VcardEmailReminder parses one or multiple addressbook VCARD files (.vcf) and checks the Birthday field ("BDAY") to verify if its the persons birthday today. If yes a email is sent as an reminder.

Best to be used with [vdirsyncer](https://github.com/pimutils/vdirsyncer) to download the VCARD files from a CardDav server and scan this local files with VcardEmailReminder and put both programms in a CronJob.

* Repository: <https://github.com/obsti8383/VcardEmailReminder>

* Releases: <https://github.com/obsti8383/VcardEmailReminder/releases>



_Written in [golang](https://golang.org/)._
