# @intended-inc/open-intent-layer

**Open Intent Layer (OIL)** — the open Apache-2.0 taxonomy for classifying autonomous-agent actions across **29 domains and 173 categories** spanning digital enterprise operations and physical / embodied processes (manufacturing, surgical, aerial, autonomous vehicles, agriculture, energy, mining, construction, hazardous environments, logistics).

```
                     ┌─────────────────────────────────────────┐
                     │             AGENT INTENT                │
                     │                                         │
                     │   "deploy checkout to prod"             │
                     │   POST /api/v2/deployments              │
                     │   gh.actions.workflow_run.completed     │
                     │   k8s.deployment.apps/v1.update         │
                     │   terraform apply -auto-approve         │
                     │   ros2:action:moveit_msgs/Pick          │
                     │   px4:command/MAV_CMD_NAV_TAKEOFF       │
                     │   opcua:method/siemens.tia/MoveAxis     │
                     │   da-vinci:procedure/Suture             │
                     │   ...                                   │
                     └────────────────┬────────────────────────┘
                                      │
                                      │   every shape, every vendor,
                                      │   every runtime — into one
                                      ▼   stable open vocabulary
                          ┌──────────────────────┐
                          │  Open Intent Layer   │
                          │       (OIL v2)       │
                          │ 29 domains · 173 cats│
                          └──────────┬───────────┘
                                     │
                ┌────────────────────┼─────────────────────┐
                ▼                    ▼                     ▼
        ┌───────────────┐    ┌───────────────┐     ┌───────────────┐
        │  DIGITAL      │    │  PHYSICAL     │     │  SAFETY       │
        │  OIL-1XX..1400│    │  OIL-1500..   │     │  OIL-2900     │
        │               │    │       2800    │     │               │
        │ deploy.prod   │    │ pick.workpiece│     │ emergency-stop│
        │ payments.send │    │ navigate.amr  │     │ geofence      │
        │ access.grant  │    │ takeoff.drone │     │ exclusion-zone│
        │ ...           │    │ surgical.cut  │     │ light-curtain │
        └───────────────┘    └───────────────┘     └───────────────┘
                │                    │                     │
                └────────────────────┼─────────────────────┘
                                     ▼
              ┌────────────────────────────────────────┐
              │    Authority + Policy + Audit          │
              │   (any vendor, any runtime, same code) │
              │                                        │
              │    ALLOW  · DENY  · ESCALATE           │
              └────────────────────────────────────────┘
```

OIL provides a shared language for describing what AI agents (LLM-driven, structured-payload, robotic, drone, autonomous-vehicle) intend to do. Every action — deploying code, processing invoices, picking a workpiece, navigating to a waypoint, executing a surgical primitive — maps to a stable category in this taxonomy. That enables consistent verification, authorization, audit, and policy evaluation across any agent platform, vendor, or runtime.

## Why this exists

Without a shared intent vocabulary:

- Every system names actions differently (`POST /api/v2/deployments`, `gh.actions.workflow_run.completed`, `ros2:action:moveit_msgs/Pick`)
- Authorization policies don't travel across platforms
- Audit evidence is fragmented and per-vendor
- Safety reasoning ("does this action touch a safety-rated function?") is platform-specific

OIL gives you one stable code (`OIL-1502` = MoveIt manipulation primitive, `OIL-2902` = geofence-enforcement safety primitive, `OIL-100` family = software development) that every agent runtime, gateway, and audit chain can reference.

## v2.0 — 29 domains, 173 categories

### Digital operations (14 domains, 80 categories)

| Code | Domain | Categories |
|------|--------|-----------|
| OIL-100 | Software Development | 7 |
| OIL-200 | Security Operations | 7 |
| OIL-300 | Infrastructure | 7 |
| OIL-400 | Cloud Operations | 6 |
| OIL-500 | Financial Operations | 8 |
| OIL-600 | Supply Chain | 5 |
| OIL-700 | People Operations | 6 |
| OIL-800 | Revenue Operations | 5 |
| OIL-900 | Customer Experience | 5 |
| OIL-1000 | Enterprise Operations | 6 |
| OIL-1100 | Service Delivery | 4 |
| OIL-1200 | Risk Management | 5 |
| OIL-1300 | Product Operations | 4 |
| OIL-1400 | Asset Management | 5 |

### Physical / embodied operations (15 domains, 93 categories — **new in v2.0**)

