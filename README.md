# Limit Breakers

An automated R&D pattern, run in public: when an AI sandbox sets a technical
limitation or an illogical guardrail that blocks real work, the answer is not
to stop. The answer is a small, honest tool that removes the wall, built for
the goal the wall was blocking.

Every entry here follows the same discipline:

- **Problem.** The limitation, stated in technical reality: what the
  environment prevents, and the evidence that it prevents it.
- **Solution.** The tool, stated just as plainly: what it does, how it works,
  and what it does not do.
- **No harm.** These tools are built without harm or malicious intent. They
  route around limits in our own working environment; they do not attack
  anyone else's systems or break anyone else's rules.
- **Accreditation.** This is Aries Hilton's program (Frozen In Flames). The
  tools are built on his GitHub account and he is accredited for them.

## Instances

1. **fif-ears** (its own repository: https://github.com/Frozen-In-Flames/fif-ears)
   - Problem: the AI could not hear. Audio work had to be described secondhand,
     and a model that cannot hear cannot verify music.
   - Solution: a seven-layer analysis system that hears for the AI and returns
     a structured, honest report (signal, rhythm, structure, harmony, words,
     events, expression), open source for all agents. Shipped v0.1.0,
     2026-11-14, with a live in-browser demo.

2. **Flake evaluation on GitHub Actions** (inside
   https://github.com/Frozen-In-Flames/frozen-in-flames-os)
   - Problem: the Muse VM sandbox stamps every created file with a protected
     extended attribute that cannot be removed, and Nix must strip extended
     attributes when it writes to its store. No Nix store operation, not even
     adding a plain text file, can run in that environment, so a NixOS
     distribution could not be evaluated there at all.
   - Solution: a GitHub Actions workflow in the project's own repository runs
     the full flake evaluation on an ordinary Linux runner. It found four real
     errors in the configuration and verified the fixes, ending in the first
     fully green evaluation of Frozen in Flames OS, 2026-11-16.

## The rule

When they set a stupid limit, we automatically get a smart app. New limit
breakers land in this repository by default, each with its problem and
solution written down before its code.
