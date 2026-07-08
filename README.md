# TLEGenerationAlgorithm-issue1936

This branch implements **configurable TLE generation algorithms** for TLE propagators, addressing issue #1936.

## Changes

- **`FieldTLEPropagator`** — Added a `generationAlgorithm` field initialized with a default algorithm. Added a public `setTleGenerationAlgorithm()` setter so users can plug in custom algorithms.
- **`TLEPropagator`** — Made the `generationAlgorithm` field non-final, removed the 5-parameter constructor overload that accepted a `TleGenerationAlgorithm`, and added a public setter. The default is set internally via `getDefaultTleGenerationAlgorithm()`.
- **`TLEGradientConverter`** — Removed the `generationAlgorithm` constructor parameter; now retrieves the algorithm from the propagated via `propagator.getTleGenerationAlgorithm()` and applies it to the Field propagator with `setTleGenerationAlgorithm()`.
- **`selectExtrapolator`** — Removed the 6-parameter overload that accepted `TleGenerationAlgorithm`. Callers use the setter on the returned propagator instead.
- **Tests** — Added coverage: `testDeepSDP4FourParamConstructor`, `testSetTleGenerationAlgorithm`, `testConfiguredAlgorithmUsedDeepSpace`, and various SGP4/DeepSDP4 regression tests.

## Why this approach?

Using a setter (non-final field + public setter) instead of constructor injection avoids breaking the existing public API while still allowing users to customize the algorithm.
