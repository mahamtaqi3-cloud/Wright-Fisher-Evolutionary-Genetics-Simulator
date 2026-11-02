# Wright-Fisher Evolutionary Genetics Simulator 🧬

An interactive Python application modeling allele frequency changes over generations under the core evolutionary forces of **genetic drift**, **natural selection**, and **mutation** using the Wright-Fisher model. Optimized for exploration within Google Colab.

---

## Overview

The Wright-Fisher model is a foundational framework in population genetics. This simulator allows you to study how discrete, finite populations evolve over time. By combining stochastic processes with deterministic selection and mutation pressures, the project illustrates key evolutionary dynamics such as allele fixation, loss, and population divergence.

---

## Core Evolutionary Mechanics

- **Stochastic Genetic Drift:** Simulates random sampling error in finite diploid populations ($2N$ chromosomes) using binomial distributions.
- **Natural Selection:** Implements selection coefficients ($s$) to model fitness advantages or disadvantages for specific alleles.
- **Mutation:** Accounts for recurrent forward and backward mutation rates ($\mu, v$) altering allele states before drift and selection take effect.
- **Interactive UI:** Leverages interactive widget sliders for real-time manipulation of population size ($N$), selection coefficient ($s$), and initial frequencies ($p_0$).

---

## Project Structure

```text
├── wright_fisher_simulator.py      # Core simulation functions and statistical engine
├── interactive_ui.py               # Google Colab ipywidgets interface setup
└── requirements.txt                # Required python dependencies
