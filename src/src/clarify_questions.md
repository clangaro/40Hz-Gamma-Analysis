## **A. Clarify the Scientific Questions**

### **A1. Explicit Statistical Questions**

#### **1. Circadian Rhythms**

The circadian analysis addresses the following questions:

* Does 40 Hz gamma light stimulation change any circadian parameter compared with control light, beyond baseline variation?
* Does the **change** from PRE → POST differ between Light Groups (i.e., is there a **Light × PRE/POST** interaction)?
* Are any effects modulated by Age or Age Group?

#### **2. Barnes Maze (Spatial Learning & Memory)**

The Barnes maze analysis evaluates:

* Whether 40 Hz stimulation improves acquisition (learning curve across trials) and/or probe performance (memory) relative to controls.
* Whether 40 Hz stimulation affects path efficiency (latency, distance moved) and/or search strategy (quadrant times, target errors).
* Whether behavioural effects are stable across days/trials or specific to early versus late learning phases.

#### **3. Novel Object Recognition (NOR)**

Key NOR-related questions include:

* Whether 40 Hz stimulation alters recognition memory, such as the discrimination index or novel object preference, relative to controls.

#### **4. T-maze**

The T-maze analysis aims to determine:

* Whether 40 Hz stimulation alters performance (e.g., spontaneous alternation rate, correct choice rate, latency) relative to controls.

#### **5. Global Question Across Domains**

A broader integrative question:

* Across all tasks and circadian measures, is there **any statistically and biologically meaningful effect** of 40 Hz stimulation on behavioural or circadian systems?

---

## **A2. Implicit Scientific Questions**

Additional implicit questions inherent in the design include:

* Whether effects of 40 Hz stimulation remain robust after controlling for Age (continuous and Age Group), Sex, and baseline performance.
* Whether effects of Light depend on baseline circadian health (e.g., PRE RA/IV/IS levels).
* Whether circadian changes induced by 40 Hz stimulation correlate with behavioural improvements across domains.
* Whether observed effects are reproducible across cohorts or batches.

---

## **A3. Candidate Dependent and Independent Variables**

### **Independent Variables (IVs / Predictors)**

* **Light Group**: e.g., 40 Hz vs control; potentially OFF vs 40 Hz vs other frequencies — categorical, between-subject.
* **PRE/POST**: circadian epoch before vs after stimulation — categorical, within-subject.
* **Age at Start (Month)**: continuous.
* **Age Group**: categorical (e.g., young, middle-aged, aged).
* **Sex**: categorical (M/F).
* **Trial / Day / Session**: categorical or numeric (for modelling learning trajectories).
* **Task-specific predictors:**

  * Barnes maze: Trial number, Day, Phase (training vs probe).
  * NOR: Object type (novel vs familiar), Session.
  * T-maze: Trial number, Session, Arm type.

---

### **Circadian Dependent Variables (DVs)**

* **Amplitude** (cosinor or fitted rhythm amplitude) — continuous.
* **Period** — continuous, typically near 24 h.
* **Phase** — circular (acrophase; timing within the 24-h cycle).
* **Mean** — overall activity level.
* **% Variance explained** — circadian model fit quality.
* **MESOR** — circadian mean level.
* **F, p** — diagnostic metrics from rhythm fits (not treated as primary biological outcomes).
* **RA** — relative amplitude (0–1).
* **IV** — intradaily variability.
* **IS** — interdaily stability.

---

### **Barnes Maze Dependent Variables**

* **Entry_latency** — continuous, typically right-skewed.
* **DistanceMoved_cm** — continuous.
* **EntryZone_nose_freq** — count (entries).
* **Target visit errors** — count (errors).
* **Quadrant durations (% or proportions)** — compositional time spent in each quadrant.
* Additional goal-box measures may also be analysed.

---

### **NOR Dependent Variables (typical)**

