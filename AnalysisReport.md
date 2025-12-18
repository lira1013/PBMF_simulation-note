## **Simulate Patient Generation and PBMF Model Application Analysis Report**

**Executive Summary**

The AstraZeneca team published an AI-driven predictive biomarker
discovery study (PBMF framework) in Cancer Cell. This report presents an
in-depth analysis of the virtual-patient component and provides critical
references for technical selection and methodological optimization.

AstraZeneca's study achieved three major advances

Innovation: The contrastive-learning-based PBMF framework breaks the
limitation of traditional Cox models that require explicit covariate
enumeration, enabling automated and unbiased predictive-biomarker
discovery while effectively distinguishing predictive from prognostic
markers---addressing challenges in complex therapies such as
immuno-oncology.

Performance: Consistently outperforms mainstream methods (VT, SIDES)
across nine real clinical datasets (including Phase III trials and
real-world data), improving patient survival risk by 10--15%. Model
distillation produces interpretable decision trees ready for clinical
translation.

Standardization: Established a standardized virtual-patient generation
system that provides a fair and controllable validation platform for
biomarker-discovery methods, greatly reducing R&D barriers caused by
scarce real data.

Deep analysis of the virtual-patient module is practically meaningful:
generation does not simply replicate real cases but uses a
"mixed-distribution strategy + feature-independence design +
clinical-core-law replication" to accurately simulate high-dimensional
noise, survival-time distributions, and treatment-response differences.
This ensures precise and reproducible model validation while offering a
standardized benchmark for methodological comparisons. Rigorous
considerations of feature distributions, proportion controls, and
correlation constraints provide a methodological reference for future
virtual-cohort algorithms, especially in "controlled-variable
validation" and "signal-purity assurance."

Although the original virtual-patient system focuses on "methodological
validation" (e.g., 1:1 grouping ratio, feature-independence design) and
is not fully aligned with the team's core demand of
"biomarker-agent-driven clinical-grade virtual-cohort generation,"
systematic analysis identified key optimization directions (introducing
real clinical covariance matrices, adjusting biomarker positivity rates,
adding comorbidities and other heterogeneity features) and produced this
detailed report.

**I. Research Background & Core Value**

**1. Background**

Precise biomarker discovery is central to precision medicine.
Traditional Cox proportional-hazards models require explicit enumeration
of covariates and interactions, struggling with high-dimensional
clinical complexity. To address this, the authors proposed the PBMF
(Predictive Biomarker Discovery Framework) and employed virtual patients
(synthetic data) to overcome scarcity, heterogeneity, and uncertain
predictive signals in real-world data, enabling robust model validation.

**2. Core Value**

Standardized validation platform: Virtual patients simulate core
clinical laws, offering fair comparison environments for PBMF, VT,
SIDES, etc., avoiding real-data bias.

Accelerated R&D iteration: Flexible adjustment of feature distributions,
proportions, and noise rapidly validates model performance under diverse
clinical scenarios.

Clinical-translatability support: Provides preliminary evidence for
predictive biomarkers and assists future clinical-trial design
optimization.

**II. Virtual-Patient Generation Method & Workflow**

**1. Core Logic**

Virtual patients adopt a parametric survival model + mixed-distribution
strategy, centering on "replicate core clinical statistics +
controllable-variable validation." Based on clinical laws
(proportional-hazards assumption, feature-interaction relations),
multi-type distributions are combined to simulate patient data. A
"forced feature-independence" design ensures validation precision rather
than full replication of real cases.

**2. Detailed Workflow**

#### (1) Feature Design & Parameter Constraints

![9564a1595ade594019f8af63e9db554d](media/media/image1.png){width="5.571527777777778in"
height="3.3534722222222224in"}

#### (2) Survival-Time & Data Generation

Distribution: Weibull with shape γ = 1.5 (hazard increases over time,
matching oncology) and scale λ = 0.07 (median survival = 0.07 × (ln
2)\^(1/1.5)).

Time calibration: Latest patient-entry time = 0; accrual window −64 to
0; follow-up 18 months; analysis cutoff −48.

Censoring: Random censoring rate 0.05 (5 %), consistent with real trials
(5--20 %).

Sample size: 10,000 virtual patients for statistical representativeness
and 100 repeated subsamplings.

#### (3) Data Pre-processing & Validation Preparation

Feature processing: Core features binarized via normal-distribution
quantile thresholds (e.g., var1 positivity 75 % corresponds to 25 %
quantile); x3/x4 retained as uniform continuous values; function
binary_to_continuous2 restores clinical continuous characteristics
(e.g., gene expression, tumor size).

Example (p = 0.75)

![75e8b0f2-495e-49ce-aac9-ad9ae64371f9](media/media/image2.png){width="5.7625in"
height="1.625in"}

Data splitting: Randomly draw 2,000 patients per repeat, split 50:50
into training/testing; 100 repeats to reduce random error.

Hidden biomarker: Rename var1 to vard and hide it, validating the
model's ability to identify nonlinear combinatorial signals from raw
features (x3, x4).

**III. Core Applications & Operational Logic of PBMF**

**1. Application Scenarios**

#### (1) Benchmark Testing & Performance Validation

Goal: Verify PBMF's ability to identify predictive biomarkers under
prognostic-signal interference, redundant noise, and nonlinear feature
combinations.

Method: Use virtual patients as test set; compare PBMF vs. VT vs. SIDES;
primary metric: AUPRC.

#### (2) Fair Methodological Comparison

Advantage: Virtual data allow exact control of distributions
(normal/uniform/Weibull), ratios (1:1), noise (999 uninformative
features), and feature-independence---excluding confounding correlations
present in real data.

Focus: Nonlinear combinatorial signal (x3+x4→var1) detection and noise
robustness.

