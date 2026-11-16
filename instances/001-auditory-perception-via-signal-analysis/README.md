# 001. Auditory perception for language models via deterministic signal analysis

**Domain:** digital signal processing; multimodal representation
**Status:** complete. Released as fif-ears v0.1.0, 2026-11-14.
**Implementation:** https://github.com/Frozen-In-Flames/fif-ears

## Problem statement

The language model employed in this working environment has no auditory
input channel. It can reason about audio only through secondhand textual
description, and therefore cannot independently verify musical material: a
claim about a recording's tempo, key, or structure could be neither
confirmed nor refuted from the artefact itself.

## Analysis

The deficit is one of perception, not of reasoning. The appropriate
correction is therefore a translation layer: a deterministic analysis
pipeline that converts an audio signal into a structured representation
that a language model can consume and check, with every derived quantity
computed by a stated algorithm and reported with its confidence, including
honest nulls where the evidence does not support a determination.

## Corrective implementation

fif-ears implements the translation in seven layers over Python, NumPy, and
SciPy, with no learned model in the core pipeline: signal statistics;
rhythm (onset-based tempo estimation); structural segmentation by a novelty
function over the signal's own features; harmony (fundamental-frequency
estimation by the YIN algorithm, key estimation by Krumhansl-Schmuckler
profile correlation, chord estimation with margin-based confidence);
transcription, where a local speech-recognition engine is present; acoustic
event description; and an expression layer (dynamics contour, microtiming,
timbral proxies, harmonic tension). Output is a machine-readable report
(JSON) with a human-readable rendering (Markdown). Unimplemented
translations are recorded as open methods, never as impossibilities, and
the system fabricates no output: where a layer cannot determine a value, it
reports that it cannot.

## Verification

The test suite (15 tests) asserts numeric results on synthetic signals of
known properties and includes mutation controls: sign-flipped and reversed
inputs must change the corresponding measurements, ensuring the suite tests
the physics and not merely the execution. On an independently produced
recording of 91.7 seconds, the pipeline reported a tempo of 151.74 BPM and
a key of D minor at 0.866 profile correlation, and its first internal
structural boundary (2.66 s) agreed exactly with an independent
implementation of the same segmentation algorithm in a separate project
(Reactor), providing cross-implementation corroboration. A live in-browser
demonstration, running the identical package under Pyodide, is available
from the implementation repository.
