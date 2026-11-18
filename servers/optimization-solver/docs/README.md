# Optimization Solver (LP / MIP Scheduling)

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/optimization-solver`. You describe an objective and the limits, and the server returns a plan that is optimal for that model, plus what each tight limit is worth.

People search for this as linear programming, mixed-integer programming, an optimization solver, scheduling optimization, or resource allocation. The questions are “a plan within the limits” and “where to put the resources.” The short install page is the [server README](../README.md). This page is what a shadow price means, why a spreadsheet search is not an optimum, and how to fill a template.

| | |
|---|---|
| **Registry name** | `ai.saifs/optimization-solver` |
| **Endpoint** | `https://optimization-solver-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

Most of these decisions are the same shape: maximize or minimize one number, subject to limits. What to make, given machine hours and material. The cheapest blend that still meets a spec. Which source ships to which destination. Which people cover which shifts. How to split a business budget across projects.

Four templates take a table. A fifth call is a general linear program for someone who already writes constraints. A sixth call explains the plan: which limits bind, the shadow price of each, and how the answer moves if a number changes a little.

The optimum is the optimum of the model you sent. It is not a promise about next quarter’s profit. Against a plan built by trying numbers in a spreadsheet, plans from this class of model are associated with about 3–15% better cost or capacity. That is a range across problems, not a promise for one plant.

The call is stateless. Send the table. Nothing is stored. If the limits cannot be satisfied together, the reply names the constraints that conflict. It does not return a plan that breaks them.

A lane table, with supply, demand, and a cost per lane, belongs here. A fleet, a list of stops, and road constraints belong on [Route & Logistics Optimizer](../../route-optimizer/README.md). How many to reorder, given a demand history, belongs on [Inventory Intelligence & Optimizer](../../inventory-intelligence-optimizer/README.md).

## Why trial and error leaves a better plan on the table

George Dantzig’s simplex method (1947) finds the optimum of a linear program at a corner of the feasible region. Interior-point methods find the same optimum by another path. When a decision has to be a whole number, a person, a vehicle, or a yes or no, the model is a mixed-integer program and the search is branch-and-cut. Shift rules that are logical rather than linear, one person on one shift, a break, a coverage count, can be handed to constraint programming (CP-SAT).

A language model can write the inequalities. It does not run the solver, and it does not report the dual value of a tight machine. That dual value, the shadow price, is often the sentence a manager acts on: the plan is this mix, and one more hour of the binding resource is worth this much.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Fill a template. You do not need to write a matrix.

```json
{
  "products": [
    {"id": "A", "profit": 420, "hours": {"cnc": 2, "assembly": 1}},
    {"id": "B", "profit": 310, "hours": {"cnc": 1, "assembly": 2}}
  ],
  "resources": {"cnc": 800, "assembly": 900}
}
```

A manufacturer’s reading of that kind of model looks like this: make so many of A and so many of B, the CNC line is the bottleneck, and each extra CNC hour is worth a stated rupee amount, in one illustration ₹4,200. Your hours and your profits produce their own quantities and their own shadow price. `explain_solution` is where that sentence is spelled out: which constraints bind, the shadow price, and the sensitivity if profit or hours move a little.

### Production

`optimize_production_mix` maximizes the profit you state, inside the resource limits you state. Quantities come back with the profit of that mix. If a product must be made in whole units, say so. The model becomes a mixed-integer program and the quantities are integers.

### Blend

`optimize_blend` is the diet problem Stigler posed in 1945, and the same shape for feed, fuel, or an alloy. Ingredients have costs and nutrients or specs. Requirements are minimums and maximums. The plan is the cheapest mix that meets every spec. If protein and cost cannot be met together from the ingredients you listed, the reply names those requirements. It does not return a mix that misses the spec.

### Transportation

`optimize_transportation` is Hitchcock’s problem (1941). Sources have supply, destinations have demand, and each lane has a cost. The plan is how many units move on each lane at minimum cost. Supply that cannot cover demand, or a lane set that leaves a destination unreachable, comes back as a conflict between those constraints.

### Shifts and jobs

`optimize_schedule` covers shifts or jobs at minimum cost, inside the rules you send: how many people a shift needs, who is qualified, how many shifts one person may work. People are integers. A schedule that cannot cover a shift with the staff you listed names that shift and that rule.

