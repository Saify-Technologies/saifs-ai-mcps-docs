# Scientific Calculator

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/scientific-calculator`. Each call is one named formula. The reply is the number or the symbolic result, and the formula or method that produced it.

People search for this as a scientific calculator, a quadratic solver, an EMI calculator, a unit converter, or a checked formula for physics and chemistry. The short install page is the [server README](../README.md). This page is what comes back with the number, why the model must call the tool, and which assumptions the formula already states.

| | |
|---|---|
| **Registry name** | `ai.saifs/scientific-calculator` |
| **Endpoint** | `https://scientific-calculator-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

School arithmetic, algebra, geometry, statistics, calculus, matrices, units, physics, electrical formulas, chemistry, programmer conversions, dates, and India-shaped finance (simple and compound interest, EMI, SIP, GST, CAGR) each have their own tool. Symbolic algebra, calculus, and matrices are computed with SymPy. Closed-form tools in finance, physics, and chemistry evaluate an explicit formula and return that formula with the number.

The same inputs always give the same answer. Constants are fixed in the handlers: standard gravity `g = 9.80665` unless you override it, the gas constant `R = 0.082057 L·atm/(mol·K)`, the thermochemical calorie `4.184 J`, mechanical horsepower about `745.7 W`, and a Julian year of `365.25` days. A missing input is an error. The tool does not invent a value.

The call is stateless. Nothing is stored, and the server does not build a learner profile. A homework question is a calculation. The reply states the formula. It does not grade the person.

A dated GST or TDS table, margin, and the other invoice formulas live on [Business Calculator: GST, Margin & Everyday Formulas](../../business-calculator/README.md). That server refuses EMI and interest. Those annuities are here. A Python program that prints a result belongs on [Mathematical Code Execution](../../code-execution/README.md).

## Why the model has to call the tool

A language model drops a sign, mixes degrees with radians, invents a GST split, or quotes a derivative it did not compute. The methods themselves are old and public: the quadratic formula, `F = ma`, the annuity EMI, `pH = −log10[H+]`, Simpson’s rule. Each one is a tool, with the units and the edge cases written down, so the host shows a formula a person can check.

The server does not claim every formula in science. It claims the named tools, and the formula string each one returns.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Call the tool that matches the formula. Pass units explicitly.

A quadratic call for `2x² − 5x − 3 = 0` comes back with the roots `3` and `−1/2` and the formula `x = (−b ± √(b² − 4ac)) / (2a)`. The leading coefficient must be nonzero. An identity, or an equation that is not quadratic, is rejected.

```json
{ "a": 2, "b": -5, "c": -3, "variable": "x" }
```

An EMI call takes principal, annual rate, and the number of months. The reply is the instalment to two decimal places and the formula

```text
EMI = P · r · (1 + r)^n / ((1 + r)^n − 1)
```

where `r` is the monthly rate. When the monthly rate is 0, the instalment is `P / n`. A 20-year loan has an EMI. The month-by-month schedule is capped at 120 rows, so a longer amortisation is the formula and the instalment, not a bank’s full table.

### Arithmetic and scientific expressions

`calculator_basic`, `calculator_percentage`, `calculator_fraction`, `calculator_mixed_fraction`, `calculator_ratio`, `calculator_proportion`, and `calculator_average` cover school arithmetic. `calculator_scientific` parses an expression and evaluates it, so precedence and functions are computed. Trigonometry takes an explicit `angle_unit`, degrees or radians. Logarithms take a base. Roots take an index. Factorial rejects a negative integer. Modulo and reciprocal reject division by zero.

### Algebra

A linear equation is solved for the variable you name. A quadratic requires `a ≠ 0`. Cubics and higher polynomials, systems, inequalities, simplify, factor, and expand are computed. GCD, LCM, a prime check, and prime factors run on the integers you send.

### Geometry, statistics, matrices

