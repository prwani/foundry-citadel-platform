# 🤖 Agent Types Mapping

### How each agent type connects to Citadel's four control layers

[⬅️ Back to Overview](./README.md)

The Citadel layers apply across agent platforms, but the integration path is not identical for every runtime. Some capabilities are native, some need configuration, and external agents may need federation. **An agent's platform does not determine its coverage by itself: identity, traffic routing, and telemetry determine which controls can act.**

## 📑 At a glance

| Agent type | Typical hosting | Integration pattern |
|------------|-----------------|---------------------|
| **Copilot Studio** | Microsoft-managed SaaS | Native Microsoft 365 integration; opt in and configure connections and telemetry |
| **Microsoft Foundry** | Foundry Agent Service | Native integration across runtime, identity, operations, and security |
| **Your platform (BYO)** | AKS, Azure Container Apps, or another managed/self-hosted runtime | Configure gateway access, workload identity, and OpenTelemetry; register with Agent 365 |
| **Third-party or on-premises** | Partner platform, another cloud, or local infrastructure | Federate identity and inventory; configure gateway access and supported telemetry |

## 🧭 Mapping by control layer

| Control layer | Copilot Studio | Microsoft Foundry | Your platform (BYO) | Third-party or on-premises |
|---------------|----------------|-------------------|----------------------|----------------------------|
| **🔷 L1 · Governance Hub**<br>Govern every call | Configure connections to Citadel-governed models, tools, or agents through the gateway and their access contracts. Copilot traffic that does not use the gateway is not governed there. | Configure Foundry agents to use governed model connections (such as Admin-Connected-Models); route calls to models and tools through the gateway where gateway policy is required. | Route model, tool, and agent calls through the AI Gateway; register and publish approved assets in API Center and apply access contracts. | Connect supported endpoints through the gateway and define access contracts. Only calls routed through it receive gateway enforcement and telemetry. |
| **🔶 L2 · Agent Operations**<br>Build, run, observe | Use Copilot Studio's managed authoring and runtime. Configure supported OpenTelemetry GenAI telemetry to Foundry-connected Application Insights for shared visibility; Foundry runtime guardrails do not automatically apply. | Native Foundry runtime and Control Plane capabilities include tracing, evaluations, guardrails, identity, grounding, tools, and fleet operations. | Keep the runtime and framework; instrument with OpenTelemetry GenAI semantic conventions and export to Foundry-connected Application Insights. Runtime-specific guardrails remain the platform owner's responsibility. | Keep the partner runtime; configure supported OpenTelemetry GenAI telemetry to the shared observability plane. Runtime-specific guardrails remain with the runtime or gateway controls on routed calls. |
| **🟢 L3 · Agent Management**<br>Identify and own | Native Agent 365 integration is opt-in. Agents in development share an environment identity/blueprint; configure ownership and lifecycle before publishing. | Native Entra Agent ID integration. Development agents share a project identity; a published agent receives its own identity and blueprint and registers with Agent 365. | Use workload identity and the Agent 365 SDK to register each agent, its owner, and lifecycle. | Federate through the Agent 365 SDK or an existing partner integration; map the external identity and provide ownership and lifecycle metadata. |
| **🛡️ L4 · Security Foundation**<br>Protect and detect | Use Microsoft 365's native protections with Entra, Defender, and Purview configured. Confirm the applicable tenant settings and data sources. | Native Entra identity and Foundry security signals connect to Defender and Purview capabilities. Configure the tenant-level security baseline. | Connect agent identity and cloud/runtime signals to Entra and Defender; apply Purview data controls to the data sources the agent can reach. Coverage depends on the configured services and signals. | Federate identity and connect available runtime, cloud, and data signals to the security stack. Coverage depends on the partner integration and available telemetry; Microsoft controls do not automatically enforce inside an external runtime. |

## 🔍 What native, configured, and federated mean

- **Native** means the platform has a direct Citadel or Microsoft integration. Tenant settings, onboarding, and ownership still need to be configured; native does not mean every control is enabled automatically.
- **Configured** means the agent stays on its current runtime, while its traffic, identity, or telemetry is explicitly connected to the relevant control plane.
- **Federated** means an external platform or agent identity is connected to enterprise identity, inventory, and management through the Agent 365 SDK or a supported partner integration.

These are integration patterns, not mutually exclusive deployment choices. For example, a Foundry-hosted agent can have native Agent Operations and identity while still routing model calls through the Governance Hub.

## 🧩 Coverage follows the path

1. **Runtime controls act where the agent runs.** Foundry Agent Service guardrails are native to Foundry; they do not transfer to a separate runtime.
2. **Gateway controls act on calls that traverse the gateway.** Registering an agent does not route its traffic or make direct calls subject to gateway policy.
3. **Inventory and identity provide accountability, not runtime mediation.** Agent 365 can manage an agent that never calls the gateway, but it does not make that traffic gateway-governed.
4. **Security visibility depends on connected signals and data sources.** Confirm which agent identities, cloud resources, audit events, and grounding data are visible in your tenant before relying on a control.

For agents whose traffic bypasses both Foundry and the gateway, use their native protections together with Agent Management and the Security Foundation. Where calls can be routed through the gateway, that adds an independent enforcement and audit point.

## ✅ Onboarding checklist

- [ ] Identify the runtime, owner, publishing path, and data/tools the agent can access.
- [ ] Register the agent and establish its supported identity path: native Entra Agent ID, workload identity, or federation.
- [ ] Route model, tool, and agent calls through the Governance Hub when central policy, access contracts, or cost attribution are required.
- [ ] Enable native runtime guardrails, or explicitly configure the controls available for a non-Foundry runtime.
- [ ] Configure telemetry for non-Foundry agents and verify traces appear in the intended observability plane.
- [ ] Confirm Defender, Purview, and Entra signals and policies cover the agent's identity, runtime, and data sources.
- [ ] Test ownership, audit attribution, and revocation at both the agent-management and gateway layers where applicable.

## 📚 Layer details

- [Layer 1 · Governance Hub](./layer-1-governance-hub.md)
- [Layer 2 · Agent Operations](./layer-2-agent-operations.md)
- [Layer 3 · Agent Management](./layer-3-agent-management.md)
- [Layer 4 · Security Foundation](./layer-4-security-foundation.md)
- [Technical Activation Paths](./technical-activation-paths.md)
