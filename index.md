## Introduction

Metadata for preservation, including descriptive metadata, administrative metadata, and technical metadata, supports the long-term access to and use of electronic files. The primary metadata schema used for preservation metadata at the RAC is PREMIS (PREservation Metadata:
Implementation Strategies), defined in the *PREMIS Data Dictionary*.[^1]
PREMIS is also one of the digital preservation standards upon which Archivematica is built.

Administrative metadata is used for the management of digital content, and includes information about rights and permissions. The Rights entity in the *PREMIS Data Dictionary* provides a highly flexible framework for recording information about the rights and permissions pertaining to digital objects. The PREMIS Rights statement is an assertion of rights, not a record of information from which rights can be determined. In other words, **the purpose of the PREMIS Rights entity is to provide actionable information to a digital preservation system, not to help humans make rights determinations on an ongoing basis.** The RAC has been using PREMIS Rights statements to record machine actionable information about access to and use of digital objects since implementing Archivematica, but has not yet created local guidelines for the content of PREMIS Rights statements.

The Rights entity aggregates information about rights and permissions pertaining to objects in a digital preservation system so that the digital preservation system can perform necessary preservation actions. The distinction between rights and permissions is important: rights are entitlements allowed to agents by copyright or other legally binding agreements. Permissions flow from these rights, being powers or privileges granted or restrictions placed by agreement between a rightsholder and another party or parties. At minimum, the digital preservation system must know what rights or permissions it has in order to carry out actions related to objects within the repository.

PREMIS defines three specific types of rights bases - Copyright, License, and Statute - as well as the Other basis which can be used for rights not based on any of these. Once a rights basis is determined, permissions and restrictions can be recorded and associated with that basis. PREMIS does this by allowing one or more acts to be associated with a rights basis, and permitting each act to be refined by restrictions if there are any. A simple rights statement can consist of a rights basis, an act, and information about any restriction on the act. PREMIS also provides elements for recording information about start and end dates of rights bases and permissions. These dates may be different: for example, a donor agreement may apply to an object for as long as it is held by the archive, but a restriction may apply for only 25 years under the terms of the agreement.

For any given object, one or more rights bases may be assigned, and one or more permissions may be granted (or restrictions placed) for each basis. PREMIS rights-based metadata should support automating processes undertaken by the digital preservation and collections management systems. This means that there need to be strict rules for capturing information about rights, and a clear distinction must be made between actionable metadata fields and narrative fields which may provide valuable contextual information for archivists but which cannot necessarily be parsed and acted upon by software.

## PREMIS Rights in Archivematica

Archivematica provides the three bases defined by PREMIS, and also allows the user to select either Donor or Policy as an implementation of the Other basis. Additionally, Archivematica enforces constraints on the semantic unit *restriction*. From the chapter "Implementing Rights Metadata for Digital Preservation" in *Digital Preservation Metadata for Practitioners*[^2]:

Archivematica's designers decided early on to make \[the Restriction\]
a more structured and limited field in order to both simplify the metadata entry process and to make the value machine-actionable to inform automated preservation or access decisions.

Thus the metadata template only allows one of three possible values to be selected: "Allow," "Disallow," and "Conditional," which enforces specific combinations and makes the information machine-actionable by the system. If the user selects "Allow," Archivematica automatically creates a *termOfGrant* semantic unit to capture information about the start and end dates of the grant; if either "Disallow" or
"Conditional" is selected, Archivematica creates a *termOfRestriction*
semantic unit instead. More importantly, specific combinations of values in *act* and *restriction* can be used to automate rules relating to processing and access. For example...if *act* is
"Disseminate," an access system can be programmed to check whether
*restriction* is "Allow," "Disallow," or "Conditional" and display or hide the digital object based on this information...

Note that a user can select "Conditional" as a value for
*restriction*. This is to capture information about acts that can be carried out under certain circumstances. These circumstances should be specific in the *rightsGranted* semantic unit ("Grant/restriction note" in the metadata entry template). For example, the user might select "Publication" as the *act* and "Conditional" as the
*restriction*, and add a note that permission to publish must be obtained from the copyright holder. As for "Disallow," if one selects
"Conditional" in the *restriction,* this causes the
*termOfRestriction* semantic to be used the PREMIS output, rather than the *termOfGrant*.

## PREMIS Implementation at the RAC

Historically, there have been two types of rights bases applied to objects ingested into the digital preservation system at the RAC: those governing access (the Donor or Policy types of Other) and those governing use (Copyright). In the RAC’s implementation of PREMIS going forward:

-   All digital objects ingested into the digital preservation system
    must have copyright information expressed in PREMIS. This includes
    cases where the copyright status is unknown or when the object is in
    the public domain.