Plane figures and solids return area, surface area, or volume from the lengths you send. Statistics select sample or population variance and standard deviation with `variance_type` and `std_type`. Percentiles use nearest rank. Combinations are `n choose r`. Permutations are `nPr`. Binomial probability is the probability mass at `k`. The normal distribution returns the density and the cumulative `Φ(z) = 0.5 (1 + erf(z))`.

Matrices and vectors add, multiply, invert, and return the determinant, the transpose, eigenvalues, eigenvectors, and rank, plus dot product, cross product, magnitude, and a unit vector. A singular matrix or a shape that does not match returns an error.

### Calculus and numerical methods

Derivatives, second derivatives, partial derivatives, integrals, limits (two-sided, left, and right), Taylor and Maclaurin series, critical points, and gradients are symbolic. When a definite integral has no closed form, `calculator_numerical_integral` uses the quadrature you name: Simpson’s rule by default, or the trapezoidal rule, with interval count `n`. A numerical derivative is a central difference.

Newton–Raphson iterates `x ← x − f(x) / f'(x)`, with an analytic derivative when you supply one, and stops on a non-finite value or at the iteration cap. Linear regression fits `y = a + bx` and needs at least two points with `x` not constant. Quadratic regression fits `y = ax² + bx + c`, needs at least three points, and refuses a singular design.

### Units, physics, and electrical

Conversions cover length, weight, area, volume, temperature, time, speed, pressure, energy, and power. The reply names the units it used. Kinematics uses `v = d / t`, `d = vt`, and `a = (v − u) / t`. A zero time is rejected. Newton’s second law is `F = ma`. Work is `W = F d cos θ` with `θ` in degrees and a default of 0. Power is `P = W / t`. Kinetic energy is `½mv²`. Gravitational potential is `mgh`. Momentum is `p = mv`. Density is `ρ = m / V`. Waves use `v = fλ`.

Ohm’s law solves `V = IR` for the one missing quantity. Electrical power uses `P = VI`, `P = I²R`, or `P = V²/R`. Resistor colour codes, series and parallel resistance, capacitors, inductors, a voltage divider, a current divider, and AC reactance are the same pattern: the formula, the SI units, and an error when an input is illegal.

### Finance

Simple interest is `SI = (P × R × T) / 100`. Compound amount is `A = P (1 + r/n)^(nt)`, with the compounding frequency mapped from annual through daily. SIP future value is

```text
FV = M × [((1 + i)^n − 1) / i] × (1 + i)
```

CAGR is `(Final / Initial)^(1 / Years) − 1`. GST is either added on top or extracted from a tax-inclusive price, at the rate you send. Discount, profit and loss, and percentage change use the inputs you send. Money is rounded to two decimal places. The result is the formula applied to those inputs. It is not a lender’s offer and it will not match a bank schedule past the 120-row cap.

### Chemistry

Molar mass comes from a formula against a built-in table for atomic numbers 1 through 118. Moles are `n = m / M`. Dilution is `C1 V1 = C2 V2`. pH and pOH use `pH + pOH = 14` and `Kw = 1×10^−14`, which is a dilute aqueous solution near 25 °C. The ideal gas law converts temperature to kelvin and uses the `R` above. Stoichiometry uses the masses and the coefficients you send. Outside those assumptions the number is still the formula, and the reply says which assumption it used.

### Programmer tools and dates

Base conversion covers binary, decimal, hexadecimal, and octal, plus bitwise operations, shifts, ASCII, and data-storage units. Dates cover a difference, an age, a count of days, a time difference, a leap year, and a weekday. A random number from the calculator is a sample for a fixture. It is not a key.

### Prompts

- “Solve `2x² − 5x − 3 = 0` and show the quadratic formula.”
- “What is the EMI on a ₹12,00,000 principal at 8.5% for 20 years?”
- “Differentiate `sin(x²)`, then evaluate the definite integral of `x²` from 0 to 3 with Simpson’s rule.”
- “Convert 25 °C to kelvin and say which scale you used.”
- “pH of a solution with `[H+] = 1×10^−3`, and state the 25 °C assumption.”

## Tools

Each domain is a set of named tools. Call the one whose formula matches the question.

