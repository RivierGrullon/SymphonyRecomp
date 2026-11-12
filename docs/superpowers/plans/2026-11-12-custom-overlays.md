# Custom Overlays Implementation Plan

> Execute inline using superpowers:executing-plans and test-driven-development.

**Goal:** Implement SymphonyRecomp issue #50 with persistent custom artwork.

**Architecture:** Extend the RecompOne submodule's output panel and Display
settings. A focused image cache owns decoding and GPU texture lifetime.

**Tech stack:** C#/.NET 10, ImGui.NET, StbImageSharp, existing OpenGL helpers.

**Spec:** ../specs/2026-11-12-custom-overlays-design.md

## Constraints and review focus

- Keep game aspect ratio, rendering, and input behavior intact.
- Default disabled; save path and enable state through existing Options.
- Cache successes and failures; release replaced and disabled textures.
- Catch image errors without interrupting gameplay; permit explicit retry.
- Artwork must stay inside the output panel and behind game/UI at any size.
- Work locally; do not publish AI-generated contributions upstream.

## Task 1: Image loading and lifetime

Files: RecompOne.Runtime/Host/Window/OutputOverlay.cs;
RecompOne.Runtime.Tests/OutputOverlayTests.cs and test project.

- [x] Write tests for real PNG/JPEG decoding and transparency, one upload per
  path, replacement/disabling/disposal, invalid/missing files, and reload.
- [x] Run tests and observe the missing-feature failure.
- [x] Implement OutputOverlay with injected GPU upload/delete operations,
  Get(path, enabled), Clear(), Error, and IDisposable.
- [x] Run the tests successfully.

## Task 2: Presentation and settings

Files: RuntimeOptions.cs, OutputPanel.cs, HostWindow.cs, DisplayPage.cs,
language resources, rendering integration tests.

- [x] Write an ImGui draw-data test showing artwork before gameplay, correct
  aspect/centering/clipping on wide and tall output panels, and unchanged game
  dimensions when the overlay is disabled or cannot load.
- [x] Observe the missing-feature failure, then implement drawing and settings.
- [x] Dispose the overlay before destroying the OpenGL context.
- [x] Verify tests, runtime/recompiler builds, and localization resources.

## Task 3: Delivery

Files: README.md and local patch/bundle artifacts outside the repository.

- [x] Document image selection, persistence, scaling, retry, and clearing.
- [x] Review the complete diff, commit engine changes locally, update the
  parent submodule pointer, and package both local commits for reproducibility.
- [x] Report verification evidence and any disc-dependent checks not run.
