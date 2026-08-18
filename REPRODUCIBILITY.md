# Reproducing the demonstrations

## Recommended route: Google Colab

Open `All_NN_Demos.ipynb` in Colab and run the notebook from the beginning. The notebook installs packages, downloads public NASA assets, builds or loads each verification target, runs both verifiers, and writes a combined evidence package.

Colab runtimes are temporary. If the runtime disconnects or is reset, installed packages and files under `/content` disappear and the setup cells must be rerun. Save or download the evidence bundle before ending the session.

## Environment notes

- The normal notebook kernel is used for data preparation, visualization, PyTorch inference, and ONNX export.
- The notebook creates a separate Python 3.11 virtual environment for α,β-CROWN and Marabou.
- A GPU is useful for α,β-CROWN. The verified final-layer and small-network cases are much lighter than the attempted full MicroNet ResNet query.
- Package APIs change. If a future package release breaks a cell, compare the installed commit/version with the version printed in the notebook output.

## How to read a result

For every case, record five items:

1. **Model boundary:** exactly which network layers are included?
2. **Allowed inputs:** which values vary, and by how much?
3. **Bad condition:** what outcome would count as a violation?
4. **α,β-CROWN result:** is the certified lower margin positive?
5. **Marabou result:** is the negated property `UNSAT`, or does the solver return a satisfying counterexample?

A property is reported as proved only when α,β-CROWN returns a sound positive bound and Marabou finds the corresponding violation query unsatisfiable.

## Case-specific boundaries

### MicroNet

The notebook first runs NASA’s pretrained ResNet-18 on a microscopy image and extracts its 512-dimensional representation. Verification begins at that feature vector and includes the exact released final linear layer. The proved property is pairwise: output 20 remains above output 30 in the chosen feature box.

The notebook also documents why the original 8×8 pixel-patch query through the full ResNet was not completed on the tested Colab T4. Keep that unsuccessful attempt separate from the successful feature-level proof.

### Airfoil

The notebook uses the NASA Airfoil Learning architecture and data, retrains the network, fixes geometry, and folds the fixed inputs into the first-layer bias. Both tools verify the exact resulting 3-input reduced network.

### NASA-IMPACT text classifier

The downloadable NASA model uses a RoBERTa encoder followed by a 768→5 classification layer. Verification includes the exact released classification layer and varies the 768-dimensional document representation. It does not verify arbitrary word or token substitutions through the encoder.

### GIBS anomaly classifier

The notebook follows the published GIBS architecture and uses real GIBS imagery to construct normal and controlled-anomaly examples. The 256-bin hue histogram is calculated outside the verifier. The complete trained 256→100→2 ReLU classifier is inside the formal boundary.

## Evidence checklist

Before presenting or reporting a result, keep:

- the nominal input and nominal output;
- the exact lower and upper input bounds;
- the property or violation query;
- the α,β-CROWN status and certified margin;
- the Marabou status and any counterexample assignment;
- the model provenance and any retraining or reduction step;
- the exact notebook output and clean summary table.

## LaTeX paper

From the repository root:

```bash
cd paper
pdflatex -interaction=nonstopmode formal_verification_nasa_neural_networks_final.tex
pdflatex -interaction=nonstopmode formal_verification_nasa_neural_networks_final.tex
```

The checked-in PDF is the visually reviewed reference build.
