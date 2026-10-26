End-to-end weighted scoring

WORKFLOW TRACE
      │
      ├── Agent A
      │     │
      │     ├── Sub-agent A1
      │     │      ├── Span 1 → Score: 90
      │     │      ├── Span 2 → Score: 80
      │     │      └── Span 3 → Score: 70
      │     │
      │     │      Weighted span scores
      │     │             ↓
      │     │      Sub-agent score: 81
      │     │
      │     └── Sub-agent A2
      │            ├── Span 4 → Score: 100
      │            └── Span 5 → Score: 90
      │
      │            Weighted span scores
      │                   ↓
      │            Sub-agent score: 97
      │
      │      Weighted sub-agent scores
      │                  ↓
      │           Agent A score: 89
      │
      ├── Agent B
      │      ├── Sub-agent B1
      │      │      ├── Span 6 → Score: 85
      │      │      └── Span 7 → Score: 95
      │      │
      │      │      Sub-agent score: 90
      │      │
      │      └── Agent B score: 90
      │
      └── Workflow-level evaluation
               ├── Agent scores
               ├── Goal achievement
               ├── Workflow sequence
               └── Policy compliance
                         ↓
               Weighted workflow score
                         ↓
                 Workflow score: 92
                         ↓
                  Final evaluation
              ┌──────────┴──────────┐
              │                     │
        Execution status     Evaluation status
          COMPLETE              COMPLETE
              │                     │
              └──────────┬──────────┘
                         ↓
                 PASS / FAIL / REVIEW




              How the scores are calculated
1. Span → Sub-agent

Combine the scores of spans belonging to that sub-agent using configured weights.

Example:
Span	Score	Weight
Span 1	90	50%
Span 2	80	30%
Span 3	70	20%

Sub-agent score = \(90(0.5)+80(0.3)+70(0.2)=83\).

2. Sub-agent → Agent

Combine the sub-agent scores using their configured importance.

Example: Sub-agent A1 = 83 (60% weight), A2 = 97 (40% weight).

Agent score = \(83(0.6)+97(0.4)=88.6\).

3. Agent → Workflow

Use agent scores alongside direct workflow-level metrics. Do not rely only on the average of agent scores.
Workflow metric	Score	Weight
Agent aggregate	89	30%
Goal achievement	95	40%
Workflow sequence	90	20%
Policy compliance	100	10%

Workflow score = \(89(0.3)+95(0.4)+90(0.2)+100(0.1)=92.2\).

4. Workflow score → Final result
- Quality score: 92.2/100
- Execution status: Was the workflow completed?
- Evaluation status: Were all required metrics evaluated?
- Final decision: Apply the score threshold and any critical-failure rules.
Important: These values and weights are examples. If a required sub-agent or metric was not executed or evaluated, mark the relevant score as incomplete rather than automatically treating it as zero or silently ignoring it. A critical policy failure can also cause a failure even when the numerical score is high.
