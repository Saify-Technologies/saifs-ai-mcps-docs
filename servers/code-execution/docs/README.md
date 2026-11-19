# Mathematical Code Execution

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/code-execution`. It runs Python that prints a result, or writes a one-page PDF, inside a lock. The host does not guess the number, and it does not run another language.

People search for this as a Python sandbox, a code execution MCP, or a way for Cursor, Claude, or ChatGPT to print a calculation. The short install page is the [server README](../README.md). This page is what the sandbox runs, why the model should not do the arithmetic itself, and how to get stdout or a PDF back.

| | |
|---|---|
| **Registry name** | `ai.saifs/code-execution` |
| **Endpoint** | `https://code-execution-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

One tool, `execute_code`. The model writes Python, or you paste Python. The server runs that source in a fresh child process and returns stdout, stderr, and the exit code. A PDF written with the preinstalled `fpdf` library comes back as well: the visible text, the file bytes, and a download link.

The process has no network and no access to your filesystem. Nothing can be installed while it runs. numpy is already there. scipy and pandas are not, on purpose. The allow-list is mathematical Python plus `fpdf`, not a general computer.

A named business formula with a rounding rule belongs on the [Business Calculator](../../business-calculator/README.md). A checkable scientific formula belongs on the [Scientific Calculator](../../scientific-calculator/README.md). This server is for the case where the answer is whatever the Python prints.

There is no separate price table for this server. A connected client can run Python that prints a result, inside the time, size, and memory caps below.

## Why the host needs a runner

Cursor, Claude, ChatGPT, and other MCP clients often want a number, a short calculation, or a PDF. Left alone, the model guesses, or it reaches for its own shell, a code interpreter, or another language. A wrong total, a JavaScript snippet, or a `pip install` on the user’s machine is not an answer.

The host needs one tool that always runs Python, prints the result, and refuses everything else. That means a language check, an allow-list parsed before the process starts, a child process with no network and no user files, and a short-lived way to hand back a PDF. A chat model cannot be that runner. When this MCP is connected, numeric results, printed text, and PDFs come from this process.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Ask for the calculation in ordinary language, or paste Python. The only tool that runs code is `execute_code`.

| Argument | What to send |
|---|---|
| `code` | Python source. It must print the result. Empty source is rejected. |
| `language` | `python`, `py`, `python3`, or blank. Blank counts as Python. |

What comes back is stdout, stderr, the exit code, and, when a PDF was written, the file and a link.

```python
import numpy as np

a = np.array([[1.0, 2.0], [3.0, 4.0]])
b = np.array([[5.0, 6.0], [7.0, 8.0]])
print(a @ b)
```

A syntax error is returned as Python’s own traceback, so you can fix the snippet and call again.

### A PDF

Use preinstalled `fpdf`. Write a file whose name ends in `.pdf`.

```python
from fpdf import FPDF

pdf = FPDF()
pdf.add_page()
pdf.set_font("Helvetica", size=12)
pdf.cell(text="Invoice summary")
pdf.output("invoice.pdf")
```

The server keeps that file only when the name matches `name.pdf`, the size is between 1 byte and 1 MB, and the bytes start with `%PDF`. The reply includes the visible text, the bytes, and a download link at `/files/{id}/{filename}`. The link expires in one hour. At most 50 PDFs are held in memory at once. A process restart clears them. Download the file when you receive the link.

### Prompts

- “Run Python to compute the mean and standard deviation of this list and print the result.”
- “Write a short numpy snippet that multiplies these two matrices and prints the product.”
- “Generate a one-page PDF invoice summary with fpdf and give me the download link.”

## What is allowed

Imports are checked before the process starts. A name that is not on the allow-list never loads.

| May be imported | Role |
|---|---|
| `math`, `cmath`, `decimal`, `fractions`, `statistics` | Exact and floating arithmetic |
| `random` | Draws inside this process only |
| `datetime`, `json`, `re`, `itertools`, `collections` | Small data and text beside the calculation |
| `numpy` | Arrays, already installed. Do not install it |
| `fpdf` | PDFs. The image prefers `fpdf2`. A bundled `fpdf` covers the same `from fpdf import FPDF` import when `fpdf2` is absent |

Other standard-library modules used for maths and small data are on the same allow-list. The list is not the whole standard library. `scipy` and `pandas` are absent. The honest reply to “install pandas” is that nothing can be installed.

These never run, even inside otherwise valid Python:

- `pip`, `conda`, `apt`, `uv pip`, `poetry add`, `ensurepip`
- `eval`, `exec`, `compile`, `open`, `__import__`, `input`
- A subprocess, a network call, or a read of your files

numpy is loaded before the memory limit is applied, so the extension is already mapped. BLAS threads are pinned to one.

## What is refused

Any `language` other than Python is rejected and is not executed. The source is scanned as well. JavaScript, Java, Go, C, C++, PHP, shell, R, and similar markers stop the run. Non-Python is never rewritten into Python. The only reply is:

```text
WARNING: This MCP only supports and generates Python code.
```

The same one-line warning is used when Python merely mentions another language, or when the source contains `=>` or `===`. Those are false stops: the snippet looked like another language, so it was not run. Rephrase it as ordinary Python and call again.

Source larger than 65,536 bytes is rejected. The absolute cap is 200,000 bytes.

## Limits of a run

| Limit | Value |
|---|---|
| Wall clock | 15 seconds by default, never more than 60 |
| Address space | 512 MB |
| File size | 1 MB |
| Open files | Capped where the operating system allows it |
| stdout and stderr | Clipped at 65,536 characters |

A long run can be cut by the timer or by that clip. The lock is the product: Python only, no install, no network, no user filesystem, and no subprocess from user code. Widening the library list is a change to this server, not something a caller can do with `pip`.

## How to read a result

- **stdout is the answer.** If the snippet does not print, the numeric result is not in the reply.
- **stderr and the exit code** are the Python error, when there is one. A syntax error is yours to fix. An import error means the module is not on the allow-list.
- **The Python-only warning** means the source was not executed. It is one line, with no translation into Python.
- **A PDF link** is valid for one hour and lives in memory. Save the file. Do not treat the link as a permanent URL.
- **Pasted source is untrusted input.** The runner is locked. Secrets still do not belong in the snippet, because stdout and the PDF text come back to the chat.

## Data and what this will not do

The server receives the source, runs it, and returns the output. User code is not written to a durable filesystem. PDFs live in process memory until they expire or the process restarts. Each user signs in with OAuth. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies to what you paste stay with you.

This will not run JavaScript or any other language, install a package, open a network connection, read your disk, or act as a general code runner.

Privacy policy: https://saifs.ai/privacy_policy

## Support

- Install and client setup: [server README](../README.md)
- Named business formulas (GST, margin, EOQ, DSO): [Business Calculator](../../business-calculator/README.md)
- Checkable scientific formulas: [Scientific Calculator](../../scientific-calculator/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
