🌿 TomatoPGT Tools — CloudSeg + CloudGraph

Organ-Level Digital Twin Modeling from 3D Tomato Plant Point Clouds
<p align="center"> <img src="figures/images/TomatoPGT_Pipeline.png" width="95%"> </p>

🔬 Overview

TomatoPGT Tools provide a reproducible pipeline for transforming raw 3D tomato plant point clouds into:

🌱 Structurally annotated organ-level data

🌳 Semantic plant graphs

📊 Quantitative phenotypic traits

🧠 Digital twin representations

These tools were developed to support the TomatoPGT Data in Brief publication and enable reproducible structural annotation, semantic graph extraction, and phenotype computation.

Tool	Purpose	Output
CloudSeg	Manual structural annotation of raw .ply point clouds	Annotated .txt
CloudGraph	Semantic graph extraction + phenotype computation	*_graph.json, *_phenotypes.csv
Digital Twin Pipeline

📦 Tools
Tool	Input	Output	Purpose
CloudSeg	.ply	Annotated .txt	Manual structural labeling
CloudGraph (Graph)	Annotated .txt	*_graph.json	Semantic graph construction
CloudGraph (Phenotypes)	*_graph.json	*_phenotypes.csv	Trait computation
⚙️ Installation
System Requirements

Windows 11 (64-bit)
Python 3.11 [1]
Open3D [2]
NumPy [3]
SciPy [4] 
scikit-learn [5]

1️⃣ Create Environment
conda create -n TomatoPGT python=3.11 -y
conda activate TomatoPGT
python -m pip install -U pip
pip install open3d==0.19.0 numpy scipy pandas scikit-learn

2️⃣ Install Tools
pip install wheels/cloudseg-1.0.0-cp311-cp311-win_amd64.whl
pip install wheels/cloudgraph-1.0.0-cp311-cp311-win_amd64.whl

3️⃣ Run
python -m cloudseg.runner
python -m cloudgraph.runner

📂 Sample Data

You should now add data to the tools:

