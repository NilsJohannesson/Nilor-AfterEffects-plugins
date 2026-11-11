# Nilor After Effects Plugins

Native After Effects plugins for the Nilor pipeline.

| Plugin | Effect | What it does |
|---|---|---|
| `Nilor_Dither.aex` | `Nilor > Dither` | Ordered Bayer dithering (4x4 / 8x8), adjustable levels, animated optional |
| `Nilor_Feedback.aex` | `Nilor > Feedback` | True recursive frame feedback: current frame composited over a decayed, transformed copy of the previous output — the TouchDesigner `feedbackTOP` equivalent |

## Building

- After Effects 27 SDK (expected at `C:\Users\accou\Downloads\AfterEffectsSDK_27.0.0_win\AfterEffectsSDK`, override with the `AE_SDK` env var or the `AeSdkDir` MSBuild property)
- Visual Studio 2022, "Desktop development with C++"

```
msbuild NilorPlugins.sln /p:Configuration=Release /p:Platform=x64
```

Outputs land in `dist/` and (Release) are copied to AE's Plug-ins folder if writable.

## Installing

Copy `dist\Nilor_Dither.aex` and `dist\Nilor_Feedback.aex` into
`C:\Program Files\Adobe\Adobe After Effects <version>\Support Files\Plug-ins\` (admin), restart AE.

## Project layout

```
src/
  common/      shared SDK helpers (params, iterations, PiPL resources)
  dither/      Nilor Dither
  feedback/    Nilor Feedback
```

Built on the Adobe After Effects 27 SDK. `Feedback` uses `PF_OutFlag_NON_PARAM_VARY` +
frame checkout of its own layer at the previous frame; see `src/feedback/README.md` for the
reasoning (AE plugins are stateless per frame; the recursion is explicit and that has rules).