#### (3) Clinical-Scenario Extension

Operation: Vary noise features (7--999), censoring rates, HR parameters,
distribution types to simulate diverse clinical settings
(high-dimension, low event rate, skewed features).

Goal: Demonstrate generalizability and support real high-dimensional
clinical applications.

**2. Key Operational Details**

Model fitting: CoxPH from lifelines library to test treatment×biomarker
interaction.

Significance: Likelihood-ratio test (LRT = 635.70, 6 df, p \< 0.05)
confirms overall model validity; Wald test yields HR p-values.

Visualization: Kaplan--Meier curves visually show survival differences
across subgroups (trt±×var1±).

![36a1fa8f-0c99-4d4a-88b7-853e8f35419e](media/media/image3.png){width="7.290972222222222in"
height="5.397222222222222in"}

**IV. Validation Results & Core Findings**

**1. Virtual-Patient Data Validity**

#### (1) Distribution & Ratio Clinical Plausibility

Distribution fit: Core features cover common clinical distributions
(normal, uniform, Weibull); Weibull (γ = 1.5) accurately mimics
increasing hazard; normal binarization reflects clinical dichotomy;
uniform x3/x4 enables clear var1 grouping via geometric cut.

Ratio plausibility: 1:1 var1 ratio differs from real clinical positivity
(\< 50 %) but is validation-appropriate---balanced groups ensure
sufficient learning and avoid class-imbalance bias; adjustable later.

Feature-independence design: "Forced no correlation" is not ignoring
reality but a staged-validation strategy---test core ability first, then
add complexity;"inducing covariance may be interesting".

Statistical-law fit: Data strictly follow proportional-hazards
assumption; LRT significant (p \< 0.05).

#### (2) Clinical-Core-Feature Fitting Degree

High-fit dimensions (70--80 %): survival-time distribution,
treatment-response difference (HR ≈ 0.50), censoring (5 %),
high-dimensional noise---mirror real scenarios where signals are masked.

Fit limitations (40--50 %): Simplified correlations (e.g., age vs. BP),
no individual heterogeneity (comorbidities, polymorphisms), no skewed
distributions---necessary simplifications for validation, not technical
flaws.

**2. PBMF Performance**

  ----------------------------------------------------------------------------------------------
  **Evaluate**   **Result**                                          **Conclusion**
  -------------- --------------------------------------------------- ---------------------------
  AUPRC          PBMF：0.918±0.047；VT：0.858±0.029；SIDES：Unable   PBMF exhibits the best
                 to resolve any simulated scenarios                  performance and strongest
                                                                     stability in predictive
                                                                     biomarker recognition, and
                                                                     is particularly adept at
                                                                     capturing nonlinear
                                                                     combined signals.

  HR and         Treatment + var1=1 subset HR≈0.50 (95% CI           PBMF can precisely capture
  statistical    clinically significant), P\<0.05 (Wald test)        the interaction between
  significance                                                       treatment and biomarkers,
                                                                     identifying the truly
                                                                     benefiting subgroups.

  KM curve       The survival curve of the treatment group (var1=1+) The biomarkers identified
                 was significantly better than that of other         by PBMF were validated as
                 subgroups, while there was no difference between    \"predictive\" (effective
                 the no-treatment group and the control group        only in the treatment
                 (var1=1).                                           group), rather than
                                                                     \"prognostic.\"

  Noise          Even after adding 999 noise features, PBMF can      PBMF exhibits strong
  robustness     still stably identify trt×var1 interaction signals. robustness in
                                                                     high-dimensional noisy
                                                                     data.
  ----------------------------------------------------------------------------------------------

3.  **Methodological Comparison Conclusion**

    PBMF: Captures nonlinear combinatorial predictive signals, highly
    noise-robust, suitable for high-dimensional clinical data.

    VT: Second-best, effective for linear/simple interactions, less
    stable in complex scenarios.

    SIDES: Fails to detect nonlinear combinatorial signals, completely
    ineffective in simulations.

**V. Conclusions & Limitations**

**1. Core Conclusions**

Virtual-patient method: "Mixed distributions + 1:1 ratio +
feature-independence" delivers controllability, standardization, and
validation precision---forced independence avoids confounding,
accurately tests PBMF core ability; effectively simulates key clinical
statistics (survival, response, noise), providing a reliable validation
tool.

PBMF model: Demonstrates significant advantages in predictive-biomarker
discovery, especially for high-dimensional, noisy, nonlinear-interaction
clinical data.

Prospects: Virtual patients can assist hypothesis verification for
clinical-trial design; PBMF offers clinically translatable biomarker
candidates; covariance-matrix extension for higher clinical fidelity.

2.  **Study Limitations**

    Virtual-patient limits: Feature-independence design differs from
    real highly correlated data; mathematically modeled, lacks
    individual-specific variables (lifestyle, polymorphisms) and skewed
    distributions---simplifications necessary for validation, not
    technical flaws.

    Model-fitting limits: Cox fitting can be further improved with more
    real clinical data (e.g., introduce real covariance matrices).

    Clinical-translatability limits: Current validation based on virtual
    patients and limited real datasets; PBMF-identified biomarkers
    require additional prospective clinical trials for effectiveness
    confirmation.

3.  **Optimization Directions**

    Feature-distribution optimization: Replace uniform x3/x4 with more
    clinical-plausible distributions (e.g., log-normal); calibrate
    mean/SD from real data.

    Ratio & correlation adjustment: Adjust grouping ratio to real
    biomarker positivity; introduce real clinical covariance matrices to
    simulate natural feature correlations, enhancing clinical fidelity.

    Detail supplementation: Add comorbidities, treatment-adherence
    variables to increase individual-heterogeneity simulation.
