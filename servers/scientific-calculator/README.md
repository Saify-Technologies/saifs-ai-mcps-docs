# Scientific Calculator

A checked calculator the model must call instead of doing the maths itself. Each tool takes validated inputs and returns a numeric or symbolic result plus the formula or method that produced it — school arithmetic, algebra, calculus, matrices, units, physics, chemistry, electrical engineering, programmer conversions, and India-shaped finance (EMI, SIP, GST).

| | |
|---|---|
| **Registry name** | `ai.saifs/scientific-calculator` |
| **Endpoint** | `https://scientific-calculator-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-scientific-calculator": {
      "type": "http",
      "url": "https://scientific-calculator-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://scientific-calculator-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-scientific-calculator https://scientific-calculator-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://scientific-calculator-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

Named handlers cover these domains (symbolic work via SymPy; finance, physics and chemistry return the closed-form formula with the number):

| Domain | What it covers |
|---|---|
| Basic & scientific | Arithmetic, percentage, fractions, ratios, averages, expressions, trig, logs, roots, factorial |
| Geometry | 2D shapes, 3D solids, surface area and volume |
| Algebra | Linear, quadratic, cubic and polynomial roots, systems, inequalities, factor, expand, GCD/LCM |
| Statistics | Mean, variance, percentiles, combinations, binomial and normal probability |
| Calculus | Derivatives, integrals, limits, Taylor series, critical points, Simpson / trapezoidal quadrature |
| Matrix & vector | Inverse, determinant, eigenvalues, rank, dot and cross product |
| Units | Length, weight, area, volume, temperature, time, speed, pressure, energy, power |
| Physics & electrical | Kinematics, `F = ma`, Ohm’s law, work, energy, waves, resistors, AC reactance |
| Finance (India-shaped) | Simple / compound interest, EMI, SIP, GST, CAGR, profit/loss |
| Chemistry | Molar mass, moles, molarity, dilution, pH, ideal gas law, periodic table (Z = 1–118) |
| Programmer & dates | Base convert, bitwise, ASCII, date difference, age, leap year |

## Example prompts

- “Solve 2x² − 5x − 3 = 0 and show the quadratic formula.”
- “What is the EMI on a ₹12,00,000 home loan at 8.5% for 20 years?”
- “Differentiate sin(x²) and then evaluate the definite integral of x² from 0 to 3 with Simpson’s rule.”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
