# Hubble-tension pipeline: working outline for the next update

**Planned follow-up:** 10 days after this outline  
**Status:** Proposed analysis plan. The model-to-ladder derivation and independent validation are not yet complete.

## Objective

Test the CoDE4 expansion assumptions inside the published SH0ES three-rung distance-ladder framework, while keeping each observational calibration distinct from the cosmological distance law. The aim is to determine what the model changes, which assumptions remain inherited from the SH0ES/Pantheon+ release, and whether the ladder data support the resulting local-H0 inference.

## 1. Derive the CoDE4 distance law before fitting the ladder

1. Write down the model's starting equations and assumptions, including the Hamiltonian/action formulation if that is part of the model.
2. Derive the background expansion equation (H(z)) and define (E(z)=H(z)/H_0). State every parameter, unit, sign convention, and fixed or fitted quantity.
3. Derive the luminosity distance using the redshift convention used by the supernova data:
   [
   D_L(z_{m HD},z_{m HEL})=(1+z_{m HEL}),rac{c}{H_0}
   int_0^{z_{m HD}}rac{dz'}{E(z')}.
   ]
   If the model predicts curvature, modified photon propagation, or a nonstandard distance-duality relation, derive and use the corresponding expression instead of assuming this flat-FLRW formula.
4. Check limiting cases and units. In particular, establish whether and under what parameter limit the model reduces to flat (Lambda)CDM. Do not assume that correspondence without deriving it.

## 2. Reproduce the released SH0ES ladder first

The SH0ES release represents the ladder as one correlated linear system, conventionally written (y=Aq) with covariance (C). Its three links are:

1. **Geometric anchors to Cepheids.** Anchor distances come from geometric measurements such as Milky Way parallaxes, detached eclipsing binaries in the LMC, and the NGC 4258 water-maser distance. These calibrate the Cepheid period-luminosity/Wesenheit relation.
2. **Cepheids to calibrator supernovae.** Cepheids in supernova-host galaxies provide host distances and calibrate the standardized absolute magnitude of Type Ia supernovae.
3. **Calibrator supernovae to Hubble-flow supernovae.** The calibrated supernova brightness is combined with Hubble-flow redshifts and the model distance law to infer local (H_0).

The likelihood is solved jointly, not as three independent Hubble-constant measurements:
[
hat q=(A^{T}C^{-1}A)^{-1}A^{T}C^{-1}y.
]
Use the exact data vector, design matrix, and full covariance from the public release. First reproduce the release's baseline result with its fiducial distance prescription before making a CoDE4 change.

## 3. Map CoDE4 into the ladder only where the derivation requires it

The default test changes the Hubble-flow supernova distance terms using the derived CoDE4 (D_L(z)). Keep the geometric anchor measurements, Cepheid photometry/relation, and calibrator-supernova observations as the released empirical inputs unless a derivation shows that CoDE4 changes one of those local rungs.

If only the Hubble-flow distance law changes, update those model-dependent terms in the data vector/design equations and solve the full covariance system again. Do not multiply the fitted answer by a separate scale factor. If the model changes a rung equation, derive that modified equation explicitly and identify the new data or calibration needed before fitting it.

## 4. Separate model assumptions from inherited calibrations

Document these assumptions and their provenance:

- geometric anchor distance measurements and zero points;
- transferability of the Cepheid period-luminosity/Wesenheit relation across anchor and supernova-host galaxies;
- metallicity, extinction/dust, crowding/blending, and photometric calibration corrections;
- the released SALT2 light-curve standardization, supernova absolute-magnitude relation, host corrections, and selection-bias corrections;
- redshift-frame, peculiar-velocity, and Hubble-flow sample-cut conventions;
- the covariance terms and any parameters held fixed or refitted.

The released standardized supernova magnitudes already contain the survey's standardization and bias-correction choices. Reusing them tests CoDE4 conditional on those choices; it does not independently validate or retrain them.

## 5. Pre-register and report the comparison

Before examining a designated holdout, freeze the CoDE4 parameters, data cuts, covariance treatment, and holdout rule. Use no CMB-derived (H_0) or SH0ES (H_0) scalar as an input to the local ladder fit.

Report separately:

- baseline SH0ES-matrix recovery;
- CoDE4 ladder result and the exact changed equations;
- which observations calibrate each rung;
- training and held-out supernova residuals and predictive coverage;
- sensitivity to anchor choice and supernova standardization assumptions;
- uncertainty conditional on fixed model inputs versus uncertainty after propagating their fit.

A local distance-ladder fit is an inference from local calibration data, not a theory-only prediction. Planck's CMB value is a separate early-universe inference under base (Lambda)CDM; disagreement between the two does not by itself establish that either pipeline is wrong.

## 6. Independent checks needed before claiming validation

Where the released products permit it, repeat the ladder using anchor subsets (Milky Way, LMC, NGC 4258), and compare with independent distance indicators or calibrations such as TRGB/JAGB. Recomputing the Cepheid and supernova standardization from raw photometry requires the corresponding light-curve data, calibration products, simulations, and covariance; it cannot be established by changing (H(z)) alone.

## Sources

- SH0ES three-rung method: [Riess et al. (2022)](https://arxiv.org/abs/2112.04510)
- Public SH0ES/Pantheon+ matrices and data: [PantheonPlusSH0ES/DataRelease](https://github.com/PantheonPlusSH0ES/DataRelease)
- Planck 2018 base-(Lambda)CDM inference: [Planck Collaboration (2020)](https://arxiv.org/abs/1807.06209)