sample_dataset/
  Data_cSeg_Raw/
    BFS_R_05082025.ply
  Data_cGraph_Annotated
    BFS_05082025_annotated_ext.txt`


## 🧪 Sample Dataset

A minimal dataset is provided in `sample_dataset/` to test the full pipeline:

1. Open `Data_cSeg_raw/BFS_R_05082025.ply` in CloudSeg
2. Annotate with CloudSeg tool and save `BFS_05082025_annotated_ext.txt`
3. Load into CloudGraph tool : `BFS_05082025_annotated_ext.txt`
4. Generate graph and phenotypes


🌿 CloudSeg — Structural Annotation
Input
Raw .ply point cloud only


CloudSeg will not load .txt or .json files.

Output
Annotated .txt file

This file must follow the simplified structural schema.

GUI Overview
<p align="center"> <img src="figures/images/CloudSeg_GUI.png" width="85%"> </p>

CloudSeg implements a Parent–Child Open3D workflow:

Parent → Full plant view

Child → Region selection and cropping

🧭 Annotation Workflow

| **Step 1 — Load Point Cloud**                 | **Step 2 — Adjust View**                                                                                                                        | **Step 3 — Set Class + Instance**                                                                                               | **Step 4 — Select Region**                                                                                                                                                                                               | **Step 5 — Verify**                                                                |
| --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------- |
| Click **Open Cloud…** and load a `.ply` file. | **Open3D Controls:**<br>• Mouse wheel → Zoom<br>• Drag → Rotate<br>• Pan → Reposition<br><br>⚠ View may require manual adjustment after redraw. | Before selecting:<br>• Choose structural class<br>• Set correct instance ID<br><br>⚠ Critical for correct graph reconstruction. | Click **Select Region…**<br><br>Inside child window:<br>• Press **K** → Activate selection<br>• Ctrl + Left Click → Polygon selection<br>• Drag → Rectangle selection<br>• Press **C** → Crop<br>• Press **Q** → Confirm | Selected region appears in palette color.<br><br>If incorrect:<br>• Undo<br>• Redo |


Structural Schema for Annotation workflow:
<p align="center"> <img src="figures/images/cloudseg_schema.png" width="80%"> </p>

## Simplified Annotation Schema

| **Class name used**     | **Class Description**                           | **Common Annotation Errors**        |
|-------------------------|-------------------------------------------------|-------------------------------------|
| **Root-Node**           | Origin of the plant                             | Broken stem chain                   |
| **Junction-Nodes**      | Fork on the Stem segment                        | Leaf and stalk mixed                |
| **mainStem-Seg**        | Stem segments / Internode                       | Duplicate instance IDs              |
| **Compound Leaf-Node**  | Leaflets + Rachis                               | Missing Root-Node                   |
| **Stalk-Seg**           | Petiole (Junction node to Compound Leaf)        | —                                   |
| **Sucker-Seg**          | Sucker / Branch / Axil                          | —                                   |

🌳 CloudGraph — Semantic Graph Extraction
Input
Annotated .txt file from CloudSeg


⚠ Never load raw .ply.

Output
*_graph.json

Inspect Tab
<p align="center"> <img src="figures/images/cloudgraph_gui_inspect.png" width="70%"> </p>

Used to validate:

Structural annotation

Color mode

Point size

Camera reset

Graph Tab
<p align="center"> <img src="figures/images/cloudgraph_gui_control_graph_extraction.png" width="70%"> </p>
Workflow

Review parameters

Click Extract Graph

Confirm *_graph.json

|  |  |
|---|---|
| <img src="figures/images/cloudgraph_gui_control_graph_extraction.png" alt="Control (left)" width="90%"> | <img src="figures/images/cloudgraph_gui_control_phenotype_extraction.png" alt="Control (right)" width="90%"> |

*Left: Control tab (Graph)* | *Right: Control tab (Phenotype)*

</div>

🌱 CloudGraph — Phenotypes
Input
*_graph.json

Output
*_phenotypes.csv

Phenotype Tab
<p align="center"> <img src="figures/images/cloudgraph_gui_control_phenotype_extraction.png" width="70%"> </p>
Workflow

Select units (cm default)

Click Compute Phenotypes

Export CSV

Open Phenotype Viewer




Workflow:

Select units (default: cm)

Click Compute Phenotypes

Export *_phenotypes.csv

🎞 Graph Extraction  and Phenotype Visual Animation
<p align="center"> <img src="figures/gifs/cloudgraph_extraction.gif" width="80%"> </p>

<div align="center">

🎞 Phenotype Visualization Animation
<p align="center"> <img src="figures/gifs/CloudGraph_Phenotype.gif" width="80%"> </p>

|  |  |
|---|---|
| <img src="img src="figures/gifs/cloudgraph_extraction.gif" width="80%"> | <img src="figures/gifs/CloudGraph_Phenotype.gif" width="80%"> |

*Left: Control tab (Graph)* | *Right: Control tab (Phenotype)*

</div>

🌱 Computed Traits
📊 Phenotypes Generated by CloudGraph
## 📊 Computed Traits

| Category | Description |
|----------|------------|
| **Internode Lengths** | Distance between consecutive Junction-Nodes along the main stem |
| **Stem Height** | Vertical extent of the reconstructed plant graph |
| **Branching Metrics** | Structural connectivity and branching order derived from the semantic graph |
| **Leaf Insertion Angles** | Angular orientation between main stem and Compound Leaf attachment |
| **Sucker Orientation** | Direction and angle of lateral vegetative shoots |

🔄 Recommended Workflow
## 🔄 Recommended Workflow

| Step | Action | Tool |
|------|--------|------|
| 1 | Annotate full plant structure | **CloudSeg** |
| 2 | Export annotated `.txt` | CloudSeg |
| 3 | Load annotated file | **CloudGraph → Inspect tab** |
| 4 | Validate annotation colors & structure | CloudGraph |
| 5 | Extract semantic graph (`*_graph.json`) | **Graph tab** |
| 6 | Validate topology in Graph Viewer | CloudGraph |
| 7 | Compute phenotypes (`*_phenotypes.csv`) | **Phenotypes tab** |

🧪 Troubleshooting
## 🧪 Troubleshooting

| Issue | Possible Cause | Recommended Fix |
|-------|----------------|----------------|
| CloudGraph freezes on load | Missing Root-Node | Ensure Root-Node is annotated in CloudSeg |
| Graph extraction fails | Broken junction chain | Verify continuous Junction-Nodes along stem |
| Incorrect attachments | Leaf and stalk mixed labels | Separate Compound Leaf and Stalk-Seg during annotation |
| Graph ambiguity | Duplicate Instance IDs | Ensure unique instance numbering |
| Slow processing / instability | Large unannotated (-1) regions | Complete annotat
Uninstall
pip uninstall cloudseg cloudgraph

📖 Citation
@article{TomatoPGT2026,
  title   = {TomatoPGT: A 3D point cloud dataset of tomato plants for segmentation and plant-trait extraction},
  author  = {Nethala, Prasad et al.},
  journal = {Data in Brief},
  year    = {2026}
}

📜 License

MIT License
See LICENSE file.

## References

[1] Python Software Foundation, “Python Language Reference (Version 3.11.x).” Available: https://www.python.org

[2] Q.-Y. Zhou, J. Park, and V. Koltun, “Open3D: A Modern Library for 3D Data Processing,” *arXiv:1801.09847*, 2018.

[3] C. R. Harris *et al*., “Array programming with NumPy,” *Nature*, vol. 585, pp. 357–362, 2020.

[4] P. Virtanen *et al*., “SciPy 1.0: Fundamental algorithms for scientific computing in Python,” *Nature Methods*, vol. 17, pp. 261–272, 2020.

[5] F. Pedregosa *et al*., “Scikit-learn: Machine Learning in Python,” *Journal of Machine Learning Research*, vol. 12, pp. 2825–2830, 2011.
