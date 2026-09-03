# Adaptive-Residual-and-Uncertainty-Sampling-PINN

# ARUS-PINN: Data-driven modeling of cell invasion and wound healing with a new adaptive physics-informed neural networks and time–space fractional Fisher–KPP equations

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.0+-orange.svg)](https://www.tensorflow.org/)

## Overview

This repository contains the official implementation of **ARUS-PINN (Adaptive Residual and Uncertainty Sampling for Physics-Informed Neural Networks)** , a novel adaptive training framework that significantly enhances the performance of Physics-Informed Neural Networks for solving and parameter estimation of partial differential equations (PDEs), with a focus on **space-time fractional Fisher-KPP equations** for modeling cell invasion and wound healing.

### Key Features

- **Adaptive Sampling Strategy**: Dynamically refines training points based on a composite score combining PDE residual and model uncertainty
- **Fractional PDE Support**: Handles time-fractional Caputo derivatives and space-fractional Riesz derivatives using finite difference approximations
- **Parameter Estimation**: Simultaneously learns solution fields and unknown PDE parameters (diffusion coefficient, growth rate, fractional orders)
- **Bidirectional Point Management**: Removes well-learned points while adding challenging ones for efficient training
- **Biological Applications**: Pre-configured for scratch assay (wound healing) and cell invasion datasets

## Method Overview

ARUS-PINN addresses two major challenges in standard PINNs: slow convergence and reduced accuracy in regions with complex solution behavior. The method iteratively:

1. **Trains** the PINN on the current set of collocation points
2. **Evaluates** residual error and uncertainty (via second-derivative sensitivity) on a candidate set
3. **Removes** points that are well-learned (low residual AND low uncertainty)
4. **Adds** points with the highest composite scores (balancing residual and uncertainty)

This targeted refinement allows the neural network to focus learning on the most challenging and informative regions of the solution domain.

### Composite Scoring Function

$$S(\mathbf{x}, t) = \alpha \cdot \tilde{R}(\mathbf{x}, t) + (1 - \alpha) \cdot \tilde{U}(\mathbf{x}, t)$$

where:
- $\tilde{R}$: Normalized PDE residual
- $\tilde{U}$: Normalized uncertainty (estimated via second derivatives)
- $\alpha$: Balancing coefficient (recommended: 0.5)

### Space-Time Fractional Fisher-KPP Equation

The model extends the classical Fisher-KPP equation using fractional derivatives:

$$\frac{\partial^p u}{\partial t^p} = -D (-\Delta)^{\tfrac{q}{2}}u + r u \left(1 - \frac{u}{K}\right)$$

- $p$: Temporal fractional order (memory effects)
- $q$: Spatial fractional order (anomalous diffusion)
- $D$: Diffusion coefficient
- $r$: Growth rate
- $K$: Carrying capacity


## Data Source

### Experimental Data

The primary experimental dataset is taken from:

> **Jin W, Shah ET, Penington CJ, McCue SW, Chopin LK, Simpson MJ.** (2016)  
> *Reproducibility of scratch assays is affected by the initial degree of confluence: experiments, modelling and model selection.*  
> Journal of Theoretical Biology, 390:136–145.  
> [DOI: 10.1016/j.jtbi.2015.11.020](https://doi.org/10.1016/j.jtbi.2015.11.020)

This dataset consists of cell density profiles measured over space and time from scratch assay experiments under controlled initial confluence conditions. The raw data are stored in `data/raw/` and are used as the ground truth for all model calibrations.

### Methodological Framework

The parameter estimation and model selection methodology follows:

> **Liu Y, Suh K, Maini PK, Cohen DJ, Baker RE.** (2024)  
> *Parameter identifiability and model selection for partial differential equation models of cell invasion.*  
> Journal of the Royal Society Interface, 21(212):20230607.  
> [DOI: 10.1098/rsif.2023.0607](https://doi.org/10.1098/rsif.2023.0607)
>
> So all dataset are taken from references:

> https://doi.org/10.5281/zenodo.8377953.
> 
> https://github.com/liuyue002/woundhealing.
> 
> http://dx.doi.org/10.1016/j.jtbi.2015.10.040.

## System Configuration for Model Training
The codes were executed using two different computational environments. The first environment was the Ferdowsi Cloud, using the computational resources provided by K. N. Toosi University of Technology, equipped with an NVIDIA RTX 3080 Ti GPU, 32 CPU cores, 64 GB of RAM, 200 GB of disk storage, and a shared network bandwidth of 1024 Mb/s. In addition, the computations were performed on the Kaggle platform using two NVIDIA T4 GPUs.

Please cite this paper as follows:

@article{pashapour2026arus,
  title={ARUS-PINN: Data-driven modeling of cell invasion and wound healing with a new adaptive physics-informed neural networks and time--space fractional Fisher--KPP equations},
  author={Pashapour, Mahya and Abbaszadeh, Mostafa and Dehghan, Mehdi},
  journal={Engineering Analysis with Boundary Elements},
  volume={192},
  pages={106941},
  year={2026},
  publisher={Elsevier}
}
