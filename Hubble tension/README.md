# Hubble Tension: CoDE-4 Three-Rung Ladder

## Short answer

The CoDE-adjusted three-rung result is obtained by refitting the released SH0ES ladder system. The model's distance-shape correction is applied once to the Hubble-flow supernova entries **before** the fit. The fit then returns (H_0) from its fitted ladder intercept.

The engine no longer computes or prints the separate weighted result formed by multiplying a supplied SH0ES value by an averaged scale factor. That post-hoc number is not used in, or multiplied onto, the three-rung result.

## How the local ladder H₀ is calculated

The three links are:

1. Geometric anchors set distances to nearby calibrators.
2. Cepheids transfer those distances to Type Ia supernova host galaxies.
3. Calibrated supernovae set the absolute magnitude used with Hubble-flow supernova redshifts to infer the local expansion rate.

For a background model,

$$
E(z)=H(z)/H_0,
\qquad
d_L(z)=(1+z)\frac{c}{H_0}\int_0^z\frac{dz'}{E(z')}.
$$

The code compares the model luminosity-distance shape with the fiducial cosmographic shape:

$$
f(z)=\frac{1+z}{z}\int_0^z\frac{dz'}{E(z')},
\qquad
\Delta\mu(z)=5\log_{10}\!\left[\frac{f_{\rm CoDE}(z)}{f_{\rm fid}(z)}\right].
$$

It applies the corresponding magnitude correction to the **277 Hubble-flow rows only**. The anchor, Cepheid, and calibrator rows remain as released. It then solves all ladder parameters by generalized least squares using the released design matrix (A) and full covariance (C):

$$
\hat q=(A^T C^{-1}A)^{-1}A^T C^{-1}y_{\rm model},
\qquad
H_0=10^{\hat q_{46}/5}\;{\rm km\,s^{-1}\,Mpc^{-1}}.
$$

There is no scalar input (H_0=73.04) in this refit, and no Planck/CMB prior enters it. The anchors and SH0ES ladder observations do set the absolute scale, so this is a data-based ladder inference under the selected CoDE expansion-shape parameters, not a data-free prediction.

The engine reports a baseline matrix recovery near 73.0428 and a CoDE-adjusted ladder fit near 72.9331 km/s/Mpc. The model parameters (Omega_m) and (epsilon) are held fixed in this ladder refit; (Omega_m) comes from the separate Pantheon+ SN-only fit.

## What was removed and what remains separate

The engine previously printed a second, weighted estimate made by applying an averaged distance-shape ratio to an already supplied SH0ES H₀. That calculation reused the SH0ES normalization and could be mistaken for another correction to the fitted ladder result. It has been removed from the engine and its diagnostic output.

A separately labeled **forward SH0ES-style projection** remains in the main report. It starts from the engine's global (H_0), which is calibrated using the CMB acoustic angle, and asks what a fiducial low-redshift distance-shape fit would infer. It is not multiplied into the three-rung ladder result and is not an independent CMB-free local measurement. Keep it separate from the direct ladder refit.

## Reconciliation reading

Use the three-rung refit as the local ladder value. Compare it with the global value from the CMB-calibrated background as a distinct result. Do not apply an additional weighted factor to either the fitted ladder (H_0) or its result. Agreement between central H₀ values alone does not establish overall model consistency; the underlying ladder, supernova, BAO, CMB, and growth constraints still need their own likelihood and covariance treatment.

## Implementation

See `CoDE4_engine.py`, especially `shoes_full_ladder_local_refit` and the SH0ES section in `main`. The calculation reconstructs the association between the released ladder's Hubble-flow rows and Pantheon+ rows by matching fiducial right-hand-side values because the public matrix omits those supernova identifiers. The engine reports residual checks for this matching.
