# 5\. Common SBOM Document Quality Issues and Improvement Measures {#5.-common-sbom-document-quality-issues-and-improvement-measures}

This section highlights the challenges often encountered when generating and managing SBOM Document, and introduces best practices for addressing them. These measures are designed to enhance the accuracy, consistency, and transparency of SBOM Document, as well as to improve the overall processes involved in handling them.

## 5.1 Ensuring Accurate and Consistent “Value” Information {#5.1-ensuring-accurate-and-consistent-“value”-information}

### 5.1.1 Issue Overview {#5.1.1-issue-overview}

Challenges exist in the inconsistent representation of information such as package names, versions, and supplier names across different companies and tools. Without unified standards, automatic analysis of SBOM Document or vulnerability matching becomes challenging, leading to inaccuracies.

### 5.1.2 Detailed Description {#5.1.2-detailed-description}

In many SBOM guides and standards, the element ‘keys’ defining what should be included are explicitly specified along with the corresponding ‘value’ ranges and representation methods. However, since a precise format for these values is often not defined, issues can arise during practical implementation.

For example, the two SBOMs below use different package names \- one as 'hello' and the other as 'hello 0.0.1'. This discrepancy is due to differences in the output formats of the tools used.

* [example7-bin.spdx.json](https://github.com/spdx/spdx-examples/blob/master/software/example7/spdx2.2/example7-bin.spdx.json#L44)  
  "name": "hello",  
* [hello-dist.spdx.json](https://github.com/spdx/spdx-examples/blob/master/software/example12/spdx2.2/hello-dist.spdx.json#L56)  
  "name": "hello 0.0.1",

Although verifying that these represent the same package is possible – for example, by comparing their PURLs – the fact that different tools may output different values for the same element name frequently leads to confusion and poses challenges to the smooth operation of SBOM management.

### 5.1.3 Improvement Measures {#5.1.3-improvement-measures}

* The SBOM Document shall include the items documented in "[3.2 SBOM Elements](03-guidelines-to-enhance-sbom-document-quality.md#3.2-elements-to-be-included) \- Package information, Information that can uniquely identify a software package”, ensuring their consistent use throughout the entire supply chain.  
* Additionally, since the identifiers used to uniquely identify a package may differ depending on the entity distributing the SBOM Document, it is necessary to also provide information on which combination of elements and corresponding values should be used to accurately identify the software package. 

### 5.1.4 Risks and Considerations {#5.1.4-risks-and-considerations}

* If the documentation rules are inadequately defined or overly complex, there is a risk of misclassification due to incomplete handling of exceptional cases or unique notations.  
* If the rules deviate from actual operational practices, there is a risk of misclassification resulting from either insufficient or excessively stringent checks.  
* It should be noted that complete automation of tools and processes is challenging; therefore, final checks and exception handling will require manual review.

## 5.2 Standardization and Normalization of Component Granularity {#5.2-standardization-and-normalization-of-component-granularity}

### 5.2.1 Issue Overview {#5.2.1-issue-overview}

In SBOM terminology, 'granularity' refers to the level of detail at which software components are described — primarily either at the file level or the package level. When SBOM Documents exchanged across the supply chain use different levels of granularity, it becomes difficult to describe and interpret dependency relationships consistently.

This is especially true in complex supply chains where the entity that ultimately creates the SBOM cannot affect the entire supply chain.

### 5.2.2 Detailed Description {#5.2.2-detailed-description}

Consider a simplified three-party scenario: **Vendor1** (a component supplier), **Maker1** (a product manufacturer), and **User1** (an end user). If **Vendor1** supplies a file-level SBOM but **Maker1** requires a package-level SBOM, **Maker1** must perform the conversion.  
The reverse situation creates the same burden. Such conversion requires a full review of the SBOM, increasing workload for the receiving entity.  
The assumptions are as follows: 

* **Vendor1** provides ‘application A‘ binary along with SBOM Document to **Maker1**  
* **Maker1** supplies ‘product X‘ (which includes application A and other OSS) along with SBOM Document to **User1**  
* ‘application A‘ depends on the OSS used in product X.

Under these conditions, the following table summarizes the possible combinations of component granularity in the SBOM Document.

Table 5.2.2

|  |  | SBOM Document for ‘product X’ to User1 | SBOM Document for ‘product X’ to User1 |
| :---- | :---- | :---- | :---- |
|  | component granularity | file-level | package-level |
| SBOM Document for ‘application A’ from **Vendor1** | file-level | All dependency shall be recorded and provided at the file-level. | **Maker1** needs to convert file-level information to package-level for application A. |
| SBOM Document for ‘application A’ from **Vendor1** | package-level | **Maker1** needs to decompose package-level information and convert them to  file-level. | All dependency shall be recorded and provided at the package-level. |

Differences in component granularity within SBOM Document require a full review to determine its granularity level, leading to an increased workload for the integrating entity.

### 5.2.3 Improvement Measures {#5.2.3-improvement-measures}

* When creating an SBOM, explicitly state within the document whether it is at the file level or the package level.Where the requesting party is able to specify the required  
  granularity level, this should be agreed upon in advance.    
  * If the SBOM requestor can specify the granularity, it should. However, in the case of a complex supply chain, there may be situations where the specification from the requester cannot affect the entire supply chain.  
* Define explicit granularity elements in future versions of the Data format specifications to clarify whether the information is at the file or package level.

### 5.2.4 Risks and Considerations {#5.2.4-risks-and-considerations}

* There is a potential for issues when linking with vulnerability information. For example, if component granularity is mixed, it may not be possible to automatically identify the corresponding vulnerability information, leading to concerns over increased manual effort and time.

## 5.3 Complementing Source Code Information and Enhancing Transparency {#5.3-complementing-source-code-information-and-enhancing-transparency}

### 5.3.1 Issue Overview {#5.3.1-issue-overview}

Software is often distributed in binary form (compiled executables). If the corresponding source code is not recorded in the SBOM Document, it becomes difficult to:

* Verify that the appropriate license terms have been correctly applied  
* Assess whether known vulnerabilities affect the exact source code used, or whether they have already been remediated

 Including source code information  in the SBOM Document significantly improves transparency and traceability throughout the software supply chain.

### 5.3.2 Detailed Description {#5.3.2-detailed-description}

Adhering to "[3.2 SBOM Elements](03-guidelines-to-enhance-sbom-document-quality.md#3.2-elements-to-be-included) – Package information, Information that can uniquely identify a software package" can sometimes allow you to locate source code in repositories like GitHub. However, when creating binary components, patches are often applied for customization or to fix vulnerabilities and bugs. In such cases, it becomes challenging to pinpoint the exact source code used. Furthermore, even for the same component and version, differences in SBOM types can lead to discrepancies: Source SBOMs do not provide the additional build information, while Build SBOMs include redundant details that are absent from the delivered binary component. Therefore, it is necessary to retain the exact source code information used during the build that is included in the binary component.

### 5.3.3 Improvement Measures {#5.3.3-improvement-measures}

To improve supply chain transparency, the SBOM Document author can attach source code information for binary components as follows:

* Add the source code information for a binary component as a separate source code component in the SBOM Document and manage its details. For example, SPDX allows linking binary components and source code components using relationships such as GENERATED\_FROM, DESCENDANT\_OF, or CONTAINS (see also Section 5.8).    
* CycloneDX provides a “pedigree” attribute for holding source code details, allowing the SBOM Document to include direct source code and patch information for the binary component.

The source code information should include, when available:  

* The source URL (this may be the URL of a tar.xz file or a version control system URL).    
* A hash value of the source code, with a clear indication of what the hash covers and at which level it was computed. Specifically:  
  * For archive files (e.g., .tar.xz): provide the hash algorithm (e.g., SHA-512) and confirm that the hash applies to the archive file as a whole, not to individual files within it.  
  * For version control systems: provide the commit hash, which unambiguously identifies the exact revision of the source tree.  
  * Alternatively, a SWHID (Software Heritage Identifier) may be used, as it encodes both the object type and scope explicitly.

In all cases, the hash entry should specify: (1) the hash algorithm, (2) the hash value, and (3) the scope of the hash (e.g., archive-level, file-level, or commit-level), so that recipients can reliably reproduce and verify the hash.

* If modifications were made during the build process, corresponding patch information should be included alongside the source code details.

### 5.3.4 Risks and Considerations {#5.3.4-risks-and-considerations}

Verifying the validity of the source code information attached by the SBOM Document provider to binary components is challenging for SBOM Document recipients.   
One potential method of verification is for the recipient to rebuild the binary and confirm that it matches. However, to do so, the SBOM Document provider must supply additional build environment information and ensure that the build is reproducible.

## 5.4 Component Identification Issues Affecting Vulnerability Handling and License Compliance {#5.4-component-identification-issues-affecting-vulnerability-handling-and-license-compliance}

### 5.4.1 Issue Overview {#5.4.1-issue-overview}

SBOM is expected to enhance vulnerability handling and license compliance by providing:

* Component identification information that can be cross-referenced with vulnerability databases or license databases to determine if a vulnerability or a license problem exists  
* Supplier information for reporting to remediate found such problems  
* Information of inter-component dependencies to assess how a vulnerability in one component might affect others or a license conflicts among their components

However, these elements are not always captured accurately or comprehensively, leading to:

* Inconsistent naming and identifier usage that hampers effective correlation with vulnerability or license data  
* Inadequateness or ambiguity of supplier information reducing the ability to address vulnerability and license problems  
* Incomplete dependency of inter-component relationships inducing oversight of cross-component impacts from the problems

### 5.4.2 Detailed Description {#5.4.2-detailed-description}

1. When the identification relies on component names and version numbers, inconsistencies such as label variations (as highlighted in [Section 5.1](#5.1-ensuring-accurate-and-consistent-“value”-information)) can occur, leading to missed detection of vulnerabilities or licenses of the components. Even the inclusion of unique identifiers like PURLs or CPE names does not guarantee a match in vulnerability databases or license databases, since those identifiers may not be present there.  
2. SBOM Document generated by automated tools often fall short in providing thorough supplier information. The supplier fields may be omitted or described in a way that does not clearly identify the organization or individual responsible – for instance, sometimes only a name is provided without accompanying contact information such as an email address. This lack of precise supplier information complicates reporting of vulnerability and license problems and their follow-up actions.  
3. The completeness of inter-component dependency information depends heavily on the tools, settings, and the technologies (programming languages or development environments) used. This can result in an incomplete mapping of dependency relationships, which in turn may lead to an underestimation of how vulnerabilities in one component can impact others, and overlook a license conflict between interdependent components, within the software product.

This detailed breakdown underscores that despite SBOM Document’s potential in improving vulnerability handling and license compliance, issues with inconsistent component identification and incomplete data often limit its effectiveness.

### 5.4.3 Improvement Measures {#5.4.3-improvement-measures}

1. Refer to Section 5.1 and use the naming conventions employed by vulnerability databases such as Open Source Vulnerabilities (OSV) and National Vulnerability Database (NVD) to record the component name, and include the corresponding unique software identifiers (e.g., a PURL for OSV or a CPE name for NVD) within the component information.  Furthermore, include as much information as possible that is recommended for inclusion in the SBOM, such as the supplier name or hash value to be referenced when component names are identical.  
2. Ensure that the supplier information identifies a real, publicly recognized entity or individual by verifying its existence and providing clear contact details such as the name, a valid email address, or a URL.    
3. Utilize tools that can extract inter-component dependency information from package managers to generate a comprehensive SBOM Document, and when possible, use build tools capable of producing a Build SBOM according to CISA’s SBOM Type Classification.


### 5.4.4 Risks and Considerations {#5.4.4-risks-and-considerations}

1. More kinds of SBOM information does not always improve the comprehensiveness of vulnerability and licensing problem detection, since (set of) information in the SBOM used to identify components and algorithm used to match those components against vulnerability and licensing databases depends on a tool for them. Moreover, dynamically loaded components at runtime may not be captured in the SBOM, which further compromises the accuracy of vulnerability and license correlation.  
2. Since a component's supplier may change over time due to mergers, business closures, or shifts in support responsibilities, it is essential to verify and update supplier information on a regular basis.  
3. Automatically extracted dependency information, especially when derived from static analysis or less comprehensive techniques rather than robust build tools, may not accurately capture the actual runtime inter-component relationships, particularly in cases where components are dynamically loaded, leading to gaps in the overall depiction of dependencies within the SBOM Document.

## 5.5 Establishing a Tamper Detection and Change Management System {#5.5-establishing-a-tamper-detection-and-change-management-system}

### 5.5.1 Issue Overview {#5.5.1-issue-overview}

In SBOM operations, the SBOM Document may be updated on a different schedule than the  provided software itself, and its content may contain errors. However, there is currently no sufficient mechanism to detect modifications to the SBOM Document, making it difficult to guarantee consistency with the software.

### 5.5.2 Detailed Description {#5.5.2-detailed-description}

* Manual modifications leading to data inconsistencies  
  When the SBOM Document is later edited or supplemented by hand, the resulting document may no longer match the original software configuration, risking unintended changes such as the addition of unneeded component information.    
* Lack of version control causing opaque modification history  
  Without a system to record changes, it becomes impossible to trace who modified the SBOM Document, when, why, or how, making it difficult to distinguish between intentional tampering and routine updates, and preventing rollback to previous, trusted versions.    
* Inconsistent formats and missing information  
  When SBOM Documents are automatically generated by multiple tools, differences in format and content can result in the loss of critical details – such as dependency information – during integration or editing, potentially leading to incomplete security assessments.

### 5.5.3 Improvement Measures {#5.5.3-improvement-measures}

* Unique identification and digital signatures  
  Every SBOM Document, regardless of format, should carry a unique identifier that is distinct from any digital signature. Upon generation, a digital signature or hash value should be immediately appended to enable later detection of any modifications.   
* Change history management  
  SBOM Documents should be treated as immutable. Any modification should result in the creation of a new SBOM Document with a new unique identifier, rather than overwriting the existing one. For example, where supported by the format, relationships such as AMENDED\_BY or ANCESTOR\_OF in SPDX should be used to link the updated document to its previous version. All changes should be recorded and tracked using a version control system.   
* Automated verification  
  Implement tools that automatically check the integrity of digital signatures or hash values.  
* Automated monitoring  
  Establish a tamper-detection process that regularly verifies signatures and issues alerts when abnormalities are detected.    
* Update and redistribution policies  
  Formalize procedures for regularly updating the SBOM Documents, reapplying signatures, and automatically redistributing the revised document. Any distribution constraints applicable to the SBOM Document should be explicitly identified and communicated to recipients as part of these procedures.    
* Audit and review framework  
  Complement automated measures with periodic reviews, log audits, and manual final verifications.

### 5.5.4 Risks and Considerations {#5.5.4-risks-and-considerations}

* Key management risks  
  Be alert to the leakage or expiration of signing keys and certificates; implement proper key management and revocation procedures.    
* Increased operational burden  
  Introducing new processes may necessitate changes to development environments and incur additional team training costs.    
* Inter-tool compatibility issues  
  Differences in SBOM tool formats can compromise the consistency of hash values and signatures.    
* Minimizing manual intervention  
  Maximize automation to eliminate human errors, limiting manual verification to the minimum required.

## 

## 5.6 Clarifying the Scope of Descriptions and Defining Accountability {#5.6-clarifying-the-scope-of-descriptions-and-defining-accountability}

### 5.6.1 Issue Overview {#5.6.1-issue-overview}

SBOM documents largely rely on the provider's discretion, resulting in inconsistent comprehensiveness and accuracy of the required information. Consequently, essential data may be missing across the software supply chain, increasing the risk of confusion in managing security and license compliance.

### 5.6.2 Detailed Description {#5.6.2-detailed-description}

Various guidelines and specifications exist, yet the criteria for including dependent components in an SBOM and the level of detail required for each component remain unclear.

* Only a limited set of necessary details, such as the scope of software dependencies and version information, may be included.  
* It is often unclear who is accountable for each component, as contact information and management responsibilities are not clearly defined. This lack of clarity hampers rapid response when issues arise.

Consequently, incomplete information within the SBOM can prevent a full understanding of vulnerabilities and license-related risks across the entire software supply chain, potentially impeding effective security measures.

### 5.6.3 Improvement Measures {#5.6.3-improvement-measures}

* Establish clear, standardized criteria for the items that need to be included in an SBOM Document across the entire software supply chain, with particular emphasis on clarifying the scope of dependencies.  
* Clearly define accountability for each item by including specific responsible individuals and contact details within the document. 

When considering the software supply chain, the supplier field should be treated as the primary field for identifying the party accountable for the component, as it most directly represents the entity responsible for its distribution and support across each tier of the supply chain.

When recording contact information, note that available fields differ between formats. In SPDX, the relevant fields are `supplier` and `originator`, while in CycloneDX, `supplier`, `manufacturer`, and `authors` are available. Among these, the `supplier` field should be prioritized in both formats, as it most clearly indicates the party bearing accountability for the component at each point in the supply chain. The `originator` field in SPDX and the `manufacturer` field in CycloneDX may be used to record the entity that originally created the component, which may differ from the supplier in cases involving repackaging or redistribution through multiple supply chain tiers.

Where the existing fields are insufficient to clearly express who holds accountability across the supply chain, for example when distinguishing between legal responsibility and technical contact at different tiers, a Comment field should be used as a supplement to explicitly state the accountable party and the nature of their responsibility.

This approach will enable effective information sharing and rapid response when issues arise. Following diagrams are the software supply chain as an illustrative example.

![][image2]  
*Fig.5-7.1 Entire software supply chain*

![][image3]  
*Fig.5-7.2 Three entities for “App-A SBOM Document” provided to ”User”.*

“Entity-B” is generally responsible for direct vulnerability response and OSS license compliance for “User”. There is no requirement to provide “User” with the contact details of "Entity-A" and "Entity-C"; however, "Entity-B" must know the proper contact information for "Entity-A" and "Entity-C". Consequently, the SBOM Document provided by "Entity-A" and "Entity-C" must include their respective appropriate contact details.

![][image4]  
*Fig.5-7.3 Independent SBOM Document Creation by Entity-A and Entity-C*

Furthermore, if 'Entity-B' is aware that 'App-A' has a dynamic dependency on 'Lib-C.so' but does not know from whom 'Lib-C.so' is actually distributed, it is essential that the SBOM Document for 'App-A' explicitly indicates this dependency. Additionally, the SBOM Document for "App-A" must clearly indicate that its distribution does not include "Lib-C.so" and that detailed information, such as the version number, about "Lib-C.so" remains unavailable, classifying it as "Known Unknown".  
![][image5]  
*Fig.5-7.4 “Known Unknown” for Dynamic runtime dependencies*

### 5.6.4 Risks and Considerations {#5.6.4-risks-and-considerations}

* Delays in updating the SBOM Documents in response to software version upgrades or changes in dependencies may result in decisions being made based on outdated information.    
* A lack of uniform criteria across the supply chain can lead to inconsistencies among SBOM Documents, causing confusion.  
* If the responsible parties or contact information are not clearly specified or are recorded incorrectly, the ability to respond swiftly during issues may be compromised, thereby increasing security risks.

## 5.7 Unified Expression of Component Relationships {#5.7-unified-expression-of-component-relationships}

### 5.7.1 Issue Overview {#5.7.1-issue-overview}

Inconsistent depiction of relationships between components (e.g., dependency, containment, derivation) may hinder accurate automated analysis and risk assessment.

### 5.7.2 Detailed Description {#5.7.2-detailed-description}

SBOM is used to list not only the direct software components but also the libraries, development tools, and developers as SBOM elements, and to describe the relationships among them. However, due to the varying complexity and scale of software, several challenges arise in describing these relationships:

1. The method for describing relationships between components (e.g., depends, contains, generates) varies significantly, and the selected notation differs among individuals, leading to confusion.  
2. The notations used are ambiguous, as a single keyword may be applied to different relationships. This ambiguity causes interpretation differences between providers and recipients, potentially impacting risk assessment.

### 5.7.3 Improvement Measures {#5.7.3-improvement-measures}

Although various keywords may be used to describe relationships, it is recommended that the following primary relationships be consistently included in the SBOM Document and uniformly defined across the entire supply chain.

1. Keywords for describing the current state of a component  
   1. “contains” / ”composition-assemblies”  
      Indicates that a component includes or is composed of another.    
   2. “dependsOn” / “composition-dependencies”  
      Indicates that a component depends on or requires another.  
2. Keywords for describing the origin of a component    
   1. “generatedFrom” / “components-pedigree”  
      Indicates that a component was generated (replicated, modified, or built) from another.  

### 5.7.4 Risks and Considerations {#5.7.4-risks-and-considerations}

These recommendations allow the inclusion of additional relationship keywords alongside the primary relationships, provided all terms are coordinated in advance to avoid confusion.

## 5.8 Interoperability and Flexibility Among Tools {#5.8-interoperability-and-flexibility-among-tools}

### 5.8.1 Issue Overview {#5.8.1-issue-overview}

Different SBOM generation and analysis tools, such as ORT (OSS Review Toolkit), FOSSology, or ScanCode etc., may produce outputs with variations that complicate integration in a unified processing workflow.  
Furthermore, optional fields such as “comment” may carry information critical to specific operational contexts, for example, build options or provenance remarks agreed upon between supplier and recipient. However, such information is frequently lost when SBOM Documents are processed, merged, or transformed by tools. Without prior agreement between the supplier and recipient on which fields are to be preserved and how they are to be interpreted, relying on these fields for operational purposes can lead to inconsistencies and workflow failures.

### 5.8.2 Detailed Description {#5.8.2-detailed-description}

Many software tools designed for generating and managing SBOM Document are typically developed with interoperability in mind. However, in practical settings, there have been few comprehensive evaluations of the compatibility across these various tools, rendering their true degree of interoperability uncertain.   
Moreover, even when compatibility exists, there is generally insufficient documentation on the integration process, which consequently forces users to resort to trial-and-error methods to achieve effective configurations.  
Certain SBOM fields, such as Comment, are optional and intended to carry supplementary information. In practice, however, the content of these fields is often lost or discarded when SBOM Documents pass through tools for processing, merging, or format conversion. This problem is compounded by the fact that the meaning and intended use of such fields may vary between the supplier and the recipient. When a supplier records operationally significant information, such as license analysis notes, provenance remarks, or exception justifications, in a Comment field without prior agreement with the recipient, that information may be silently dropped or misinterpreted downstream. As a result, operational decisions that depend on such information can fail or produce inconsistent outcomes, particularly in multi-tier supply chains where SBOM Documents are handed off across organizational boundaries.

### 5.8.3 Improvement Measures {#5.8.3-improvement-measures}

When employing a mixed environment of tools for handling SBOM Documents, manual adjustments such as value conversions and schema modifications are often unavoidable. To alleviate these issues, it is effective to designate a primary format at the operational level, taking into account the following criteria:

* Existing assets  
* The extent of compatibility across a wide range of tools  
* The proficiency of the personnel responsible

SBOM Documents received from upstream sources should retain identifiers such as PURL and generator information. Format transition procedures should be standardized through configuration files and minimal sample setups, and syntactic validation (such as with JSON Schema) should be automated in continuous integration environments where possible.

For optional fields such as Comment, suppliers and recipients should establish prior agreement on which fields must be preserved during tool-based processing and how their content is to be interpreted, as such information is frequently lost during merging or format conversion.

To identify and address interoperability gaps, it is strongly encouraged to produce SBOM Documents in both SPDX and CycloneDX formats using multiple tools and the latest version of each standard. A representative process would involve producing at least four SBOM Documents, with two tools each generating both a SPDX and a CycloneDX output. The resulting SBOM Documents should be systematically compared to identify discrepancies in field mapping, value representation, and the preservation of optional fields such as Comment. The findings from such comparisons should be documented and shared with the relevant tool development and standards communities to drive concrete improvements in interoperability. Looking ahead, tool developers, format stewards, and the wider SBOM community should engage in active dialogue to establish common interoperability requirements and resolve compatibility issues across supply chains.

### 5.8.4 Risks and Considerations {#5.8.4-risks-and-considerations}

1. **Semantic Loss During Format Conversion**  
   Even when syntactic compatibility between SPDX and CycloneDX is achieved, information expressed in one format cannot always be accurately reproduced in the other due to structural differences in their respective fields. This risk is particularly pronounced for optional fields such as Comment, where information loss or unintended changes in meaning during format conversion cannot be fully eliminated.  
2. **Maintaining Agreements Across Multi-tier Supply Chains**  
   Even when operational agreements are established with direct trading partners, there is no guarantee that equivalent agreements are maintained further upstream or downstream. The longer and more complex the supply chain, the more difficult it becomes to ensure consistent preservation and interpretation of fields across all parties involved.  
3. **Keeping Pace with Evolving Specifications**  
   Both SPDX and CycloneDX specifications are subject to continuous revision. When tool implementations or organizational procedures fail to keep pace with these changes, interoperability issues are likely to recur. Even as community-level discussions on compatibility progress, whether each tool implements the resulting agreements promptly and accurately remains a separate concern, making ongoing validation and update cycles indispensable.

[image2]: <../assets/images/sbom-document-quality-guide/fig-5-7-1-entire-software-supply-chain.png>
[image3]: <../assets/images/sbom-document-quality-guide/fig-5-7-2-three-entities-for-app-a-sbom-document-provided-to-user.png>
[image4]: <../assets/images/sbom-document-quality-guide/fig-5-7-3-independent-sbom-document-creation-by-entity-a-and-entity-c.png>
[image5]: <../assets/images/sbom-document-quality-guide/fig-5-7-4-known-unknown-for-dynamic-runtime-dependencies.png>
