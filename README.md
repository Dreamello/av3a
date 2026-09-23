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
- float synthesis output API (Avs3SynthOutputFloat / Avs3DecodeFloat /
  avs3_decode_float): the SDK's only native output saturated its internal
  float synthesis to s16 at unity gain, itself a clipping stage — hot
  broadcast masters legitimately reconstruct above full scale; float output
  preserves the excursions and defers range decisions downstream
- range decoder guards structurally impossible escape sequences (>= 32-bit
  shift = C undefined behavior on corrupt bitstreams) by neutralizing the
  latent instead
- implausible-latent clamp: |latent| > 2^20 is zeroed after range decoding
  (legitimate LC latents are structurally bounded near 2^15 by unnormalized
  int16 PCM through a unity-gain MDCT with attenuate-only feature scaling;
  the one observed corruption event decoded 7,930,568 and synthesized a
  +34 dBFS tonal blast). Trips log to stderr and a durable file
  ($AV3A_LATENT_LOG, default ~/Library/Logs/av3a-latent-clamp.log)
