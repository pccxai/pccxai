# AI-assisted engineering discipline

This document defines how PCCX AI uses AI workers in its development workflow.
It is short and practical — operating principles, not a manifesto.

## 1. Human strategy, AI execution

The human maintainer owns system architecture, release decisions, and critical
verification. AI workers implement local changes within agreed boundaries.

AI workers do not expand architecture speculatively. Tasks that would introduce
a new crate, a new public API, or a change to the hardware target require
explicit human alignment before implementation begins.

## 2. Interface first

Agree on the public interface or boundary before implementation begins.

For non-trivial tasks, use a planning step: state assumptions, surface
ambiguities, and confirm scope before writing code. A written spec alone is
not sufficient — intent must be clarified.

Shared vocabulary matters. Do not rename core concepts casually, and prefer
small glossary sections where terminology is ambiguous.

## 3. Tests and feedback loops

Prefer the tightest available feedback loop:

- `cargo check` before `cargo build`
- `pytest -q` before a full test run
- `git diff --check` before staging
- CI green before merging

Do not write large patches without test coverage. Smoke scripts, type checks,
and lint runs count as feedback loops.

## 4. Deep modules over shallow modules

Public interfaces should be small and stable-looking. Internal complexity
belongs behind clear module boundaries.

Avoid many shallow modules with overlapping responsibilities. A new module
or crate should justify its existence — the default is to extend an existing
one within its documented boundary.

## 5. Gray-box delegation

Humans review public interfaces, IPC contracts, and hardware-sensitive
boundaries. AI workers can implement internals behind those boundaries.

For critical or hardware-sensitive modules, stricter human review is required.
Gray-box means: humans own the box edges; AI workers fill the box interior.

## 6. Evidence-first releases

No claim is made without evidence:

- No "production-ready" claim without a defined and tested release path.
- No "stable API/ABI" claim without an explicit decision record.
- No KV260 inference claim without a verified end-to-end hardware path.
- No timing-closure claim without timing evidence from the bring-up work.

Early-stage surfaces use explicit markers: "pre-stable", "early-scaffold",
"host-dry-run", "development preview".

## 7. PCCX-specific boundaries

**pccx-lab is CLI/core first; GUI second.** All IDE, editor, MCP, and AI
worker workflows sit on the documented CLI/core boundary. There is no private
back channel from any integration layer into pccx-lab internals.

**The FPGA bring-up path is human-owned and human-verified.** RTL, synthesis
configuration, bitstream generation, and hardware timing evidence are not
delegated to AI workers.

**AI-assisted workflows use controlled interfaces.** The planned MCP interface
is the entry point for AI-assisted SystemVerilog development workflows. The
evolutionary generate/simulate/evaluate/refine loop is driven through the
pccx-lab CLI boundary; it does not bypass it.

---

*See also*:
[CONTRIBUTING.md](https://github.com/pccxai/.github/blob/main/CONTRIBUTING.md) ·
[pccx-lab CLI/core boundary](https://github.com/pccxai/pccx-lab/blob/main/docs/CLI_CORE_BOUNDARY.md)
