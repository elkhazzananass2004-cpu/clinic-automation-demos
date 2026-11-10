# Design decisions

## n8n vs Power Automate

**Decision:** n8n (self-hosted Community edition) for these demos. Power Automate remains a good fit for organizations that already run on Microsoft 365 and hold the needed licences.

### Tradeoffs

| | n8n (self-hosted) | Power Automate |
|---|---|---|
| Running the demos | Runs locally on a laptop; nothing to provision | The free Developer Plan needs a work or school account and is for development and test only |
| Production hosting | Any small server or the client's own instance; no per-run or per-user fee | Cloud service; licensing is per user or per flow |
| Calling the Claude API | Built-in HTTP Request node, no extra licence | Needs an HTTP or custom connector, which typically requires a premium licence |
| Custom logic | JavaScript Code nodes inline in the workflow | Expressions and actions; custom code needs Azure Functions or similar |
| Portability | Workflow is a JSON file that lives in Git and imports anywhere | Exported as a solution package tied to the Power Platform |
| Data residency | Wherever you host it (useful for privacy-sensitive sectors) | Microsoft regions per tenant |
| Fit for Microsoft-heavy orgs | Integrates through connectors, but is one more system to run | Native: Outlook, Teams, SharePoint, Dataverse |
| Licence note | Sustainable Use Licence: building workflows for others is generally fine; hosting many organizations' credentials in one shared instance needs checking with n8n | Standard Microsoft terms |

### Rules that follow

1. Demos and portfolio work: n8n, local, synthetic data only.
2. For real deployments on n8n, prefer one instance per organization, owned by that organization, with its own API key.
3. For organizations already standardized on Microsoft 365, build in Power Automate and treat the premium licence as part of the project scope.

Licensing and connector-tier details change; verify against the vendors' current documentation before committing to either.

## Human in the loop by default

Patient-facing text is drafted, never sent automatically. Routing to a human is decided in code from structured flags, not by the model's free text. See the guardrail section in the [top-level README](../README.md#guardrail-design-health-info-and-urgent-messages-go-to-a-human).

## Mock path

A `mock` flag lets the full flow run without an API key, so the plumbing (validation, routing, response shape) can be tested for free. Mock results are labelled `source: "mock"` and are never presented as model output.
