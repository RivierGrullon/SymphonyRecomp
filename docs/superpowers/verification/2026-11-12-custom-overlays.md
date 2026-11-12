# Custom overlay verification

- Branch (parent and engine): `feat/custom-game-overlays`.
- .NET SDK: 10.0.404, installed locally at
  `/var/home/rgrullon/.local/share/symphony-dotnet`.
- `dotnet build RecompOne.sln -c Release --no-restore --nologo` in RecompOne:
  passed, zero warnings and zero errors, including runtime and recompiler.
- `dotnet test RecompOne.sln -c Release --no-restore --nologo` in RecompOne:
  passed, 22 tests, zero skipped and zero failures.
- Localization JSON: valid, no duplicate keys, seven new entries translated
  into all six declared languages.
- `git diff --check`: passed in both repositories.
- Offline bundle restoration: both repositories restored successfully, clean
  working trees, and the engine commit matches the parent's submodule pointer.

Tests exercise PNG alpha and original dimensions, JPEG decoding, caching,
missing/invalid files, explicit retry, upload refusal, replacement, disabling,
clearing and disposal. Rendering tests inspect real ImGui draw commands for
ordering, clipping, centering, wide/tall windows, widescreen games, high DPI,
and deferral of deletion while the current frame still references the texture.
The GPU upload/delete boundary is substituted; decoding and ImGui are real.

The independent code review found no Critical or Important issues. One Minor
finding remains in the existing generic `HostWindow.UploadTexture` helper:
it returns a generated texture ID without checking whether the driver accepted
the allocation. Images exceeding the GPU's maximum texture size can appear
blank instead of showing an error. This is outside the scoped loader change;
the loader handles an explicit upload refusal, which is covered by a test.

Live native file-dialog, fullscreen, and disc-dependent gameplay checks were
not run. The checkout has no legally owned disc or generated game code, so a
full SymphonyRecomp executable was not built. Both repositories explicitly
decline AI-generated PRs. At the user's request, the feature branches are
published to `RivierGrullon/SymphonyRecomp` and `RivierGrullon/RecompOne`, with
the engine submodule URL pointing to the user's fork. No upstream PR was opened.
