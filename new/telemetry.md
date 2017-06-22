---
RFC: unassigned
Title: Anonymous user behaviour data collection
Author: Thom May <thom@chef.io>, Charles Johnson <charles@chef.io>
Status: Draft
Type: Informational
---

# Anonymous user behaviour data collection

In order to provide greater insight into how our Chef users interact with Chef
tools, this RFC introduces a framework for defining the policies for the
anonymous collection (sharing), storage, and deletion of product usage data
that can be applied to any Chef project.

## Scope

This RFC only covers Chef's open source ecosystem. It does not apply to proprietary,
commercially available tools made by Chef Software.

## Motivation

    As a UX designer at Chef,
    I want to understand how people use Chef tools,
    so that I can improve their experience.

    As a product owner at Chef,
    I want to learn which Chef tools and commands are most used,
    so I can prioritise development.

## Specification

This RFC defines the core of product usage data sharing. It does not define how
individual apps will implement product usage data sharing; any app that
wishes to do so MUST create a follow up RFC that details individual behaviour,
and the types of data that will be shared with Chef Software.

### Sharing by default

In order to gather meaningful data, Chef tools will share anonymized usage data
by default. Experience (with Habitat and Automate) has shown that disabling
sharing by default provides sample sets that are too small (and potentially
biased) to draw meaningful conclusions.  The Chef tools must provide clear
information so that users always know how to easily change their data sharing
preference. Once set, any data sharing preference saved on an individual host
will be honored by all Chef tools run on that host.

Server tools may each require their own separate data sharing preference.

The Chef Client will never collect or share telemetry.

### High-Level Implementation

All Chef tools will use a centralized configuration for product usage data
sharing, in order to provide a consistent experience. If the configuration is
not present, a data-sharing-enabled application will create a well-formatted
configuration file. Upon creating the configuration file, the tool will notify
the user that product usage data sharing is enabled, and will provide clear
information so that users always knowhow to easily change their data sharing
preference.

Tools should provide a command that allows a user to see and understand
what data the tool is collecting and reporting.

All product usage data sharing enabled applications will provide simple ways
for a user to check their data sharing preference, and to change that
preference.

### Privacy and Data Retention

To ensure that sensitive data is not collected, Chef tools should never
collect parameters passed to command-line options, and tools will be able to
filter more aggressively if required.

For example, knife may report that a `-P` option was invoked, but will never
collect the password parameter that was passed to that option.

Chef is committed to protecting user privacy and as product usage data is
shared with Chef, it must be stored with protection from de-anonymization
attacks in mind. All product usage data must be anonymized, and must not
share personally identifiable information with Chef.

All data storage must be compliant with EU privacy law.

All collected data will be subject to Chef's [privacy policy](https://www.chef.io/privacy-policy/),
which is Privacy Shield certified. Any collected data will be deleted upon
request.

### Prior Art

Many open source projects collect data from their users to help allocate
resources. 
* Both [Ubuntu](https://wiki.ubuntu.com/Apport) and
[Fedora](https://retrace.fedoraproject.org/) collect crash dumps from their
users, along with system information, to help developers better debug and
improve their systems. 
* The Debian project runs [PopCon](https://popcon.debian.org/) to better
understand what packages are installed and which architectures are in
use.
* Habitat, our sister project, collects [analytics](https://www.habitat.sh/docs/about-analytics/)
related to user interactions.
* Mac Homebrew collects user behavior [analytics](https://github.com/Homebrew/brew/blob/master/docs/Analytics.md).

### Types of collected data

Some data we envisage collecting includes, but is not limited to:

 - Community Cookbooks used, and versions (in policyfiles or berkshelf)
 - Kitchen plugins
 - Vagrant boxes
 - Commands and options attempted
 - Versions of chef and other tools used

### Examples

```json
{
  "instance_id":"00000000-0000-0000-0000-000000000000",
  "message_version":1.0,
  "payload_version":1.0,
  "license_id":"00000000-0000-0000-0000-000000000000",
  "origin": "command-line",
  "type": "track",
  "product": "chefdk",
  "timestamp": "2017-02-06T17:25:42   Z",
  "payload":{  
     "event":"user-command",
	 "anonymousId":"2d5a4d61-d79a-4ff9-aa71-a403c2d5a001",
     "properties":{  
        "command": "chef generate cookbook",
        "timestamp": "2017-02-06T17:25:42   Z"
     }
  } 
}
```

## Copyright

This work is in the public domain. In jurisdictions that do not allow for this,
this work is available under CC0. To the extent possible under law, the person
who associated CC0 with this work has waived all copyright and related or
neighboring rights to this work.
