---
layout: docs
title:  "Rockefeller Archive Center PREMIS Rights Statements Guidelines"
---

## Introduction

Metadata for preservation, including descriptive metadata, administrative metadata, and technical metadata, supports the long-term access to and use of electronic files. The primary metadata schema used for preservation metadata at the RAC is PREMIS (PREservation Metadata:
Implementation Strategies), defined in the *PREMIS Data Dictionary*.[^1] PREMIS is also one of the digital preservation standards upon which Archivematica is built.

Administrative metadata is used for the management of digital content, and includes information about rights and permissions. The Rights entity in the *PREMIS Data Dictionary* provides a highly flexible framework for recording information about the rights and permissions pertaining to digital objects. The PREMIS Rights statement is an assertion of rights, not a record of information from which rights can be determined. In other words, **the purpose of the PREMIS Rights entity is to provide actionable information to a digital preservation system, not to help humans make rights determinations on an ongoing basis.** The RAC has been using PREMIS Rights statements to record machine actionable information about access to and use of digital objects since implementing Archivematica, but has not yet created local guidelines for the content of PREMIS Rights statements.

The Rights entity aggregates information about rights and permissions pertaining to objects in a digital preservation system so that the digital preservation system can perform necessary preservation actions. The distinction between rights and permissions is important: rights are entitlements allowed to agents by copyright or other legally binding agreements. Permissions flow from these rights, being powers or privileges granted or restrictions placed by agreement between a rightsholder and another party or parties. At minimum, the digital preservation system must know what rights or permissions it has in order to carry out actions related to objects within the repository.

PREMIS defines three specific types of rights bases - Copyright, License, and Statute - as well as the Other basis which can be used for rights not based on any of these. Once a rights basis is determined, permissions and restrictions can be recorded and associated with that basis. PREMIS does this by allowing one or more acts to be associated with a rights basis, and permitting each act to be refined by restrictions if there are any. A simple rights statement can consist of a rights basis, an act, and information about any restriction on the act. PREMIS also provides elements for recording information about start and end dates of rights bases and permissions. These dates may be different: for example, a donor agreement may apply to an object for as long as it is held by the archive, but a restriction may apply for only 25 years under the terms of the agreement.

For any given object, one or more rights bases may be assigned, and one or more permissions may be granted (or restrictions placed) for each basis. PREMIS rights-based metadata should support automating processes undertaken by the digital preservation and collections management systems. This means that there need to be strict rules for capturing information about rights, and a clear distinction must be made between actionable metadata fields and narrative fields which may provide valuable contextual information for archivists but which cannot necessarily be parsed and acted upon by software.

## PREMIS Rights in Archivematica

Archivematica provides the three bases defined by PREMIS, and also allows the user to select either Donor or Policy as an implementation of the Other basis. Additionally, Archivematica enforces constraints on the semantic unit *restriction*. From the chapter "Implementing Rights Metadata for Digital Preservation" in *Digital Preservation Metadata for Practitioners*[^2]:

> Archivematica's designers decided early on to make \[the Restriction\]
a more structured and limited field in order to both simplify the metadata entry process and to make the value machine-actionable to inform automated preservation or access decisions.
> 
> Thus the metadata template only allows one of three possible values to be selected: "Allow," "Disallow," and "Conditional," which enforces specific combinations and makes the information machine-actionable by the system. If the user selects "Allow," Archivematica automatically creates a *termOfGrant* semantic unit to capture information about the start and end dates of the grant; if either "Disallow" or "Conditional" is selected, Archivematica creates a *termOfRestriction* semantic unit instead. More importantly, specific combinations of values in *act* and *restriction* can be used to automate rules relating to processing and access. For example...if *act* is "Disseminate," an access system can be programmed to check whether *restriction* is "Allow," "Disallow," or "Conditional" and display or hide the digital object based on this information...
> 
> Note that a user can select "Conditional" as a value for *restriction*. This is to capture information about acts that can be carried out under certain circumstances. These circumstances should be specific in the *rightsGranted* semantic unit ("Grant/restriction note" in the metadata entry template). For example, the user might select "Publication" as the *act* and "Conditional" as the *restriction*, and add a note that permission to publish must be obtained from the copyright holder. As for "Disallow," if one selects "Conditional" in the *restriction,* this causes the *termOfRestriction* semantic to be used the PREMIS output, rather than the *termOfGrant*.

[^1]: Library of Congress and PREMIS Editorial Committee, “PREMIS Data Dictionary for Preservation Metadata, Version 3.0 (Library of Congress).”

[^2]: Evelyn McLellan, "Implementing Rights Metadata for Digital Preservation," in *Digital Preservation Metadata for Practitioners*.
    New York, NY: Springer Berlin Heidelberg, 2016.