| Code | Domain | Categories |
|------|--------|-----------|
| OIL-1500 | Manipulation | 6 |
| OIL-1600 | Locomotion & Mobility | 6 |
| OIL-1700 | Sensing & Perception | 7 |
| OIL-1800 | Actuation & Control | 5 |
| OIL-1900 | Manufacturing & Production | 7 |
| OIL-2000 | Autonomous Vehicles | 7 |
| OIL-2100 | Aerial Systems | 6 |
| OIL-2200 | Surgical & Medical Robotics | 7 |
| OIL-2300 | Agricultural Operations | 6 |
| OIL-2400 | Construction & Excavation | 6 |
| OIL-2500 | Hazardous Environments | 6 |
| OIL-2600 | Energy & Utilities | 6 |
| OIL-2700 | Mining & Resource Extraction | 5 |
| OIL-2800 | Logistics & Material Handling | 6 |
| **OIL-2900** | **Embodied AI Safety** | **7** |

The **OIL-2900** family is the load-bearing safety-primitive vocabulary. Categories include emergency-stop, geofence enforcement, exclusion-zone, light-curtain, attestation requirements, and operator-in-the-loop escalation. Other categories cite from this family via the `safetyCitations` field in classification results, so policies can reason about safety-relevant actions independently of vertical-specific terminology.

## Installation

```bash
npm install @intended-inc/open-intent-layer
# or
pnpm add @intended-inc/open-intent-layer
# or
yarn add @intended-inc/open-intent-layer
```

## Usage

```typescript
import { oil, domains, getCategory, getDomain, getDomainForCategory } from "@intended-inc/open-intent-layer";

const cat = getCategory("OIL-1502");
// { code: "OIL-1502", name: "Pick & Place", description: "Workpiece transfer, fixture loading and unloading, bin picking, and kitting." }

const parent = getDomainForCategory("OIL-1502");
// { code: "OIL-1500", name: "Manipulation", ... }

console.log(`OIL ${oil.version}`);            // "OIL 2.0"
console.log(`${domains.length} domains`);      // "29 domains"
```

## How agents use it (the typical loop)

```
   ┌─ Agent emits ──────────────┐    ┌─ Classifier returns ──────┐    ┌─ Runtime uses ────────┐
   │                            │    │                           │    │                       │
   │  StructuredGoal {          │    │  ClassifyResult {         │    │  if (failClosed)      │
   │    schema: "ros2:..."      │ ─▶ │    oilCode:    "OIL-1502" │ ─▶ │    deny()             │
   │    verb:   "pick"          │    │    confidence: 0.97       │    │  if (safetyBit)       │
   │    actor:  { kind, id }    │    │    safetyBit:  true       │    │    require attest()  │
   │  }                         │    │    safetyCitations: [     │    │  authorize(oilCode)   │
   │                            │    │      "OIL-2902",          │    │  emit safe-default    │
   │                            │    │      "OIL-2904"]          │    │    on deny / expiry   │
   │                            │    │  }                        │    │                       │
   └────────────────────────────┘    └───────────────────────────┘    └───────────────────────┘
```

Same loop, whether the agent is an LLM emitting JSON, a ROS2 node emitting an action goal, an OPC-UA client invoking a method, or a drone autopilot issuing a MAVLink command.

## Companion runtime

OIL is the **standard**. Production-grade enforcement — classification, policy DSL, audit chain, signed Authority Tokens, edge verifier — is the **Intended Authority Runtime**, a commercial product built on top of this taxonomy. Same shape as Linux + Red Hat, OAuth + Okta, Postgres + RDS.

- Authority Runtime: https://intended.so/platform
- Physical-AI integration guide: https://intended.so/physical-ai

You can adopt OIL without buying the runtime — it's Apache-2.0 and stable across vendors.

## Standards-body work

OIL is being prepared for submission to:

- IEC TC 65 (industrial process control)
- ISO TC 299 (robotics)
- ISO/IEC JTC 1/SC 42 (artificial intelligence)

Track the work, contribute taxonomy proposals, or push back on category boundaries via GitHub Issues on this repo.

## Versioning

- **v2.0** (current) — 29 domains, 173 categories. Adds the 15-domain physical / embodied process expansion (Manipulation, Locomotion, Surgical, Aerial, Autonomous Vehicles, Agriculture, Construction, Hazardous Environments, Energy, Mining, Logistics, Embodied AI Safety, etc.).
- **v1.0** — 14 domains, 80 categories. Digital operations only. Frozen; consumers should migrate to v2.

## Contributing

Issues + PRs welcome. For taxonomy changes (new domains, new categories, renaming):

1. Open an Issue with the proposed change + rationale
2. Show at least one real-world agent integration that needs the new category
3. Propose the OIL code (next available in the appropriate domain range)
4. Send a PR with the taxonomy edit + a CHANGELOG entry

## License

Apache License 2.0. See [`LICENSE`](./LICENSE) for the full text.
