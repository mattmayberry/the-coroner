# The Coroner

**A production incident investigator for Claude Code. Reconstructs timelines, measures blast radius, runs the 5-Why chain, and produces a signed post-mortem.**

Free skill for Claude Code, Claude Desktop, and any OpenClaw-compatible agent.

---

## What This Does

Installs a post-mortem methodology on your agent so it can investigate production incidents the way a senior SRE would: systematically, without blame, and without stopping at symptoms.

The Coroner runs six procedures on every incident:

1. **Intake** — Collects what failed, when, how it was detected, and current status. Does not begin analysis until the incident is resolved.
2. **Timeline Reconstruction** — Establishes T=0 (first evidence of failure, not when it was noticed), works backward to the precipitating event, and works forward through the failure cascade.
3. **Blast Radius Assessment** — Counts affected users, data impact, revenue window, downstream failures, and the detection gap.
4. **5-Why Root Cause Chain** — Runs until it hits a human decision or process gap. Technical causes are never the final answer.
5. **Contributing Factors** — Conditions that made the incident worse, faster, or harder to detect. Each factor produces one action item.
6. **Post-Mortem Document** — A complete, signed post-mortem using Template A. Blameless by design.

---

## Install

Add `SKILL.md` to your project's `.claude/` directory, or paste the contents into your Claude system prompt.

```bash
curl -O https://raw.githubusercontent.com/themeridianlab/the-coroner/main/SKILL.md
```

---

## Usage

Once installed, describe a resolved incident and The Coroner takes over:

```
"We had a Worker outage this morning. It's resolved now."

→ Coroner: "What failed, when was it first detected, and how was it discovered?"
→ You provide logs and deployment history
→ Coroner reconstructs timeline, measures blast radius, runs 5-Why chain
→ Produces signed post-mortem document
```

The Coroner will not begin analysis on a live incident. It will tell you to stabilize first.

---

## What You Get

A complete post-mortem document every time:

```
INCIDENT POST-MORTEM
Date: 2025-11-14
Incident Window: 14:22 UTC to 15:48 UTC
Status: Resolved
Severity: P1

SUMMARY
[Three sentences: what broke, how long, what resolved it.]

TIMELINE
14:22 UTC — First 500 errors observed in production logs
14:31 UTC — Error rate reached 100% of requests
14:44 UTC — On-call notified via PagerDuty alert
...

ROOT CAUSE
Proximate cause: Worker threw unhandled exception on every request.
5-Why Chain:
1. Why? Required environment variable was undefined.
2. Why? Variable was not set after wrangler.toml restructuring.
3. Why? Deployment checklist had no step to verify secrets after config changes.
4. Why? Checklist predated environment-specific secret scoping.
5. Why? No process to update the checklist when infrastructure patterns change.
Root cause: Deployment process did not account for secret validation after configuration restructuring.

ACTION ITEMS
| Item | Type | Owner Role | Due |
|------|------|------------|-----|
| Add secrets verification step to deploy checklist | Process | DevOps | 2025-11-21 |
| Set up alerting on Worker error rate > 5% | Monitoring | Platform | 2025-11-21 |

Investigation closed: 2025-11-14
```

---

## Escalation Rules

The Coroner stops and alerts you immediately if:

- The incident involves data loss that has not been acknowledged
- The 5-Why chain reveals a security issue
- The blast radius is significantly larger than you believed
- The timeline reveals a prior unresolved incident that contributed to this one

---

## Limitations

- Works on resolved incidents only
- Analysis is only as complete as the logs and deployment records you provide
- Does not have independent access to production systems
- Blameless by design: identifies decisions and processes, not individuals

---

## License

MIT. Free to use, fork, and extend.

A [Meridian Lab](https://themeridianlab.com) product.
