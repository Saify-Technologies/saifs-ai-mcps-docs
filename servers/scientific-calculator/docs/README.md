```
# Scientific Calculator — Exact Arithmetic & Domain Formulas MCP
*Shipped MCP · Host: scientific-calculator-mcp.saifs.ai · Phase: live tool
server · Pairs with Saifs AI OAuth + GA4*
```

```
*Classical models: school arithmetic · quadratic / polynomial roots · SymPy
symbolic calculus · Newton’s laws & Ohm’s law · annuity EMI & compound interest
· ideal gas law & pH · matrix eigen-decomposition · Newton–Raphson · Simpson’s
rule*
```

# `## Problem statement` 

```
Chat assistants, tutoring apps, finance sites, and engineering tools ask an LLM
to “just calculate it.” Language models are unreliable at arithmetic, algebra,
unit conversion, and exam formulas: they drop a sign, mix degrees with radians,
invent a GST split, or quote a derivative they did not compute. The correct
methods are old and public — quadratic formula, F = ma, EMI = P r (1+r)^n /
((1+r)^n − 1), pH = −log10[H+], Simpson’s rule — but wiring every one of them,
with units, edge cases, and a formula the user can check, is a large surface. An
LLM cannot be the calculator. It can only call one.
```

# `## Solution` 

```
A stateless scientific-calculator MCP. Each call is one named tool, validated
inputs, a numeric or symbolic result, and the formula or method that produced
it. The model must not do the maths itself.
```

```
*Tool set (v1)* — 169 handlers in saifsai_server_python/tools, registered as MCP
tools in saifs_ai_mcp.py:
- Basic — calculator_basic, calculator_percentage, calculator_fraction,
calculator_mixed_fraction, calculator_ratio, calculator_proportion,
calculator_average
```

```
- Scientific — calculator_scientific(expression), trigonometry, logarithm,
exponential, power, root, factorial, absolute value, modulo, reciprocal
- Geometry — triangle, right triangle, circle, rectangle, square, trapezoid,
parallelogram, polygon, 3D solids, surface area, volume
- Algebra — linear, quadratic, cubic, polynomial, system of equations,
inequality, simplify, factor, expand, GCD, LCM, prime check, prime factors
- Statistics — mean, median, mode, range, variance, standard deviation,
percentile, quartile, weighted average, combinations, permutations, probability,
binomial, normal
- Calculus — derivative, second derivative, partial derivative, integral,
numerical integral, limit, Taylor series, critical points, gradient, numerical
derivative
- Matrix & vector — add, multiply, inverse, determinant, transpose, eigenvalues,
eigenvectors, rank, dot, cross, magnitude, unit vector
- Units — length, weight, area, volume, temperature, time, speed, pressure,
energy, power
- Physics — speed, distance, acceleration, force, work, power, kinetic energy,
potential energy, momentum, density, Ohm’s law, electrical power, wavelength,
frequency, pressure
- Finance (India-shaped) — simple interest, compound interest, EMI, loan
schedule, investment, SIP, GST, discount, profit/loss, CAGR, percentage change
- Electrical engineering — Ohm’s law, power, resistor, colour code,
series/parallel resistance, capacitor, inductor, voltage divider, current
divider, AC, reactance
- Programmer — binary, decimal, hex, octal, bitwise, shifts, base convert,
ASCII, data-storage units
```

```
- Date & time — date difference, age, days, time difference, leap year, weekday
- Chemistry — molar mass, moles, molarity, dilution, pH, concentration, gas law,
stoichiometry, periodic table (Z = 1–118)
- Advanced — complex numbers, Newton–Raphson, root finding, interpolation,
linear/quadratic regression, scientific and mathematical constants, random
numbers
```

```
Symbolic work (equations, calculus, matrices) runs through *SymPy 1.13*. Closed-
form domain tools (finance, physics, chemistry, units) use explicit formulas in
```

```
the handler and return that formula with the number.
```

# `## THE ALGORITHMS (implementation guide) ### Arithmetic and scientific expressions` 

