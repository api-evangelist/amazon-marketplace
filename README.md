# Amazon Marketplace (amazon-marketplace)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

AWS Marketplace is a curated digital catalog that makes it easy to find, buy, deploy, and manage third-party software, data, and services that run on AWS. It offers thousands of software listings from independent software vendors. The Marketplace Catalog API enables programmatic management of marketplace entities including products, offers, and data products through change sets and entity description operations.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/amazon-marketplace/refs/heads/main/apis.yml)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Tags:

 - AWS, Commerce, ISV, Marketplace, Software Catalog

## Timestamps

- **Created:** 2026-03-16
- **Modified:** 2026-04-19

## APIs

### AWS Marketplace Catalog API
The AWS Marketplace Catalog API provides programmatic access to manage entities and change sets for publishing and updating software products, data products, and machine learning products on AWS Marketplace. Covers 13 operations for entity discovery, change set lifecycle management, resource policies, and resource tagging.

**Human URL:** [https://aws.amazon.com/marketplace/](https://aws.amazon.com/marketplace/)

#### Tags:

 - Commerce, ISV, Marketplace, Software Catalog

#### Properties

- [Documentation](https://docs.aws.amazon.com/marketplace-catalog/latest/api-reference/welcome.html)
- [OpenAPI](openapi/amazon-marketplace-openapi-original.yaml)
- [GettingStarted](https://aws.amazon.com/marketplace/management/portal/)
- [Pricing](https://aws.amazon.com/marketplace/pricing/)
- [FAQ](https://aws.amazon.com/marketplace/help/)

## Common Properties

- [Portal](https://aws.amazon.com/marketplace/)
- [Documentation](https://docs.aws.amazon.com/marketplace/)
- [TermsOfService](https://aws.amazon.com/service-terms/)
- [PrivacyPolicy](https://aws.amazon.com/privacy/)
- [Support](https://aws.amazon.com/premiumsupport/)
- [Blog](https://aws.amazon.com/blogs/awsmarketplace/)
- [GitHubOrganization](https://github.com/aws)
- [Console](https://console.aws.amazon.com/marketplace/)
- [SignUp](https://portal.aws.amazon.com/billing/signup)
- [Login](https://signin.aws.amazon.com/)
- [StatusPage](https://health.aws.amazon.com/health/status)
- [Contact](https://aws.amazon.com/contact-us/)
- [SpectralRules](rules/amazon-marketplace-spectral-rules.yml)
- [Vocabulary](vocabulary/amazon-marketplace-vocabulary.yaml)
- [NaftikoCapability](capabilities/marketplace-catalog-workflow.yaml)

## Features

| Name | Description |
|------|-------------|
| Entity Management | Programmatically list and describe marketplace entities including software products, data products, and offers. |
| Change Set Lifecycle | Start, monitor, and cancel change sets for publishing new listings or updating existing ones. |
| Resource Policies | Attach, retrieve, and remove resource-based policies to control access to marketplace entities. |
| Resource Tagging | Tag marketplace resources with key-value pairs for organization and cost allocation. |
| Multi-Region Support | Access marketplace entities across multiple AWS regions through regional catalog endpoints. |
| Publishing Automation | Integrate catalog API with CI/CD pipelines for automated product publishing and updates. |

## Use Cases

| Name | Description |
|------|-------------|
| Product Publishing Automation | Automate publishing and updating software listings on AWS Marketplace from CI/CD pipelines. |
| Marketplace Catalog Discovery | Programmatically discover and evaluate available software products and data products. |
| Change Set Monitoring | Track the status of publishing operations and receive change set completion notifications. |
| Multi-Account Marketplace Management | Manage marketplace listings across multiple AWS accounts with shared resource policies. |
| ISV Self-Service Publishing | Enable ISV teams to self-service publish and update product listings through the catalog API. |

## Integrations

| Name | Description |
|------|-------------|
| AWS IAM | Control access to catalog API operations through IAM policies and roles. |
| Amazon EventBridge | Subscribe to marketplace events for change set completions and entity state changes. |
| AWS CloudFormation | Deploy and manage marketplace subscriptions as infrastructure-as-code. |
| AWS Organizations | Share private marketplace listings across accounts in an AWS organization. |
| Amazon SNS | Receive notifications for marketplace change set status updates. |

## Artifacts

Machine-readable API specifications organized by format.

### OpenAPI

- [Amazon Marketplace Catalog API OpenAPI](openapi/amazon-marketplace-openapi-original.yaml)

### JSON Schema

80 schema files available in the [json-schema/](json-schema/) directory.

### JSON Structure

80 structure files available in the [json-structure/](json-structure/) directory.

### JSON-LD

- [Amazon Marketplace Context](json-ld/amazon-marketplace-context.jsonld)

### Examples

80 example files available in the [examples/](examples/) directory.

## Capabilities

Naftiko capabilities organized as shared per-API definitions composed into customer-facing workflows.

### Shared Per-API Definitions

- [AWS Marketplace Catalog API](capabilities/shared/marketplace-catalog.yaml) — 11 operations for entity management, change set lifecycle, and resource policies

### Workflow Capabilities

| Workflow | APIs Combined | Tools | Persona |
|----------|--------------|-------|---------|
| [Marketplace Catalog Workflow](capabilities/marketplace-catalog-workflow.yaml) | AWS Marketplace Catalog API | 7 | ISV Seller, Marketplace Operator |

## Vocabulary

- [Amazon Marketplace Vocabulary](vocabulary/amazon-marketplace-vocabulary.yaml) — Unified taxonomy mapping 4 resources, 7 actions, 1 workflow, and 2 personas across operational (OpenAPI) and capability (Naftiko) dimensions

## Rules

- [Amazon Marketplace Spectral Rules](rules/amazon-marketplace-spectral-rules.yml) — Rules enforcing Amazon Marketplace Catalog API conventions

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
