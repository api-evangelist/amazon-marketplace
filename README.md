# Amazon Marketplace (amazon-marketplace)
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
