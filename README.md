# ramanujan-breath

> **What is the probability you just inhaled a molecule once breathed by Srinivasa Ramanujan?**

This project explores a beautiful probabilistic question: given how many molecules of air a person breathes over a lifetime, and how many molecules are in Earth's atmosphere, what is the chance that at least one molecule in your current breath was also breathed by Ramanujan?

The answer uses a Poisson approximation, arriving at the elegant expression **P ≈ e⁻λ**, where **λ = x · A / B**.

| Symbol | Meaning |
|--------|---------|
| `x` | Molecules in a single breath you take now |
| `A` | Total molecules Ramanujan breathed over his lifetime (~32 years) |
| `B` | Total molecules in Earth's atmosphere |

- 📖 [Read the full blog post](https://neelsoumya.github.io/science_blog_fun/ramanujan_breath.html)
- 🌐 [Live R Shiny app](https://sb2333.shinyapps.io/ramanujan_app/)

---

## Repository Structure

```
ramanujan-breath/
├── ramanujan_simple_python.py        # Simple Monte Carlo simulation (CLI)
├── ramanujan_interactive_python.py   # Interactive Jupyter widget with sliders
├── ramanujan_breath_interactive.ipynb # Jupyter notebook (interactive)
├── app.R                             # R Shiny web application
├── requirements.txt                  # Python dependencies
└── README.md
```

---

## Quick Start

### Option 1: Python (Command Line)

Run the simple simulation directly:

```bash
git clone https://github.com/neelsoumya/ramanujan-breath.git
cd ramanujan-breath
pip install -r requirements.txt
python ramanujan_simple_python.py
```

**Expected output:**
```
lambda: 1.0
simulated P(none): 0.3672
exp(-lambda): 0.36787944117144233
```

This prints λ (expected overlap count), the Monte Carlo simulated P(no shared molecule), and the analytical approximation e⁻λ. The close agreement between the simulated and theoretical values confirms the Poisson approximation.

---

### Option 2: Interactive Jupyter Notebook

For an interactive exploration with sliders:

```bash
pip install -r requirements.txt
jupyter lab ramanujan_breath_interactive.ipynb
```

Or run the interactive Python script directly in JupyterLab/Jupyter Notebook:

```bash
jupyter lab
# Then open and run ramanujan_interactive_python.py as a notebook cell
```

This launches a widget with three logarithmic sliders (x, A, B) and a live plot of e⁻λ vs. λ, with a red dashed line showing the current λ value on a log-scale y-axis.

---

### Option 3: R Shiny App (Local)

To run the Shiny app locally you need R and the following packages:

```r
install.packages(c("shiny", "ggplot2"))
```

Then launch:

```r
shiny::runApp("app.R")
```

Or from the terminal:

```bash
Rscript -e "shiny::runApp('app.R')"
```

The app opens in your browser with a dark-themed dashboard. Three sliders control x, A, and B (all on log₁₀ scales). The panel on the right shows the curve of e⁻λ against λ, with a live-updating badge displaying the current λ and P(sharing a molecule).

A hosted version is available at: https://sb2333.shinyapps.io/ramanujan_app/

---

## Installation

### Python

**Requirements:** Python 3.8+

```bash
pip install -r requirements.txt
```

Dependencies installed:

```
scikit-learn
pandas
numpy
matplotlib
seaborn
ipywidgets
jupyterlab
ipykernel
```

### R

**Requirements:** R 4.0+

```r
install.packages(c("shiny", "ggplot2"))
```

---

## The Mathematics

Each breath contains roughly **x = 10,000 – 100,000** molecules. Ramanujan breathed for ~32 years, inhaling roughly **A ~ 10⁶** molecules per breath × ~8 breaths/minute × ~32 years ≈ **~10¹² – 10¹³** molecules total. Earth's atmosphere contains roughly **B ~ 10⁴⁴** molecules.

The fraction of the atmosphere Ramanujan cycled through is **f = A / B**. Each molecule in your breath independently has probability f of being one "he breathed". With x molecules, the expected count of shared molecules is:

```
λ = x · A / B
```

By the Poisson approximation, the probability of zero shared molecules is:

```
P(none) ≈ e⁻λ
```

And therefore:

```
P(at least one shared molecule) = 1 − e⁻λ
```

Depending on parameter assumptions, this probability can be surprisingly high — close to 1.

---

## Files in Detail

### `ramanujan_simple_python.py`

A self-contained script. Sets toy parameter values (`x = 10,000`, `A = 1,000,000`, `B = 10,000,000`), runs a Monte Carlo simulation over 20,000 trials, and compares the simulated probability against the analytical e⁻λ. Uses only the Python standard library (`math`, `random`).

**Customising parameters:**

Edit the bottom of the file:

```python
x = 10_000       # molecules per breath
A = 1_000_000    # total molecules Ramanujan breathed
B = 10_000_000   # molecules in the atmosphere
lam, sim, approx = compare_to_exp(x, A, B, trials=20000)
```

### `ramanujan_interactive_python.py`

Designed to run inside JupyterLab. Uses `ipywidgets.interact` with `FloatLogSlider` controls for x, A, and B. Plots the e⁻λ curve and marks the current λ with a red dashed line. Log scale on the y-axis. Requires `ipywidgets`, `matplotlib`, and `numpy`.

### `ramanujan_breath_interactive.ipynb`

Jupyter notebook version of the above. Open with `jupyter lab` and run all cells.

### `app.R`

A full R Shiny application with a custom dark-mode CSS theme. Provides three log-scale sliders (x_exp, A_exp, B_exp representing powers of 10), live-rendered scientific-notation labels, a λ/e⁻λ badge, and a ggplot2 curve rendered against a dark background. Can be run locally or deployed to shinyapps.io.

---

## Contributing

Contributions, issues, and feature requests are welcome. Feel free to open a pull request or raise an issue on GitHub.

---

## License

GPL-3.0 — see [LICENSE](LICENSE) for details.

---

## Citation 

Forthcoming.
