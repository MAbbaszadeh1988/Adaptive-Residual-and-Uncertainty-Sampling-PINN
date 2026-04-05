# Adaptive-Residual-and-Uncertainty-Sampling-PINN

# ARUS-PINN: Adaptive Residual and Uncertainty Sampling for Physics-Informed Neural Networks

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

@article{pashapour2024arus,
  title={Data-driven modeling of cell invasion and wound healing with a new adaptive physics-informed neural networks and time-space fractional Fisher-KPP equations},
  author={Pashapour, Mahya and Abbaszadeh, Mostafa and Dehghan, Mehdi},
  journal={Engineering Analysis with Boundary Elements},
  year={2024}
}
