# Agent Identity Protocol (AIP)

An open standard for AI agent identity, capabilities, and trust.

AIP answers "Who is this agent, what can it do, and should I trust it?" with cryptographic identity, structured capabilities, and multi-factor trust scoring.

## Quick Start

```bash
# Create an agent identity
npx opena2a-cli identity create --name my-agent

# Verify an agent
curl "https://aim.opena2a.org/api/v1/did/did:aip:aim_7f3a9c2e"

# Discover an identity provider
curl https://aim.opena2a.org/.well-known/aip
```

## Specification

[AIP-SPEC.md](AIP-SPEC.md) — the full protocol specification.

## Conformance Levels

| Level | Name | What It Means |
|-------|------|---------------|
| 1 | Local Identity | SDK/CLI keypair. No server needed. |
| 2 | Managed Identity | + verification, trust scoring, audit. |
| 3 | Federated Identity | + DIDs, verifiable credentials, cross-platform. |

## Interoperability

AIP is designed to complement:
- [Google A2A Protocol](https://github.com/google/A2A) — AIP identity in agent cards
- [Anthropic MCP](https://modelcontextprotocol.io) — capability-based tool access control
- [OpenID Connect](https://openid.net/connect/) — JWT tokens for machine-to-machine auth
- [WebAuthn/FIDO2](https://www.w3.org/TR/webauthn-3/) — hardware key storage for browsers
- [W3C Verifiable Credentials](https://www.w3.org/TR/vc-data-model-2.0/) — trust scores as VCs
- [ATP (Agent Trust Protocol)](https://github.com/opena2a-org/agent-trust-protocol) — ecosystem trust

## How AIP and ATP Work Together

```
AIP (identity + behavior)     ATP (code + provenance)
  "Who is this agent?"           "Is this agent's code safe?"
         ↓                              ↓
              Combined Trust Decision
              "Is this agent safe to use?"
```

## Reference Implementation

The [OpenA2A AIM Platform](https://github.com/opena2a-org/agent-identity-management) implements AIP at Level 2.

## Related Standards

- [ATP (Agent Trust Protocol)](https://github.com/opena2a-org/agent-trust-protocol) — ecosystem trust
- [OASB (Open Agent Security Benchmark)](https://github.com/opena2a-org/open-agent-security-benchmark) — security controls
- [CAAT (Content-Addressed Adaptive Trust)](https://github.com/opena2a-org/caat-framework) — scan infrastructure

## License

Apache-2.0
