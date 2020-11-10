# **Computation of Bending Magnet Radiation**

## **Introduction**
This part of SRW computes spectral flux per unit surface of radiation emitted by an electron
beam with non-zero emittance in a bending magnet. Typical computation is a spectral flux per
unit surface at fixed photon energy in a transverse plane at some distance from the bending
magnet, or a spectrum vs photon energy at fixed direction (observation point).

## **Assumptions**
The following **assumptions** are made when computing spectral angular distributions of **Bending Magnet Radiation**:
- Electrons are relativistic.
- Radiation from different electrons is incoherent: the flux is proportional to the number of
electrons.
- Only transverse SR polarization components are considered.
- Spectral-angular distribution of the bending magnet radiation is computed in far-field
approximation.

## **Examples**
* **Standard Bending Magnet Radiation**

This example computes spectral flux per unit surface and power per unit surface of synchrotron
radiation emitted by a non-zero emittance electron beam in a bending magnet. The bending
magnet and electron beam parameters are those of ESRF (B = 0.85 T, energy 6 GeV, current
200 mA).