-   All digital objects ingested into the digital preservation system
    must have a PREMIS rights statement detailing any conditions which
    govern access to the object. Ideally, the rights basis for this
    PREMIS entity is Donor, and contains information from a donor
    agreement. If there is no donor agreement governing the digital
    object, the rights basis Policy must be used. Additionally, the
    rights basis Policy should be used in addition to the rights basis
    Donor if a RAC Policy places restrictions on the object beyond what
    is outlined in a donor agreement.

Below is detailed information about each semantic unit in the PREMIS Rights entity. It includes the description provided by the *Data Dictionary*, as well as notes on value constraints as defined by the
*Data Dictionary*, and, where applicable, Archivematica and local policy.

### Controlled Vocabularies and Standards

Note that, as recommended by the *Data Dictionary* and enforced by Archivematica, all dates should be expressed according to ISO 8601. From the *Data Dictionary*: Use “open” for an open-ended term of grant. Omit
*endDate* if the ending date is unknown or the permission statement applies to many objects with different end dates. Additionally, statute and copyright jurisdiction values should be taken from ISO 3166 Country Codes.

### Act (rightsGranted)

While corresponding information about the donor agreement or policy may be relevant to include in a rights basis note, a permission/restriction should not be created if the permission/restriction expired before ingest into the digital preservation system.

#### Act

**PREMIS Rule:** The action the preservation repository is allowed to take.

**Values:**

-   *disseminate:* object is open for research, and associated metadata
    is available in discovery system
-   *publish:* create a copy or version for use outside the preservation
    repository

Above values are used in the Archivematica/ArchivesSpace DIP Upload integration and definitions are local. In past practice at the RAC, a permission/restriction flowing from Copyright with the Act "publish"
related to whether the RAC could publish the digital object online,
and a permission/restriction flowing from Donor or Policy with the Act
"disseminate" related to whether the RAC could provide any access to the digital object. Note that the *PREMIS Data Dictionary* suggests the following values and corresponding definitions, which are *not* in use at the RAC:

-   replicate: make an exact copy
-   migrate: make a copy identical in content in a different file format
-   modify: make a version different in content
-   use: read without copying or modifying
-   disseminate: create a copy or version for use outside the
    preservation repository
-   delete: remove from the repository

#### Restriction (restriction)

**PREMIS Rule:** A condition or limitation on the act.

**Values:**

-   *allow*
-   *disallow*
-   *conditional*

Above values are defined and enforced by Archivematica. Note: If the user selects "Allow," Archivematica automatically creates a
*termOfGrant* semantic unit to capture information about the start and end dates of the grant; if either "Disallow" or "Conditional" is selected, Archivematica creates a *termOfRestriction* semantic unit instead.

#### Term of Grant/Restriction (termOfGrant OR termOfRestriction)

**PREMIS Rule:** The time period for the permissions or restriction granted.

**Local Rule:** The start date is generally the same date as the beginning of the rights basis which this act is connected to, and/or the accession (born digital) or creation (digitized) date of the digital object. The end date is when the specified act expires. This may be unknown (i.e., blank) or open. For digitized materials,
permissions/restrictions apply to the digital object, not the original material. Therefore, start dates should not be before the creation of the digital object.

#### Grant/Restriction Note (rightsGrantedNote)

**PREMIS Rule:** Additional information about the permissions or restrictions granted if a textual description is needed for further explanation. This semantic unit may include a statement about risk assessment, for example, when a repository is not certain about what permissions have been granted.

**Local Rule:** This note must be used if restriction is
"conditional." If the restriction is "allow" or "disallow," the note should only be used for more explanation on the act (e.g., a statement about risk assessment). This note should not be purely duplicative of the machine-readable portions of the semantic unit (e.g., "Open without restrictions" or "Closed until 2025"). Additionally, this note should not contain information about permissions/restrictions that have expired, especially if this information is included in the rights basis note (e.g., "The twenty-year embargo has expired for these materials").

### Basis: Copyright (copyrightInformation)

#### Copyright Status (copyrightStatus)

**PREMIS Rule:** A coded designation for the copyright status of the object at the time the Rights statement was recorded. Mandatory.

**Values:**

-   *copyrighted:* Under copyright.
-   *public domain:* In the public domain and may be used without
    copyright restriction.
-   *unknown:* Copyright status of the resource is unknown.

The above values are from the controlled vocabulary suggested by the
*Data Dictionary* and implemented by Archivematica.

#### Copyright Jurisdiction (copyrightJurisdiction)

**PREMIS Rule:** The country whose copyright laws apply. Mandatory.

**Values:**

-   us

The US would be the value for all current RAC use cases.

