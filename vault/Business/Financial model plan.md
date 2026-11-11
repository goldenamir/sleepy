---
type: model-specification
status: planned-no-values
created: 2026-11-11
---
# Financial model plan

## Purpose

Specify the model needed before a substantial build or commercial pilot. This is a future modelling task under [[Business development plan]], with no revenue forecast or selected price.

Use one consistent currency and show monthly cash timing. Keep customer, organization, and service-project models separate. Unmeasured inputs should remain unknown, not silently become zero.

## Model sections

| Section | Inputs to collect or estimate | Evidence / treatment |
| --- | --- | --- |
| Demand and conversion | Qualified reach, offer visits/conversations, activation, payment conversion | Separate counts by channel, cohort, and buyer type |
| Pricing | Billing interval, discounts, refunds, tax treatment, payment timing | Test a concrete offer; confirm applicable accounting/tax treatment |
| Retention | Renewal, cancellations, continued outcome use, cohort duration | Do not infer annual retention from a short pilot |
| Variable delivery costs | Hosting, processing/AI, data/vendor fees, payment fees, support and onboarding labor | Measure actual pilot usage and time; identify minimum fees |
| Fixed operating costs | Necessary tooling, accounting, reviews, insurance if required, maintenance, other overhead | Separate recurring costs from one-off launch work |
| Founder capacity | Hours for research, development, support, sales, operations | Show a cash view and a labor-inclusive view |
| Acquisition | Advertising, content/partnership effort, sales time, incentives | Attribute to channels; do not count a cost twice |
| Cash and funding | Starting cash, stage ceilings, customer receipts, supplier timing, reserves | Show monthly downside and the maximum affordable loss |

## Core calculations

**Net service revenue per paying customer per month** is revenue excluding pass-through sales taxes and after discounts/refunds, on a consistent basis. Record the assumptions rather than making a tax determination in this note.

**Monthly contribution per customer** = net service revenue − variable delivery costs − payment costs − allocated support/onboarding costs. Define which costs are included in each term to avoid double counting.

**Simple steady-state operating break-even customers** = monthly fixed operating costs ÷ positive monthly contribution per customer. This simplified calculation excludes growth/acquisition expenditure unless explicitly added; show that expenditure separately in the cash model. If contribution is zero or negative, more customers do not resolve the underlying problem.

**Customer acquisition cost by channel** = attributable acquisition spending and valued effort ÷ new paying customers acquired through that channel. Report uncertainty when attribution is weak.

**Cash payback period** uses actual contribution earned over time against acquisition cost. Model retention and timing explicitly. Do not present a reliable lifetime-value ratio until adequate cohort evidence exists; early lifetime values are scenarios.

## Scenarios and sensitivity

Build downside, base, and upside cases with an explanation for every changed input. Prioritize sensitivity to conversion, price, retention, onboarding effort, support time, integration maintenance, and acquisition cost.

Include a case with failed BLE access, a supported fallback with additional customer effort, and a case where willingness to pay is lower than expected. These are scenarios, not claims about actual results.

## Investment decisions

Before each phase, record the cash ceiling, founder-hour ceiling, evidence the spending is intended to buy, review date, and stopping condition. No amount is approved by this specification.

Before a paid pilot, the founder should be able to answer:
- How much can the pilot lose, and is that affordable?
- What must improve for contribution to become positive?
- How many customers can the founder support?
- What cash is needed before receipts arrive?
- Which assumptions would change the Continue/Stop decision?

Choose a funding approach after clarifying ambition and evidence. Evaluate the milestones and constraints attached to funding; do not assume a startup needs external investment.

## Output when commissioned

A scenario model with an assumptions register, monthly cash view, contribution analysis, capacity estimate, and a short recommendation. No spreadsheet or forecast has been built yet.
