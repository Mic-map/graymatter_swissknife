# Gray Matter Microstructure models estimator for gray matter diffusion MRI

[![PyPI - Version](https://img.shields.io/pypi/v/graymatter_swissknife.svg)](https://pypi.org/project/graymatter_swissknife)
[![PyPI - Downloads](https://img.shields.io/pypi/dm/graymatter_swissknife)](#)
[![GitHub](https://img.shields.io/github/license/QuentinUhl/graymatter_swissknife)](#)
[![GitHub top language](https://img.shields.io/github/languages/top/QuentinUhl/graymatter_swissknife?color=lightgray)](#)
[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/graymatter_swissknife.svg)](https://pypi.org/project/graymatter_swissknife)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

-----

**Table of Contents**

- [Installation](#installation)
- [Usage](#usage)
- [Prerequisites](#prerequisites)
- [Citation](#citation)
- [License](#license)

## Installation

```console
pip install graymatter_swissknife
```

## Usage

### Estimate Gray Matter microstructure model parameters

To estimate any gray matter model parameters with Nonlinear Least Squares using the graymatter_swissknife package, you can use the estimate_model function. This function takes several parameters that you need to provide in order to perform the estimation accurately.

```
estimate_model(model_name, dwi_path, bvals_path, td_path, lowb_noisemap_path, out_path)
```

`model_name`: Choose your gray matter model between `'Nexi'` (or `'Nexi_Narrow_Pulses_Approximation'`), `'Smex'` (or `'Nexi_Wide_Pulses'`), `'Sandi'`, `'Sandix'` and `'Gem'`.

`dwi_path`: The path to the diffusion-weighted image (DWI) data in NIfTI format. This data contains the preprocessed diffusion-weighted volumes acquired from your imaging study.

`bvals_path`: The path to the b-values file corresponding to the DWI data, in ms/µm². B-values specify the strength and timing of diffusion sensitization gradients for each volume in the DWI data.

`td_path`: The path to the diffusion time (td) file, also known as Δ, in ms. This file provides information about the diffusion time for each volume in the DWI data. The diffusion time is the time between the two gradient pulses. 

`lowb_noisemap_path`: The path to the noisemap calculated using only the small b-values (b < 2 ms/µm²) and Marchenko-Pastur principal component analysis (MP-PCA) denoising. This noisemap is used to calculate the signal-to-noise ratio (SNR) of the data.

`out_path`: The folder where the estimated parameters will be saved as output.

## Prerequisites

### Data Acquisition

For accurate parameter estimation using the graymatter_swissknife package, acquire PGSE EPI (Pulsed Gradient Spin Echo Echo-Planar Imaging) diffusion MRI data with diverse combinations of b values and diffusion times. Ensure reasonable signal-to-noise ratio (SNR) in the data for accurate parameter estimation.

### Preprocessing

Before proceeding, make sure to preprocess your data with the following steps:
- Marchenko-Pastur principal component analysis (MP-PCA) denoising ([Veraart et al., 2016](https://doi.org/10.1016/j.neuroimage.2016.08.016)). Recommended algorithm : [dwidenoise from mrtrix](https://mrtrix.readthedocs.io/en/dev/reference/commands/dwidenoise.html)
- Gibbs ringing correction ([Kellner et al., 2016](https://doi.org/10.1002/mrm.26054)). Recommended algorithm : [FSL implementation](https://bitbucket.org/reisert/unring/src/master/)
- Distortion correction using FSL topup ([Andersson et al., 2003](https://doi.org/10.1002/mrm.10335), [Andersson et al., 2016](https://doi.org/10.1016/j.neuroimage.2015.10.019)). Recommended algorithm : [FSL topup](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/topup)
- Eddy current and motion correction ([Andersson and Sotiropoulos, 2016](https://doi.org/10.1016/j.neuroimage.2015.12.037)). Recommended algorithm : [FSL eddy](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/eddy)

Additionally, you need to compute another noisemap using only the small b-values (b < 2 ms/µm²) and MP-PCA. This noisemap will be used to calculate the signal-to-noise ratio (SNR) of the data.

Furthermore, you can provide a mask of grey matter tissue if available. This mask can be used to restrict the processing to specific regions of interest. If a mask is not provided, the algorithms will be applied to the entire image, voxel by voxel, as long as there are no NaN values present.

To compute a grey matter mask, one common approach involves using a T1 image, [FastSurfer](https://deep-mi.org/research/fastsurfer/), and performing registration to the diffusion (b = 0 ms/µm²) space. However, you can choose any other method to compute a grey matter mask.

## Citation

If you use this package in your research, please consider citing the following papers:

## Original gray matter model papers

### Generalized Exchange Model ($GEM$) / Development of this package
Quentin Uhl, Tommaso Pavan, Inès de Riedmatten, Jasmine Nguyen-Duc, and Ileana Jelescu, [GEM: a unifying model for Gray Matter microstructure](https://www.ismrm.org/24m/), Proc. Intl. Soc. Mag. Reson. Med. 2024. 
Presented at the Annual Meeting of the ISMRM, Singapore, Singapore, p. 7970.

### Neurite Exchange Imaging ($NEXI$, or $NEXI_{Narrow Pulse Approximation}$)
Ileana O. Jelescu, Alexandre de Skowronski, Françoise Geffroy, Marco Palombo, Dmitry S. Novikov, [Neurite Exchange Imaging (NEXI): A minimal model of diffusion in gray matter with inter-compartment water exchange](https://www.sciencedirect.com/science/article/pii/S1053811922003986), NeuroImage, 2022.

### Standard Model with Exchange ($SMEX$, or $NEXI_{Wide Pulses}$)
Jonas L. Olesen, Leif Østergaard, Noam Shemesh, Sune N. Jespersen, [Diffusion time dependence, power-law scaling, and exchange in gray matter](https://doi.org/10.1016/j.neuroimage.2022.118976), NeuroImage, 2022.

###  Soma And Neurite Density Imaging ($SANDI$)
Marco Palombo, Andrada Ianus, Michele Guerreri, Daniel Nunes, Daniel C. Alexander, Noam Shemesh, Hui Zhang, [SANDI: A compartment-based model for non-invasive apparent soma and neurite imaging by diffusion MRI](https://doi.org/10.1016/j.neuroimage.2020.116835), NeuroImage, 2020.

###  Soma And Neurite Density Imaging with eXchange ($SANDIX$)
Jonas L. Olesen, Leif Østergaard, Noam Shemesh, Sune N. Jespersen, [Diffusion time dependence, power-law scaling, and exchange in gray matter](https://doi.org/10.1016/j.neuroimage.2022.118976), NeuroImage, 2022.

## License

`graymatter_swissknife` is distributed under the terms of the [Apache License 2.0](https://spdx.org/licenses/Apache-2.0.html).