```
calculator_scientific parses an expression and evaluates it with SymPy, so
operator precedence and functions are the library’s, not the model’s. Trig tools
take an explicit angle_unit (degrees or radians). Logs take base; roots take
index. Factorial, modulo, and reciprocal reject the usual illegal inputs
(negative factorial, divide by zero).
```

# `### Algebra (closed form via SymPy)` 

```
Linear: solve ax + b = 0 in the chosen variable. Quadratic: ax² + bx + c = 0
with a ≠ 0; identity and non-quadratic equations are rejected. Cubic and general
polynomials use SymPy solve / Poly. Systems, inequalities, simplify, factor, and
expand are the same engine. GCD/LCM and prime factorisation are integer
algorithms on the supplied numbers.
```

# `### Calculus` 

```
Symbolic derivative, second derivative, partials, indefinite/definite integrals,
limits (two-sided, left, right), Taylor/Maclaurin series, critical points, and
gradient are SymPy diff, integrate, limit, series, and solve. When a definite
integral has no closed form, calculator_numerical_integral falls back to a
quadrature the caller names: *Simpson’s rule* (default) or the *trapezoidal
rule, with interval count n. Numerical derivative is a **central difference*.
```

# `### Statistics` 

```
Sample vs population variance and standard deviation are selected by
variance_type / std_type. Percentiles use nearest-rank. Combinations and
permutations are n choose r and nPr. Binomial probability is the PMF at k (or
the mode the tool supports). Normal distribution uses the PDF and Φ via
math.erf: cdf = 0.5 (1 + erf(z)).
```

# `### Linear algebra` 

```
Matrices and vectors are SymPy Matrix operations: addition, multiplication,
inverse, determinant, transpose, eigenvalues, eigenvectors, rank, dot product,
cross product, magnitude, unit vector. Singular or shape-mismatched inputs
return an error instead of a guessed matrix.
```

# `### Physics and electrical (SI defaults)` 

```
Kinematics: v = d/t, d = vt, a = (v − u)/t. Newton’s second law: F = ma. Work: W
= F d cos θ (θ in degrees, default 0). Power: P = W/t. Kinetic energy: ½mv².
Gravitational potential: mgh with g = 9.80665 unless overridden. Momentum p =
mv. Density ρ = m/V. Ohm’s law solves V = IR for the one missing quantity.
Electrical power uses P = VI, P = I²R, or P = V²/R. Waves: v = fλ.
```

# `### Finance` 

```
Simple interest: SI = (P × R × T) / 100. Compound amount: A = P(1 + r/n)^(nt)
with frequency mapped to periods per year (annual through daily). EMI: P r
(1+r)^n / ((1+r)^n − 1), and P/n when the monthly rate is 0. SIP future value:
FV = M × [((1+i)^n − 1) / i] × (1+i). CAGR: (Final/Initial)^(1/Years) − 1. GST
is add-on or extract-from-inclusive. Money results are rounded to 2 decimal
places. Loan amortization is capped at 120 schedule rows.
```

# `### Chemistry` 

```
Molar mass from a formula against a built-in IUPAC-style table (Z = 1–118).
Moles n = m/M. Molarity and dilution C1 V1 = C2 V2. pH / pOH with pH + pOH = 14
and Kw = 1e−14, which assumes a dilute aqueous solution near 25 °C. Ideal gas
law uses R = 0.082057 L·atm/(mol·K) with temperature converted to kelvin
internally.
```

# `### Numerical methods` 

```
Newton–Raphson iterates x ← x − f(x)/f'(x) with an optional analytic derivative,
and stops on non-finite values or an iteration cap. Linear regression fits y = a
+ bx (needs ≥ 2 points, non-constant x). Quadratic regression fits y = ax² + bx
```

```
+ c (needs ≥ 3 points) and refuses a singular design.
```

```
*Numerical/data notes:* tools are stateless — no per-user memory of past
calculations. SymPy is the symbolic dependency (requirements.txt). Constants (g,
R, thermochemical calorie 4.184 J, mechanical horsepower ≈ 745.7 W, Julian year
365.25 d) are fixed in the handlers so two calls with the same inputs match.
```

# `## Pressure test` 

