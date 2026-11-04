# CS2 Source Merge Audit

## Scope

Reviewed the three source archives currently stored in this repository:

- `velocity-main.zip`
- `Axion-CS2-RAGE-CHEAT-main.zip`
- `reveriesrc hackvshack.net.zip`

Velocity is treated as the preferred architectural reference, as requested. This document records source-level observations and merge constraints; it does not copy implementation code from the archives.

## Repository comparison

| Project | Structure and feature areas | Build characteristics | Provenance / license |
|---|---|---|---|
| Velocity | Modular `combat`, `esp`, `movement`, `world`, and `changer` areas, with separate core systems, settings, rendering, and menu code | x64 DLL; Development/Ship configurations; ClangCL with LLVM 19; unity build enabled; latest C++ language mode | No root license file found. Its README describes the source as leaked and outdated. Redistribution rights are not established by the archive. |
| Axion | Rage, anti-aim, engine prediction, lag compensation, legit, penetration, skins, movement, and visuals | Visual Studio DLL project; MSVC v142/v143; C++20/latest language modes | Root MIT license is present. Preserve its copyright and license notice for any permitted reuse. Its README describes the project as needing updates and calls out stale schema/signatures and known bugs. |
| Reverie | Anti-aim, ragebot, colored models, events, grenade tools, movement, particles, player, prediction, skins, and world modules | Visual Studio project; v145 toolset; x64 DLL and Win32 application configurations; C++20/23/latest language modes | No root license or README found in the archive. Redistribution rights are not established. |

## Findings

### 1. Velocity is the strongest architectural starting point

Its separation between feature modules, core systems, rendering, settings, and menu code gives the cleanest target structure of the three. The x64-only build and explicit Development/Ship configurations also provide a clearer single-platform baseline.

That does not make its source safe to redistribute: the included README explicitly describes the source as leaked, and the archive contains no license file. The layout can inform an independently written design, but source-level reuse needs permission from the relevant rightsholder.

### 2. Axion is the only project with a clear root license

The MIT license permits reuse subject to retaining its copyright and license notice. That grant does not automatically cover third-party dependencies bundled in the archive; those notices need their own review.

The README itself flags the project as outdated and identifies schema/signature maintenance and functional bugs. It should not be treated as a drop-in upgrade for Velocity.

### 3. Reverie has useful feature breadth but unresolved rights and compatibility

Its feature tree is broader in several areas, including grenade-related modules, events, particles, and player/world systems. However, no root license is present. It also targets a different compiler/toolset and contains a different dependency and SDK layout. No implementation should be imported until its license and dependency provenance are established.

## Recommended integration design

1. Keep Velocity's *module boundaries* as the architectural reference: feature modules, core systems, settings/configuration, rendering, and menu.
2. Do not merge source files from Velocity or Reverie unless written permission or a license covering redistribution is supplied.
3. Treat Axion as a separately licensed candidate only after reviewing each bundled dependency's license and retaining required notices.
4. Compare candidate features by documented behavior and tests, not by copying overlapping implementations. Prefer one canonical implementation per feature and one shared settings/configuration model.
5. Establish explicit interfaces between game-state access, feature logic, rendering, and configuration so that SDK/schema updates do not require edits across every feature.
6. Add compile checks, unit tests for independent math/configuration components, and a repeatable Windows build before claiming the resulting project is integrated or working.

## Current blockers

- Rights to reuse Velocity source are not established.
- Rights to reuse Reverie source are not established.
- Axion's bundled third-party dependency notices have not yet been audited.
- The three projects target different toolsets and use different project layouts.
- No Windows/Visual Studio build or in-game validation has been performed in this environment.

## Next inputs needed for a source-level merge

Provide written permission or a license for Velocity and Reverie that covers modification and redistribution. Once those rights are clear, the projects can be evaluated for source-level integration against the Velocity architecture. Until then, this repository should not republish their unlicensed code.
