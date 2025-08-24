# 6) Behavioral & Troubleshooting

**Q137. Tell me about a time you prevented an outage.**

**Answer (Natural):**  
Use STAR: Situation (fragile deploys), Task (reduce risk), Action (added canaries, feature flags, and automated rollbacks), Result (reduced change failure rate from 20%→3%). Emphasize metrics and collaboration.


👉 **Example:** Bring dashboards and postmortems to illustrate impact.


---

**Q138. You broke production—what next?**

**Answer (Natural):**  
Acknowledge, communicate, roll back/mitigate, create a timeline, capture logs, open an incident, and later run a blameless postmortem with action items.


👉 **Example:** Stop the bleeding first, then learn.


---

**Q139. Debug: 500s after a rollout?**

**Answer (Natural):**  
Check error budget burn, compare metrics before/after, inspect diff (image tag/config), roll back if needed. Use canary metrics and logs.


👉 **Example:** `kubectl rollout undo deploy/api` after spike in p99.


---

**Q140. Latency spike only in one AZ?**

**Answer (Natural):**  
Suspect network path, overloaded node, or specific dependency. Drain nodes in that AZ, shift traffic with weighted routing, investigate infra metrics.


👉 **Example:** Route 53 weighted record moves 20% away from us-east-1a temporarily.


---

**Q141. S3 access denied from EC2?**

**Answer (Natural):**  
Check IAM role attached via instance profile, policy allows actions, bucket policy conditions (`aws:SourceVpce`, `aws:PrincipalArn`), and IMDSv2 token usage.


👉 **Example:** Missing S3 VPC endpoint explains NAT costs and denies.


---

**Q142. Database CPU pegged—what do you do?**

**Answer (Natural):**  
Find top queries, missing indexes, and new deployments. Add read replicas temporarily, then fix queries and right-size.


👉 **Example:** Enable slow query log and use APM traces.


---

**Q143. Cost suddenly doubled this month—approach?**

**Answer (Natural):**  
Use Cost Explorer and Anomaly Detection, break down by service/tag, identify data transfer/NAT, right-size or shut down waste. Add budgets and guardrails.


👉 **Example:** Turn on S3 lifecycle and switch to GP3 volumes.


---

**Q144. Conflict with a teammate?**

**Answer (Natural):**  
Seek shared goals, clarify expectations, listen actively, propose options, agree on experiments/time-boxed trials, and document decisions.


👉 **Example:** Move from Jenkins to GitHub Actions with a 2-week spike + success criteria.


---

**Q145. Handling ambiguous requirements?**

**Answer (Natural):**  
Ask about users, SLOs, constraints, and risks; propose a strawman architecture; iterate quickly with feedback.


👉 **Example:** Present 2–3 options with trade-offs and costs.


---

**Q146. Favorite project—what made it successful?**

**Answer (Natural):**  
Clear goals, small increments, observability from day 1, automated tests, and strong stakeholder communication. Share concrete metrics (latency reduced, MTTR improved).


👉 **Example:** Reduced p95 from 800ms to 230ms with caching + DB index.


---

**Q147. Quick-fire: concept #147.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---

**Q148. Quick-fire: concept #148.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---

**Q149. Quick-fire: concept #149.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---

**Q150. Quick-fire: concept #150.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---

**Q151. Quick-fire: concept #151.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---

**Q152. Quick-fire: concept #152.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---

**Q153. Quick-fire: concept #153.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---

**Q154. Quick-fire: concept #154.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---

**Q155. Quick-fire: concept #155.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---

**Q156. Quick-fire: concept #156.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---

**Q157. Quick-fire: concept #157.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---

**Q158. Quick-fire: concept #158.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---

**Q159. Quick-fire: concept #159.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---

**Q160. Quick-fire: concept #160.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---

**Q161. Quick-fire: concept #161.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---

**Q162. Quick-fire: concept #162.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---

**Q163. Quick-fire: concept #163.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---

**Q164. Quick-fire: concept #164.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---

**Q165. Quick-fire: concept #165.**

**Answer (Natural):**  
Concise explanation tied to a production scenario. Know the trade-offs, defaults, and failure modes.


👉 **Example:** Give a one-line example relevant to your stack.


---
