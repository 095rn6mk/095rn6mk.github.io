# MRI Physics Primer: Narration Script

Oct 29, 2026

*Read conversationally. Italic lines are on-screen cues.*

## Objectives and conventions

By the end of this primer you should be able to:

1.  Describe the classical model of a nuclear spin and its precession in a static field.

2.  Explain how a resonant B₁ pulse tips the net magnetization, and compute the flip angle.

3.  Write the Bloch equations and the T₁ and T₂ relaxation they describe.

4.  Distinguish T₂, T₂′ and T₂\*, and state which of them a spin echo refocuses.

5.  Write the spin-echo signal model, choose an echo time, and explain how a multi-echo acquisition yields a T₂ map.

Conventions used throughout:

-   SI units. Vectors are bold in equations, and B₀ points along z.

-   For ¹H, γ > 0. A moment in B₀ therefore precesses clockwise as seen from +z.

-   The rotating frame has axes x′, y′, z. It turns about z at the RF frequency, in the same sense as the precession.

-   Signal amplitudes S are in arbitrary units.

| Symbol | Quantity | SI unit |
|:--|:--|:--|
| L | spin angular momentum | J·s |
| μ | magnetic moment | A·m² |
| γ | gyromagnetic ratio | rad·s⁻¹·T⁻¹ |
| B₀, B₁ | static field, RF field amplitude | T |
| ω₀, f₀ | Larmor angular frequency, frequency | rad·s⁻¹, Hz |
| M, M₀ | net magnetization, its equilibrium value | A·m⁻¹ |
| ρ | proton number density | m⁻³ |
| α, τ | flip angle, pulse duration | rad, s |
| T₁, T₂, T₂′, T₂\* | relaxation times | s |
| TE, TR | echo time, repetition time | s |

## 1. Introduction: tiny magnets in a big magnet

This primer covers the physics needed before the qMRLab practical: what a scanner measures, why that signal decays, and how a spin echo isolates the part of the decay that reports on tissue. At the end we will have the signal model S(TE) = S₀ exp(−TE/T₂) that we fit in the practical.

A hydrogen nucleus, a single proton, has spin and a magnetic moment, so it behaves like a small bar magnet spinning about its axis. MRI places the tissue in a strong static field, uses short radio-frequency (RF) magnetic fields to change the orientation of the resulting net magnetization, and records the voltage that the evolving magnetization induces in a receiver coil.

Hydrogen is the nucleus of choice for two reasons. Water and fat contain it in great abundance, about 6.7 × 10¹⁹ protons per mm³ in pure water. And ¹H has the largest gyromagnetic ratio of the stable nuclei, which makes its signal comparatively strong.

Tissues differ in proton density and in how their local magnetic environment shapes the signal after excitation. Measuring those differences gives image contrast, and maps of them help clinicians separate, for example, healthy tissue from a lesion.

The plan: sections 2 to 5 model a single spin and then an ensemble; section 6 treats excitation; section 7 treats relaxation; sections 8 and 9 treat dephasing and the spin echo; sections 10 and 11 turn these into T₂-weighted imaging and T₂ mapping.

## 2. The classical model of spin

We model the nucleus as a sphere of mass m and charge q spinning about an axis. The picture is inexact, as it obscures the quantum nature of spin, but it gives the correct form of the equations of motion.

The sphere has a spin angular momentum **L** (J·s) along the rotation axis. The rotating charge is a current loop, so it also has a magnetic moment **μ** (A·m²) along the same axis. The two are proportional:

$$\boldsymbol{\mu} = \gamma\,\mathbf{L}, \qquad \gamma_{\text{classical}} = \frac{q}{2m}$$

The constant γ is the gyromagnetic ratio, in rad·s⁻¹·T⁻¹, usually quoted as γ/2π in MHz/T. Because μ and L are parallel, a change of the magnetic moment is a change of angular momentum, and this link produces precession in section 4.

The classical value of γ/2π for a proton is 7.62 MHz/T, whereas the measured value for ¹H is

$$\frac{\gamma}{2\pi} = 42.577\ \mathrm{MHz/T}, \qquad \gamma = 2.675\times 10^{8}\ \mathrm{rad\,s^{-1}\,T^{-1}}$$

The ratio, 5.586, is the proton g-factor and accounts for the quantum nature of the spin. We keep the relation μ = γL with the measured γ and do not use the classical model beyond this point.

![](media/image1.png){width=3.5in}

*Figure 1, the classical model of a hydrogen nucleus.*

## 3. The static field B₀

