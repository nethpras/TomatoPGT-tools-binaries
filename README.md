🌿 TomatoPGT Tools
CloudSeg + CloudGraph (Binary Release)
📌 Overview

TomatoPGT provides two GUI tools for creating organ-level digital twin representations of tomato plants from 3D point clouds.

Tool	Purpose	Output
CloudSeg	Manual structural annotation of raw .ply point clouds	Annotated .txt
CloudGraph	Semantic graph extraction + phenotype computation	*_graph.json, *_phenotypes.csv

These tools were developed for the TomatoPGT Data in Brief publication.

📊 Visual Pipeline
<p align="center"> <img src="figures/images/TomatoPGT_Pipeline.png
" width="100%"> </p>

Raw Point Cloud → Structural Annotation → Semantic Graph → Phenotypes → Digital Twin

⚡ Quick Installation
1️⃣ Create environment
conda create -n TomatoPGT python=3.11 -y
conda activate TomatoPGT
python -m pip install -U pip
pip install open3d==0.19.0 numpy scipy pandas scikit-learn

2️⃣ Install tools
pip install wheels/cloudseg-1.0.0-cp311-cp311-win_amd64
pip install wheels/cloudgraph-1.0.0-cp311-cp311-win_amd64

3️⃣ Run
python -m cloudseg.runner
python -m cloudgraph.runner

🌿 CloudSeg — Annotation Tool
📥 Input

Raw .ply point cloud only

🖥 GUI Overview
<p align="center"> <img src="figures/images/CloudSeg_GUI.png
" width="80%"> </p>

CloudSeg uses a Parent–Child workflow built on Open3D:

Parent window → full point cloud

Child window → region selection & cropping

🔹 Step-by-Step Annotation Tutorial
Step 1 — Load .ply

Click Open Cloud…

Step 2 — Adjust View (Open3D behavior)

Use:

Mouse wheel → zoom

Drag → rotate

Pan → reposition

⚠ Camera may require manual re-centering after redraws (Open3D limitation).

Step 3 — Set Class + Instance

Before selecting:

Choose class from dropdown

Set correct instance ID

This is critical.

Step 4 — Select Region

Click Select Region…

Child window opens.

Inside child window:

Press K → activate selection

Select using:

Ctrl + Left Click → polygon selection

Drag → rectangle selection

Press C → crop

Press Q → confirm and return

Step 5 — Verify Result

Parent window updates.

Selected region appears in palette color.

If incorrect:

Undo

Redo

🌱 Structural Schema (Important)
<p align="center"> <img src="figures/cloudseg_schema.png" width="80%"> </p>

Simplified topology:

Root-Node

Junction-Nodes

mainStem-Seg

Compound Leaf-Node

Stalk-Seg

Sucker-Seg

This preserves topology while avoiding per-leaflet labeling.

⚠ Common Mistakes
❌ Broken stem chain

Graph extraction will fail.

❌ Leaf and stalk mixed

Wrong attachment geometry.

❌ Duplicate instance IDs

Graph ambiguity.

❌ Missing root node

Graph cannot initialize.

🌳 CloudGraph — Graph Extraction + Phenotypes
📥 Input Requirement

Must load:

Annotated .txt exported from CloudSeg.

Never load raw .ply.

🔍 Tab 1 — Inspect
<p align="center"> <img src="figures\images\CloudGraph_gui.png" width="80%"> </p>

Use to:

Validate annotation

Switch color mode (Annotated / Original)

Adjust point size

Reset camera

🌿 Tab 2 — Graph Extraction
<p align="center"> <img src="figures\images\cloudgraph_gui_control_graph_extraction.png" width="80%"> </p>

Review extraction parameters

Click Extract Graph

Confirm *_graph.json saved

Open Graph Viewer

Viewer overlays:

Nodes

Edges (tubes)

Bounding boxes

Anchors

🌱 Tab 3 — Phenotypes
<p align="center"> <img src="figures\images\cloudgraph_gui_control_phenotype_extraction.png" width="80%"> </p>

Select units (default: cm)

Click Compute Phenotypes

Export *_phenotypes.csv

Computed traits include:

Internode lengths

Stem height

Branching metrics

Leaf insertion angles

Sucker orientation

🔄 Recommended Workflow

Annotate plant in CloudSeg

Export annotated .txt

Load in CloudGraph

Inspect

Extract graph

Validate

Compute phenotypes
![CloudGraph graph extraction](figures/gifs/cloudgraph_graph_extraction.gif)
![CloudGraph phenotype extraction](figures/gifs/cloudgraph_phenotype.gif)

🧪 Troubleshooting

If CloudGraph freezes:

Check missing Root-Node

Check broken junction chain

Check large number of -1 labels

Verify instance IDs are consistent

📦 Uninstall
pip uninstall cloudseg cloudgraph

📖 Citation
@article{TomatoPGT2026,
  title   = {TomatoPGT: Organ-level structural annotations and graph-based phenotypes of tomato plants from 3D point clouds},
  author  = {Nethala, Prasad et al.},
  journal = {Data in Brief},
  year    = {2026}
}

📜 License

MIT License
See LICENSE file.