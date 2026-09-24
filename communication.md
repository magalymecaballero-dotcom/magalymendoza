1.	Reply to Dana (≤200 words), to be sent now.

Answer
Hi Dana,

Thank you for bringing this to our attention.

We understand the urgency, particularly with the QMS window opening on Wednesday, and we're currently investigating both issues as a priority.

Regarding the stale positions, we have confirmed receipt of your Friday data file and are reviewing the processing and valuation update workflow to determine why the latest NAVs are not being reflected in Liquidity Hub.

For the sign-in issue affecting Kevin Brennan and Julia Lam, we are investigating the email verification code delivery process to identify why the authentication emails are not reaching these users. At this time, the issue appears to be limited to the individuals you identified, but we are validating whether there is any broader impact.

We also want to reassure you that there is currently no indication of a data integrity or data security issue. Our investigation is focused on data processing and authentication services, and we are assessing whether any other clients may be experiencing similar symptoms.

We will provide an update as soon as we have additional findings and a clearer remediation plan. If needed, we are available to join a call today before 17:00 to discuss the situation and answer any questions.

Thank you for your patience while we work through this.

Kind regards,

Magaly Mendoza

2.	Slack reply to Sam (≤120 words), before his 14:00 call with Meridian's COO.

"We've reviewed the information currently available to us and are still validating the root cause. At this stage we don't have evidence that Meridian's file is incorrect, and we also don't yet have enough evidence to attribute the issue to our platform. Our team is actively reviewing the transaction logs and file processing details, and we'll share our findings as soon as the analysis is complete."

3.	Hand-off ticket for Ravi, the on-call engineer: title, impact, evidence, the exact changes you need in order, how to verify each one, and how to roll back.

Service: ingest-worker & lh-prod-albEnvironment: Production (lh-prod)Current Status: Ingestion pipeline is functionally degraded (Meridian client is completely blocked). A transient high-severity web alert was triggered and mitigated on Saturday.
Behavior: The Application Load Balancer crossed the error threshold (>25 errors/min), peaking and then dropping back to 9.0 errors at 02:24:00 UTC before recovering to an OK state at 02:25:31 UTC.
///Hotfix might apply///


4.	Update plan: who hears what, and when, until this is resolved (5 bullets max).

Required Action Items (Next Steps)Deploy Hotfix Task Definition (lh-prod-ingest-worker:89): Revert the environment or configuration variable mapping the file validation filter. Restore support for both compressed formats and the client prefix pattern: ^(meridian_)?positions_\\d{8}\\.csv(\\.gz)?$.Force-Reprocess Meridian's Skipped Data: Once revision 89 is active, manually trigger a manual pipeline run or temporarily clear S3 metadata tracking to parse meridian_positions_20261002.csv.gz.Audit Log Snippets for 5xx Burst: Use CloudWatch Logs Insights on the ALB network logs access trail for 2026-10-03T02:20:00Z to pinpoint the specific microservice or container target group throwing gateway faults.
