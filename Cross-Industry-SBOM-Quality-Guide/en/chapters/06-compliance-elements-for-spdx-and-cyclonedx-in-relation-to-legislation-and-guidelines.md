# 6. Compliance Elements for SPDX and CycloneDX in Relation to Legislation and Guidelines

This chapter provides a comprehensive analysis of the compliance elements associated with various guidelines within the frameworks of SPDX and CycloneDX. It is organized into sections that detail each guideline's requirements, categorizing them as mandatory or recommended. For each guideline, the chapter outlines the key elements, including their names, associated data fields, and supplementary remarks. Emphasis is placed on establishing a clear mapping between the guideline elements and the corresponding data fields defined in SPDX and CycloneDX. This structured approach facilitates cross-referencing and ensures consistency in documenting compliance requirements across multiple frameworks.

## 6.1 References

* SPDX v2.2+  
  [https://spdx.github.io/spdx-spec/v2.3/how-to-use/\#k2-satisfying-ntia-minimum-elements-for-an-sbom-using-spdx](https://spdx.github.io/spdx-spec/v2.3/how-to-use/#k2-satisfying-ntia-minimum-elements-for-an-sbom-using-spdx)  
* SPDX v3.0  
  [https://github.com/spdx/using/blob/main/docs/comply-with-norms.md\#satisfying-ntia-minimum-elements-for-an-sbom-using-spdx--us-executive-order-14028](https://github.com/spdx/using/blob/main/docs/comply-with-norms.md#satisfying-ntia-minimum-elements-for-an-sbom-using-spdx--us-executive-order-14028)  
* CycloneDX v1.7  
  [https://cyclonedx.org/guides/OWASP\_CycloneDX-Authoritative-Guide-to-SBOM-en.pdf](https://cyclonedx.org/guides/OWASP_CycloneDX-Authoritative-Guide-to-SBOM-en.pdf)  
* NTIA \- The Minimum Elements For a Software Bill of Materials (SBOM)  
  [https://www.ntia.gov/report/2021/minimum-elements-software-bill-materials-sbom](https://www.ntia.gov/report/2021/minimum-elements-software-bill-materials-sbom)  
* CISA \- 2025 Minimum Elements for a Software Bill of Materials (SBOM)  
  [https://www.cisa.gov/resources-tools/resources/2025-minimum-elements-software-bill-materials-sbom](https://www.cisa.gov/resources-tools/resources/2025-minimum-elements-software-bill-materials-sbom)  
* BSI TR-03183 Cyber Resilience Requirements for  Manufacturers and Products Part 2: Software Bill of Materials (SBOM) Version 2.1.0  
  “8.2 Mapping of the individual data fields”: Cyber Resilience Requirements for Manufacturers and Products \- Part 2: Software Bill of Materials (SBOM) Version 2.1.0  
  [https://www.bsi.bund.de/EN/Themen/Unternehmen-und-Organisationen/Standards-und-Zertifizierung/Technische-Richtlinien/TR-nach-Thema-sortiert/tr03183/TR-03183\_node.html](https://www.bsi.bund.de/EN/Themen/Unternehmen-und-Organisationen/Standards-und-Zertifizierung/Technische-Richtlinien/TR-nach-Thema-sortiert/tr03183/TR-03183_node.html)  
* OpenChain Telco SBOM Guide Version 1.1  
  [https://github.com/OpenChain-Project/Telco-WG/blob/main/OpenChain-Telco-SBOM-Guide\_EN.md](https://github.com/OpenChain-Project/Telco-WG/blob/main/OpenChain-Telco-SBOM-Guide_EN.md)


## 6.2 Cross-Regulation Comparison Table

This table consolidates all element requirements from the four standards covered in this chapter. It enables implementers to identify which elements satisfy multiple regulations simultaneously and to design a single SBOM that meets all applicable obligations.

| Category | Element | NTIA 2021 | CISA 2025 | BSI TR-03183 | OpenChain Telco Guide | Notes / Points of Attention |
| :---- | :---- | ----- | ----- | ----- | ----- | :---- |
| **SBOM Document Metadata** | **SBOM Author / Creator of SBOM** | **SHALL** | **SHALL** | **SHALL** | **SHALL** | Different names across standards: 'Author of SBOM Data' (NTIA), 'SBOM Author' (CISA), 'Creator of the SBOM' (BSI). Contact details (email or URL) should be included where possible. |
|   | **Timestamp** | **SHALL** | **SHALL** | **SHALL** | **SHALL** |   |
|   | **Tool Name** | — | **SHALL** | — | **SHALL** | New mandatory field added in CISA 2025; absent from NTIA 2021\. Enables recipients to assess SBOM data quality and reproducibility. Tool version SHOULD also be recorded. |
|   | **Generation Context / Lifecycle Phase** | ***SHOULD*** | **SHALL** | — | ***SHOULD*** | Upgraded from SHOULD (NTIA 2021\) to SHALL (CISA 2025). Records whether the SBOM was generated before build, during build, or after build. BSI does not address this field explicitly. Build-time SBOMs are considered the most accurate. |
|   | **Unique ID for SBOM Document** | **SHALL** | **SHALL** | **SHALL** | **SHALL** | BSI names this 'SBOM-URI'. In CycloneDX a BOM-Link (urn:cdx:{serialNumber}/{version}) is used for cross-referencing. Must be globally unique. |
|   | **License for the SBOM Document** | — | — | — | **SHALL** | Telco guide-specific: SPDX v2 requires the DataLicense field (CC0-1.0). Not addressed by NTIA, CISA, or BSI. |
| **Software Package Information** | **Software Producer / Supplier Name** | **SHALL** | **SHALL** | **SHALL** | **SHALL** | CISA 2025 renamed 'Supplier Name' (NTIA) to 'Software Producer' to avoid confusion with distributors. BSI additionally requires a URL or email contact address (mandatory, not optional). |
|   | **Component Creator / Originator** | — | — | **SHALL** | — | BSI-specific field. Distinguishes the entity that wrote the code from the entity that supplies/distributes it. Must include a URL or email address. Not present in NTIA or CISA. |
|   | **Component Name** | **SHALL** | **SHALL** | **SHALL** | **SHALL** | Naming conventions differ across vulnerability databases (OSV, NVD). Suppliers and recipients should agree on which naming convention to follow (see Section 5.1). |
|   | **Component Version** | **SHALL** | **SHALL** | **SHALL** | **SHALL** |   |
|   | **Software Identifiers (PURL / CPE / SWHID)** | **SHALL** | **SHALL** | **SHALL** | ***SHOULD*** | Different names: 'Other Unique Identifiers' (NTIA), 'Software Identifiers' (CISA). PURL is supported by OSV; CPE names are used by NVD. Recording both improves cross-database coverage. Telco recommends PURL specifically. BSI requires CPE, PURL, or SWID. |
|   | **Component Hash** | ***SHOULD*** | **SHALL** | **SHALL** | ***SHOULD*** | Upgraded from SHOULD (NTIA) to SHALL (CISA 2025). BSI explicitly requires the hash of the deployable artifact (binary); source code hash is a separate optional field. SHA-256 or stronger is recommended. |
|   | **Filename of the Component** | — | — | **SHALL** | — | BSI-specific. Mandatory for traceability of deployable artifacts. Not addressed by NTIA, CISA, or Telco. |
|   | **Package Download / Distribution URL** | — | — | **SHALL** | **SHALL** | Required by both BSI and Telco, but not by NTIA or CISA. BSI also separately requires the URI of the deployable form of the component. |
|   | **Source Code URI** | — | — | **SHALL** | — | BSI-specific. Enables traceability from binary back to source. Particularly important for CRA compliance and Open Source license verification. See also Section 5.3 on source code transparency. |
|   | **File Type Properties (Executable / Archive / Structured)** | — | — | **SHALL** | — | BSI-specific. Classifies the type of each deployable artifact (executable binary, archive, or structured data such as container/firmware). Required for CRA-aligned artifact traceability. |
| **License Information** | **Declared License** | ***SHOULD*** | **SHALL** | **SHALL** | **SHALL** | Upgraded from SHOULD (NTIA 2021\) to SHALL (CISA 2025). Use SPDX License Expressions for consistency. For proprietary licenses, include contact information alongside the Proprietary Software indicator. |
|   | **Concluded License** | ***SHOULD*** | **SHALL** | **SHALL** | **SHALL** | Upgraded from SHOULD (NTIA 2021\) to SHALL (CISA 2025). If the license cannot be determined, mark with 'NOASSERTION' rather than omitting the field. BSI calls this 'Distribution licenses'. |
|   | **Effective Licence** | — | — | *MAY* | — | BSI-specific optional field. Records the license that effectively applies after considering dual-licensing, dependency relationships, and distribution context. Not addressed by other standards. |
|   | **Copyright Text** | — | — | — | **SHALL** | Required by Telco guide (and mandatory in SPDX v2 format). Not required by NTIA, CISA, or BSI at the SBOM element level. |
| **Relationship Information** | **Dependency Relationship** | **SHALL** | **SHALL** | **SHALL** | **SHALL** | All standards require this. NTIA/CISA: document must DESCRIBE at least one package and record CONTAINS relationships. BSI specifically requires 'contains' or 'dependsOn' relationship types. See Section 5.7 for guidance on unified relationship expressions. |
|   | **Other Component Relationships** | ***SHOULD*** | — | ***SHOULD*** | — | NTIA recommends recording additional relationships such as GENERATES, ANCESTOR\_OF, and VARIANT\_OF. CISA and Telco do not address these. See Section 5.7 for recommended keywords. |



## 6.3 NTIA - The Minimum Elements For a Software Bill of Materials (SBOM)

### 6.3.1 Required Elements

| Req. | Element Name | SPDX v2.2+ | SPDX v3.0 | CycloneDX v1.7 |
| ----- | :---- | :---- | :---- | :---- |
| **SHALL** | **Supplier Name** | (7.5)PackageSupplier | Core/Classes/Artifact.suppliedBy | bom.metadata.supplier bom.components\[\].supplier |
| **SHALL** | **Component Name** | (7.1)PackageName | Software/Classes/Package.name | bom.components\[\].name |
| **SHALL** | **Version of the Component** | (7.3)PackageVersion | Software/Classes/Package.packageVersion | bom.components\[\].version |
| **SHALL** | **Other Unique Identifiers** | (7.2)Package SPDX Identifier<br>(6.5)SPDX Document Namespace<br>(7.21)ExternalRef: PURL/CPE | Software/Classes/SoftwareArtifact.contentIdentifier Software/Classes/Package.packageUrl Core/Classes/Element.externalIdentifier | bom.components\[\].cpe, .purl, .swid |
| **SHALL** | **Dependency Relationship** | (11.1)Relationship: CONTAINS, DESCRIBES Document must DESCRIBE ≥ 1 package | Core/Classes/Relationship | bom.dependencies\[\] |
| **SHALL** | **Author of SBOM Data** | (6.8)Creator | Core/Classes/CreationInfo.createdBy | bom.metadata.author |
| **SHALL** | **Timestamp** | (6.9)Created | Core/Classes/CreationInfo.created | bom.metadata.timestamp |
| ***SHOULD*** | **Hash of the Component** | (7.10)PackageChecksum | Core/Classes/Element.verifiedUsing | bom.components\[\].hashes\[\] |
| ***SHOULD*** | **Lifecycle Phase** | N/A → (6.10)CreatorComment (workaround) | Software/Sbom/sbomType | bom.metadata.lifecycles\[\] |
| ***SHOULD*** | **Other Component Relationships** | (11.1)GENERATES, ANCESTOR\_OF, VARIANT\_OF… | Core/Classes/Relationship (various types) | bom.components\[\].pedigree |
| ***SHOULD*** | **License Information** | (7.13)PackageLicenseConcluded<br>(7.15)PackageLicenseDeclared | RelationshipType::hasConcludedLicense RelationshipType::hasDeclaredLicense | bom.components\[\].licenses\[\] |


## 6.4 CISA - 2025 Minimum Elements for a Software Bill of Materials (SBOM)

### 6.4.1 Required Elements

| Req. | Element Name | SPDX v2.2+ | SPDX v3.0 | CycloneDX v1.7 |
| ----- | :---- | :---- | :---- | :---- |
| **SHALL** | **SBOM Author** | (6.8)Creator | Core/Classes/CreationInfo.createdBy | bom.metadata.author |
| **SHALL** | **Software Producer** | (7.5)PackageSupplier | Core/Classes/Artifact.suppliedBy | bom.components\[\].supplier |
| **SHALL** | **Component Name** | (7.1)PackageName | Software/Classes/Package.name | bom.components\[\].name |
| **SHALL** | **Component Version** | (7.3)PackageVersion | Software/Classes/Package.packageVersion | bom.components\[\].version |
| **SHALL** | **Software Identifiers** | (7.2)Package SPDX Identifier<br>(6.5)SPDX Document Namespace<br>(7.21)ExternalRef: PURL/CPE | Software/Classes/SoftwareArtifact.contentIdentifier | bom.components\[\].cpe, .purl, .swid |
| **SHALL** | **Component Hash** | (7.10)PackageChecksum | Core/Classes/Element.verifiedUsing | bom.components\[\].hashes\[\] |
| **SHALL** | **License** | (7.13)PackageLicenseConcluded<br>(7.15)PackageLicenseDeclared | RelationshipType::hasConcludedLicense RelationshipType::hasDeclaredLicense | bom.components\[\].licenses\[\] |
| **SHALL** | **Dependency Relationship** | (11.1) Relationship: CONTAINS, DESCRIBES Document must DESCRIBE ≥ 1 package | Core/Classes/Relationship | bom.dependencies\[\] |
| **SHALL** | **Tool Name** | (6.8) Creator: Tool keyword | Core/Tool | bom.metadata.tools |
| **SHALL** | **Timestamp** | (6.9) Created | Core/Classes/CreationInfo.created | bom.metadata.timestamp |
| **SHALL** | **Generation Context** | (6.9) Created → model-definition | Software/Sbom/sbomType (via model-definition) | bom.metadata.lifecycles\[\] |

### 6.4.2 Table of Minimum Elements Data Fields

| Data Field | Description |
| :---- | :---- |
| **SBOM Author** | The name of the entity that creates the SBOM data for this component. |
| **Software Producer** | The name of an entity that creates, defines, and identifies components. |
| **Component Name** | The name assigned by the Software Producer to a software component. |
| **Component Version** | Identifier used by the Software Producer to specify a change in software from a previously identified version. |
| **Software Identifiers** | Identifier(s) used to identify a component or serve as a look-up key for relevant databases. |
| **Component Hash** | The cryptographic value generated from taking the hash of the software component. |
| **License** | The license(s) under which the software component is made available. |
| **Dependency Relationship** | The relationship between two software components, specifically noting that Software X includes Component Y or that Component A is largely derived from Component B. |
| **Tool Name** | The name of the tool used by the SBOM Author to generate the SBOM. |
| **Timestamp** | Record of the date and time of the most recent update to the SBOM data. |
| **Generation Context** | The relative software lifecycle phase and data available at the time the Software Producer generated the SBOM (before build, during build, after build). |

## 6.5 BSI TR-03183 Cyber Resilience Requirements for  Manufacturers and Products Part 2: Software Bill of Materials (SBOM) Version 2.1.0

### 6.5.1 Required Elements

TR-03182 version 2.1.0 does not allow SPDX v2.

| Req. | Element Name | SPDX v3.0 | CycloneDX v1.7 |
| ----- | :---- | :---- | :---- |
| **SHALL** | **Creator of the SBOM** | Core/Classes/CreationInfo.createdBy | bom.metadata.manufacturer\[\].url XOR bom.metadata.manufacturer\[\].contact\[\].email |
| **SHALL** | **Timestamp** | Core/Classes/CreationInfo.created | bom.metadata.timestamp |
| **SHALL** | **Component creator** | Software/Classes/Package.originatedBy | bom.components\[\].manufacturer\[\].url XOR bom.components\[\].manufacturer\[\].contact\[\].email |
| **SHALL** | **Component name** | Software/Classes/Package.name | bom.components\[\].name  (+ optionally .group) |
| **SHALL** | **Component version** | Software/Classes/Package.packageVersion | bom.components\[\].version |
| **SHALL** | **Filename of the component** | Software/Classes/File.name | bom.components\[\].properties\[name=bsi:component:filename\] |
| **SHALL** | **Dependencies on other components** | Core/Classes/Relationship (contains / dependsOn) | bom.components\[\].components bom.dependencies\[\] bom.compositions.assemblies\[\] / .dependencies\[\] |
| **SHALL** | **Distribution licenses** | Core/Classes/Relationship (hasConcludedLicense) | bom.components\[\].licenses\[\].expression  \[acknowledgement=concluded\] |
| **SHALL** | **Hash value of deployable component** | Software/Classes/File.verifiedUsing | components\[\].externalReferences\[\].hashes\[\]  \[type=distribution\] |
| **SHALL** | **Executable property** | Software/Classes/File.additionalPurpose=\[executable\] | bom.components\[\].properties\[name=bsi:component:executable\] |
| **SHALL** | **Archive property** | Software/Classes/File.additionalPurpose=\[archive\] | bom.components\[\].properties\[name=bsi:component:archive\] |
| **SHALL** | **Structured property** | Software/Classes/File.additionalPurpose=\[container|firmware\] | bom.components\[\].properties\[name=bsi:component:structured\] |
| **SHALL** | **SBOM-URI** | Software/Classes/Sbom.spdxId | bom.serialNumber (BOM-Link: urn:cdx:{serialNumber}/{version}) |
| **SHALL** | **Source code URI** | Software/Classes/SoftwareArtifact.externalRef (externalRefType=SourceArtifact) | bom.components\[\].externalReferences\[type=source-distribution\].url |
| **SHALL** | **URI of deployable form of component** | Software/Classes/File.externalRef (externalRefType=binaryArtifact) \[under discussion\] | bom.components\[\].externalReferences\[type=distribution\].url |
| **SHALL** | **Other unique identifiers** | Software/Classes/Package.externalIdentifiers (cpe22, cpe23, swid, packageURL) | bom.components\[\].cpe, .swid, .purl |
| **SHALL** | **Original licences (declared)** | Core/Classes/Relationship (hasDeclaredLicense) | bom.components\[\].licenses\[\].expression  \[acknowledgement=declared\] |
| *MAY* | **Effective licence** | Core/Classes/Relationship (type=other, comment=hasEffectiveLicense) | bom.components\[\].properties\[name=bsi:component:effectiveLicense\] |
| *MAY* | **Hash value of source code** | Software/Classes/SoftwareArtifact.verifiedUsing \+ Relationship (generates) | bom.components.externalReferences\[\].hashes\[\]  \[type=source-distribution\] |
| *MAY* | **URL of security.txt** | Software/Classes/Package.externalRef (externalRefType=securityOther) | bom.components\[\].externalReferences\[type=rfc-9116\].url |


## 6.6 OpenChain Telco SBOM Guide Version 1.1

### 6.6.1 Required Elements

The Telco SBOM Guide is primarily based on SPDX v2.2. SPDX v3.0 and CycloneDX equivalents are shown for reference only; they are not formally mandated by the Telco guide. Elements with no direct CycloneDX counterpart are marked with a dash (—).

#### 6.6.1.1 Document Creation Information

| Req. | Element Name | SPDX v2.2+ | SPDX v3.0 | CycloneDX v1.7 |
| ----- | :---- | :---- | :---- | :---- |
| **SHALL** | **SPDXVersion** | (6.1)SPDXVersion | Core/CreationInfo.specVersion | specVersion |
| **SHALL** | **DataLicense** | (6.2)DataLicense | Core/SpdxDocument.dataLicense | metadata.licenses |
| **SHALL** | **SPDXID (document)** | (6.3)SPDXID | Core/SpdxDocument.spdxId | serialNumber |
| **SHALL** | **DocumentName** | (6.4)DocumentName | Core/SpdxDocument.name | — |
| **SHALL** | **DocumentNamespace** | (6.5)SPDX Document Namespace | Core/SpdxDocument.nameSpaceMap | — |
| **SHALL** | **Creator: Organization \+ Tool** | (6.8)Creator: Organization, Tool | Core/CreationInfo.createdBy Core/CreationInfo.createdUsing | metadata.{component.manufacturer|authors} metadata.tools |
| **SHALL** | **Created** | (6.9)Created | Core/CreationInfo.created | metadata.timestamp |
| **SHALL** | **CreatorComment (SBOM Build Information)** | (6.10)CreatorComment | Core/CreationInfo.comment | metadata.lifecycles |

#### 6.6.1.2 Package Information

| Req. | Element Name | SPDX v2.2+ | SPDX v3.0 | CycloneDX v1.7 |
| ----- | :---- | :---- | :---- | :---- |
| **SHALL** | **PackageName** | (7.1) PackageName | Software/Package.name | metadata.component.name components.name |
| **SHALL** | **SPDXID (package)** | (7.2) Package SPDX Identifier | Software/Package.spdxId | metadata.component.bom-ref components.bom-ref |
| **SHALL** | **PackageVersion** | (7.3) PackageVersion | Software/Package.packageVersion | metadata.component.version components.version |
| **SHALL** | **PackageSupplier** | (7.5) PackageSupplier | Software/Package.suppliedBy | metadata.component.supplier components.supplier |
| **SHALL** | **PackageDownloadLocation** | (7.7) PackageDownloadLocation | Software/Package.downloadLocation | metadata.component.externalReferences components.externalReferences |
| **SHALL** | **PackageLicenseConcluded** | (7.13) PackageLicenseConcluded | Relationship (hasConcludedLicense) | metadata.component.licenses\[\]{expression, acknowledgement=concluded} |
| **SHALL** | **PackageLicenseDeclared** | (7.15) PackageLicenseDeclared | Relationship (hasDeclaredLicense) | metadata.component.licenses\[\]{expression, acknowledgement=declared} |
| **SHALL** | **PackageCopyrightText** | (7.17) PackageCopyrightText | Software/Package.copyrightText | metadata.component.copyright components.copyright |
| **SHALL** | **Relationship: DESCRIBES \+ CONTAINS** | (11.1) Relationship: CONTAINS, DESCRIBES | Relationship (contains, describes) | components.components dependencies |
| ***SHOULD*** | **PackageChecksum** | (7.10) PackageChecksum | Software/Package.verifiedUsing | metadata.component.hashes components.hashes |
| ***SHOULD*** | **ExternalRef: PURL (PackageURL)** | (7.21) ExternalRef: PACKAGE-MANAGER purl | Software/Package/externalIdentifier (packageUrl) | purl |
