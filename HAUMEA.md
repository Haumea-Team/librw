# librw for Haumea Studio

This is [aap/librw](https://github.com/aap/librw) with the changes Haumea Studio
needs. The `haumea` branch holds them as separate commits on top of upstream, and
Haumea pins it as the `librw` submodule. `master` stays equal to upstream.

What the branch adds:

- Skeleton and ImGui backend fixes: real window size, idle frame throttling,
  high-DPI scaling, clipboard, modifier keys, more keys, and the
  `requestedMultiSamplingLevels` setting.
- GL3 and D3D9 fixes, and failing cleanly instead of crashing on bad textures and
  failed conversions.
- Dear ImGui `v1.92.2b-docking` (tag commit
  `1f7f1f54af38b0350d8c0008b096a9af6de299c7`) in `skeleton/imgui`, the docking
  build of the release upstream bundles, so Haumea's panels can be docked.
  `skeleton/imgui/backends` holds the Win32 and DirectX 9 backends from the same
  tag for the Windows D3D9 build. They have one change, marked "Haumea" in
  `imgui_impl_dx9.cpp/.h`: `ImGui_ImplDX9_SetTextureResolver`, so the editor can
  pass its `rw::Texture` pointers as ImTextureIDs.
- macOS platforms and ARM64 builds in `premake5.lua`.

## Taking upstream changes

```sh
git remote add upstream https://github.com/aap/librw.git
git fetch upstream
git checkout master && git merge --ff-only upstream/master && git push
git checkout haumea && git merge master   # or rebase, then push
```

Then update the submodule in Haumea (`git -C librw pull` and commit the new
pointer).

To update Dear ImGui, replace the files in `skeleton/imgui` with those from the
`-docking` tag matching the version upstream ships, and carry over the "Haumea"
change in the DX9 backend.
