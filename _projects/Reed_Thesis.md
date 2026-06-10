---
title: Reed Thesis - Mesh Simplification with the Gauss-Bonnet Theorem
# excerpt: "During my senior year at [Reed College](https://www.reed.edu), I researched, wrote, and defended a 40+ page Mathematics thesis, titled \"Keeping the Curves: Guiding Mesh Simplification with the Gauss-Bonnet Theorem.\""
featured: true
show_overlay_excerpt: false
header:
  teaser: /assets/images/odb.jpg
  overlay_image: /assets/images/bunny_iters.png
  overlay_filter: rgba(0, 125, 0, 0.75)
  actions:
    - label: "Read Here!"
      url: /assets/documents/ZaayerFinalThesis.pdf
---

<!--more-->

During my senior year at [Reed College](https://www.reed.edu), I researched, wrote, and defended a 40+ page Mathematics thesis, titled "Keeping the Curves: Guiding Mesh Simplification with the Gauss-Bonnet Theorem." During this process I was advised by [Professor Kyle Ormsby](https://kyleormsby.github.io).

## Abstract
Meshes representing 3-dimensional objects are used extensively in numerous areas of
industry and study. In particular, a mesh may be used as an approximation for a
real-life object, which is then used in practical simulations (for example, a mesh of a
car being used for aerodynamic analysis). While higher vertex-count meshes better
approximate the true results for a smooth surface, they also incur heavy computational
cost. One solution is to produce a “coarsened” or “simplified” mesh that has a lower
vertex count while maintaining similarity to its original counterpart. Coarsening
schemes like Garland and Heckbert’s Surface Simplification Using Quadric Error
Metrics [GH97] provide decent approximations of an input mesh, but leave room for
improvement in preserving key mesh features. Alternatively, intrinsic quantities like
discrete Gaussian curvature can be defined and leveraged alongside the discrete Gauss–
Bonnet Theorem to better maintain similarity between coarsened iterations of a mesh.
In particular, mesh simplification using Intrinsic Curvature Error [Liu+23] utilizes
these quantities to determine how much the removal of any vertex contributes to the
difference between a fine and coarsened mesh. When comparing the two coarsening
methods, Intrinsic Curvature Error yields a substantial increase in accuracy over
Quadric Error Metrics while maintaining competitive runtimes.