#### Copyright Determination Date (copyrightStatusDeterminationDate)

**PREMIS Rule:** The date that the copyright status recorded in
*copyrightStatus* was determined.

**Local Rule:** This is generally the date of creation of the PREMIS rights statement.

#### Copyright Note (copyrightNote)

**PREMIS Rule:** Additional information about the copyright status of the object.

**Local Rule:** Include more information about the copyright status
(e.g., whether the work was published or unpublished) and information about copyright included in the donor agreement. For example:

*Work for hire - copyright term 120 years from date of creation. Copyright held by the Rockefeller Foundation.*

#### Copyright Documentation Identifier (copyrightDocumentationIdentifier)

**PREMIS Rule:** A designation used to identify documentation supporting the specified Rights granted according to copyright uniquely within the repository system. This semantic unit is intended to refer to a document detailing the granting of permission when the Rights basis is copyright.

**Local Rule:** As the RAC does not currently use identifiers for donor agreements or internal policies, this semantic unit is not used at this time. This may change in the future. (Note that this is a container, and elements within the container have been omitted as we are not using them currently.)

#### Copyright Applicable Dates (copyrightApplicableDates)

**PREMIS Rule:** The date range during which the particular copyright applies or is applied to the content. This is distinct from
*termOfGrant*, which applies to a particular act expressed in
*rightsGranted* and may differ from the period of time the license,
statute, or other basis applies to the content.

**Local Rule:** When copyright status is "copyrighted," or "unknown,"
the start date is the creation date of the content (i.e., the creation date of the original item in the case of digitized material). When copyright status is "copyrighted," the end date is the date that copyright expires. If an item is in the public domain because copyright expired before ingest into the repository, record the date that it passed into the public domain. If an item is in the public domain at the time of its creation, record the date of creation for the item.

Basis: Donor (otherRightsInformation)-------------------------------------

#### Donor Documentation Identifier (otherRightsDocumentationIdentifier)

**PREMIS Rule:** A designation used to uniquely identify documentation supporting the specified Rights within the repository system, when the basis for these Rights is something other than copyright, license or statute.

**Local Rule:** As the RAC does not currently use identifiers for donor agreements or internal policies, this semantic unit is not used at this time. This may change in the future. (Note that this element is a container, and sub-elements within it have been omitted as we are not using them currently.)

#### Donor Agreement Applicable Dates (otherRightsApplicableDates)

**PREMIS Rule:** The date range during which the particular rights applies or applied to the content. This is distinct from
*termOfGrant*, which applies to a particular act expressed in
*rightsGranted* and may differ from the period of time the license,
statute or other basis applies to the content.

**Local Rule:** Record the start date of the donor agreement. Generally, RAC donor agreements apply in perpetuity; in these cases the end date should be recorded as "open." Otherwise, record the end date specified in the agreement.

#### Donor Agreement Note (otherRightsNote)

**PREMIS Rule:** Additional information about the rights of the object.

**Local Rule:** Include the portion(s) of the donor agreement that pertain to access and use of the object. If restrictions apply to a certain type of record covered by this rights statement (e.g., board meeting minutes are restricted for 10 years), include this information.

### Basis: Policy (otherRightsInformation)

#### Policy Documentation Identifier (otherRightsDocumentationIdentifier)

**PREMIS Rule:** A designation used to uniquely identify documentation supporting the specified Rights within the repository system, when the basis for these Rights is something other than copyright, license or statute.

**Local Rule:** As the RAC does not currently use identifiers for donor agreements or internal policies, this semantic unit is not used at this time. This may change in the future. (Note that this element is a container, and sub-elements within it have been omitted as we are not using them currently.)

#### Policy Applicable Dates (otherRightsApplicableDates)

**PREMIS Rule:** The date range during which the particular rights applies or applied to the content. This is distinct from
*termOfGrant*, which applies to a particular act expressed in rightsGranted and may differ from the period of time the license,
statute or other basis applies to the content.

**Local Rule**: Policy start date should be the date the policy was enacted. If unknown, use 1974, the year the RAC was founded. Policy end date is when the policy itself ends, not its application to the content (that is recorded in the associated act); therefore, the end date is "open" in most cases.

#### Policy Note (otherRightsNote)

**PREMIS Rule:** Additional information about the rights of the object.

**Local Rule**: Include information about the policy that applies to the object. For example:

*Personnel files (and/or personnel-like records) are restricted to ensure personal privacy and restrict access to materials generally considered confidential between an individual and his or her employer. Personnel records include: employment applications, resumes/CVs,
letters of appointment, personal references, salary or withholding statements, performance evaluations, and other correspondence discussing general or particular circumstances of an individual’s employment or job performance. Personnel records also often include personal injury and/or workers compensation insurance records or claims and pension documents. Records gathered in the course of conducting an executive search, or in filling a professional position,
may also be restricted. *