Without an applied field, the moments of the nuclei in a piece of tissue point in random directions. Their vector sum, the net magnetization of section 5, averages to zero, so there is no signal to detect.

In a field B₀ along the z axis, a moment has energy E = −μ·B₀. The energy is lowest when μ is parallel to B₀ and highest when antiparallel. For a proton at 3 T the two states differ by 8.5 × 10⁻²⁶ J, whereas thermal energy at 310 K is kT = 4.3 × 10⁻²¹ J. The preference for alignment is therefore only a small statistical bias, quantified in section 5. Individual moments do not fall into line, because conservation of angular momentum makes them precess (section 4).

B₀ is produced by a superconducting magnet cooled with liquid helium. Scanners are named after their field strength: a 3 T scanner has B₀ = 3 T. Clinical scanners operate at 1.5 T or 3 T, and research systems at 7 T and above. For scale, the Earth's field is 25 to 65 µT, so a 3 T scanner is 5 × 10⁴ to 1.2 × 10⁵ times stronger.

## 4. Precession and the Larmor frequency

The torque on a moment in a field **B** is **μ** × **B**, and torque is the rate of change of angular momentum. With **L** = **μ**/γ,

$$\frac{d\boldsymbol{\mu}}{dt} = \gamma\,\boldsymbol{\mu}\times\mathbf{B}$$

For a static field **B** = B₀ **z** and an initial moment **μ**(0), the solution is, with ω₀ = γB₀,

$$\mu_\perp(t) = \mu_\perp(0)\,e^{-i\omega_0 t}, \qquad \mu_z(t) = \mu_z(0)$$

where μ⊥ = μx + iμy is the transverse component written as a complex number. The tilt angle between **μ** and **B₀** never changes. The moment sweeps out a cone about **B₀**, clockwise as seen from +z, at the Larmor frequency

$$\omega_0 = \gamma B_0 \qquad\Longleftrightarrow\qquad f_0 = \frac{\gamma}{2\pi}\,B_0$$

which is independent of the tilt angle. This motion is precession. The moment does not align with the field because the torque only changes the direction of **L**, not its length. Alignment would require the moment to give up energy to its surroundings, which is relaxation (section 7).

Numerically, f₀ = 42.577 MHz/T × B₀, which gives 63.87 MHz at 1.5 T, 127.73 MHz at 3 T and 298.04 MHz at 7 T. These are radio frequencies, which is why the pulses that excite the spins are called RF pulses.

![](media/image4.png){width=3in}

## 5. Net magnetization

A voxel contains a very large number of nuclei, and the scanner measures their combined effect. At thermal equilibrium the populations of the parallel and antiparallel states follow Boltzmann statistics. Because γħB₀ ≪ kT, the fractional excess of parallel spins is

$$\frac{N_{\uparrow}-N_{\downarrow}}{N} \approx \frac{\gamma\hbar B_0}{2kT} = 9.9\times 10^{-6} \qquad (3\ \mathrm{T},\ 310\ \mathrm{K})$$

This is only about 10 per million, but pure water holds 6.7 × 10¹⁹ protons per mm³, so about 6.6 × 10¹⁴ spins per mm³ are net aligned. That is what makes the signal measurable.

The quantity of interest is therefore the net magnetization, the total magnetic moment per unit volume (A/m):

$$\mathbf{M} = \frac{1}{V}\sum_{i\in V}\boldsymbol{\mu}_i$$

At equilibrium **M** points along **B₀** with magnitude

$$M_0 = \frac{\rho\,\gamma^2\hbar^2\,B_0}{4kT}$$

M₀ is proportional to the proton density ρ, so water-rich tissue gives more signal. It is proportional to B₀, which is why higher-field scanners have more signal. For pure water at 3 T and 310 K, M₀ = 9.3 × 10⁻³ A/m.

If the field is uniform over the volume, **M** obeys the same equation as a single moment, d**M**/dt = γ **M** × **B**. In B₀, the longitudinal component Mz is constant, and the transverse magnetization, written as the complex number Mxy = Mx + iMy, rotates clockwise:

$$M_{xy}(t) = M_{xy}(0)\,e^{-i\omega_0 t}$$

In general, the precession frequency is set by the field the spins actually experience, ω = γB. If two parts of a sample experience fields that differ by ΔB, their frequencies differ by Δω = γΔB. For example, a field offset of 0.1 ppm of B₀ at 3 T is 0.3 µT, which shifts the frequency by 12.8 Hz and accumulates a phase difference of 1.6 rad in 20 ms. This is the origin of dephasing, treated in section 8.

