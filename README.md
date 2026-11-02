# Wright-Fisher Evolutionary Genetics Simulator 🧬

An interactive Python simulation modeling allele frequency changes over generations under the influence of **genetic drift**, **natural selection**, and **mutation** using the classic Wright-Fisher model. Designed for Google Colab with real-time interactive widgets.

---

## Features
- **Stochastic Genetic Drift:** Models random sampling error in finite populations via binomial distributions.
- **Natural Selection:** Implements selection coefficients ($s$) to simulate advantageous or deleterious alleles.
- **Mutation Rates:** Incorporates recurrent forward and backward mutation rates ($\mu, v$).
- **Interactive UI:** Features `ipywidgets` sliders for real-time adjustment of population size ($N$), selection coefficient ($s$), and initial allele frequencies ($p_0$).
- **Multi-Replicate Trajectories:** Generates comparative "spaghetti plots" to visualize evolutionary divergence, fixation, and loss.

---

## Requirements
The project runs seamlessly in Google Colab or any standard Python environment with the following libraries pre-installed or installable:
- `numpy`
- `matplotlib`
- `ipywidgets`

---

## Quick Start (Google Colab)

1. Open a new notebook in [Google Colab](https://colab.research.google.com/).
2. Copy and paste the core simulation code into your code cell:

```python
import numpy as np
import matplotlib.pyplot as plt
import ipywidgets as widgets
from IPython.display import display

def run_wright_fisher(N, p0, generations, s=0.0, mu=0.0, v=0.0):
    """
    Simulates allele frequency over generations using the Wright-Fisher model.
    """
    frequencies = np.zeros(generations)
    frequencies[0] = p0
    two_N = 2 * N
    
    for t in range(1, generations):
        p = frequencies[t-1]
        
        # Apply mutation
        p_mut = p * (1 - mu) + (1 - p) * v
        
        # Apply selection
        if s != 0:
            w_bar = p_mut**2 * (1 + s) + 2 * p_mut * (1 - p_mut) * (1 + s/2) + (1 - p_mut)**2
            p_sel = (p_mut**2 * (1 + s) + p_mut * (1 - p_mut) * (1 + s/2)) / w_bar
        else:
            p_sel = p_mut
            
        p_sel = np.clip(p_sel, 0.0, 1.0)
        
        # Genetic drift via binomial sampling
        successes = np.random.binomial(two_N, p_sel)
        frequencies[t] = successes / two_N
        
        if frequencies[t] == 0.0 or frequencies[t] == 1.0:
            pass
            
    return frequencies

# Interactive Widget Execution
@widgets.interact(
    N=widgets.IntSlider(min=10, max=1000, step=10, value=100, description='Pop Size (N):'),
    s=widgets.FloatSlider(min=-0.1, max=0.1, step=0.005, value=0.0, description='Selection (s):'),
    p0=widgets.FloatSlider(min=0.05, max=0.95, step=0.05, value=0.5, description='Initial freq (p0):')
)
def interactive_wf(N, s, p0):
    plt.figure(figsize=(10, 5))
    for _ in range(15):
        traj = run_wright_fisher(N=N, p0=p0, generations=100, s=s)
        plt.plot(traj, lw=1.5, alpha=0.8)
    plt.axhline(0, color='gray', linestyle='--')
    plt.axhline(1, color='gray', linestyle='--')
    plt.title(f"Wright-Fisher Simulation: N={N}, s={s}, p0={p0}")
    plt.xlabel("Generations")
    plt.ylabel("Allele Frequency")
    plt.ylim(-0.05, 1.05)
    plt.show()
