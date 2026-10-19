# Integration branch: open upstream PRs

`integration/upstream-prs` collects the open pull requests of
[AjncSystem/ShadowOfTheColossus-PC](https://github.com/AjncSystem/ShadowOfTheColossus-PC/pulls)
for testing while they await review. Base: upstream `main` (0.57, 70bb438), then #99 as the spine.

## Merged

| PR | How | Notes |
|---|---|---|
| #99 cp-mt | merge | Spine. Contains #46 #47 #48 #52 #53 #59 #60 #83 #103 #106 (patch-equivalent). |
| #105 #108 #110 #118 #121 | merge | Clean. |
| #54 widescreen | merge | Build-fix conflicts (sirit `<cstdlib>`, `common_init`): kept #99's. |
| #93 docs + AMD cache variant | merge | README/CHANGELOG: kept #99's. Cache folder = `_nodma` + `_neo` + wave64 variant. |
| #32 / #98 AMD | cherry-pick | `276c4eb` AMD periodic submits default, `d79b6fd` allocation failure report, `7200908` + `33f48ed` pipeline cache fixes, `1f98632` census opt-in, `25fa302` flush pipeline cache on close (after the save wait). |
| #81 Intel Arc | cherry-pick | `9420ff6` reader-preferring region lock (deadlock), `8a84696` recorder 2 ms sleep on Windows (kept `SOTC_REC_SPIN_US`), `be29e86` shader tools script. |
| #130 Linux | merge | `build.sh` is LF now (only change); `SOTC_FLUSH_US` default kept at #99's 1000 with GCC casts. |

## Left out

| PR / commit | Why |
|---|---|
| #132 | Older copy of #99; its heap guard commit is already in #99 (0.67). |
| #107 | Backport of #99's 21a2a0e. |
| #94 overlay | Restores the Bloodborne settings menu (FSR/DLSS options) that #60 removed. |
| #32 `6222dd8` async readbacks default | #99 keeps `SOTC_RB_ASYNC` opt-in on purpose (cf74c35). |
| #32 `9563b7d` `a4aed81` arena chunks/groups | Competes with #99's arena slabs + eviction. |
| #32 `9dda2b8` wave64 default on | Contradicts upstream (issue #7: black characters on AMD). Test with `SOTC_WAVE64_UNIFORM=1`. |
| #32 `3394cd0` `4c77d39` hook queue | #99 already has a 4096 ring; the consumer change broke it. |
| #32 stats/stall logging, `15b2bc1`, `56adc03` | Diagnostics only / already covered by #93 and #105. |
| #81 `6fef9af` `8d044d4` Intel async pipelines | Overlaps #99's `SOTC_ASYNC_SHADERS`; needs an Arc card to rework. |
| #69 command processor | Rewrites the same code as #99's draw thread; needs #32 in full. |
| #39 Linux port | Superseded by #130. |

## Test focus

- Shrine FPS (`SOTC_FPS=1`) vs #99 alone.
- Quitting during/after a save (save wait + pipeline cache flush).
- AMD: wave64 on/off, periodic submits. Intel: region lock deadlock fix.
