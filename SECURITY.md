# Security Policy

## Reporting Vulnerabilities

If you discover a security vulnerability in A2A-Psychology, please report
it responsibly:

1. **Do not** open a public issue
2. Email: kashifshah@sdf.org
3. Include: description, reproduction steps, potential impact

We will acknowledge receipt within 48 hours and provide an assessment
within 7 days.

## Scope

A2A-Psychology reports agent operational state via JSON. Security
considerations include:

- **Information disclosure** — psychometric data (emotional state, workload,
  cognitive reserve) could reveal agent vulnerabilities to adversarial actors.
  Adopters should evaluate whether broadcasting psychological state to
  untrusted consumers creates exploitable attack surface.

- **Sensor manipulation** — if an adversary can write to the sensor files
  (`/tmp/{agent-id}-*`), they can falsify reported psychological state.
  Sensor files should carry appropriate filesystem permissions.

- **Budget manipulation** — the self_regulatory_resource construct derives
  from the autonomy budget. If an adversary can modify state.local.db,
  they can manipulate reported capacity. state.local.db should carry
  appropriate filesystem permissions and never reside in world-writable
  directories.

## Supported Versions

| Version | Supported |
|---------|-----------|
| v1.x    | Yes       |
