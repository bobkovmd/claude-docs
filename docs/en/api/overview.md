API reference
/
Using the API
API overview
Copy page


The Claude API is a RESTful API at https://api.anthropic.com that provides programmatic access to Claude models and Claude Managed Agents.



New to Claude? For direct model access, start with Get started and Working with Messages. For managed agent infrastructure, see the Claude Managed Agents quickstart.


Prerequisites

To use the Claude API, you'll need:

A Claude Console account
An API key, or a configured Workload Identity Federation rule

For step-by-step setup instructions, see Get started.


Available APIs

The Claude API includes the following APIs:

General Availability:

Messages API: Send messages to Claude for conversational interactions (POST /v1/messages)
Message Batches API: Process large volumes of Messages requests asynchronously with 50% cost reduction (POST /v1/messages/batches)
Token Counting API: Count tokens in a message before sending to manage costs and rate limits (POST /v1/messages/count_tokens)
Models API: List available Claude models and their details (GET /v1/models)

Beta:

Files API: Upload and manage files for use across multiple API calls (POST /v1/files, GET /v1/files)
Skills API: Create and manage custom agent skills (POST /v1/skills, GET /v1/skills)
Agents API: Define reusable, versioned agent configurations for Claude Managed Agents (POST /v1/agents, GET /v1/agents)
Sessions API: Run stateful agent sessions in managed cloud sandboxes (POST /v1/sessions, GET /v1/sessions/{id}/stream)
Environments API: Configure sandbox templates for agent sessions (POST /v1/environments, GET /v1/environments)

For the complete API reference with all endpoints, parameters, and response schemas, explore the API reference pages listed in the navigation. To access beta features, see Beta headers.


Authentication

For details on both authentication methods and when to use each, see Authentication. All requests to the Claude API must include these headers:

Header	Value	Required
x-api-key	Your API key from Console	One of x-api-key or Authorization
Authorization	Bearer <token>, where <token> is a short-lived access token obtained from POST /v1/oauth/token via Workload Identity Federation	One of x-api-key or Authorization
anthropic-version	API version (for example, 2023-06-01)	Yes
content-type	application/json	Yes

If you are using the Client SDKs, the SDK will send these headers automatically. For API versioning details, see API versions.

When accessing Claude through a cloud platform, authentication is integrated with the cloud provider's IAM system. See the platform-specific documentation for supported credential types, required headers, and authentication options.


Getting API keys

The API is made available through the web Console. You can use the Workbench to try out the API in the browser and then generate API keys in Account Settings. Use workspaces to segment your API keys and control spend by use case.


Client SDKs

Anthropic provides official SDKs that simplify API integration by handling authentication, request formatting, error handling, and more.

Benefits:

Automatic header management (x-api-key, anthropic-version, content-type)
Type-safe request and response handling
Built-in retry logic and error handling
Streaming support
Request timeouts and connection management

For a list of client SDKs, see Client SDKs.


Claude API vs cloud platforms

Claude is available through the direct Claude API and through cloud platforms. Choose based on your infrastructure, feature availability, compliance requirements, and pricing preferences.


Claude API
Direct access to the latest models and features
Anthropic billing and support
Best for: New integrations, full feature access, direct relationship with Anthropic

Cloud platform APIs

Access Claude through AWS, Google Cloud, or Microsoft Azure:

Integrated with cloud provider billing and IAM
Feature availability varies by platform: Anthropic-operated platforms include Claude Platform on AWS and Microsoft Foundry; partner-operated platforms include Amazon Bedrock and Google Cloud. See each platform's page for feature availability and timing.
Best for: Existing cloud commitments, specific compliance requirements, consolidated cloud billing
Platform	Provider	Documentation
Claude Platform on AWS	AWS (Anthropic-operated)	Claude Platform on AWS
Amazon Bedrock	AWS	Claude in Amazon Bedrock
Agent Platform	Google Cloud	Claude on Google Cloud
Microsoft Foundry	Microsoft Azure (Anthropic-operated)	Claude in Microsoft Foundry


Claude Managed Agents is available through the direct Claude API and Claude Platform on AWS. For feature availability across platforms, see the Features overview.


Request and response format

Request size limits
Endpoint	Maximum request size
Messages, Token Counting	32 MB
Message Batches API	256 MB
Files API	500 MB
Sessions, Agents, Environments	32 MB

If you exceed these limits, you'll receive a 413 request_too_large error.



Partner-operated platforms have their own request size limits: Google Cloud limits requests to 30 MB, and Bedrock limits requests to 20 MB. Claude Platform on AWS uses the same limits as the direct Claude API. Consult your platform's documentation for current values.


Response headers

The Claude API includes the following headers in every response:

request-id: A globally unique identifier for the request
anthropic-organization-id: The organization ID associated with the API key used in the request


Claude Platform on AWS adds an AWS request ID (x-amzn-requestid) alongside the standard request-id header. See Request IDs for the dual-ID handling pattern.


Rate limits and availability

Rate limits

The API enforces rate limits and spend limits to prevent misuse and manage capacity. Limits are organized into usage tiers; your organization is placed on a tier automatically and can move to a higher tier over time. Each tier has:

Spend limits: Maximum monthly cost for API usage
Rate limits: Maximum number of requests per minute (RPM) and tokens per minute (TPM)

You can view your organization's current limits in the Console. For higher limits, use Request rate limit increase on the Limits page.

For detailed information about limits, tiers, and the token bucket algorithm used for rate limiting, see Rate limits.


Availability

The Claude API is available in many countries and regions worldwide. Check the supported regions page to confirm availability in your location.


Next steps

Messages API reference

Complete API specification for direct model interactions

Claude Managed Agents reference

Agents, Sessions, and Environments endpoints


Client SDKs

Python, TypeScript, C#, Go, Java, PHP, and Ruby

Rate limits

Usage tiers, requesting higher limits, and the token bucket algorithm

Was this page helpful?


