# Q3 Chat Dump — AI Transparency Log

**Student:** Carl Graf von Moltke  
**AI tool:** Claude (Anthropic), via claude.ai with web search  
**Date:** 2026-06-02  
**Purpose:** Collaborative development of Q3 cost model + unit economics for DevOps final project (DBAI23)

---

## Process Overview

The Q3 answer was developed through an iterative discussion between Carl and Claude across eight rounds of back-and-forth before the final document was generated. Carl provided a structured handover document specifying deployment details, Q3 requirements, and the production architecture from Q2. Claude performed pricing research and built the cost model; Carl challenged assumptions, corrected methodology, and shaped the analytical framing at each stage.

---

## What Claude Provided

- **AWS pricing research:** Claude performed web searches for current pricing of EC2 (t3.large, t3.xlarge), RDS (db.t3.small Multi-AZ), S3, ALB, Secrets Manager, CloudWatch, EBS, and Route 53 in eu-west-1. Prices were sourced from third-party aggregators (CloudPrice, Vantage Instances) and cross-referenced against official AWS pricing pages. Claude flagged uncertainty on several prices and provided links for verification.
- **DeepSeek API pricing:** Claude searched for current DeepSeek pricing. Multiple sources gave conflicting numbers (V4 Flash at $0.14/$0.28 vs V4 at $0.30/$0.50). Claude flagged the discrepancy and recommended using the lower figure with verification at the official documentation.
- **Initial usage assumptions:** Claude proposed initial assumptions (messages/user/day, tokens/message, RAG parameters). These were subsequently revised through Carl's corrections.
- **Cost arithmetic:** Claude performed all line-item calculations, per-message cost derivation, and unit economics decomposition.
- **Sensitivity analysis:** Claude ran sensitivity analysis showing the relative impact of token assumptions vs volume vs instance sizing on total cost.
- **Structural analysis:** Claude identified the infrastructure-dominance finding, the step-function behaviour of the cost curve, and the conditional nature of the finding.

---

## What Carl Contributed (Corrections and Critical Insights)

Carl's contributions significantly reshaped the analysis at nine key points:

### 1. Fixed engagement rate across scales
Claude initially varied messages/user/day by scale (5 at 10 MAU, 8 at 100 MAU, 10 at 1,000 MAU). Carl pointed out that MAU and engagement are independent metrics — it is more methodologically sound to hold engagement constant and let volume do the scaling. Adopted: 8 messages/user/day across all scales.

### 2. Token assumptions too conservative
Carl argued that 500 input + 300 output tokens underestimated real academic usage, especially with PDF uploads and RAG context. Revised to 2,000 input (500 user + 1,500 RAG) and 400 output. Carl also noted that DeepSeek's pricing is so low that even doubling token estimates barely moves the total — a key insight for the sensitivity discussion.

### 3. Qdrant corpus sizing must be bottom-up
Claude initially proposed "~5 GB at 1,000 MAU" with no derivation. Carl flagged this as arbitrary and requested a traceable estimate based on number of courses, documents per course, chunks per document, and vector dimensions. This produced the bottom-up calculation (courses → documents → chunks → vectors) showing ~150–200 MB — far smaller than assumed — and the conclusion that Qdrant is not a cost driver.

### 4. User upload retention policy matters
Carl identified that the Qdrant sizing conclusion depended on whether user uploads accumulate indefinitely or are pruned. Claude had not considered this distinction. Carl's challenge led to the semester-scoped retention policy assumption (justified via GDPR, data isolation, and pedagogical logic) and the acknowledgment that indefinite accumulation would change the growth dynamics.

### 5. Conditional framing of infrastructure dominance
Claude initially stated "this business is infrastructure-constrained, not AI-constrained" as an absolute finding. Carl argued this is only valid under the specific premises (DeepSeek pricing, 1,000 MAU, simple RAG workload) and that any single parameter change — premium LLM, agentic workflows, larger context windows — would invert the ratio. This led to the conditional framing with boundary conditions and sensitivity quantification.

### 6. RDS sizing justification needed
Carl noted that holding RDS constant while scaling EC2 required explicit justification — an examiner could ask why the database doesn't scale with users. This prompted the explanation of transactional vs application-layer workloads and storage growth as the actual RDS scaling trigger.

### 7. Lever 2 contradicts Q2 architecture
Carl pointed out that replacing ALB with Caddy contradicts the Q2 production architecture (ALB + ACM for reliability and scalability). He required that Lever 2 explicitly acknowledge this as a conscious trade-off, not present it as a pure optimisation. He also noted ALB's value (health checks, certificate management, routing) is not scale-dependent.

### 8. Lever 2 bundling problem
Carl flagged that combining two sub-changes (Multi-AZ → Single-AZ and ALB → Caddy) into a single "lever" made it difficult to attribute savings. The final document separates the savings attribution.

### 9. Adoption risk in pricing recommendation
Carl identified that the pricing recommendation assumed sufficient adoption to reach the flat segment of the unit economics curve (~200–300 MAU), but did not address the risk of adoption stalling at 50–100 MAU. He pointed out that the fixed-cost structure that makes the product cheap at scale also makes it expensive at low adoption, and that this dependency should be stated explicitly. Added: adoption risk paragraph with the strategic implication that the school must treat adoption as a deliberate priority.

---

## Sources Used

### AWS Pricing (all for eu-west-1 unless noted)
- EC2 on-demand: https://aws.amazon.com/ec2/pricing/on-demand/
- EBS: https://aws.amazon.com/ebs/pricing/
- RDS PostgreSQL: https://aws.amazon.com/rds/postgresql/pricing/
- S3: https://aws.amazon.com/s3/pricing/
- ALB: https://aws.amazon.com/elasticloadbalancing/pricing/
- Route 53: https://aws.amazon.com/route53/pricing/
- Secrets Manager: https://aws.amazon.com/secretsmanager/pricing/
- CloudWatch: https://aws.amazon.com/cloudwatch/pricing/

### Third-party pricing aggregators (cross-referencing)
- CloudPrice (cloudprice.net) — EC2 and RDS instance pricing by region
- Vantage Instances (instances.vantage.sh) — RDS instance specs and pricing
- CostGoat (costgoat.com) — DeepSeek API and Secrets Manager calculators

### DeepSeek API
- Official: https://platform.deepseek.com
- Pricing guides: costgoat.com, nxcode.io, cloudzero.com (conflicting V4 pricing; $0.14/$0.28 per million tokens used as conservative estimate)

### Uncertainty flags
- EC2 and RDS prices from third-party aggregators may lag official AWS pricing; all should be verified on official pages
- DeepSeek pricing has changed multiple times (most recently September 2025); verify before submission
- ALB LCU-based charges are estimated; actual cost depends on request rate and active connections
- RI discount percentage (~38%) is an estimate; confirm in the AWS console

---

## Division of Labour Summary

| Aspect | Claude's contribution | Carl's contribution |
|---|---|---|
| Pricing research | Web searches, compilation, source linking | Verification requirements, uncertainty standards |
| Usage assumptions | Initial proposals | Corrections on engagement independence, token sizing, Qdrant derivation, retention policy |
| Cost calculations | All arithmetic and table construction | Validation of methodology |
| Analytical framing | Initial findings (infrastructure dominance, cost drivers) | Conditional framing, boundary-condition analysis, Q2 consistency checks, adoption risk |
| Lever design | Initial lever proposals | Architectural consistency review, attribution separation |
| Unit economics & pricing | Curve decomposition, break-point analysis, pricing math | Adoption risk identification, strategic context |
| Writing | Full draft of a3.md | Review, corrections, and strategic direction throughout |
