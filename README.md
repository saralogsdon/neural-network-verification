# Neural Network Verification for NASA-Related Models

This repository is a hands-on study of a simple question:

> Can we prove that a neural network keeps a specific decision or numerical output inside a clearly defined region of possible inputs?

The notebook applies two independent neural-network verifiers—[α,β-CROWN](https://github.com/Verified-Intelligence/alpha-beta-CROWN) and [Marabou](https://github.com/NeuralNetworkVerification/Marabou)—to four NASA or NASA-related case studies. The goal is not to label an entire model “safe.” Each experiment states a narrow property, runs both tools on the same verification boundary, and records what was actually proved.

[![Open the notebook in Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/saralogsdon/neural-network-verification/blob/main/All_NN_Demos.ipynb)
### Paper

📄 [Verification and Validation of AI-Enabled Systems](papers/V_V_Lit_Review.pdf)

## Case studies and results

| Case study | Network verified | Property | α,β-CROWN | Marabou |
|---|---|---|---|---|
| NASA MicroNet | Exact released 512→1000 final linear layer | Every simultaneous feature perturbation with ε = 0.083227 keeps output 20 above output 30 | Verified; certified margin 4.713961 | UNSAT violation query |
| NASA Airfoil Learning | Exact reduced 3-input neural network after fixed geometry is folded into the bias | Three normalized operating inputs vary by ±0.001 while lift coefficient remains within ±0.01 of nominal | Verified; CI range 0.654637–0.655141 | Both band-violation queries UNSAT |
| NASA-IMPACT text classifier | Exact released 768→5 final classification layer | Every feature perturbation with ε = 0.148291 keeps Earth Science above all four alternatives | Verified; minimum margin 2.827524 | 4/4 violation queries UNSAT |
| NASA GIBS anomaly classifier | Complete retrained 256→100→2 ReLU classifier | Every hue-histogram feature vector in the selected ε = 0.002594 box keeps the normal score above the anomaly score | Verified; certified margin 3.831 | UNSAT violation query |

“Verified” and “UNSAT” are two views of the same conclusion. α,β-CROWN proves that a worst-case margin stays positive. Marabou searches for an input that violates the property; `UNSAT` means no such input exists in the allowed region.

## What the results do—and do not—mean

The results are formal and conclusive for the stated models, boundaries, input regions, and output conditions.

They do **not** establish that an entire deployed system is safe. In particular:

- The MicroNet and text-classifier experiments begin at learned feature vectors, so they are not pixel-level or token-level robustness guarantees.
- The MicroNet result is pairwise: output 20 remains above output 30. It does not compare output 20 with the other 998 outputs.
- The Airfoil result uses the exact reduced network for one fixed geometry and one chosen operating point.
- The GIBS neural classifier is retrained from the published NASA architecture and workflow; it is not an official downloadable NASA checkpoint. Feature extraction is outside the verification boundary.
- The original full pixel-level MicroNet query was inconclusive on the tested Colab T4 because of an unsupported MaxPool backward-bound path and GPU-memory limits. That is a tool/hardware limitation, not evidence that the robustness property is false.

## Repository contents

- [`All_NN_Demos.ipynb`](All_NN_Demos.ipynb) — complete Google Colab workflow and evidence generation
- [`paper/formal_verification_nasa_neural_networks_final.pdf`](paper/formal_verification_nasa_neural_networks_final.pdf) — readable technical report
- [`paper/formal_verification_nasa_neural_networks_final.tex`](paper/formal_verification_nasa_neural_networks_final.tex) — LaTeX source
- [`presentation/formal_verification_nasa_neural_networks_final.pptx`](presentation/formal_verification_nasa_neural_networks_final.pptx) — presentation deck with appendix
- [`REPRODUCIBILITY.md`](REPRODUCIBILITY.md) — run notes, environment details, and result-reading guide
- [`CITATION.cff`](CITATION.cff) — citation metadata

## Quick start

1. Open the notebook with the Colab badge above.
2. Select **Runtime → Run all** for a fresh session, or run the numbered sections in order.
3. Keep the Colab tab connected while installations and verifiers run.
4. Read the clean result tables near the end of the notebook; the raw logs are retained as supporting evidence.
5. Download the generated evidence bundle before the temporary Colab runtime disconnects.

The notebook creates a Python 3.11 environment for the verification tools because the current α,β-CROWN package metadata requires it. A GPU helps with α,β-CROWN, but the final feature-level and small-network demonstrations are designed to be practical in Colab.

## Sources and acknowledgments

The demonstrations build on these public projects:

- [NASA pretrained microscopy models / MicroNet](https://github.com/nasa/pretrained-microscopy-models)
- [NASA Airfoil Learning](https://github.com/nasa/Airfoil-Learning)
- [NASA-IMPACT document classification model](https://huggingface.co/nasa-impact/nasa-smd-ibm-v0.1)
- [NASA GIBS anomaly detection](https://github.com/nasa-gibs/gibs-ml-anomaly-detection)
- [α,β-CROWN](https://github.com/Verified-Intelligence/alpha-beta-CROWN)
- [Marabou](https://github.com/NeuralNetworkVerification/Marabou)

This is an independent internship project, not an official NASA validation or certification.
