<!-- DRAFT for review. Do not replace intro.md until this file is accepted.
FIELD BLOCK (Microsoft validated fields). Confirm before submission.

Title (21 characters): Scientific Calculator
Description (409 characters):
Returns an exact numeric or symbolic result and the formula that produced it. Named tools cover arithmetic, algebra, calculus, statistics, geometry, units, physics, chemistry, electrical formulas, and finance. Formula tools return the same result for the same inputs. Missing or illegal inputs return an error. It calculates and shows the working. It does not file taxes, originate a loan, or grade a student.

Operation summary (24 characters): Exact result and formula
Operation description:
Returns a numeric or symbolic result and the formula that produced it. The same inputs return the same result.
-->

# Scientific Calculator - Saifs AI

**Scientific Calculator - Saifs AI** helps students, tutors, finance assistants, and engineers get **the exact result, and the formula that produced it**. You send the values for one named calculation. It returns the answer from that formula, across arithmetic, algebra, calculus, statistics, geometry, units, physics, chemistry, electrical formulas, and finance, instead of leaving the arithmetic to the language model.

It is built for tutoring, personal-finance, and engineering assistants that need a number they can rerun and check.

| If you are | Start with |
| --- | --- |
| A tutor, student, or finance user | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [How it works](#how-it-works) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [For developers](#for-developers) |
| A reviewer checking the formula | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

Most agents still ask the language model to calculate. That can drop a sign, mix degrees with radians, or invent a tax split or a derivative it did not compute. The formulas are public. Wiring each one, with units and the cases that should error, is a large surface.

Scientific Calculator makes those formulas available to any AI agent:

- **What is the result**: a number or a symbolic expression from the named tool.
- **Which formula produced it**: the working sits beside the result.
- **What it will not invent**: a missing or illegal input returns an error.
- **What it will not do**: it does not file a tax return, originate a loan, grade a student, or update a bank, a laboratory, or an ERP.

## Why choose it: the same result every time

Scientific Calculator is deterministic on its formula tools. The same inputs, on the same tool, produce the same result and the same formula. Results are computed from fixed mathematics. They are not written afresh by a language model each time you ask. `calculator_random` repeats only when you pass `seed`. A secure token does not use that seed.

That matters because a wrong sign or an invented tax split can land on a homework check, a quote, or a working paper. The agent's wording around the result may vary. The result does not.

| What it does | Established method |
| --- | --- |
| Arithmetic, percent, fractions, ratio, proportion, and average | Standard arithmetic |
| Expressions, equations, simplify, factor, and expand | Symbolic evaluation of the stated formula |
| A definite integral with no closed form | Simpson's rule or the trapezoidal rule, with the interval count you supply |
| Roots when a closed form is not returned | Newton–Raphson, on the numerical tools |
| Mean, spread, combinations, binomial probability, and the normal curve | Classical statistics |
| Force, motion, energy, and electrical relations | The stated physics or electrical formula, in SI defaults |
| Molar mass, dilution, pH, and the gas laws | Stoichiometry, pH, and the ideal gas law, under the assumptions in Known issues |
| Simple and compound interest, EMI, SIP, CAGR, and GST | Annuity and percentage formulas. Money is rounded to 2 decimal places |

**It does not guess.** A missing value, division by zero, a zero leading coefficient, a singular matrix, or an empty list returns an error. The service does not fill in a missing input. Symbolic cases with no closed form may error rather than approximate.

**It calculates and does not act.** It does not file taxes, originate loans, grade a student, or write to a bank, a laboratory, or Dataverse. Money results are calculations, not tax, legal, or financial advice. A loan schedule is capped at 120 rows and will not match a lender's full amortisation.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Students and tutors** in school and entrance-exam maths, physics, and chemistry | The matching tool returns the result and the formula |
| **Personal-finance assistants** | EMI, SIP, GST, CAGR, discount, and profit or loss, with the formula visible |
| **Finance and accounts teams** | A checkable interest, GST split, or growth figure for a quote or a working paper |
| **Engineering sidekicks** | Ohm's law, dividers, unit conversion, matrices, and numerical roots |
| **Electrical and programmer utilities** | Resistor colour codes, series and parallel resistance, AC reactance, and base conversion |
| **Agent makers and developers** | Add named calculation tools so the agent does not do the arithmetic itself |

**Best fit:** tutoring and homework assistants, personal-finance assistants, and engineering assistants that must show a checkable formula.

**Not required:** a separate computer-algebra install, or a spreadsheet rebuilt for every formula. The values in the question are enough to call the matching tool.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. Tutoring and homework**
Scenario: a learner asks for the roots of a quadratic, a derivative, or a molar mass.
Outcome: the matching tool returns the result and the formula, so the tutor can show the working.
Ask: "Roots of 2x^2 + 3x - 5 = 0, and show the formula."

**2. A result the model must not invent**
Scenario: an agent is about to answer a percent, a fraction, or a scientific expression from memory.
Outcome: it calls `calculator_percentage`, `calculator_fraction`, or `calculator_scientific` and reports that result.
Ask: "What is 15% of 500?"

**3. Personal finance**
Scenario: a visitor asks for the EMI on a principal, an annual rate, and a tenure, or for the future value of a monthly SIP.
Outcome: `calculator_emi` or `calculator_sip` returns the amount, rounded to 2 decimal places, with the formula. This is a calculation, not a loan offer.
Ask: "EMI on 500000 at 10% for 5 years."

**4. GST on a price**
Scenario: a quote needs GST added to a base amount, or removed from a tax-inclusive amount, at a stated rate.
Outcome: `calculator_gst` splits the amount. It does not file a return.
Ask: "Split 1180 at 18% GST into base and tax."

**5. Physics and electrical checks**
Scenario: an engineer or a student needs force from mass and acceleration, or the missing quantity in Ohm's law.
Outcome: the physics or electrical tool solves the stated formula and names the relation it used.
Ask: "Force from mass 10 kg and acceleration 2 m/s^2."

**6. Chemistry**
Scenario: a student needs a molar mass, a dilution, or a pH from a hydrogen ion concentration.
Outcome: the chemistry tool returns the result under the stated assumptions. pH assumes a dilute aqueous solution near 25 °C.
Ask: "pH if the hydrogen ion concentration is 0.0001."

**7. Trigonometry with a stated angle unit**
Scenario: a class is working in degrees, and a scientific expression elsewhere is in radians.
Outcome: `calculator_trigonometry` uses `angle_unit` of degrees or radians. Expressions in `calculator_scientific` use radians.
Ask: "sin of 30 degrees."

**8. Units, dates, and bases**
Scenario: a user needs a unit conversion, an age in days, or a hex conversion.
Outcome: the matching conversion, date, or programmer tool returns the value and the method.
Ask: "Convert 10 km to miles."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Homework agent | Returns a result and the formula for algebra, calculus, or chemistry | The values from the learner's question | 1, 6, 7 |
| Arithmetic check agent | Stops the model answering a percent, a fraction, or an expression from memory | Nothing else. Call the named tool | 2 |
| Personal-finance agent | Returns EMI, SIP, interest, CAGR, or a GST split as a calculation | Principal, rate, tenure, or the amount and the tax rate | 3, 4 |
| Engineering agent | Returns physics, electrical, matrix, and unit results | The quantities and, for trigonometry, the angle unit | 5, 8 |
| Programmer agent | Converts bases and runs bitwise operations | The value and the target base | 8 |
| Lab-check agent | Returns a chemistry or physics result under the stated assumptions | The measured inputs. A person still owns the lab record | 5, 6 |
| Scheduled check agent | Recomputes a named formula on a cadence, for example a quote total | A flow with a recurrence trigger. The tool itself has no schedule | 4 |

The tools run on request. Scheduling comes from the flow, not from the connector. There is no local engine. Each call is one named tool. Stock catalogues, hiring files, and overdue books belong in the Saifs connector built for that job.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | 169 named calculation tools. There is no local-engine tool |
| Minimum input | The fields that one tool requires. There is no uploaded corpus |
| Size | One calculation per call. A loan schedule is capped at 120 rows |
| Writes to your systems | None |
| Memory of past calls | None. The server does not store earlier calculations or a learner profile |
| Pricing | [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment) |

<!-- TODO Saifs AI: confirm this marketplace URL is the pricing page for this offer, and confirm the count of 169 tools still matches the published package. -->

## Where the data comes from

Scientific Calculator does not connect to your systems. The agent sends the values for one calculation. It does not read a spreadsheet of business records, and it has not been certified against any ledger, laboratory, or tax system.

| Data source | How the agent gets the data | What to send |
| --- | --- | --- |
| The user's question | The agent copies the numbers into the matching tool | Only the fields that tool requires |
| A quote, a worksheet, or a lab note the user pasted | The agent calls one tool per calculation | Do not send a whole workbook as if it were one formula |
| A flow | The flow passes the inputs on a schedule | The same named tool, with those inputs |

There is no second route that keeps inputs off the server. Every call sends its inputs to Saifs AI for calculation. Do not send a calculation you are not authorised to process. Do not send a learner's name. The formula does not need it.

## How it works

Each tool is one operation. The agent picks the tool whose name matches the task. It does not compute the value itself.

### Inside the connector

1. The user asks for a calculation and supplies the values.
2. The agent calls one named tool. The connector passes the call to the Saifs AI MCP server over HTTPS at `https://scientific-calculator-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The tool evaluates the formula and returns the result, or an error.
5. The agent reports the result and the formula. The number comes from the tool. The wording around it comes from the agent.

### Inside the calculation

| Step | What the tool does | Method |
| --- | --- | --- |
| 1. Check the inputs | Rejects a missing or illegal value. It does not invent one | Validation |
| 2. Apply the named formula | Evaluates that tool's formula or numerical method | The method named in the result |
| 3. Honour units | Trigonometry uses `angle_unit`. Scientific expressions use radians. Gas-law temperature is converted to kelvin inside the tool | Stated units |
| 4. Round money | Finance results are rounded to 2 decimal places. Loan schedules stop at 120 rows | Fixed rounding and cap |
| 5. Return the working | The result includes the formula or the method | Shown beside the number |

**Why the answer does not change.** On the formula tools, the same inputs return the same result. Physical constants used by the handlers are fixed, including g = 9.80665. The ideal-gas tool uses R = 0.082057 L·atm/(mol·K). `calculator_random` repeats only when you pass `seed`. A secure token does not use that seed.

**What happens when data is missing.** The tool returns an error. It does not remember an earlier calculation, so a later question must send its own inputs.

**Where the working is shown.** The formula or method is part of the tool result. Graph tools return points for the host to draw. They do not attach a picture by themselves.

## Data handling and security

| Question | What happens |
| --- | --- |
| Do the values in the call reach Saifs AI? | Yes. There is no local-engine route |
| Where are they processed? | `scientific-calculator-mcp-microsoft.saifs.ai`, over HTTPS |
| Does the server remember earlier calculations? | No. It does not store homework text or build a learner profile |
| What is retained for billing or audit? | <!-- TODO Saifs AI: confirm. The current product statement is that the server is stateless for calculation history. Confirm whether request values are logged. --> |

**Sign-in.** Users sign in with Microsoft single sign-on (OAuth 2.0). No API key is created, shared, or stored.

**No write access.** The connector does not file a return, originate a loan, write a grade, or update a bank, a laboratory, or Dataverse.

**No local-only switch.** This connector has no hosted-tool-off path, because every tool runs on the server. If a calculation must not leave your environment, do not send it.

## Security and compliance

This section separates what an independent party has reviewed from what Saifs AI states about itself.

**Reviewed by Microsoft**

| Item | What it covers |
| --- | --- |
| Microsoft connector certification | Microsoft reviewed and tested this connector before listing it as a certified connector. The review covers the connector's function and packaging, not Saifs AI's internal systems. |

**Self-assessed by Saifs AI**

Self-assessments are statements Saifs AI makes about its own practices. They are published so that you can check them, and they are not independent audits.

<!-- PUBLISH EACH ROW ONLY AFTER IT IS COMPLETED. Until then, keep the row inside this comment.

| Self-assessment | What it is | Where to read it |
| --- | --- | --- |
| Microsoft Publisher Attestation | Saifs AI's answers to Microsoft's questionnaire. Microsoft publishes the answers and does not verify them. Renewed every year. | [link] |
| CSA STAR Level 1 | Saifs AI's completed CAIQ on the public STAR Registry. Updated every year. | [link] |
| CSA STAR for AI Level 1 | Saifs AI's completed AI-CAIQ on the same registry. | [link] |

-->

| Design fact | Detail |
| --- | --- |
| Calculation history stored | None. The server does not remember earlier calculations or build a learner profile |
| Request-log retention | <!-- TODO Saifs AI: confirm. --> |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Token usage and cost

There is no local engine. Every call sends its inputs and reads back a result and a formula. A normal formula call stays under $0.01. Cost grows only when the result itself is long, such as a loan schedule.

| Call | Tokens and cost | What was measured |
| --- | --- | --- |
| 15% of 500 | about 30 tokens, under $0.01 | Inputs `percentage` and `number`, plus a one-line result and formula |
| EMI on 500000 at 10% for 5 years | about 50 tokens, under $0.01 | `principal`, `rate`, and `tenure`, plus EMI, totals, and the formula |
| Loan schedule of 120 rows | about 4,600 tokens, under $0.01 | A short request, and 120 schedule rows read back |

The schedule is input tokens, the cheaper kind, because the model reads it. The request the model sends is only a few dozen tokens. There is no file of business rows, so there is no second route that keeps a flat token cost.

**Hosts that bill per action.** Some agent hosts charge a fixed amount for each tool call and do not bill by token. On those hosts a percentage and a loan schedule can cost the same even though the schedule is longer to read.

How these figures were measured: token counts use the `o200k_base` tokenizer on generated sample JSON, on 16 November 2026. Costs use $1.25 per million input tokens and $10 per million output tokens. The values the model sends are output tokens. The result it reads is input tokens. The 120-row schedule is a representative result, not a captured engine file. Counts vary by tool.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- The values the chosen tool requires

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Scientific Calculator** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any arithmetic, algebra, science, or finance calculation, call the matching Scientific Calculator tool. Do not calculate the value yourself. Report the formula with the result."
5. Ask for one calculation and include the values. Name the angle unit when you use trigonometry.
6. Try "What is 15% of 500?" or "sin of 30 degrees."

Read the result next to the formula. Treat finance and chemistry results as calculations under the stated assumptions.

## For administrators

Enabling this connector gives the organisation one approved way to get a checkable number, and the formula that produced it.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to calculate in the chat. A language model can give a different figure on each run, and the result cannot be rerun as a formula. A governed connector uses the named tool.

- **You choose who can connect it.** Tenant and environment permissions apply.
- **You keep your system of record.** The connector calculates and shows the formula. It does not file a return, book a loan, or write a grade.
- **You can check the number.** The same tool and the same inputs can be called again. Missing or illegal inputs are errors.
- **You do not collect a learner history.** The server does not remember past calculations or build a student profile.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

There is no local engine to switch on. Every call sends its inputs to Saifs AI.

**Review checklist**

- Confirm users will not send data they are not authorised to process
- Assign the connector to the correct data policy group
- Review the privacy policy and terms
- Read the self-assessments under Security and compliance, once they are published
- Tell finance users that GST and EMI results are calculations, not filings or loan offers

## For developers

Call the tool whose name matches the task. The package lists 169 tools in `scientific_calculator_mcptools.json`. Coverage is that list. It is not every formula in science.

| Domain | Tools |
| --- | --- |
| Basic | `calculator_basic`, `calculator_percentage`, `calculator_fraction`, `calculator_mixed_fraction`, `calculator_ratio`, `calculator_proportion`, `calculator_average` |
| Scientific | `calculator_scientific`, `calculator_trigonometry`, `calculator_logarithm`, `calculator_exponential`, `calculator_power`, `calculator_root`, `calculator_factorial`, `calculator_absolute`, `calculator_modulo`, `calculator_reciprocal` |
| Geometry | `calculator_triangle`, `calculator_right_triangle`, `calculator_circle`, `calculator_rectangle`, `calculator_square`, `calculator_trapezoid`, `calculator_parallelogram`, `calculator_polygon`, `calculator_geometry_3d`, `calculator_surface_area`, `calculator_volume` |
| Algebra | `calculator_linear_equation`, `calculator_quadratic_equation`, `calculator_cubic_equation`, `calculator_polynomial`, `calculator_system_of_equations`, `calculator_inequality`, `calculator_simplify`, `calculator_factor`, `calculator_expand`, `calculator_gcd`, `calculator_lcm`, `calculator_prime_check`, `calculator_prime_factors` |
| Statistics | `calculator_mean`, `calculator_median`, `calculator_mode`, `calculator_range`, `calculator_variance`, `calculator_std`, `calculator_percentile`, `calculator_quartile`, `calculator_weighted_average`, `calculator_combinations`, `calculator_permutations`, `calculator_probability`, `calculator_binomial`, `calculator_normal` |
| Calculus | `calculator_derivative`, `calculator_second_derivative`, `calculator_partial_derivative`, `calculator_integral`, `calculator_numerical_integral`, `calculator_limit`, `calculator_taylor`, `calculator_critical_points`, `calculator_gradient`, `calculator_numerical_derivative` |
| Matrix and vector | `calculator_matrix`, `calculator_matrix_add`, `calculator_matrix_multiply`, `calculator_matrix_inverse`, `calculator_determinant`, `calculator_transpose`, `calculator_eigenvalues`, `calculator_eigenvectors`, `calculator_rank`, `calculator_vector`, `calculator_dot`, `calculator_cross`, `calculator_magnitude`, `calculator_unit_vector` |
| Units | `calculator_convert`, `calculator_length`, `calculator_weight`, `calculator_area`, `calculator_volume_convert`, `calculator_temperature`, `calculator_time`, `calculator_speed`, `calculator_pressure`, `calculator_energy`, `calculator_power_convert` |
| Physics | `calculator_physics_speed`, `calculator_physics_distance`, `calculator_physics_acceleration`, `calculator_physics_force`, `calculator_physics_work`, `calculator_physics_power`, `calculator_kinetic_energy`, `calculator_potential_energy`, `calculator_momentum`, `calculator_density`, `calculator_ohms_law`, `calculator_electrical_power`, `calculator_wavelength`, `calculator_frequency`, `calculator_physics_pressure` |
| Finance | `calculator_simple_interest`, `calculator_compound_interest`, `calculator_emi`, `calculator_loan`, `calculator_investment`, `calculator_sip`, `calculator_gst`, `calculator_discount`, `calculator_profit_loss`, `calculator_cagr`, `calculator_percentage_change` |
| Electrical | `calculator_ee_ohms_law`, `calculator_ee_power`, `calculator_ee_resistor`, `calculator_resistor_color_code`, `calculator_series_resistance`, `calculator_parallel_resistance`, `calculator_capacitor`, `calculator_inductor`, `calculator_voltage_divider`, `calculator_current_divider`, `calculator_ac`, `calculator_reactance` |
| Programmer | `calculator_binary`, `calculator_decimal_convert`, `calculator_hex_convert`, `calculator_octal_convert`, `calculator_binary_decimal`, `calculator_bitwise`, `calculator_shift`, `calculator_base_convert`, `calculator_ascii`, `calculator_storage` |
| Date and time | `calculator_date_difference`, `calculator_age`, `calculator_days`, `calculator_time_difference`, `calculator_leap_year`, `calculator_weekday` |
| Chemistry | `calculator_molar_mass`, `calculator_moles`, `calculator_molarity`, `calculator_dilution`, `calculator_ph`, `calculator_concentration`, `calculator_gas_law`, `calculator_stoichiometry`, `calculator_periodic_table` |
| Advanced | `calculator_equation_graph`, `calculator_function_plot`, `calculator_complex`, `calculator_complex_convert`, `calculator_numerical_solve`, `calculator_root_finder`, `calculator_newton_raphson`, `calculator_interpolation`, `calculator_regression`, `calculator_linear_regression`, `calculator_quadratic_regression`, `calculator_scientific_constants`, `calculator_unit_constants`, `calculator_math_constants`, `calculator_random`, `calculator_random_distribution` |

### Common inputs

| Field | Meaning | Needed for |
| --- | --- | --- |
| Operands such as `a`, `b`, or a list of numbers | The values for that operation | The result |
| `operation` | Which arithmetic or fraction operation to apply | Basic tools that share one schema |
| `expression` | A scientific expression. Angles inside it are radians | `calculator_scientific` |
| `angle_unit` | `degrees` or `radians` | `calculator_trigonometry` |
| `principal`, `rate`, `tenure` | Finance inputs. Rate is an annual percent | EMI and related tools |
| From-unit and to-unit | The conversion | Unit tools |

### Sample request

`calculator_emi`:

```json
{
  "principal": 500000,
  "rate": 10,
  "tenure": 5
}
```

`calculator_trigonometry`:

```json
{
  "function": "sin",
  "value": 30,
  "angle_unit": "degrees"
}
```

`angle_unit` defaults to degrees. `function` and `value` are required. EMI requires `principal` and `rate`. `tenure` is in years. `tenure_months` is the alternative.

### Sample response (shortened)

<!-- TODO Saifs AI: replace this note with a shortened response from a real call to calculator_percentage or calculator_emi. Do not publish an invented result. -->

A live shortened response is not included in this draft. Capture one call before you publish the page. The sample should show the result and the formula, or an error for an illegal input.

### Response sections

| Section | Use it for |
| --- | --- |
| Result | The number or symbolic value |
| Formula or method | The working to show beside the result |
| Error | A missing or illegal input. No value was invented |

## Known issues and limitations

- **A calculation, not a system of record.** It does not file taxes, originate a loan, grade a student, or write to a bank, a laboratory, or an ERP.
- **Not tax, legal, or financial advice.** Money results round to 2 decimal places. A loan schedule is capped at 120 rows.
- **GST tools add or remove a rate.** They do not prepare a return.
- **Chemistry assumptions.** pH and pOH assume a dilute aqueous solution near 25 °C, with pH + pOH = 14. The ideal-gas tool uses R = 0.082057 L·atm/(mol·K).
- **Angle units.** `calculator_trigonometry` needs `angle_unit`. Expressions in `calculator_scientific` use radians.
- **No silent approximation.** Illegal inputs return an error. Numerical integrals use Simpson's rule or the trapezoidal rule only when you call that tool.
- **Stateless.** The server does not remember an earlier calculation.
- **Named tools only.** The catalog is the 169 tools in this package.
- **Random draws.** `calculator_random` and `calculator_random_distribution` repeat only when you pass `seed`. A secure token ignores the seed.
- **Graphs are points.** `calculator_equation_graph` and `calculator_function_plot` return values for the host to draw. They do not attach a picture.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

<!-- TODO Saifs AI: confirm the excluded-cloud sentence for this offer. EMI and trigonometry samples now match the schema. Random tools are scoped above. -->

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or asks you to sign in again | The connection has expired | Open the connection and sign in again |
| An error instead of a number | A required input is missing, or the input is illegal, such as division by zero | Send the missing value. Do not expect the tool to invent it |
| A trigonometry result looks wrong | Degrees and radians were mixed | Set `angle_unit`. Remember that `calculator_scientific` uses radians |
| A finance figure is not the lender's schedule | The schedule is capped at 120 rows and rounded to 2 decimal places | Treat it as a calculation, not the lender's amortisation |
| The agent states a number without a tool call | Instructions do not require the tool | Add the instruction in Get started |
| Not permitted, or a subscribe link | The account cannot call the tool | Paste the full Marketplace link exactly as returned |

## FAQ

**Do I need a specialist to use this?**
No. Ask in plain language and include the values. The agent calls the matching tool.

**What is the least data I need?**
The inputs that one tool requires. There is no file to upload.

**Does my data leave my environment?**
Yes. The values in the call are sent to Saifs AI for calculation. There is no local-engine route. The server does not keep a calculation history or a learner profile.

**Will it change anything in my systems?**
No. It does not file a return, book a loan, write a grade, or update another system.

**I already have a spreadsheet. Why add this?**
A spreadsheet still needs a person to pick the formula. This connector gives the agent a named formula and an error when the input is illegal, so the model does not invent the digits.

**Why do I get the same answer every time?**
On the formula tools, the same inputs return the same result and the same formula. The agent's wording may vary. `calculator_random` repeats only when you pass `seed`. A secure token does not use that seed.

**What happens if my data is incomplete?**
The tool returns an error. It does not fill in the missing value.

**Can I check how a result was produced?**
Yes. The formula or method is returned with the number.

**How large a calculation can it handle?**
One named tool per call. A loan schedule stops at 120 rows. Large business files belong in the connector built for that job.

**Can it run on a schedule?**
Yes, from a scheduled flow that passes the inputs. The connector itself runs on request.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

## Support

**Scientific Calculator - Saifs AI** is published by Saifs AI.

- Website: [https://saifs.ai](https://saifs.ai)
- Learn more: [https://saifs.ai/scientific-calculator](https://saifs.ai/scientific-calculator)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Privacy: [https://saifs.ai/privacy_policy/scientific-calculator](https://saifs.ai/privacy_policy/scientific-calculator)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- Sign-in: Microsoft single sign-on (OAuth 2.0). No API keys.
- MCP endpoint: `https://scientific-calculator-mcp-microsoft.saifs.ai/mcp`

<!-- OPEN QUESTIONS
1. Confirm the marketplace offer URL.
2. Confirm whether request values are logged, and for how long. Calculation history is stated as not stored.
3. Capture a shortened live response for one finance tool and one error case.
4. Confirm EMI and trigonometry property names in the sample.
5. Confirm whether calculator_random repeats without a seed. If it does not, narrow the deterministic claim.
6. Confirm excluded clouds.
7. Token counts for three calls are measured on generated JSON. The 120-row schedule is a representative result, not a captured response. There is no local engine.
8. Publisher Attestation and STAR rows stay commented until completed and linked.
-->
