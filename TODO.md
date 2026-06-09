# TODO

##Features
- [ ] Estimated FIle Size for Export
- [ ] Cursor movement path editing
- [ ] Check the current version and look for updates
- [ ] Separate webcam track with visibility and position controls
- [ ] [WQHD/High-DPI compatibility and Zoom Algorithm Refinements](https://github.com/siddharthvaddem/openscreen/pull/372)
- [ ] [Add post-processing noise reduction with RNNoise](https://github.com/siddharthvaddem/openscreen/pull/374)
- [ ] [Add editor autosave recovery](https://github.com/siddharthvaddem/openscreen/pull/538)
- [ ] 


## Performance improvements
- [ ] [zero-copy hardware-accelerated hybrid FFmpeg export pipeline](https://github.com/siddharthvaddem/openscreen/pull/443)
- [ ] [Native C++ export engine intended to replace the current WebCodecs-based pipeline as the primary export path](https://github.com/siddharthvaddem/openscreen/pull/678). CapCut export optimisation strategies: full-stack hardware acceleration, background pre-render cache, and on-demand trim-aware decode.
  - [ ]  WebCodecs is single-threaded serial loop, no GPU zero-copy, opaque HW encoder selection, real-time audio bottleneck
  - [ ] Decode → GPU composite → HW encode → mux pipeline
  - [ ] Per-platform priority table covering VideoToolbox (macOS), NVENC / AMF / Quick Sync (Windows), VAAPI (Linux), and a libx264 software fallback.
  - [ ] finalizeNativeWindowsRecording in src/hooks/useScreenRecorder.ts reads the entire on-disk screen recording back into renderer memory via window.electronAPI.readBinaryFile(nativeScreenPath) and ships those bytes to the main process via storeRecordedSession. 
- [ ] [stream native-capture webcam to disk to prevent OOM crash on stop](https://github.com/siddharthvaddem/openscreen/pull/687). Add a dedicated main-process IPC handler (e.g. attach-webcam-to-screen-recording) that accepts:
  - [ ] The native screen file path (already on disk)
  - [ ] The webcam sidecar file name / whether it is streamed
  - [ ] A small duration-fixed webcam blob buffer (only when not streamed)
  - [ ] The main process reads, patches, and merges the files itself — the renderer never marshals the multi-GB screen bytes over IPC.
  - [ ] readBinaryFile(nativeScreenPath) is removed from the renderer finalize path
  - [ ] A main-process handler owns the screen + webcam disk merge/patch
  - [ ] Renderer only marshals small in-memory webcam data (if any)
