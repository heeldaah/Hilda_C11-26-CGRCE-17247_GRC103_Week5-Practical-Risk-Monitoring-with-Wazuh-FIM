# Practical Risk Monitoring with Wazuh – File Integrity Monitoring

**Course:** GRC103 – Risk Assessment and Management Techniques  
**Module:** Practical Risk Assessment Workshop  
**Learner:** Hilda Odein Joshua-Jack  
**Registration number:** C11/26/CGRCE/17247

## Project summary

This project addresses the risk of unauthorised modification or deletion of sensitive financial forecast documents. It uses Wazuh File Integrity Monitoring (FIM) as a detective control concept for monitoring a dedicated Windows test directory and centralising file-integrity events for investigation and management reporting.

The laboratory design uses an Ubuntu Server virtual machine for the Wazuh management platform and a Windows endpoint running the Wazuh agent. Dummy financial forecast files are used to examine file creation, modification and deletion activity.

## Business risk

Unauthorised changes to financial forecasts can undermine the integrity of management information. Decisions based on altered figures may result in inaccurate budgets, inappropriate allocation of funds, financial loss, operational disruption, reputational damage and audit findings.

The risk is not addressed by alerting alone. Effective treatment combines preventive measures, detective monitoring, investigation, recovery and management oversight.

## Control objectives

The FIM control is intended to:

- Monitor a designated folder on a Windows endpoint.
- Record file creation, modification and deletion activity.
- Forward endpoint events to a central monitoring platform.
- Support investigation through event timestamps, file paths and available integrity metadata.
- Provide information for risk monitoring, trend analysis and management reporting.

**Control classification:** Detective.

FIM identifies changes; it does not prevent them or determine by itself whether a change was authorised. Alerts must be reviewed against user responsibilities, approved change records and business context.

## Laboratory architecture

| Component | Platform | Function |
|---|---|---|
| Wazuh manager | Ubuntu Server VM | Receives and analyses endpoint events |
| Wazuh indexer | Ubuntu VM | Stores indexed security events |
| Wazuh dashboard | Ubuntu VM | Central event review and monitoring |
| Wazuh agent | Windows endpoint | Monitors the designated test folder |
| Test data | Windows endpoint | Dummy financial forecast documents |

### Network design

The VirtualBox laboratory uses NAT for Ubuntu internet access and a Host-only Adapter for private communication between the Windows host and Ubuntu guest. The configured host-only network is `192.168.56.1/24`. The Ubuntu guest address is obtained from the guest operating system and used as the manager address for agent communication.

### Endpoint monitoring scope

**Designated folder:** `C:\Users\<WindowsUser>\SensitiveFiles`  
**Test document:** `Q4_Financial_Forecast.docx`

Only dummy files should be used. Actual company, customer or confidential records must not be copied into the laboratory.

## Implementation methodology

1. Prepare the Ubuntu virtual machine and verify its network interfaces and internet connectivity.
2. Deploy the Wazuh management components using the official installation documentation.
3. Access the dashboard and verify the health of the manager, indexer and dashboard.
4. Install the Wazuh agent on Windows and register it with the manager.
5. Confirm agent connectivity from the dashboard.
6. Configure File Integrity Monitoring for the designated test directory.
7. Generate controlled file creation, modification and deletion activity.
8. Review the corresponding FIM events and record timestamps, file paths, event types and available checksums.
9. Evaluate the control and prepare management reporting.

## Control verification scenarios

| Test | Controlled action | Expected FIM observation |
|---|---|---|
| FIM-01 | Create `Q4_Financial_Forecast.docx` | File addition event |
| FIM-02 | Edit and save the dummy document | File modification event |
| FIM-03 | Delete the dummy document | File deletion event |

For each test, the action time should be recorded and matched to the corresponding dashboard event. Event processing delays should be considered when comparing timestamps.

## GRC assessment

### Business impact