* **Time exploring novel vs familiar object** (seconds).
* **Novel preference index** = novel / (novel + familiar).
* **Discrimination index** = (novel − familiar) / (novel + familiar).

---

### **T-maze Dependent Variables (typical)**

* **Correct choice / alternation** (0/1 per trial) — binary.
* **Proportion correct / alternation rate per session** — proportion.
* **Choice latency** — continuous.

---

## **A4. Likely Confounds / Nuisance Variables**

Factors that may bias estimates of stimulation effects and therefore must be checked or modelled:

* **Age at Start / Age Group**: ageing may alter responsiveness to 40 Hz light.
* **Sex**: may modulate both circadian and behavioural outcomes.
* **Baseline differences**: PRE circadian metrics and initial behavioural performance may differ by group by chance; modelling change scores or including baseline values as covariates is recommended.
* **Cohort / Batch / Cage effects**: mice housed together experience shared environments that may introduce pseudo-replication.
* **Handling, experimenter identity, time of testing**: systematic variation may bias outcomes.
* **Order effects**: behavioural task order may influence performance if not counterbalanced.
* **Missing data patterns**: attrition or incomplete data may deviate from missing-at-random assumptions.

To address these, covariates and random effects should be specified appropriately, ensuring repeated observations at the mouse level (and, if necessary, cage/cohort level) are treated correctly.

---

# **B. Evaluation of Data Structure**

## **B1. Variable Types**

* **Categorical (nominal)**: Light Group, Sex, Age Group, ID, Trial (if categorical), Cohort, Cage.
* **Categorical (ordinal)**: Trial number, Day (if treated as ordered).
* **Continuous**: Amplitude, Period, Mean, MESOR, RA, IV, IS, distance, latency.
* **Proportions / percentages**: Quadrant durations, NOR indices, % Variance.
* **Counts**: nose entries, target errors, exploratory counts.
* **Binary**: correct vs incorrect responses (T-maze).
* **Circular**: Phase (acrophase, rhythmic timing).

---

## **B2. Within-Subject vs Between-Subject Structure**

### **Between-subject factors**

Light Group, Age Group, Sex, Cohort/Cage.

### **Within-subject repeated factors**

PRE/POST (circadian), multiple Barnes trials, NOR sessions (if applicable), multiple T-maze trials.

Because each subject contributes multiple observations, the data are **longitudinal**, and inference depends on correctly handling repeated-measures structure.

---

## **B3. Repeated-measures and Random Effects**

The following structure is appropriate:

* **Random intercepts for Mouse ID** — essential for all repeated-measures analyses.
* **Random slopes** (e.g., for Trial in Barnes maze) — appropriate when sufficient repeated measures per mouse exist.
* **Random intercepts for Cohort/Cage** — recommended if metadata are available.

---

## **B4. Assumptions for Parametric Models**

Mixed-effects modelling assumptions include:

* Approximate normality of residuals (achievable through transformation if needed).
* Linearity of relationships between predictors and outcomes.
* Homogeneity of residual variance.
* Independence of residuals after accounting for random effects.

Typical considerations:

* Latency and distance measures are often right-skewed → log-transform or use Gamma GLMM.
* Proportions bounded between 0 and 1 may require logit or Beta regression.
* Circular data require specialised circular models or transformations.
* Count data require Poisson or negative binomial models.
* Small sample sizes necessitate caution regarding over-parameterisation and may require permutation or bootstrap methods.

---

# **C. Recommended Statistical Models**

## **C1. Circadian Parameters**

For continuous outcomes (Amplitude, RA, IV, IS, etc.), the general linear mixed-effects model is:

[
Y_{ij} = \beta_0 + \beta_1\text{Light}_i + \beta_2\text{Time}_j + \beta_3(\text{Light}_i\times\text{Time}_j) +
\beta_4\text{AgeGroup}*i + \beta_5\text{Sex}*i + b*{0i} + \varepsilon*{ij}
]

