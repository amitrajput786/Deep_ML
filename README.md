# AlphaFold → PyMOL Workflow: Protein Structure Prediction and Analysis

This README documents the complete workflow used to predict and analyze a protein structure using **AlphaFold (via ColabFold)** and visualize it with **PyMOL**. The example used is the classic enzyme **lysozyme (129 amino acids)**.

This pipeline represents a modern, AI-driven solution to the protein folding problem.

---

## Overview

We start with a raw amino‑acid sequence and predict its full 3D atomic structure using AlphaFold. The resulting structure is then analyzed and visualized using PyMOL.

This approach replaces years of experimental crystallography with minutes of computation.

---

## Input Example: Lysozyme Protein Sequence

```
KVFGRCELAAAMKRHGLDNYRGYSLGNWVCAAKFESNFNTQATNRNTDGSTDYGILQINSRWWCNDGRTPGSRNLCNIPCSALLSSDITASVNCAKKIVSDGNGMNAWVAWRNRCKGTDVQAWIRGCRL
```

Length: **129 amino acids**

Function: Enzyme that breaks bacterial cell walls.

---

## What AlphaFold Does

AlphaFold does not simulate physics directly. Instead, it learns from evolution.

It searches large protein databases to find thousands of related sequences and builds a **Multiple Sequence Alignment (MSA)**. From evolutionary correlations, it infers which residues interact in 3D space.

This information is used by a deep neural network to predict the full atomic structure.

---

## Step-by-Step Pipeline

### 1. MSA Generation (Evolutionary Search)

AlphaFold searches databases such as:

* UniRef
* Environmental protein datasets

This produces thousands of aligned sequences related to the input protein.

### 2. Sequence Coverage Plot

The coverage plot shows:

* X‑axis → amino‑acid positions (1–129)
* Y‑axis → number of aligned sequences (~5000)
* Color → sequence identity
* Black curve → coverage depth per position

This indicates how much evolutionary information supports each residue.

High coverage = high confidence.

---

### 3. Co‑evolutionary Geometry Inference

If two residues mutate together across evolution, AlphaFold infers that they are spatially close.

From thousands of such correlations, AlphaFold builds a distance/contact map that encodes 3D geometry.

---

### 4. Neural Network Folding

AlphaFold uses a deep attention-based neural network with:

* Pairwise residue geometry
* Iterative refinement ("recycles")
* Physical constraints (bond lengths, angles)

It predicts all backbone and side‑chain atoms.

---

### 5. Ranking and Confidence

AlphaFold produces 5 candidate models and ranks them using:

* **pLDDT** → per-residue confidence (0–100)
* **PAE** → pairwise alignment error
* **pTM** → global fold confidence

The best model is saved as:

```
ranked_0.pdb
```

---

## Output Files

AlphaFold generates:

* `ranked_0.pdb` → best predicted structure
* `ranked_1.pdb` … `ranked_4.pdb` → alternative models
* `scores.json` → confidence scores
* `pae.json` → error matrix
* MSA alignment files
* Coverage and confidence plots

These files contain full atomic coordinates.

---

## Visualization with PyMOL

PyMOL is a professional molecular visualization tool used in research and drug design.

### Installation

Download from:

```
https://pymol.org
```

or via conda:

```
conda install -c schrodinger pymol
```

---

### Load AlphaFold Structure

```python
load ranked_0.pdb
```

---

### Common PyMOL Commands

Show secondary structure:

```python
show cartoon
```

Color helices and sheets:

```python
color yellow, ss h
color cyan, ss s
```

Show side chains:

```python
show sticks
```

Rainbow coloring:

```python
spectrum count, rainbow
```

Zoom:

```python
zoom
```

---

### Example: Lysozyme Active Site

Lysozyme catalytic residues:

* Glu35
* Asp52

```python
select active_site, resi 35+52
show sticks, active_site
color red, active_site
```

---

## Scientific Significance

This workflow enables:

* Structural biology without crystallography
* Drug design and docking
* Protein engineering
* Mutation analysis
* Functional annotation

AlphaFold predictions often match experimental structures within ~1 Å.

---

## Summary

This workflow solves the protein folding problem computationally:

Sequence → Evolutionary Search → Deep Learning → 3D Atomic Structure

The result is a publication‑grade protein model usable for research and engineering.

---

## Requirements

* Google Colab with GPU
* ColabFold / AlphaFold
* PyMOL

---

## License

AlphaFold parameters: CC BY 4.0
AlphaFold source: Apache 2.0
ColabFold: MIT

---

## Acknowledgments

* DeepMind AlphaFold Team
* ColabFold (Mirdita, Ovchinnikov, Steinegger)
* UniRef and metagenomic databases

---

This pipeline represents one of the most important breakthroughs in modern computational biology.