## 6. Excitation and the flip angle

A static magnetization along **B₀** induces no voltage in a receiver coil, because Faraday induction responds only to a changing flux. What we can detect is a precessing transverse magnetization, whose amplitude is proportional to |Mxy|. The magnetization must therefore be tipped away from z. This is done by resonance, with a second field **B₁**(t) that

-   is perpendicular to **B₀**,

-   varies in time, and

-   oscillates at a frequency ω close to ω₀, so that it stays perpendicular to the precessing **M** and exerts a torque in the same direction throughout.

B₁ is of the order of microtesla, about 10⁻⁶ of B₀, and such a weak field acts strongly only at resonance. After the pulse the tipped magnetization keeps precessing at ω₀ and induces the signal that we record.

To describe the tipping, we change to a frame that rotates about z at the RF frequency ω. In this rotating frame **B₁** is static along an axis x′, and the spins obey d**M**/dt = γ **M** × **B**eff with

$$\mathbf{B}_{\mathrm{eff}} = \frac{\omega_0-\omega}{\gamma}\,\hat{\mathbf{z}} + B_1\,\hat{\mathbf{x}}'$$

On resonance (ω = ω₀) the first term vanishes, and **M** precesses about x′ at ω₁ = γB₁. A pulse of duration τ therefore rotates **M** by the flip angle

$$\alpha = \gamma B_1\,\tau \qquad \left(\text{in general } \alpha = \gamma\int_0^{\tau} B_1(t)\,dt\right)$$

which does not depend on B₀. Starting from equilibrium, **M** = (0, 0, M₀),

