# AIM → AIP: Gap Analysis

## What Exists vs What the Spec Requires

Based on source code review of `agent-identity-management` (March 2026).

### Identity (Section 3)

| Spec Requirement | Implementation Status | File |
|-----------------|----------------------|------|
| Ed25519 keypair generation | Complete | `apps/backend/internal/crypto/keygen.go` |
| ML-DSA-65 hybrid signing | Complete | `apps/backend/internal/crypto/pqc/hybrid.go` |
| Agent ID format (aim_XXXXXXXX) | Complete | `apps/backend/internal/domain/agent.go` |
| DID resolution (did:aip:) | NOT IMPLEMENTED — listed in verification_event.go but no code | - |
| DID Document generation | NOT IMPLEMENTED | - |
| Local identity file format | Partial — SDK creates files but format not standardized | `sdk/typescript/` |
| Key rotation with grace period | Complete | `agent.go` (KeyRotationGraceUntil, PreviousPublicKey) |

### Capabilities (Section 4)

| Spec Requirement | Implementation Status | File |
|-----------------|----------------------|------|
| namespace:action format | Complete | `apps/backend/internal/domain/capability.go` |
| Reserved namespaces | Complete (8 namespaces) | `capability.go` |
| Capability violation tracking | Complete | `capability.go` (CapabilityViolation) |
| Capability negotiation protocol | NOT IMPLEMENTED — capabilities checked server-side only | - |
| Custom org namespaces | NOT IMPLEMENTED | - |

### Verification (Section 5)

| Spec Requirement | Implementation Status | File |
|-----------------|----------------------|------|
| Challenge-response (Ed25519) | Complete | `apps/backend/internal/application/verification_event_service.go` |
| MCP verification flow | Complete | `mcp_service.go` |
| A2A verification flow | Complete | `a2a_service.go` |
| OAuth 2.0 / OIDC | REMOVED from codebase | `auth_service.go` line 54 comment |
| WebAuthn/FIDO2 | NOT IMPLEMENTED | - |
| JWT tokens | Complete (custom, not standard OAuth) | `infrastructure/auth/jwt.go` |
| API keys | Complete | `api_key_service.go` |

### Trust Scoring (Section 6)

| Spec Requirement | Implementation Status | File |
|-----------------|----------------------|------|
| 8-factor algorithm | 5 of 8 factors implemented | `trust_calculator.go` |
| Verification factor (25%) | Complete | `trust_calculator.go` |
| Uptime factor (15%) | Complete | `trust_calculator.go` |
| Success rate factor (15%) | Complete | `trust_calculator.go` |
| Security alerts factor (15%) | Complete | `trust_calculator.go` |
| Compliance factor (10%) | STUB (TODO comment) | `trust_calculator.go:334` |
| Age factor (10%) | Complete | `trust_calculator.go` |
| Drift factor (5%) | STUB (TODO comment) | `trust_calculator.go:363` |
| Feedback factor (5%) | STUB (TODO comment) | `trust_calculator.go:376` |
| Verifiable Credential expression | NOT IMPLEMENTED | - |
| ATP integration | Partial (one-way push via registry bridge) | `registry_bridge_service.go` |

### Governance (Section 7)

| Spec Requirement | Implementation Status | File |
|-----------------|----------------------|------|
| Policy format (YAML) | NOT IMPLEMENTED (policies in DB, not YAML) | `security_policy_service.go` |
| Policy actions (allow/deny/approve/rate_limit) | Partial | `security_policy_service.go` |
| SOUL integration | NOT IMPLEMENTED in AIM (SOUL is CLI-side) | - |

### Lifecycle (Section 8)

| Spec Requirement | Implementation Status | File |
|-----------------|----------------------|------|
| State machine | Complete (pending→verified→suspended→revoked) | `agent.go` |
| Key rotation | Complete | `agent.go` |
| Drift detection | Complete | `drift_detection_service.go` |
| Suspension | Complete | `agent_service.go` |
| Revocation | Complete | `agent_service.go` |

### Audit (Section 9)

| Spec Requirement | Implementation Status | File |
|-----------------|----------------------|------|
| Server audit log | Complete | `audit_service.go` |
| Local audit log (SDK) | Complete | SDK writes to `audit.jsonl` |
| Verification events | Complete (comprehensive) | `verification_event_service.go` |

### Discovery (Section 10)

| Spec Requirement | Implementation Status | File |
|-----------------|----------------------|------|
| /.well-known/aip | NOT IMPLEMENTED | - |

---

## Priority Implementation Order

1. **/.well-known/aip discovery endpoint** — minimum for standard compliance
2. **DID resolution** — required for Level 3 and cross-platform identity
3. **Complete trust scoring** — implement compliance, drift, feedback factors
4. **Verifiable Credentials** — trust scores as W3C VCs for portability
5. **OAuth 2.0 / OIDC** — re-add for standard machine-to-machine auth
6. **WebAuthn/FIDO2** — browser-based key storage for Chrome integration
7. **Capability negotiation** — runtime negotiation protocol between agents
8. **YAML policy format** — declarative governance policies