```
G1 — exactness: a stateless model cannot be trusted to expand a polynomial,
invert a matrix, or apply Simpson’s rule the same way twice. Native tool calls
with the formula in the payload are the continuity the model does not have. Safe
as a calculator: it does not store a learner corpus and it does not compete by
accumulating private data. G2 — moat: the moat is coverage and checked edge
cases (degree vs radian, a ≠ 0, zero time, 0% EMI, GST inclusive, singular
regression), not a data flywheel. Explained formulas are the trust layer. G3 —
buyer: students, tutors, finance content, and engineering assistants in one
server; broad rather than one vertical. G4 — compounding: weak. Each call is
independent, so accuracy does not improve from usage the way a fitted memory
model does. The asset is the tool surface staying correct. *No numeric score is
assigned here* — unlike a pre-scored product brief, this file describes the
server that is already in the repo.
```

# `## Audience & user profiles` 

```
Students and tutors (CBSE / JEE-style maths, physics, chemistry), personal-
finance content (EMI, SIP, GST), electrical and programmer utilities, and any
assistant that must show a checkable formula.
```

```
1. *Tutoring / homework assistant* — calls the matching tool and shows the
formula with the result, instead of mental arithmetic.
```

```
2. *Personal finance page or bot* — EMI, SIP, GST, and CAGR with rupee rounding
and the annuity formula visible.
```

```
3. *Engineering sidekick* — Ohm’s law, dividers, unit conversion, matrices, and
numerical roots without a second CAS install on the user’s machine.
```

# `## Where/how to get data & components` 

```
| Need | Source | Access |
```

```
|---|---|---|
```

```
| Numbers, expressions, matrices, formulas | The user message or the host app |
tool arguments |
```

```
| Symbolic engine | SymPy 1.13 | requirements.txt |
```

```
| Element masses, physical constants | Tables inside the chemistry, physics, and
constants handlers | in-repo |
| Identity of the caller | Saifs OAuth (multiplewords.in) via AuthMiddleware |
bearer token |
```

```
| Usage events | GA4 Measurement Protocol (analytics.py) | GA4_MEASUREMENT_ID +
GA4_API_SECRET |
```

```
No review log, item bank, or uploaded corpus. If an input is missing, the tool
errors; it does not invent a value.
```

# `## Monetization` 

```
Capability naturally splits by depth. List prices are not stored in this
repository; they belong on the Saifs catalog.
```

```
- *Free*: basic arithmetic, percentage, fractions, ratio, average, unit
conversion
```

```
- *Pro*: scientific expression, algebra, statistics, geometry, date/time
- *Business*: calculus, matrices, physics, electrical, chemistry, finance (EMI,
SIP, GST), regression and Newton–Raphson, API volume
```

- `*Platform tier*: white-label embed for a tutoring or finance product that does not want to maintain a CAS` 

- `Positioning: “A checked calculator the model must call — formula included, so the answer can be audited.”` 

```
## India sole-prop notes
```

```
GST add/remove, SIP, and EMI are the home wedge: Indian retail finance and
school maths on one MCP, with INR-style 2-decimal money. Auth and token checks
go through https://multiplewords.in; the public host is https://scientific-
calculator-mcp.saifs.ai. Export of the same API to a foreign tutor product is
ordinary service invoicing. Student questions often come from minors — the
server does not build a learner profile, but call logs and GA4 events can still
carry an authenticated user id; keep analytics coarse and do not retain raw
homework text beyond what the host already stores. Tool copy should state the
formula, not grade the person.
```

# `## Risks` 

```
Models skipping the tool and answering from memory (instructions say to use only
these tools; hosts still have to enforce that). SymPy edge cases: non-elementary
integrals, branch cuts, and equations that are identities rather than solutions
— handlers should error, not approximate silently. Unit aliases (cm vs m,
Celsius vs Kelvin) and the chemistry assumptions (ideal gas, pH at ~25 °C) will
be wrong if the caller ignores the stated units. Finance rounding to 2 decimals
and the 120-row loan cap will not match a bank’s full schedule. Do not claim
“every formula in science”; ship the named tool list and the formula string each
handler already returns.
```