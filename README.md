# GAT (Graph Attention Network model)
GAT model to classify conformational states of protein using MD simulation data and extract communication patterns 

This repository contains the code used to train and analyze a Graph Attention Network (GAT) model for identifying conformational states of a protein system from molecular dynamics (MD) simulations. The workflow converts MD trajectory frames into residue-level graph representations and trains a neural network to classify structural states of the protein. The trained model is subsequently analyzed using attention-based methods to identify important residue level communication pathways learned by the network.

The methodological details and biological interpretation of the model are described in the associated publication.

Paper DOI:https://doi.org/10.1101/2025.06.17.659969

Due to their size, the dataset and trained model are hosted externally.
Dataset DOI:https://doi.org/10.6084/m9.figshare.31645048

Repository Contents

This repository contains two notebooks corresponding to the main stages of the workflow.

GAT_Two_Class_Training.ipynb
Loads the dataset, constructs the Graph Attention Network architecture, performs model training, and evaluates classification performance.

Analysis_GAT_Two_Class.ipynb
Loads the trained model and dataset to perform attention-based interpretability analysis, including residue interaction mapping and domain-level communication analysis.

Workflow Overview

The workflow implemented in this repository consists of the following stages:

Conversion of molecular dynamics trajectory frames into graph representations of the protein structure

Training of a Graph Attention Network to classify conformational states

Evaluation of the trained model on a held-out test set

Extraction of attention weights from the neural network

Structural interpretation of residue-level interactions learned by the model

Each frame of the MD trajectory is therefore represented as a graph describing residue interactions in the protein structure.

Dataset Description

The training notebook expects a dataset containing graph representations of molecular dynamics frames. The dataset is stored as a PyTorch object containing a list of graph samples.

Dataset file: GAT_Two_Class_data.pt

Each entry corresponds to one MD trajectory frame encoded as a graph using the PyTorch Geometric Data format.

Dataset Summary

The dataset used to train the Graph Attention Network consists of graph representations derived from molecular dynamics simulations.

Edge definition
Residue pairs with Cα distance ≤ 10 Å

Edge weights
Inverse of residue–residue distance

Graph format
PyTorch Geometric Data objects

Graph Data Format

Each graph sample contains the following components.

x: Node feature matrix
y: Class label

In PyTorch Geometric format this corresponds to

Data(
x = node_features
edge_index = connectivity
edge_attr = edge_weights
y = label
)

Node Definition

Nodes correspond to residues represented by their Cα atoms.

Node Features

The specific features used to construct the node feature vectors are described in the associated publication.



Training Procedure

The training notebook loads the dataset file

GAT_Two_Class_data.pt

and trains a Graph Attention Network using PyTorch Geometric.

The architecture consists of two graph attention layers followed by global pooling and a classification layer.

The model predicts the conformational class of each graph.

Train/Test Split

The dataset is split deterministically.

Test samples are selected by taking every tenth frame from each class.

The remaining frames are used for training.

Model Output

The trained model is saved as

gnn_model.pt

The final trained model used in the publication can be downloaded 

Model Analysis

The analysis notebook loads the trained model and the dataset to perform attention-based interpretability analysis.

The analysis extracts attention weights from the graph attention layers and maps them to residue-level interactions in the protein structure.

This allows identification of structural communication pathways learned by the neural network.

Analysis Outputs

The analysis pipeline produces several outputs including

confusion matrices
attention interaction maps
presence histograms
domain-level interaction heatmaps
variance maps of attention weights
tables of important residue interactions
correlation analysis between attention layers

These outputs help interpret how the neural network distinguishes different conformational states.

Running the Code

Install required dependencies

pip install torch torch-geometric MDAnalysis MDTraj seaborn scikit-learn

Training

Run

GAT_Two_Class_Training.ipynb

Analysis

Run

Analysis_GAT_Two_Class.ipynb

Before running the notebooks, update the file paths to the dataset and trained model.

Citation

If you use this code or dataset please cite

PAPER/DOI :  https://doi.org/10.1101/2025.06.17.659969

License

MIT License
