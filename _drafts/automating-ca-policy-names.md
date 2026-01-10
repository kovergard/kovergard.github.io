---
layout: post-image
title: "It's 10 p.m. Do you know what your Conditional Access policies are doing?"
date: 2026-01-09
tags: conditional-access automation powershell
image: /assets/images/pexels-mikhail-nilov-6963062.jpg
image_alt: people-night-smartphone-dark-693062
image_caption: Photo by Mikhail Nilov
---

Conditional Access (CA) policies are critical for securing access to everthing in your tenant. But keeping track of what they are actually doing can be complicated. 

One challange I often encounter when working on customer projects, is that few (if any) of the tenant administrators know what all their Conditional Access policies are actually doing, and why they are doing it. This can lead to both security issues due to lacking coverage from policies, as well as bad user experince when trying to access company data.

## The problem

The lack of insight into the Conditional Access policies usually stems from one or more of the following issues:

* The number of policies grow over time making it hard to keep an complete overview. Microsoft recommends keeping the total number of CA policies down, but that is not always possible due to organizational demands and the need to address specific cases. 
* Environments are managed by multiple administrators, who each have their own opinion on how policies should be configured. 
* Policies with an obscure name, created by administrators who have since left the company, or perhaps external consultants. 

The impact of these issues might be lessened, if we could add a description to CA policies. That would allow us a more detailed way to document the intent of the policy, hopefully leading to a better policy set. Unfortunately, there is no description field on CA policies. 😒

While there are excellent tools out there for documenting the content of Conditional Access policies, like the excellent [IdPowerApp CA Documenter](https://idpowertoys.merill.net/ca) by Merill Fernando, I wanted something that would make it apparant what a CA policy does, directly in the Entra ID admin portal.

Without a description field, the only option availble in the portal is to ensure that the display name of the policy actually reflects the content. This means adopting a naming standard that describes policy intent.

## Common naming standards

The following are some of the most common naming standards that I have encountered. You probably already have policies that are named after one or these standards, or a variation of them.

**Microsoft-managed policies and templates**

Microsoft-managed policies and policies based on templates provided in the Conditional Access management portal, use sentence-based names that looks like these:


* Block legacy authentication
* Securing security info registration
* Require compliant or hybrid Azure AD joined device or multifactor authentication for all users

While this is a good start, it can get a bit complicated when targetting specific locations, applications or similar. 

**Suggested naming standard from Learn**

Guidance from the [Microsoft Learn]((https://learn.microsoft.com/en-us/entra/identity/conditional-access/plan-conditional-access#set-naming-standards-for-your-policies)) site suggest using a standard prefixed with a serial number on format `<SN> - <Cloud app>: <Response> For <Principal> When <Conditions>`.

* CA01 - Dynamics CRP: Require MFA for Marketing when on external Networks
* CA02 - Office 365: Require Terms of Use for Guests when using browser
* CA03 - All apps: Block legacy authentication

The serial numbers makes it easier to identity specific policies during troubleshooting, and the structured names should make it easier to comprehend the purpose of each policy.

**Conditional Access Guidance for Zero Trust**

Prior to the current guidance on Microsoft Learn, the site linked to a GitHub repository with guidance for configuring Conditional Access for Zero Trust. That content is still available [here](https://github.com/microsoft/ConditionalAccessforZeroTrustResources).

This framework suggests names with serial numbers and introduces personas. Personas 
TODO

* CA100-Admins-BaseProtection-AllApps-AnyPlatform-CompliantandMFA
* CA206-Internals-DataandAppProtection-AllApps-iOSorAndroid-ClientAppORAPP
* CA403-Guests-IdentityProtection-AllApps-AnyPlatform-BlockLegacyAuth

This original repository work has not been updated since October 2023. Fortunately, others like MVP Joey Verlinden with his [Conditional Access Baseline](https://github.com/j0eyv/ConditionalAccessBaseline) project, have created updated policies using the similar naming.

TODO

## A different approach – automating descriptive names

Adopting a naming standard for naming your Conditional Access policies is a great start. But there are a few issues:

* The content and purpose of a policy might shift over time, so the name might not reflect the actual configuration.
* Your co-administrators might not be aware, or not understand, the naming policy.

To address these and similar issues, I wrote the script `Get-ConditionalAccessPolicyNameSuggestion.ps1`

This script can be used to generate descriptive names for Conditional Access policies, based on the content of the policy. 

TODO Personas


