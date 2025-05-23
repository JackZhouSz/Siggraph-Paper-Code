# [TOG (SIGGRAPH Asia 2024)] Refined Inverse Rigging: A Balanced Approach to High-fidelity Blendshape Animation

By Stevo Rackovic, Dusan Jakovetic, and Claudia Soares

Published at SIGGRAPH https://dl.acm.org/doi/full/10.1145/3680528.3687670

## Introduction

In this paper, we present an advanced approach to solving the inverse rig problem in blendshape animation, using high-quality corrective blendshapes. Our algorithm focuses on three key areas: ensuring high data fidelity in reconstructed meshes, achieving greater sparsity in weight distributions, and facilitating smoother frame-to-frame transitions. While the incorporation of corrective terms is a known practice, our method differentiates itself by employing a unique combination of 𝑙1 norm regularization for sparsity and a temporal smoothness constraint through roughness penalty, focusing on the sum of second differences in consecutive frame weights. A significant innovation in our approach is the temporal decoupling of blendshapes, which permits simultaneous optimization across entire animation sequences. This feature sets our work apart from existing methods and contributes to a more efficient and effective solution. Our algorithm exhibits a marked improvement in maintaining data fidelity and ensuring smooth frame transitions when compared to prior approaches that either lack smoothness regularization or rely solely on linear blendshape models. In addition to superior mesh resemblance and smoothness, our method offers practical benefits, including reduced computational complexity and execution time, achieved through a novel parallelization strategy using clustering methods. Our results not only advance the state-of-the-art in terms of fidelity, sparsity, and smoothness in inverse rigging but also introduce significant efficiency improvements.

## Installation and Requirements

The python code is made to work on Windows OS, and data is extracted usign Autodesk Maya software. 

Required Python modules: 

```python
numpy
scipy
```

## Data extraction/preparation

Avatars/animations used in this paper are private, hence we provide scripts for extracting blendshapes of your own MetaHumans (www.unrealengine.com/en-US/metahuman), or creating a random toyset, within ... folder.

To extract blendshapes from the existing avatars, run the following commands

```bash
# Extract base blendshapes
python Scripts/DataExtraction/ExtractBlendshapes.py
# Extract higher order corrective terms
python Scripts/DataExtraction/ExtractCorrectiveBlendshapes.py
```

Alternativelly, to create random toydata, run 

```bash
# Create random blendshapes and corrective shapes
python Scripts/DataExtraction/CreateRandomData.py
```

After data is extracted, and saved in ../Data, compute singular and eigen values for the blendshape matrix, by running 

```bash
# Compute eigen values and singular values for the blendshape matrix
python Scripts/DataExtraction/ComputeEigenSingularValues.py
```

All the extracted/created data will be stored in ../Data repo.

## Training and Inference

The main script for the paper results is ExecuteHolistic.py, that trains a model on specified parameters, and produces predicted weights, storing tehm in ../Predictions repo.
```bash
python Scripts/ExecuteHolistic.py
```
Within the script, a user used specify the folllowing parameter values (in the header part)
```python
train_frames = 10           # this will take the first 'train_frames' from 'weights.npy' matrix as a training set
num_iter_max = 10           # the maximum number of iterations of the CD solver
num_iter_min = 5            # the minimum number of iterations of the CD solver
lmbd1 =  1                  # the sparsity regularization parameter of the objective funciton
lmbd2 =  1                  # the temporal smoothness regularization parameter of the objective funciton
T = 10                      # Interval batch size
```
The above values are default ones, as the optimal values derived from the paper experiments. If the dimensionality of your avatar is ismilar to ours, these should work fine.

## Bibliography

If you find our paper code helpful, consider citing:

```bibtex
@inproceedings{10.1145/3680528.3687670,
author = {Rackovi\'{c}, Stevo and Jakoveti\'{c}, Du\v{s}an and Soares, Cl\'{a}udia},
title = {Refined Inverse Rigging: A Balanced Approach to High-fidelity Blendshape Animation},
year = {2024},
isbn = {9798400711312},
publisher = {Association for Computing Machinery},
address = {New York, NY, USA},
url = {https://doi.org/10.1145/3680528.3687670},
doi = {10.1145/3680528.3687670},
booktitle = {SIGGRAPH Asia 2024 Conference Papers},
articleno = {45},
numpages = {9},
keywords = {blendshape animation, inverse rig problem, face segmentation},
location = {Tokyo, Japan},
series = {SA '24}
}
```

### Acknowledgements

This work was partially supported by NOVA LINCS (UIDB/04516/2020) with the financial support of FCT I.P. and Project "Artificial Intelligence Fights Space Debris" No C626449889-0046305 co-funded by Recovery and Resilience Plan and NextGeneration EU Funds, www.recuperarportugal.gov.pt. and by the Ministry of Education of the Republic of Serbia (451-03-9/2021-14/200125).
