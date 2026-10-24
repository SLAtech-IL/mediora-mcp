# Mediora.AI — MCP Server

A hosted [Model Context Protocol](https://modelcontextprotocol.io) server that
exposes **Mediora.AI**'s medical catalog to AI agents — blood-test markers, lab
panels, clinical conditions and patient symptoms — as callable tools, in
**English, Russian and Hebrew**.

- **Endpoint:** `https://mcp.mediora.ai/mcp` (JSON-RPC 2.0 over streamable HTTP)
- **Discovery manifest:** `https://mcp.mediora.ai/.well-known/mcp.json`
- **Human landing:** `https://mcp.mediora.ai/mcp/info`
- **Website:** https://www.mediora.ai

> This repository is the public **listing** for a **hosted** server. It contains the
> registry manifest and connection instructions — not the server's source. Clients
> connect to the endpoint above; there is nothing to run locally.

## Connect

Add to your MCP client. For Claude Desktop (`claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "mediora": {
      "type": "http",
      "url": "https://mcp.mediora.ai/mcp"
    }
  }
}
```

No API key is required for the catalog tools. Restart the client and ask, e.g.,
*"What does a high ferritin mean?"* — the answer comes from Mediora's catalog.

## Tools

### Anonymous — catalog reads (no token)

| Tool | Returns |
| --- | --- |
| `mediora__list_markers` | Every blood-test marker (slug + short name) |
| `mediora__explain_marker` | Full explainer for one marker |
| `mediora__lookup_marker` | Fuzzy alias search across EN/RU/HE |
| `mediora__list_panels` | Every lab panel (CBC, lipid, metabolic, thyroid and more) |
| `mediora__explain_panel` | The markers that make up one lab panel |
| `mediora__list_conditions` | Every clinical condition (slug + ICD-10) |
| `mediora__explain_condition` | Full explainer for a clinical condition |
| `mediora__list_symptoms` | Every patient-side symptom |
| `mediora__explain_symptom` | Symptom triage explainer with lab work-up |

### Patient tools — require a Bearer JWT

| Tool | Returns |
| --- | --- |
| `mediora__whoami` | The authenticated patient's profile summary |
| `mediora__analyze_lab_pdf` | Upload a lab PDF/image for analysis (SSRF-guarded fetch) |
| `mediora__get_patient_history` | The patient's uploaded tests |
| `mediora__get_test_details` | A single test's markers + AI interpretation |
| `mediora__longitudinal_trend` | A marker's trend across the patient's uploads |

## What it is — and isn't

- **Interpretation, not a diagnosis.** Mediora explains lab results; it never
  produces a diagnosis or replaces a clinician.
- **No PHI in the read surface.** The anonymous catalog tools carry no patient data.
  Patient tools require the patient's own JWT and return only that patient's data.
- **AI-generated, not physician-reviewed.** Catalog content is written with AI and
  has not been reviewed by a physician. Reference ranges vary between laboratories; the range printed on the
  patient's own lab report applies.

## Languages

English · Russian · Hebrew. Russian and Hebrew content is AI-translated. Pass a
`lang` argument (`en` | `ru` | `he`) to any catalog tool.

## License

This repository (manifest + docs) is released under the [MIT License](LICENSE).
The catalog **content** served by the tools is licensed **CC-BY-NC 4.0** — free to
use with attribution to **Mediora.AI** (link to https://www.mediora.ai), non-commercial
unless separately licensed.
