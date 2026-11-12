# Custom in-game overlays (issue #50)

Allow a player to choose an image to decorate the unused area around the game,
particularly a 4:3 game on a widescreen display. Implement this in RecompOne's
output panel, which owns the game's presentation, rather than changing game
rendering or intercepting it in SymphonyRecomp.

The Display settings gain an image picker, an enable switch, a reload button,
and a clear button. Selecting an image enables it immediately. The selected
path and enabled state use the existing settings system. PNG (including alpha)
and JPEG are supported. The artwork is centered, preserves its aspect ratio,
and is drawn behind the game inside the output panel. It follows resizing,
docking, and fullscreen automatically. Gameplay, menus, trackers, and toasts
remain above it; game aspect ratio and rendering resolution are unchanged.

Load and upload once per selection, including caching failures. Reload retries
an edited or previously missing file. Replacing, disabling, clearing, or closing
releases its GPU texture. Missing, unreadable, and invalid images leave the game
usable and show an error in Display settings.

Alternatives considered: a floating ImGui overlay cannot reliably follow the
output panel; drawing over the game can hide gameplay; hooking internal engine
methods from the game adds unnecessary coupling. A background in the output
panel is the smallest integration with the existing renderer.

Scope: local implementation and tests. No upstream PR: both repositories
explicitly decline AI-generated contributions. Full game validation requires
the legally owned disc and generated game code; runtime tests must not require
either.