`solve_custom_lp` is for a model that is not one of those four. You send an objective and constraints. Continuous variables are a linear program:

```text
maximize cᵀx
subject to Ax ≤ b, x ≥ 0
```

Mark a variable as integer when it must be whole. The custom call is the power-user path. If the decision is a production mix, a blend, a lane table, or a roster, use the template. The template is what keeps a missing nutrient or a missing shift from being written as the wrong inequality.

### Prompts

- “Find the production mix within capacity.”
- “Allocate budget across projects.”
- “Schedule jobs on machines.”
- “What is the least-cost feed mix that still meets the protein and energy minimums?”
- “Which constraint is binding, and what is one more hour of that machine worth?”

## Tools

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `explain_solution` | A model you just solved | Which constraints bind, the shadow price of each, and how the objective moves if a limit or a profit changes a little | The model was infeasible. There is no plan to explain. Read the conflicting constraints from the solve |
| `optimize_blend` | Ingredients, costs, and minimum and maximum specs | The least-cost mix that meets every spec, or the specs that cannot be met together | You want a production quantity. That is `optimize_production_mix`. A blend that misses a spec is not returned |
| `optimize_production_mix` | Products, profit, resource use, and resource limits | Quantities and the profit of that mix | The decision is a roster or a lane table. Use the schedule or transportation template |
| `optimize_schedule` | Shifts or jobs, staff, skills, and coverage rules | A minimum-cost coverage, with whole people | You want a vehicle route with stops and road time. That is the route optimizer |
| `optimize_transportation` | Sources, destinations, supply, demand, and a cost per lane | Units to ship on each lane, at minimum cost | Supply cannot meet demand. The reply names that conflict instead of a short plan |
| `solve_custom_lp` | An objective and constraints, with integers marked where a variable must be whole | An optimal point of that model, or the constraints that make it infeasible | The problem is one of the four templates. Fill the table. Also do not use this call for a personal loan, an EMI, or a personal investment. The templates are business operations |

Shadow prices are the dual values of the linear constraints: what one more unit of a binding resource is worth at the optimum. On a mixed-integer model the explanation says so when a dual value comes from the continuous relaxation. The integer plan itself is still the plan. A shadow price is not a quote for buying that hour. It is the value of relaxing the limit inside the model you sent.

## How to read a result

- **The plan matches the objective you wrote.** Maximize profit, and the mix does that. Minimize cost, and the blend or the lanes do that. A different objective is a different plan.
- **A binding constraint is the bottleneck.** Slack means the limit was not what stopped you. The shadow price on a binding row is the one to read.
- **Infeasible means no plan.** The reply names the constraints that fight each other. Loosen one of those, or drop a requirement, and solve again. Do not average your way to a fake feasible point.
- **Integers are exact.** If you asked for whole batches or whole people, a fractional answer is not what comes back.
- **Sensitivity is local.** A small change in profit or in a limit, and the explanation says whether the mix stays. A large change is a new solve.
- **The range on the business case is not a field in the reply.** The 3–15% comparison is against hand-built plans in general. Your result is the optimum of your table.

## Data and what this will not do

Costs, hours, and recipes are operational data. The server processes the request and does not store it. Each user signs in with OAuth. If a roster includes names, send roles or ids. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies stay with you.

The server will not return a plan that violates a constraint you sent, will not hide an infeasible model behind a near-miss, and will not treat a shadow price as a market price. It will not plan a personal loan or a personal portfolio. Re-optimization is another call with the updated table.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | A small problem, up to 10 variables, on the templates |
| **Pro** | ₹4,100 / $49 per month — the full templates, shadow prices, and medium problems |
| **Business** | ₹16,500 / $199 per month — mixed-integer models, large problems, the custom builder, ERP export pulls, and a re-solve when the table changes |

The free tier is enough to fill one production or blend table and read which limit binds, with no sales call. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- Stops, a fleet, and road time: [Route & Logistics Optimizer](../../route-optimizer/README.md)
- Reorder quantity from demand history: [Inventory Intelligence & Optimizer](../../inventory-intelligence-optimizer/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
