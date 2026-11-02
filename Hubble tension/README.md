# Hubble Tension: CoDE-4 Three-Rung Ladder

This folder documents what the local distance-ladder calculation does, what the reported Hubble constants mean, and whether a separate conversion factor should be applied.

## Short answer about the conversion factor

If the value being used is the **full CoDE-adjusted three-rung SH0ES refit**, do **not** multiply it by the legacy conversion factor again. The model-dependent distance correction is already applied to the Hubble-flow supernova entries inside that refit. Multiplying the refitted result by another factor that represents the same distance correction would apply the correction twice.

That does not mean every quantity called a “conversion factor” is automatically redundant. First identify its definition and which input/output it maps. A factor may be needed when converting a *different* model quantity into a ladder-equivalent quantity, but it must not repeat a correction already included in the ladder likelihood.

## What the current repository reports

The checked-in validation output reports:

| Quantity | Reported value | Meaning |
|---|---:|---|
| Baseline SH0ES matrix recovery | 73.0428 km s⁻¹ Mpc⁻¹ | Refit of the released ladder system with its baseline right-hand side |
| CoDE local three-rung refit | 72.9331 km s⁻¹ Mpc⁻¹ | Same ladder system after the CoDE distance-shape correction to Hubble-flow supernovae |
| Legacy weighted correction | 72.8665 km s⁻¹ Mpc⁻¹ | A separate diagnostic that starts from an already measured SH0ES value; it is not the full matrix refit |
| Forward SH0ES-style value | 66.5107 km s⁻¹ Mpc⁻¹ | A forward calculation inheriting the engine’s CMB-calibrated global H₀ |

These outputs are not interchangeable. In particular, the legacy weighted correction and the full ladder refit are two ways of processing related distance information. The first is not an extra multiplier to place on top of the second.

The direct CoDE ladder result differs from the repository’s baseline matrix recovery by −0.1097 km s⁻¹ Mpc⁻¹. That is the change in the current ladder refit, not proof that the model has eliminated the Hubble tension. The independent global/CMB-calibrated prediction must be compared separately, with its own assumptions and uncertainty.

## How the three-rung calculation is constructed

The distance ladder uses three linked steps:

1. **Geometric anchors** establish distances to nearby calibrator systems.
2. **Cepheids** transfer those distances to Type Ia supernova host galaxies.
3. **Calibrated Type Ia supernovae** set the absolute magnitude used for Hubble-flow supernovae, whose redshifts and distances determine the late-time expansion normalization.

For a model expansion history, define

$$
E(z)=H(z)/H_0,
\qquad
d_L(z)=(1+z)\frac{c}{H_0}\int_0^z\frac{dz'}{E(z')}.
$$

The model changes the relative distance-redshift shape. Against the fiducial expansion shape, the corresponding distance-modulus correction is

$$
\Delta\mu(z)=5\log_{10}\!\left(\frac{f_{\rm CoDE}(z)}{f_{\rm fid}(z)}\right),
\qquad
f(z)=(1+z)\int_0^z\frac{dz'}{E(z')}.
$$

In the current implementation, the correction is applied to the 277 Hubble-flow right-hand-side entries. The anchor, Cepheid, and calibrator entries remain as released. With the released covariance matrix and design matrix, the linear ladder parameters are solved by generalized least squares,

$$
q=(A^T C^{-1}A)^{-1}A^T C^{-1}y_{\rm model},
$$

and the repository’s SH0ES parameterization maps the fitted intercept component to

$$
H_0=10^{q[46]/5}\;{\rm km\,s^{-1}\,Mpc^{-1}}.
$$

The exact implementation, data matching, sign convention, and covariance handling should be checked in `CoDE4_engine.py` and the ladder-refit function before interpreting or modifying the result.

## Why this is not a second H₀ calibration

The ladder refit returns H₀ by solving the released three-rung system after changing the model-predicted Hubble-flow distances. In schematic terms, the model correction is already inside (y_{\rm model}), before the fitted intercept and H₀ are obtained.

A separate post-fit multiplier (F) is appropriate only if it is defined as a transformation from the *specific output currently in hand* to a different, clearly specified output. If (F) is the same CoDE/fiducial distance-shape correction that was already used in (y_{\rm model}), applying it after the fit double counts it.

The repo also contains a legacy weighted calculation of the form “measured SH0ES value × averaged distance correction.” That calculation starts from the measured local value and is therefore SH0ES-anchored. It is useful as a comparison or diagnostic, but it is not independent of the SH0ES normalization and should not be described as a separate non-anchored prediction.

## What “independent” means here

The current ladder fit does **not** use a Planck/CMB prior or a target value of (H_0=73.04) in the ladder solve. Its expansion-shape inputs are held fixed at (Omega_m=0.32917) and (epsilon=0.05); the repository says (Omega_m) came from its Pantheon+ supernova-only fit. The ladder’s geometric anchors and calibrated supernova data are still observational inputs, as they must be for an absolute distance-ladder measurement.

So the precise description is:

- **No CMB prior in the three-rung H₀ refit.**
- **No imposed SH0ES scalar H₀ target in that refit.**
- **The SH0ES ladder data and geometric anchors do set its absolute scale.**
- **The model expansion-shape parameters are supplied by the separate CoDE analysis and held fixed for this reported refit.**

That is a local distance-ladder inference within the stated CoDE assumptions; it is not a completely data-free or assumption-free prediction.

## Reconciliation workflow

1. Record the exact target quantity: local ladder H₀, global CMB-calibrated H₀, or a forward-model value.
2. State which datasets set the expansion shape and which data set the absolute scale.
3. Use the full ladder refit for the CoDE-adjusted local value. Do not apply the legacy weighted factor on top of it.
4. Compare the local ladder result with the independently calculated global value. Keep the two values and their uncertainties separate.
5. Assess the joint model against the underlying supernova, BAO, CMB, and ladder data with their covariance and nuisance parameters. A closer pair of central H₀ values alone does not establish an overall successful reconciliation.

## Reproducibility source

The values above are transcribed from `outputs/validation_output.txt` in the repository. The report identifies the equations and says that the ladder result uses the released anchor–Cepheid–calibrator–Hubble-flow system, with no CMB prior or imposed SH0ES H₀ target in that fit. Re-run the pipeline and verify the exact current code and input files before treating the numbers as final.