Unauthorised changes to financial forecasts may cause management to rely on inaccurate information, resulting in poor budgeting, financial loss and inappropriate resource allocation. The organisation may also experience operational delays while staff investigate and reconstruct reliable records. Reputational damage and audit findings are possible where the integrity of financial information cannot be demonstrated.

### Risk quantification

FIM event records can help establish the frequency and nature of file changes over a defined period. Analysts can compare additions, modifications and deletions, identify affected paths and endpoints, examine recurring patterns and compare activity with approved change records. Alert volume is an indicator of observed activity, not a direct count of confirmed unauthorised changes.

### Preventive control

A complementary preventive control is role-based access control based on least privilege. Only employees with a business need should receive access to the financial forecast folder. Edit and delete permissions should be restricted, supported by access approvals, strong authentication and periodic access reviews. Document approval workflows and version control provide additional safeguards.

### Corrective control

An unexpected modification or deletion alert should initiate investigation. The analyst should validate the endpoint, file path, timestamp, account and available event details, then compare the activity with approved change records. Where the activity is unauthorised, the organisation should restrict relevant access, preserve evidence, notify the file owner and incident-response team, and restore a verified version from backup or version history where appropriate.

### Monitoring metrics

1. **File-change event volume:** Number of additions, modifications and deletions during the reporting period, separated by event type and endpoint.
2. **Investigation and response time:** Time from alert generation to triage and closure, including performance against agreed response targets.
3. **Monitoring coverage and health:** Proportion of in-scope endpoints and paths covered, agent availability and periods of interrupted event collection.

These metrics should be interpreted together. A low alert count does not automatically demonstrate effective monitoring.

### Centralised monitoring and auditability

Centralised monitoring provides authorised analysts with a consolidated view of endpoint file-integrity events. It supports consistent investigation, trend analysis, escalation and management reporting while reducing reliance on manual log checks on individual computers. Auditability depends on controlled access, reliable timestamps, suitable retention, protected logs and documented monitoring coverage.

### Compliance evidence

Wazuh can provide technical evidence of observed file changes, including event timestamps, file paths, event types and integrity metadata where available. An audit evidence package should also include the monitoring configuration, asset scope, agent-health records, access approvals, change records and investigation outcomes.

Wazuh supports compliance activities but does not, by itself, establish compliance with SOX, PCI DSS, HIPAA or another framework. The control must be mapped to the specific applicable requirement and assessed for design and operating effectiveness.

To demonstrate that no change was recorded for a particular file during a defined period, an auditor review should identify the endpoint and exact path, verify FIM coverage for the full period, confirm agent health and event collection, query the dashboard for the date range, review matching events and identify monitoring gaps. Baseline and current hashes, backups, version history and approved change records should be used as corroborating evidence.

The conclusion should be qualified as: **“No changes were recorded by the configured monitoring during the verified period.”** An empty alert search is not proof that no change occurred unless coverage, agent health, configuration and retention have also been verified.

## Repository contents

- `README.md` – project overview, architecture, methodology and GRC assessment.
- `GRC103-Wazuh-FIM-GRC-Analysis-DRAFT.docx` – editable GRC analysis report.
- `docs/FIM-Test-Log.md` – structured test log for action and alert timestamps.
- `docs/Evidence-Register.md` – evidence checklist for the practical.
- `docs/Implementation-Status.md` – laboratory preparation record and technical follow-up items.
- `docs/GRC-Analysis.md` – GRC analysis in Markdown format.
- `evidence/` – location for genuine screenshots and exported event records.

## Technical references

- Wazuh documentation: https://documentation.wazuh.com/
- Wazuh quickstart: https://documentation.wazuh.com/current/quickstart.html
- Wazuh File Integrity Monitoring: https://documentation.wazuh.com/current/user-manual/capabilities/file-integrity/index.html
- Wazuh agent deployment: https://documentation.wazuh.com/current/installation-guide/wazuh-agent/index.html