| Domain | What you send | What comes back | Do not use it when |
|---|---|---|---|
| Basic and scientific | An expression or the operands, and `angle_unit` for trigonometry | The value and the formula. Illegal factorial, modulo, and reciprocal inputs error | The question is an invoice GST split on a dated rate table. That is the business calculator |
| Geometry | Lengths of a triangle, circle, rectangle, solid, or polygon | Area, surface area, or volume, with the formula | A length is missing. The tool does not assume one |
| Algebra | Coefficients or an expression, and the variable | Roots, a simplified or factored form, or GCD, LCM, and factors | The leading coefficient is 0, or the equation is an identity. Those are rejected |
| Statistics | The observations, and sample or population for variance | Mean, spread, a percentile, or a binomial or normal probability | You want a forecast from a sales history. That is demand forecasting |
| Calculus | An expression, the variable, and limits or `n` for a numerical integral | A derivative, an integral, a limit, a series, or a Simpson or trapezoidal value | A definite integral has no closed form and you have not asked for a numerical rule |
| Matrix and vector | The entries | Inverse, determinant, eigenvalues, rank, or a product. A singular or mismatched matrix errors | You need a production plan under constraints. That is the optimization solver |
| Units | A value and the two units | The converted value and the units used | The unit alias is ambiguous and you have not said which one |
| Physics and electrical | Mass, length, time, volts, ohms, or the one missing Ohm quantity | The SI result and the formula. Zero time errors | You want a route or a fleet plan |
| Finance | Principal, rate, time or months, and the GST rate when tax is the question | Interest, EMI, SIP, CAGR, or GST to two decimal places, with the formula. Schedules stop at 120 rows | You want a lender’s contract, a tax opinion, or a dated statutory GST table |
| Chemistry | A formula, masses, or a concentration | Molar mass, moles, pH, or a gas-law result, with the stated assumption | The solution is not the dilute aqueous case near 25 °C and you need a result outside that model |
| Programmer and dates | A number, a base, or two dates | The converted value, the weekday, or the difference | You need a cryptographic key. A calculator random value is not one |

## How to read a result

- **The formula is part of the answer.** Check the number against the expression in the reply.
- **Units are the ones you named.** Degrees and radians are not mixed. Celsius is converted only when the tool says it converted to kelvin.
- **An error is the result when the input is illegal.** A zero denominator, a zero time, a negative factorial, a singular matrix, or a singular regression comes back as an error.
- **Finance is rounded to two decimals.** That rounding, and the 120-row schedule cap, will differ from a bank’s full amortisation.
- **Chemistry states its model.** Ideal gas, and pH for a dilute aqueous solution near 25 °C, are the models those tools implement.
- **The server does not grade anyone.** The formula is the whole of the reply that matters.

## Data and what this will not do

You send numbers, expressions, and matrices. The server processes the request and does not store it. Each user signs in with OAuth. There is no learner profile and no uploaded corpus. Student questions are often from minors. The calculation is not a record about the student.

The server will not invent a missing input, will not silently approximate a non-elementary integral, and will not present an EMI as a loan offer. It will not apply a GST rate you did not send. Dated statutory tables stay on the business calculator.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

List prices live on the [SAIFS AI](https://saifs.ai) catalog. Depth splits like this:

| Tier | What it covers |
|---|---|
| **Free** | Basic arithmetic, percentage, fractions, ratio, average, and unit conversion |
| **Pro** | Scientific expressions, algebra, statistics, geometry, and date and time |
| **Business** | Calculus, matrices, physics, electrical, chemistry, finance (EMI, SIP, GST), regression, and Newton–Raphson |
| **Platform** | A white-label embed for a tutoring or finance product |

The free tier is enough to check arithmetic and a unit conversion, with no sales call.

## Support

- Install and client setup: [server README](../README.md)
- Invoice GST, margin, and dated tax tables: [Business Calculator: GST, Margin & Everyday Formulas](../../business-calculator/README.md)
- A Python program instead of a named formula: [Mathematical Code Execution](../../code-execution/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
