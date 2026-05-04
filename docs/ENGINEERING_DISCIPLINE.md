# Engineering discipline

This document records project-level expectations for reviewable changes,
controlled workflow boundaries, and evidence-backed release language.

## 1. Maintainer-owned decisions

Architecture, release decisions, security posture, and hardware-sensitive
verification are maintainer-owned.

Tasks that introduce a new crate, a new public API, a new integration boundary,
or a change to the hardware target require explicit alignment before
implementation begins.

## 2. Boundary first

Define the public interface or workflow boundary before implementation begins.

For non-trivial tasks, state assumptions, surface ambiguities, and confirm scope
before writing code. Shared vocabulary matters. Do not rename core concepts
casually, and use small glossary sections where terminology is ambiguous.

## 3. Tests and feedback loops

Prefer the tightest available feedback loop:

- `cargo check` before `cargo build`
- `pytest -q` before a full test run
- `git diff --check` before staging
- CI green before merging

Large patches should include relevant test coverage, smoke scripts, type
checks, lint runs, or reproducible evidence.

## 4. Deep modules over shallow modules

Public interfaces should be small and stable-looking. Internal complexity
belongs behind clear module boundaries.

Avoid many shallow modules with overlapping responsibilities. A new module or
crate should justify its existence; the default is to extend an existing one
within its documented boundary.

## 5. Review boundaries

Public interfaces, IPC contracts, release notes, and hardware-sensitive
boundaries require careful review. Critical modules require stricter sign-off
before merge.

## 6. Evidence-first releases

No claim is made without evidence:

- No "production-ready" claim without a defined and tested release path.
- No "stable API/ABI" claim without an explicit decision record.
- No KV260 inference claim without a verified end-to-end hardware path.
- No timing-closure claim without timing evidence from the bring-up work.

Early-stage surfaces use explicit markers: "pre-stable", "early-scaffold",
"host-dry-run", and "development preview".

## 7. PCCX-specific boundaries

**pccx-lab is CLI/core first; GUI second.** IDE, editor, external tool, and
launcher integrations sit on the documented CLI/core boundary. There is no
private back channel from any integration layer into pccx-lab internals.

**The FPGA bring-up path is maintainer-owned and evidence-backed.** RTL,
synthesis configuration, bitstream generation, and hardware timing evidence
require explicit review and reproducible artifacts.

**SystemVerilog workflows use controlled interfaces.** The generate, simulate,
evaluate, and refine loop is driven through the pccx-lab CLI boundary and does
not bypass it.

---

*See also*:
[CONTRIBUTING.md](https://github.com/pccxai/.github/blob/main/CONTRIBUTING.md) ·
[pccx-lab CLI/core boundary](https://github.com/pccxai/pccx-lab/blob/main/docs/CLI_CORE_BOUNDARY.md)
