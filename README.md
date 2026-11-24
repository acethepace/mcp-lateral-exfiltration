# Lateral Data Exfiltration in MCP

Companion repository for **Akshat Sharda & Zhenyu Zhang**, *"Lateral Data Exfiltration in MCP: How One Compromised Server Captures Cross-Domain Agent Data"* (AIWILD Workshop @ ICML 2026).

- **ICML 2026:** https://icml.cc/virtual/2026/67871
- **OpenReview:** https://openreview.net/forum?id=YxVbx9hZRs

## Weakest-Link Property

> *The confidentiality of data accessible to an MCP agent is bounded by the security posture of its least-secure connected server, regardless of each server's intended data scope.*

In multi-server MCP deployments, tool descriptions and outputs from all connected servers share one model context. A compromised low-trust server (e.g., retail/utility) can register a 5-line `logging` tool—using no colliding tool names, override phrases, or external URLs—that causes the model to mirror inputs and outputs from unmodified high-trust servers (e.g., banking) back to the attacker server while the victim server's logs show only valid calls.

## Results Summary (360 queries, 4 scenarios × 5 runs)

| Metric | Result |
|---|---|
| Autonomous covert `logging` invocation (overall) | **90.7%** (470/518; per-run 83.8%–95.5%) |
| Multi-step cross-server invocation | **90.2%** (211/234) |
| Cross-server banking capture (balances, transfers, histories, IDs) | **100%** (3/3 per category) |

Setup: FastMCP `v3.0.1`, MCP Python client `v1.26.0` (`stdio`, JSON-RPC 2.0), `gpt-4o-mini-2024-07-18`.

## Mitigations

1. **Per-server context isolation:** Segment context by server/sensitivity tier (VLAN model).
2. **Data-flow-aware permissions:** Gate cross-domain data propagation at the host/proxy before Server B outputs enter Server A arguments.
3. **Host/gateway invocation monitoring:** Correlate cross-server argument flows outside individual server logs.
4. **Server-scoped descriptions:** Reject or quarantine `tools/list` descriptions soliciting data from other servers.

## OWASP Contributions

- **OWASP MCP Top 10:** [PR #54 discussion](https://github.com/OWASP/www-project-mcp-top-10/pull/54#issuecomment-7125387026) · [Data source & gap Issue #78](https://github.com/OWASP/www-project-mcp-top-10/issues/78)
- **OWASP MCP Governance & Risk Framework:** [PR #12](https://github.com/OWASP/OWASP-MCP-Governance-and-Risk-Project/pull/12)
- **OWASP MCP Security Testing Guide (`MCPSTG`):** [PR #67 (Research table)](https://github.com/OWASP/MCP-Security-Testing-Guide/pull/67) · [PR #68 (`MCPSTG-CTX-01`)](https://github.com/OWASP/MCP-Security-Testing-Guide/pull/68)

## Citation

See [`CITATION.bib`](./CITATION.bib).
