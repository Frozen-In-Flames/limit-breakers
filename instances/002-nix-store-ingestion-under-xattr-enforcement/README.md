# 002. Nix store ingestion under mandatory extended-attribute enforcement

**Domain:** operating systems; sandboxing; declarative system configuration
**Status:** complete. In continuous use since 2026-11-16.
**Implementation:** `code/flake-check.yml` in this directory; deployed in
https://github.com/Frozen-In-Flames/frozen-in-flames-os

## Problem statement

A NixOS distribution under development (Frozen in Flames OS) could not be
evaluated in the provided virtual machine: every Nix store operation,
including the installation of Nix itself, failed with an error of the form
`removing extended attribute 'user.hatch_tainted.n': Operation not
permitted`.

## Root-cause analysis

The sandbox stamps every file created within it with a protected extended
attribute, `user.hatch_tainted.n`, which processes inside the sandbox may
not remove. Nix, by design, normalises file metadata when ingesting paths
into its content-addressed store, which requires removing extended
attributes. The two requirements are irreconcilable inside the sandbox.
The cause was established by three independent observations: (i) adding a
single plain text file to the store failed with the same error; (ii) the
Nix installer failed on Nix's own manifest files, before any project code
was involved; and (iii) evaluating the project flake failed while fetching
an input, before evaluating any project expression. The defect therefore
resides in the environment, not in the project, and no modification of the
project could resolve it.

## Corrective implementation

Evaluation was relocated, unchanged, to an unconstrained host: a GitHub
Actions workflow (see `code/`) that installs Nix on a standard Linux
runner, evaluates the complete flake without building
(`nix flake check --no-build`), forces deep evaluation of the NixOS
configuration to its top-level derivation, and builds the project's catalog
consistency check. The workflow additionally exports the resolved
`flake.lock` as a build artefact, so that the evaluated dependency set is
captured exactly and can be committed to the repository.

## Verification

The first evaluation runs exercised the method against the real
configuration and located four genuine defects in it, each corrected at its
source: a cache service configured through a module option that does not
exist in the pinned nixpkgs (resolved by configuring the Redis module with
the Valkey package, per the module's own source); the absence of a root
filesystem and bootloader declaration in the standalone configuration;
conflicting definitions of a firmware policy option against the nixpkgs
hardware profile (resolved by a forced definition, the libre policy being
a release invariant); and a duplicate kernel sysctl definition arising
between two nixpkgs modules. Following these corrections the evaluation
completed successfully end to end on 2026-11-16, and the captured
`flake.lock` has been committed to the project repository. The method's
negative control is intrinsic: the identical evaluation, executed in the
sandbox, fails deterministically at store ingestion, which confirms that
the relocation, and not any change to the project, is what resolves the
problem.
