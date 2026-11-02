# Deriving the SH0ES three-rung equations from the distance definitions

This note derives the ladder relations used in the SH0ES matrix fit, then identifies where a CoDE4 expansion law can enter. It follows the public SH0ES formulation and data products; it does not claim that CoDE4 has independently verified the released anchor or stellar-calibration measurements.

## 1. Distance modulus and geometric anchors

For a source at distance (D) in Mpc, apparent magnitude (m), and absolute magnitude (M),
[
mu equiv m-M=5log_{10}(D/mathrm{Mpc})+25.
]
The geometric anchors provide measured host distance moduli (mu_i^{m anch}). In the SH0ES release these include Milky Way parallax information, the LMC detached-eclipsing-binary distance, and the NGC 4258 water-maser distance.

These distances are empirical inputs. A late-time cosmological (H(z)) law does not recalculate a parallax, eclipsing-binary distance, or maser geometry.

## 2. Cepheid relation: anchor galaxies to supernova hosts

A reddening-reduced Wesenheit apparent magnitude is constructed from the observed Cepheid photometry using the adopted extinction law. The Cepheid period-luminosity-metallicity relation can be written
[
m^W_{H,ij}
=mu_i+M^W_{H,1}
+b_W(log_{10}P_{ij}-1)
+Z_W,Delta[{m O/H}]_{ij}
+epsilon_{ij}.
]
Here (i) labels the host and (j) the Cepheid; (P) is the pulsation period; (M^W_{H,1}), (b_W), and (Z_W) are the Wesenheit zero point, period slope, and metallicity coefficient; and (epsilon) represents measurement scatter and modeled covariance.

For Cepheids in an anchor host, (mu_i) is constrained by its geometric measurement. Those rows calibrate the shared Cepheid relation parameters. For Cepheids in a Type Ia supernova host, the same relation then constrains that host's (mu_i). The transfer assumes the calibrated relation and photometric system are applicable across anchors and hosts, with the adopted metallicity, dust, crowding, and calibration corrections.

## 3. Calibrator supernovae: Cepheid host distances to SN absolute magnitude

Let (m^{m std}_{B,k}) be the released standardized apparent magnitude of calibrator SN (k). In a raw SALT2-style representation, standardization has the schematic form
[
m^{m std}_{B}
=m_B+alpha x_1-eta c+Delta_{m host}+Delta_{m bias},
]
with signs and correction definitions following the release's convention. In this pipeline the released (m_{b,m corr}) product is used as supplied; this analysis does not refit (alpha,eta) or redo the SALT2/BBC processing.

For a calibrator in host (i),
[
m^{m std}_{B,k}=mu_i+M_B+eta_k,
]
where the Cepheids provide (mu_i), (M_B) is the common standardized SN absolute magnitude, and (eta_k) represents uncertainty. This rung calibrates (M_B) from SN hosts whose distances were transferred from the geometric anchors through Cepheids.

## 4. Hubble-flow supernovae: calibrated SN brightness to (H_0)

The theoretical distance modulus is
[
mu_{m th}(z)=5log_{10}(D_L(z)/mathrm{Mpc})+25.
]
For a spatially flat background with the SH0ES redshift-frame convention,
[
D_L(z_{m HD},z_{m HEL})
=(1+z_{m HEL})rac{c}{H_0}
int_0^{z_{m HD}}rac{dz'}{E(z')},
qquad E(z)=H(z)/H_0.
]
Thus a Hubble-flow SN satisfies
[
m^{m std}_{B,k}=M_B+mu_{m th}(z_k)+eta_k.
]
The calibrated (M_B) from rung 2 breaks the SN absolute-magnitude/(H_0) degeneracy, allowing the Hubble-flow sample to determine the local (H_0).

A CoDE4 model enters here by replacing the fiducial (E(z)) and therefore the Hubble-flow (D_L(z)), provided its derivation predicts the same distance-duality and redshift conventions. If CoDE4 predicts curvature or modified photon propagation, the distance equation must be re-derived rather than patched by a shape factor.

## 5. Joint matrix fit

The SH0ES release combines all three rungs and their correlations in one system. In generic design-matrix notation,
[
y=Aq+epsilon,qquad {m Cov}(epsilon)=C.
]
For fixed model-dependent Hubble-flow terms, the generalized least-squares solution is
[
widehat q=(A^T C^{-1}A)^{-1}A^T C^{-1}y.
]
The official release uses its specified parameter ordering and matrix convention; the engine's mapping from the fitted Hubble-flow parameter to (H_0) must follow that release exactly.

If only the distance shape changes while (H_0), (M_B), and the rung equations keep the same parameterization, then the Hubble-flow right-hand side changes by
[
Delta y_k
=-5log_{10}!left(rac{f_{m CoDE4}(z_k)}{f_{m fid}(z_k)}ight),
quad
f(z)=rac{1+z_{m HEL}}{z_{m HD}}
int_0^{z_{m HD}}rac{dz'}{E(z')}.
]
This sign follows from (y_k=m^{m std}_{B,k}-mu_{m shape}(z_k)-25): a larger model distance shape lowers that residual. All anchor, Cepheid, and calibrator-SN rows remain empirical inputs unless the CoDE4 theory specifically predicts changes to those physical measurements or relations.

## What this derivation does and does not establish

It derives the ladder's algebraic links from the distance-modulus definition and the Cepheid/SN standard-candle relations. It does not prove that the anchor distances, Cepheid universality, photometric calibration, SN standardization, or selection-bias corrections are correct. The present CoDE4 matrix run inherits those from the official release; reproducing the published baseline is an implementation check, not an independent validation of the inputs.

A complete model-specific change to rung 1 or 2 would require a physical derivation for how CoDE4 affects the corresponding geometric or stellar observable, followed by a fit to the relevant raw data and covariance. Merely replacing (E(z)) in the Hubble-flow integral does not justify changing the Cepheid relation.

## Sources

- Riess et al., [A Comprehensive Measurement of the Local Value of the Hubble Constant](https://arxiv.org/abs/2112.04510)
- SH0ES public matrix description and data: [PantheonPlusSH0ES/DataRelease](https://github.com/PantheonPlusSH0ES/DataRelease)
