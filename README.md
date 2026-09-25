# Cardinal Exponential-Spline Pulse Shaping for Faster-than-Nyquist Signaling

MATLAB code accompanying the manuscript:

**S. Ebrahimi, "Cardinal Exponential-Spline Pulse Shaping for Faster-than-Nyquist Signaling," submitted to IEEE Wireless Communications Letters.**

This repository contains the MATLAB implementation used for the CES pulse optimization, pulse-property evaluation, and coded FTN turbo-equalization simulations reported in the paper.

## Files

- `Pareto_CES.m`  
  Performs the Monte-Carlo/Pareto search for the cardinal exponential-spline (CES) pulse parameters and evaluates the spectral objectives and folded-spectrum conditioning constraint.

- `pulse_property_test.m`  
  Evaluates the selected CES and SRRC pulses and reports the main pulse properties used in the paper, including occupied bandwidth, spectral efficiency, OC-AER, folded-spectrum ripple, folded-spectrum condition number, sampled ISI energy, and reduced-memory Forney-factor energy concentration.

- `FTN_Mary_Turbo_rrc_ces_main.m`  
  Main coded FTN simulation. It compares the selected CES pulse with SRRC references using 8-QAM, a reduced-memory BCJR detector, and iterative turbo equalization.

- `FTN_Mary_BCJR_Equalizer.m`  
  BCJR equalizer used by the turbo receiver.

- `Siso_Conv_Decoder_r12.m`  
  Rate-1/2 soft-input/soft-output convolutional decoder used in the iterative receiver.

## Main Simulation Parameters

- Modulation: 8-QAM
- Nominal symbol rate: `Rs = 1e6` symbols/s
- Code rate: `Rc = 1/2`
- Samples per symbol: `SPS = 10`
- FTN packing factors: `tau = 0.6, 0.7, 0.8`
- Number of CES poles: `N = 6`
- CES parameter ranges:
  - `sigma_p in [-1, 1]`
  - `omega_p in [-pi, pi]`
- Number of random Pareto-search candidates: `Nrand = 5000`
- CES numerical support: `[-8Ts, 8Ts]`
- CES construction frequency grid: 8192 points
- Cardinal alias terms: 20 on each side
- Spectral-metric grid: 16384 points

For the BER simulation at `tau = 0.7`:

- Reduced-memory BCJR detector: `Lch = 3`
- Number of BCJR states: 512
- Turbo iterations: 5
- `Eb/N0 = 0:2:14` dB
- Up to `10^6` information bits per SNR point

## Selected CES Parameters

The selected pole parameters reported in the revised manuscript are:

### `tau = 0.6`

```matlab
theta = [ ...
     0.724634,  2.504610, ...
     0.122193, -2.601821, ...
     0.870135, -1.086495];
```

### `tau = 0.7`

```matlab
theta = [ ...
    -0.687894, -1.093351, ...
    -0.988055,  1.690138, ...
    -0.116332, -2.664607];
```

### `tau = 0.8`

```matlab
theta = [ ...
    -0.056232, -1.604223, ...
    -0.443235, -0.581184, ...
    -0.517063, -2.675816];
```

The parameter ordering is

```text
theta = [sigma1, omega1, sigma2, omega2, sigma3, omega3]
```

and the complete CES pole set is formed from the opposite-sign pairs

```text
{alpha1, -alpha1, alpha2, -alpha2, alpha3, -alpha3},
alpha_p = sigma_p + j*omega_p.
```

## How to Run

1. Run `Pareto_CES.m` to reproduce the CES parameter search and Pareto screening.
2. Run `pulse_property_test.m` to evaluate the selected CES/SRRC pulse metrics.
3. Run `FTN_Mary_Turbo_rrc_ces_main.m` to reproduce the coded BER simulations.

Keep `FTN_Mary_BCJR_Equalizer.m` and `Siso_Conv_Decoder_r12.m` in the same MATLAB path as the main turbo-equalization script.

## MATLAB Requirements

The code was developed in MATLAB and uses standard communications/signal-processing functions, including SRRC pulse generation. The Communications Toolbox may therefore be required depending on the MATLAB installation.

## Notes

- The Pareto search is stochastic. The reported design corresponds to the parameter ranges and search settings used in the revised manuscript.
- BER results are Monte-Carlo estimates and may require significant runtime at high SNR.
- High-SNR BER points are based on the observed number of error events and should be interpreted accordingly.

## Citation

If you use this code in academic work, please cite the associated paper:

```text
S. Ebrahimi, "Cardinal Exponential-Spline Pulse Shaping for
Faster-than-Nyquist Signaling," IEEE Wireless Communications Letters.
```

The bibliographic information can be updated after final publication.
