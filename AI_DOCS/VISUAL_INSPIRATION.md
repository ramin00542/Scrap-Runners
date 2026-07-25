# Visual Inspiration Log

Reference-only notes for future art/presentation ideas. Entries here
carry **no implementation commitment** and do not change architecture,
tasks, or ADRs unless a separate decision is explicitly accepted.

---

## 3D Pixel Art Style (t3ssel8r-style, Dylearn video)
- Style: real 3D low-poly models + custom shader for a pixelized
  look + hybrid toon shading + a grass shader that reacts to
  character movement
- Status: reviewed and rejected for MVP — the build effort (real 3D
  modeling + writing custom shaders) is close to the effort of a full
  3D pivot, not the simplicity of 2D pixel-art
- Extractable idea for later (post-MVP, simplified 2D version):
  a small visual reaction in the environment (e.g. grass/dirt)
  when the character walks over it, done with a simple
  AnimationPlayer on a Polygon2D instead of a 3D shader
- This entry is for future reference only and carries no
  implementation commitment.
