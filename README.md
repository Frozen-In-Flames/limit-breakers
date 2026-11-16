# Limit Breakers

## Abstract

This repository documents a systematic programme of applied research in
sandboxed AI development environments. Each entry isolates a reproducible
technical limitation encountered in such an environment, establishes its
root cause empirically, and publishes the corrective implementation together
with the evidence by which the correction was verified. The programme's
premise is methodological: a limitation is a defect report against the
working environment, and the correct response to a defect report is an
engineered, tested, and documented resolution.

## Method

Each investigation proceeds in five stages.

1. **Isolation and reproduction.** The limitation is reduced to the smallest
   case that exhibits it reliably.
2. **Root-cause analysis.** The mechanism is identified at the level of the
   underlying system: the system call, the file attribute, the missing
   interface, with the evidence recorded.
3. **Corrective implementation.** A tool is constructed that resolves the
   limitation for the task it was blocking, using the minimum mechanism
   sufficient to the cause.
4. **Verification.** The implementation is tested against the original
   failing case and against negative controls, so that a passing result
   demonstrates the correction and not merely the execution of code.
5. **Documentation.** Problem, cause, implementation, and verification are
   published in full, in this repository, before the result is claimed.

## Scope and attribution

The tools in this programme operate exclusively on the author's own working
environment, machines, and accounts. They are constructed without harmful
or malicious intent, and they neither access nor modify any third-party
system. The programme is conducted by Aries Hilton (Frozen In Flames), to
whom the work is attributed.

## Index of investigations

| No. | Title | Domain | Status | Location |
|-----|-------|--------|--------|----------|
| 001 | Auditory perception for language models via deterministic signal analysis | Digital signal processing; multimodal representation | Complete; released as v0.1.0 | [instances/001](instances/001-auditory-perception-via-signal-analysis/) and [fif-ears](https://github.com/Frozen-In-Flames/fif-ears) |
| 002 | Nix store ingestion under mandatory extended-attribute enforcement | Operating systems; sandboxing; declarative configuration | Complete; in continuous use | [instances/002](instances/002-nix-store-ingestion-under-xattr-enforcement/) and [frozen-in-flames-os](https://github.com/Frozen-In-Flames/frozen-in-flames-os) |

New investigations are added to this repository as they are completed, each
under `instances/` with its documentation and, where the corrective
implementation is self-contained, its source code.