### Basis: License (licenseInformation)

#### License Documentation Identifier (licenseDocumentationIdentifier)

**PREMIS Rule:** A designation used to uniquely identify documentation supporting the specified rights granted by license within the repository system. This semantic unit is intended to refer to a document recording the granting of permission when the rights basis is license. For some repositories this may be a formal signed contract with a customer. If the granting agreement is verbal, this could point to a memo by the repository documenting the verbal agreement. The identifier is optional because the agreement may not be stored in a repository with an identifier. In the case of a verbal agreement, for example, the entire agreement may be included or described in the
*licenseTerms*.

**Local Rule:** As the RAC does not currently use identifiers for donor agreements or internal policies, this semantic unit is not used at this time. This may change in the future. (Note that this element is a container, and sub-elements within it have been omitted.)

#### License Terms (licenseTerms)

**PREMIS Rule:** Text describing the license or agreement by which permission was granted. This could contain the actual text of the license or agreement or a paraphrase or summary.

#### License Note (licenseNote)

**PREMIS Rule:** Additional information about the license. Information about the terms of the license should go in *licenseTerms*.
*licenseNotes* is intended for other types of information related to the license, such as contact persons, action dates, or interpretations. The note may also indicate the location of the license, for example, if it is available online or embedded in the object itself.

#### License Applicable Dates (licenseApplicableDates)

**PREMIS Rule:** The date range during which the license applies or is applied to the content. This is distinct from *termOfGrant*, which applies to a particular act expressed in *rightsGranted* and may differ from the period of time the license, statute or other basis applies to the content.

### Basis: Statute (statuteInformation)

#### Statute Jurisdiction (statuteJurisdiction)

**PREMIS Rule:** The country or other political body enacting the statute. The connection between the object and the rights granted is based on jurisdiction. Values should be taken from a controlled vocabulary. Mandatory.

#### Statute Citation (statuteCitation)

**PREMIS Rule:** An identifying designation for the statute. Mandatory.

**Local Rule**: Follow guidelines such as [*Introduction to Basic Legal Citation*](https://www.law.cornell.edu/citation/).

#### Statute Determination Date (statuteInformationDeterminationDate)

**PREMIS Rule:** The date that the determination was made that the statute authorized the permission(s) noted. The permission in question may be the subject of some interpretation. These assessments are made within a specific context and at a specific time. At another time the context, and therefore the assessment, could change. For this reason it can be important to record the date of the decision.

#### Statute Note (statuteNote)

**PREMIS Rule:** Additional information about the statute.

**Local Rule:** If necessary, this should contain information not recorded elsewhere in the basis or associated act about the statue's applicability to the content. If including text of the statute itself,
or other external text, link to it using
*statuteDocumentationIdentifier*.

#### Statute Documentation Identifier (statuteDocumentationIdentifier)

**PREMIS Rule:** A designation used to uniquely identify documentation supporting the specified rights granted by statute within the repository system. This semantic unit is intended to refer to a document detailing the granting of permission when the rights basis is statute. If repeated, use *statuteDocumentationIdentifierRole* to distinguish the role of the given documentation. For a particular law one might want to link to various sources of documentation, e.g. the law publication itself (role: law), the application decree that enforces it (role: application decree) or some addition text refining the law by showing a real-world verdict (role: case law).

##### Documentation Identifier Type (statuteDocumentationIdentifierType)

**PREMIS Rule:** A designation of the domain within which the statute documentation identifier is unique.

##### Documentation Identifier Value (statuteDocumentationIdentifierValue)

**PREMIS Rule:** The value of the *statuteDocumentationIdentifier*.

##### Documentation Role (statuteDocumentationRole)

**PREMIS Rule:** A value indicating the purpose or expected use of the documentation being identified. This information distinguishes the purpose of the supporting documentation especially when there are multiple documentation identifiers.

#### Statute Applicable Dates (statuteApplicableDates)

**PREMIS Rule:** The date range during which the statute applies or is applied to the content. This is distinct from *termOfGrant*, which applies to a particular act expressed in *rightsGranted* and may differ from the period of time the license, statute or other basis applies to the content.

[^1]: Library of Congress and PREMIS Editorial Committee, “PREMIS Data
    Dictionary for Preservation Metadata, Version 3.0 (Library of
    Congress).”

[^2]: Evelyn McLellan, "Implementing Rights Metadata for Digital
    Preservation," in *Digital Preservation Metadata for Practitioners*.
    New York, NY: Springer Berlin Heidelberg, 2016.