The primary term is the **Light × PRE/POST** interaction.

### **Phase (circular)**

* Convert phase to radians
* Compute circular means per Mouse × Epoch
* Compute circular shifts (POST − PRE)
* Compare groups using Watson–Williams or permutation tests

Diagnostic F and p values from rhythm models are not used as biological outcomes.

---

## **C2. Barnes Maze Models**

### **Latency and Distance**

Use log-transformed LMM:

[
\log L_{ij} = \beta_0 + \beta_1\text{Light}_i + \beta_2\text{Trial}*j + \beta_3(\text{Light}*i\times\text{Trial}*j) +
b*{0i} + b*{1i}(\text{Trial}) + \varepsilon*{ij}
]

### **Errors (Counts)**

Use Poisson or negative binomial GLMM.

### **Quadrant Durations**

Use logit-transformed LMM or Beta regression.

---

## **C3. Novel Object Recognition**

For recognition index:

[
\text{logit}(RI_i) = \beta_0 + \beta_1\text{Light}_i + \beta_2\text{AgeGroup}_i + \beta_3\text{Sex}_i + \varepsilon_i
]

---

## **C4. T-maze**

Binary outcome:

[
\text{logit}(p_{ij}) = \beta_0 + \beta_1\text{Light}_i + \beta_2\text{Trial}_j + \beta_3(\text{Light}_i\times\text{Trial}*j) + b*{0i}
]

---

## **C5. Multiple Testing Correction**

* Identify primary outcomes (e.g., RA, IV, IS; learning slope; recognition index; alternation rate).
* Use **Benjamini–Hochberg FDR** or **Holm-Bonferroni** for confirmatory inferences.
* Treat other variables as exploratory.

---

## **C6. Non-parametric and Permutation Approaches**

Recommended where assumptions may be violated:

* Permutation tests for Light × PRE/POST interactions.
* Circular non-parametric tests for phase.
* Non-parametric tests for skewed behavioural outcomes.

---

# **D. Data File Requirements**

## **D1. Required Structure for Each CSV**

Detailed specifications for circadian, Barnes, NOR, and T-maze datasets (as provided in the original message) must be followed.

## **D2. Upload Guidelines**

All required datasets should be provided as CSV files, with consistent naming and coding conventions for column variables, enabling seamless ingestion into the provided analysis pipeline.

---

# **E. Python Analysis Pipeline**

The models and statistical steps described above correspond to the Python implementations in the project’s analysis scripts, including:

* Data import and validation
* Cleaning
* Mixed-effects model fitting
* Circular analyses
* Robustness checks
* FDR correction
* Diagnostic plotting

(See **analysis.py** and modules inside `src/`.)

---

# **F. Interpretation Framework**

Upon model estimation, results should be interpreted through the following lens:

### **F1. Meaning of Effects**

* Statistical significance of Light × PRE/POST or Light × Trial interactions
* Directions and magnitudes of estimated effects
* Consistency across related measures (e.g., RA, IV, IS)

### **F2. Evaluation of 40 Hz Effects**

* Distinguish statistically significant effects, trends, and null findings
* Consideration of confidence interval width and statistical power

### **F3. Biological Relevance**

* Effect sizes relative to known biological thresholds
* Integrative interpretation across behavioural and circadian domains
* Coherence with literature on gamma entrainment, neural dynamics, and behaviour

### **F4. Alternative Explanations**

* Potential confounds (Age, Sex, Cohort, baseline differences)
* Regression to the mean
* Task artefacts (orientation, floor/ceiling effects)
* Non-specific stimulation or handling effects

### **F5. Limitations**

* Sample size and power
* Model assumptions
* Missingness
* Generalisability

### **F6. Recommendations for Future Work**

* Replication
* Mechanistic follow-up
* Dose/parameter variation
* Improved counterbalancing
* Enhanced circadian precision or extended behavioural testing

