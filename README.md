# AVS3-P3 Audio Vivid reference codec

AVS3-P3 (Audio Vivid) reference codec sources (AVS3A_RMv3.3.1 lineage),
with decoder fixes.

## Layout

- `av3adecoder/` — reference decoder + encoder (CLI usage: its `ReadMe.txt`;
  the decoder library is what FFmpeg wraps)
- `av3a_binaural_render/` — binaural renderer (linked into the FFmpeg wrapper)
- `ffmpeg/` — upstream FFmpeg-6.1 integration tree (historical, unused by our
  builds)
- `AudioFilter/` — effects SDK (historical, unused)

## Decoder fixes

- debug prints silenced on the library path (SDK left printf-based logging
  enabled, polluting stdout of every CLI consumer)

## Licensing

Research/educational use only; do not redistribute binaries.