$$M_z = M_0\cos\alpha, \qquad M_{y'} = M_0\sin\alpha$$

A 90° pulse puts all the magnetization into the transverse plane, and a 180° pulse inverts it (Mz = −M₀). The signal is proportional to M₀ sin α, which is largest at 90°.

*On screen: Figure 3, snapshots of M tipping about x′ for α = 0°, 30°, 60°, 90°, plus a plot of Mz and Mxy against α (cosine and sine curves).*

![Figure 3: in the rotating frame B1 rotates M about x-prime; Mz and Mxy follow cosine and sine of the flip angle](media/image6.png){width=6in}

## 7. Relaxation and the Bloch equations

After the pulse, the magnetization returns to its equilibrium value **M₀** along z. This is relaxation, and it is described by two time constants. The longitudinal component recovers with the spin-lattice time T₁, which governs the exchange of energy with the surroundings. The transverse component decays with the spin-spin time T₂, which governs the loss of phase coherence between the spins. The rates are R₁ = 1/T₁ and R₂ = 1/T₂.

Adding these two terms to the precession equation of section 5 gives the Bloch equation (Bloch, 1946):

$$\frac{d\mathbf{M}}{dt} = \gamma\,\mathbf{M}\times\mathbf{B} - \frac{M_x\hat{\mathbf{x}} + M_y\hat{\mathbf{y}}}{T_2} - \frac{(M_z - M_0)\,\hat{\mathbf{z}}}{T_1}$$

For a static field **B** = B₀ **z** and the magnetization right after an ideal 90° pulse, **M**(0) = (M₀, 0, 0), the solution is

$$\begin{aligned} M_x(t) &= M_0\,e^{-t/T_2}\cos(\omega_0 t) \\ M_y(t) &= -M_0\,e^{-t/T_2}\sin(\omega_0 t) \\ M_z(t) &= M_0\left(1 - e^{-t/T_1}\right) \end{aligned}$$

The transverse part rotates at ω₀ while its length decays as exp(−t/T₂), and Mz recovers as 1 − exp(−t/T₁). At t = T₁, Mz has recovered to 63% of M₀. At t = T₂, the transverse magnetization has fallen to 37% (1/e) of its initial value. In tissue T₂ < T₁.

Both times depend on the tissue, which is why they generate contrast. Illustrative values at 3 T:

| Tissue | T₁ (ms) | T₂ (ms) |
|:--|--:|--:|
| White matter | 830 | 70 |
| Gray matter | 1330 | 90 |
| CSF | 4000 | 2000 |

These are rounded values chosen for the figures, not the result of a single study. Reported 3 T values depend on region, sequence and analysis. For example, [Mumuni et al. (2015)](https://core.ac.uk/download/478377305.pdf) report white-matter T₂ of 121 to 131 ms in frontal and parietal regions, well above the 70 ms used here.

*On screen: Figure 4, two graphs side by side. Left, Mz(t) rising to M₀ for three tissues. Right, Mxy(t) decaying, with the 1/e point marked at t = T₂. Then Figure 5, a 3D plot of the Bloch solution as a spiral. For readability, the figure uses a precession frequency far lower than a real ω₀ and exaggerates the ratio T₁/T₂.*

![Figure 4: T1 recovery of Mz and T2 decay of Mxy for white matter, gray matter and CSF at 3 T](media/image7.png){width=6in}

![Figure 5: solution of the Bloch equations, a spiral from (M0,0,0) back to (0,0,M0)](media/image8.png){width=4.2in}

## 8. Dephasing: T₂, T₂′ and T₂\*

In a real voxel the spins do not all experience the same field. Each group of spins precesses at its own frequency, offset from the average by Δω = γΔB. Right after the 90° pulse their phases are equal. They then spread out, and the vector sum that the coil measures shrinks, even though no single spin has lost any magnetization. This is dephasing.

Dephasing is sorted by what a 180° pulse does to it, which defines three decay times.

-   **T₂** is the decay time caused by fluctuating fields (molecular motion and interactions between spins). A 180° pulse does not refocus it.

-   **T₂′** is the decay time caused by offsets that stay constant, such as imperfections of the main field, susceptibility differences at air-tissue interfaces and around iron, and chemical shift. A 180° pulse refocuses it completely (section 9).

-   **T₂\*** is the decay time of the free induction decay (FID), the signal recorded right after the excitation without refocusing. Both kinds of dephasing act on it.

Their rates add:

$$\frac{1}{T_2^*} = \frac{1}{T_2} + \frac{1}{T_2'}$$

so T₂\* is always shorter than T₂. T₂ is a property of the tissue, whereas T₂\* also depends on the shim, the voxel size and the orientation of the structure relative to B₀.

*On screen: the unit-circle animation from the multi-echo video, from t = 0 to the first 180° pulse, with the net arrow in black. See Figure 6.*

![Figure 6: stills from the animation at t = 0 (aligned), t = 8 ms (dephasing) and t = 20 ms (echo)](media/image9.png){width=4in}

## 9. The spin echo and the signal model

The spin echo exploits the fact that the offsets of T₂′ are constant. After the 90° pulse, the spins fan out for a time TE/2. A 180° pulse about x′ then reverses the phase of every spin, φ → −φ. Each spin keeps precessing at its own constant rate, so the phases reconverge, and at t = TE all spins are aligned again. This is the echo (Hahn, 1950). The assumptions are that the offsets are constant and that the pulses are ideal. The pulse does not refocus the random dephasing, which continues to accumulate and defines T₂.

The echo amplitude therefore contains T₂ but not T₂′:

$$S(\mathrm{TE}) = S_0\,e^{-\mathrm{TE}/T_2}, \qquad S_0 \propto \rho\left(1 - e^{-\mathrm{TR}/T_1}\right)$$

Here S₀ collects the proton density ρ, the T₁ recovery over the repetition time TR (assuming TR ≫ TE), and a constant for coil sensitivity and receiver gain.

TE is chosen by the operator, and it sets where on the decay curve we sample. For two tissues with T₂A > T₂B and equal S₀, the signal difference ΔS = S₀(e^{−TE/T₂A} − e^{−TE/T₂B}) is zero at TE = 0, peaks, and falls again as both signals vanish. The maximum is at

$$\mathrm{TE}_{\mathrm{opt}} = \frac{T_{2A}\,T_{2B}}{T_{2A} - T_{2B}}\,\ln\frac{T_{2A}}{T_{2B}}$$

which is 83 ms for T₂A = 100 ms and T₂B = 70 ms. The noise in a magnitude image does not depend on TE while the signals decrease, so the signal-to-noise ratio (SNR) falls with every millisecond. The choice of TE is therefore a trade-off between contrast and SNR.

*On screen: Figure 7. Left, SA and SB with ΔS shaded and TEₒₚₜ marked. Right, ΔS (normalized), which peaks at TEₒₚₜ, and SA, which decreases monotonically.*

![Figure 7: contrast between two tissues peaks at an optimal TE while SNR falls monotonically](media/image10.png){width=6in}

## 10. T₂-weighted versus quantitative T₂ imaging

A T₂-weighted image is acquired at one echo time and a long TR, chosen to maximize the contrast of section 9. Its pixel value is P = S₀ e^{−TE/T₂}, which still contains the proton density, the T₁ factor and the coil sensitivity and gain. The image is in arbitrary units and is only meaningful relative to other pixels in the same image.

Quantitative T₂ mapping estimates T₂, in milliseconds, in every voxel. The signal model has two unknowns, S₀ and T₂, so at least two echo times are needed. Because S₀ absorbs the other factors, T₂ does not depend on them, so the map can be compared between visits, participants and sites, provided the acquisition and the fit are controlled.

| | T₂-weighted imaging | Quantitative T₂ mapping |
|:--|:--|:--|
| Acquisition | One echo time | Two or more echo times |
| Pixel value | Arbitrary intensity | T₂ in ms |
| Comparison across sessions and sites | Only relative, within one image | Possible when protocol and fit are controlled |
| Best for | Detecting and displaying lesions | Measuring and tracking tissue change |

*On screen: a T₂-weighted slice next to a T₂ map of the same slice, with a colour bar in milliseconds and the same ROI marked on both.*

## 11. The multi-echo spin echo sequence and T₂ fitting

A multi-echo spin echo (MESE) sequence collects several echoes after a single excitation. A 90° pulse is followed by a train of 180° refocusing pulses separated by the echo spacing ΔTE, and one echo is recorded after each pulse, at

$$\mathrm{TE}_n = n\,\Delta\mathrm{TE}, \qquad n = 1, 2, \dots, N$$

Because all echoes follow one excitation, S₀ is the same for every echo. The refocusing pulses are usually phase-shifted by 90° relative to the excitation (the CPMG sequence; Carr and Purcell, 1954; Meiboom and Gill, 1958), which makes the echoes robust against small errors in the 180° flip angle.

The echo peaks trace out the exponential envelope e^{−t/T₂}. In each voxel the data are

$$S_n = S_0\,e^{-\mathrm{TE}_n / T_2} + \text{noise}$$

and T₂ fitting means finding the S₀ and T₂ that best explain them in the least-squares sense:

$$(\hat S_0, \hat T_2) = \arg\min_{S_0,\, T_2} \sum_{n=1}^{N} \left( S_n - S_0\, e^{-\mathrm{TE}_n / T_2} \right)^2$$

There are two common ways to do this. A straight-line fit of ln Sn against TEn has slope −1/T₂. Alternatively, the exponential can be fitted directly by nonlinear least squares. With only two echoes there is a closed form,

$$T_2 = \frac{\mathrm{TE}_2 - \mathrm{TE}_1}{\ln\left(S_1/S_2\right)}$$

but more echoes average the noise and let us check that the decay is a single exponential.

Two cautions. Magnitude images have a noise floor, which biases the weak late echoes upward and T₂ upward (Gudbjartsson and Patz, 1995). And imperfect 180° pulses, for example because of B₁ inhomogeneity, create stimulated echoes that distort the simple exponential.

This is all the physics we need. In the next part of the tutorial, we load a multi-echo dataset in qMRLab and fit T₂ voxel by voxel.

*On screen: Figure 8, the multi-echo timing diagram with the echo train, and a fit curve through the echo amplitudes in one voxel. Then cut to the qMRLab window.*

![Figure 8: multi-echo timing diagram and a mono-exponential fit through the echo amplitudes](media/image11.png){width=5.5in}

## Sources

-   Bloch, F. (1946). Nuclear induction. *Physical Review* 70, 460--474.

-   Hahn, E. L. (1950). Spin echoes. *Physical Review* 80, 580--594.

-   Carr, H. Y. and Purcell, E. M. (1954). Effects of diffusion on free precession in nuclear magnetic resonance experiments. *Physical Review* 94, 630--638.

-   Meiboom, S. and Gill, D. (1958). Modified spin-echo method for measuring nuclear relaxation times. *Review of Scientific Instruments* 29, 688--691.

-   Gudbjartsson, H. and Patz, S. (1995). The Rician distribution of noisy MRI data. *Magnetic Resonance in Medicine* 34, 910--914.

-   Mumuni, A. N., McLean, J. and Condon, B. (2015). [Regional cerebral relaxation times measured by magnetic resonance imaging at 3.0 Tesla](https://core.ac.uk/download/478377305.pdf). *Journal of Science and Technology (KNUST)* 35(2), 68--76.

-   Jenkinson, M., Chappell, M. and Okell, T. *Short Introduction to MRI Physics for Neuroimaging*. Oxford Neuroimaging Primers, appendix.

-   Levesque, I. R. *MDPH 607: Medical Imaging, Part 4: Nuclear Magnetic Resonance*. McGill University, Fall 2026 course notes.
