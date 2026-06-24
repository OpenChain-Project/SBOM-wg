# 3. Guidelines to Enhance SBOM Document Quality

Chapter 3 defines the criteria by which the OpenChain SBOM Document Quality Guide recognizes an SBOM Document as high-quality.

## 3.1. Data Format

An SBOM Document SHALL be provided in a machine‑processable format that enables automated processing by organizations, customers, and regulatory or supervisory authorities.  
The use of internationally recognized formats such as SPDX® or CycloneDX is therefore considered mandatory for such purposes.

### 3.1.1 Verification and reference material

SPDX®: [https://spdx.dev/use/specifications/](https://spdx.dev/use/specifications/) , [https://tools.spdx.org/app/validate/](https://tools.spdx.org/app/validate/)   
CycloneDX: [https://cyclonedx.org/specification/overview/](https://cyclonedx.org/specification/overview/) , [https://github.com/CycloneDX/sbom-utility](https://github.com/CycloneDX/sbom-utility) 

### 3.1.2 Rationale

To ensure simplified handling and streamlining of tooling and competences in the software supply chain, both for suppliers and consumers of software, SBOM Documents shall employ a format generally recognized as the standard specification for SBOMs as standardized. By harmonizing the use of this standard SBOM Data Format in an organization's external interfaces, the reduction in complexity leads to fewer errors and, as a result, improves the document quality.

## 3.2 Elements to be included

An SBOM Document SHALL include all elements defined in this chapter, which the OpenChain SBOM Document Quality Guide considers indicative of high quality.

SBOM Document Information

* Data Format Version  
  Include the SBOM Document format and version to ensure tool interoperability.  
* License for the SBOM Document  
* Unique ID for the SBOM Document  
  Include a global unique identifier in the SBOM Document to represent the exchanged  SBOM Document. The specification for this unique value varies depending on the Data Format;   
  For example, CycloneDX uses 'serialNumber' (as part of a BOM-Link: urn:cdx:{serialNumber}/{version}), SPDX v3 uses a globally unique 'SPDXID' for the SpdxDocument, and SPDX v2.x uses a combination of 'DocumentNamespace' and 'SPDXID' to ensure global uniqueness.   
* Creation information for the SBOM Document  
  It is essential to include the details of who created the SBOM Document and when, as this information is necessary for contacting the responsible entities in case of issues.  
  * Author of SBOM Document  
    If permitted by the respective Data Format specifications, the creator’s information should ideally contain a valid email address or a URL for a contact form to ensure they are reachable.  
  * Creation Date and Time of SBOM Document  
  * Generation Context  
    In an SBOM Document, element values specifying the timing and method of its creation SHOULD be included, as these details are crucial for vulnerability management. Therefore, including the generation context such as [SBOM Types defined by CISA](https://www.cisa.gov/resources-tools/resources/types-software-bill-materials-sbom) in the document can enhance its overall quality. However, since a standardized element for generation context may not exist, harmonizing their usage across the supply chain is essential.

Software Package information

* Information that can uniquely identify a software package  
  For accurate license compliance and vulnerability assessment, it is essential to correctly identify each software package. The document shall include the following elements to ensure proper identification:  
  * Package Name  
  * Package Version  
  * Details regarding the package identifiers, such as the [SoftWare Hash IDentifiers (SWHID)](https://www.swhid.org/), the [Package-URL (PURL)](https://github.com/package-url/purl-spec), [CPE name](https://nvd.nist.gov/products/cpe), [gitoid](https://www.iana.org/assignments/uri-schemes/prov/gitoid) or the URL of the package distribution site, indicating where the corresponding software package can be obtained.

  It is also important for both the supplier and the recipient to agree on which fields are most effective for identifying software packages within the SBOM document.

* Package Supplier information  
  This element should contain contact details for the supplier of the package. It is desirable to include a valid email address or a URL to a contact form, ensuring that inquiries about the package can be addressed promptly.  
* Package Hash value  
  Each Software Package listed in the SBOM Document should include at least one corresponding hash value from the distributed component. This is necessary to verify that the particular component is indeed present in the distribution. The hash algorithm must be chosen from those specified in the respective Data Format and used to calculate the hash of the corresponding source code or binary component.  
* Proprietary Software indicator  
  For proprietary software, elements like Package Name and Package Version may be custom-defined. However, the SBOM Document should clearly indicate the software is proprietary \- for example, using package comments or a purl \- and should include Package Supplier information to facilitate inquiries.

License information

* License declared in the Software Package  
  The SBOM Document shall include the license information declared by the distributed package. The declared license SHALL utilize standardized identifiers, for example, [the SPDX License Expressions](https://spdx.org/licenses/), to ensure clarity and consistency.  
  For proprietary licenses, which are often custom-defined, please provide the contact information for inquiries along with the previously mentioned Proprietary Software indicator.  
* License concluded by the Package Distributor  
  The SBOM Document should include the license determined by the package distributor. For example, when a package is dual-licensed, the relationship with other packages may dictate which license applies. In addition, considering both the declared license and the overall software structure, the distributor must conclude the  license for distribution. It is recommended that the distributor include such element in the SBOM Document. If the appropriate license cannot be concluded \- and if allowed by the applicable specifications \- the element may either be omitted or marked as a ‘known unknown’, using a placeholder such as ‘NOASSERTION’ in SPDX document.

Relationship information

* As specified by NTIA SBOM Minimum Elements, the document must specify the primary component of the top level  Software Package and containment relationships, indicating that one component contains another.  
  For more practical details, please refer to Section “[5.6 Clarifying the Scope of Descriptions and Defining Accountability](05-common-sbom-document-quality-issues-and-improvement-measures.md#5.6-clarifying-the-scope-of-descriptions-and-defining-accountability)”.

Mandatory information required by Specifications and Guidelines  
When the SBOM document is used for legal or regulatory compliance purposes, the following requirements MUST be strictly observed.

* Other mandatory Elements as specified in each Data Format  
  Include all Elements that are mandatory for each Data Format. If the appropriate value for any element is unknown, fill it in with a placeholder such as  'NOASSERTION' in SPDX document, indicating a 'known unknown’.  
  Since each Entity typically processes the SBOM Document using different tools, including these required Elements enhances interoperability and ultimately improves the quality of SBOM Documents across the entire supply chain.  
  Refer to [“6. Compliance Elements for SPDX and CycloneDX in Relation to Legislation and Guidelines”](06-compliance-elements-for-spdx-and-cyclonedx-in-relation-to-legislation-and-guidelines.md#6.-compliance-elements-for-spdx-and-cyclonedx-in-relation-to-legislation-and-guidelines) for more details.

* Other mandatory Elements as specified by the applicable specifications and guidelines that each Entity conforms  
  Include all Elements that are mandatory for each specification and guideline to which each Entity conforms. If the appropriate value for any element is unknown, fill it in with a placeholder such as  'NOASSERTION' in SPDX document, indicating a 'known unknown'.  
  When a unified specification across the entire supply chain is not mandated, it is important to clearly indicate which specification the SBOM Document conforms to. This improves the document’s quality by explicitly defining the compliance requirements.

### 3.2.1 Verification and reference material

The Minimum Elements For a Software Bill of Materials(SBOM) : [https://www.ntia.gov/report/2021/minimum-elements-software-bill-materials-sbom](https://www.ntia.gov/report/2021/minimum-elements-software-bill-materials-sbom)    
2025 Minimum Elements for a Software Bill of Materials (SBOM): [https://www.cisa.gov/resources-tools/resources/2025-minimum-elements-software-bill-materials-sbom](https://www.cisa.gov/resources-tools/resources/2025-minimum-elements-software-bill-materials-sbom)   
SPDX®: [https://spdx.dev/use/specifications/](https://spdx.dev/use/specifications/)   
CycloneDX: [https://cyclonedx.org/specification/overview/](https://cyclonedx.org/specification/overview/)   
SWHID: [https://www.swhid.org/](https://www.swhid.org/)   
PURL: [https://github.com/package-url/purl-spec](https://github.com/package-url/purl-spec) 

### 3.2.2 Rationale

Clearly defining across the entire supply chain which elements, such as SBOM Document Creation information and Package information details, must be included and distributed helps prevent gaps in the distributed data. Furthermore, clarifying the expected values for each element reduces ambiguous language and inter-tool variations.

## 3.3 File Format

An SBOM document SHALL be in a machine-processable format, such as those supported by SCA tools. In addition, to facilitate manual review (e.g., for license verification), the document SHOULD be provided in, or be easily convertible to, a human-readable format.

### 3.3.1 Verification and reference material

XML 1.0: [https://www.w3.org/TR/xml/](https://www.w3.org/TR/xml/)  
JSON-LD 1.1: [https://www.w3.org/TR/json-ld11/](https://www.w3.org/TR/json-ld11/)   
ECMA-404: The JSON data interchange syntax: [https://ecma-international.org/publications-and-standards/standards/ecma-404/](https://ecma-international.org/publications-and-standards/standards/ecma-404/)   
YAML™: [https://yaml.org/spec/](https://yaml.org/spec/) 

For other formats, please refer to the respective specifications.

### 3.3.2 Rationale

As noted in NTIA SBOM Minimum Elements, when SBOM Documents are exchanged across the supply chain, they must be provided in a machine-processable standard format to ensure interoperability among the various tools used by different entities. At the same time, since SBOM Documents may also be reviewed manually (for example, checking license information), it is equally important that the document is available in, or can be easily converted to, a human-readable format.

## 3.4 Timing of SBOM Document delivery

The SBOM Document SHALL be available no later than at the time of the delivery of the software (in either binary or source form).

### 3.4.1 Verification and reference material

The Minimum Elements For a Software Bill of Materials(SBOM), “Distribution and Delivery”: [https://www.ntia.gov/report/2021/minimum-elements-software-bill-materials-sbom](https://www.ntia.gov/report/2021/minimum-elements-software-bill-materials-sbom) 

### 3.4.2 Rationale

An SBOM Document SHOULD be available no later than at the delivery of the software to ensure that the receiving entity can ingest the software and its SBOM.

## 3.5 SBOM Document Scope

The SBOM SHALL contain all open source software that is delivered with the product. The SBOM SHOULD contain all commercial components.  
If some components are either not included or ambiguous, they SHALL be reported as “Known Unknowns”. When reporting “Known Unknowns”, the SBOM Document SHOULD distinguish between two categories:

1. Genuinely Unknown: Information that is not available at the time of SBOM creation (e.g., a transitive dependency whose details cannot be determined).  
2. Intentionally Withheld: Information that exists but is deliberately omitted (e.g., proprietary component details subject to confidentiality agreements). 

Clearly labeling the reason for omission helps downstream recipients assess risk accurately and take appropriate action.  
Refer to the “[5.6 Clarifying the Scope of Descriptions and Defining Accountability](05-common-sbom-document-quality-issues-and-improvement-measures.md#5.6-clarifying-the-scope-of-descriptions-and-defining-accountability)” for more details.

### 3.5.1 Verification and reference material

The Minimum Elements For a Software Bill of Materials(SBOM), “Known Unknowns”: [https://www.ntia.gov/report/2021/minimum-elements-software-bill-materials-sbom](https://www.ntia.gov/report/2021/minimum-elements-software-bill-materials-sbom)

### 3.5.2 Rationale

It might not be possible, advisable or feasible to have the commercial component information in the SBOM Document. However, the SBOM Document should convey as complete information as possible.

## 3.6 SBOM Document Verification

A digital signature of the SBOM Document SHOULD be provided in order to guarantee its integrity.

### 3.6.1 Verification and reference material

Sigstore: [https://www.sigstore.dev/](https://www.sigstore.dev/)

### 3.6.2 Rationale

While the need to attach a digital signature to the SBOM Document for ensuring its integrity is currently under discussion, few practical implementations exist. Consequently, this topic  will be revisited in the future, taking into account the associated costs and practical considerations.

## 3.7 SBOM Document Confidentiality

Contents of the SBOM Document may be subject to confidentiality agreements. Appropriate access controls SHOULD be implemented as necessary. However, confidentiality agreements and access controls SHALL NOT prevent a recipient from redistributing the SBOM Document along with the applicable software components. Use of the Traffic Light Protocol (TLP) as implemented by CycloneDX 1.7 is a sensible approach for managing this need.

### 3.7.1 Verification and reference material

The Minimum Elements For a Software Bill of Materials(SBOM), “Access Control”: [https://www.ntia.gov/report/2021/minimum-elements-software-bill-materials-sbom](https://www.ntia.gov/report/2021/minimum-elements-software-bill-materials-sbom)

### 3.7.2 Rationale

Information related to proprietary software may be subject to confidentiality agreements and thus cannot always be public. In such cases, it is acceptable either to restrict access solely to authorized entities within the supply chain or to substitute the information with "Known Unknowns."  
However, when these components are distributed through complex supply chains, confidentiality agreements and access controls SHALL NOT restrict SBOM Document redistribution, thereby preventing the loss of critical information during the distribution.
