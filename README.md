# Asthma-COPD Differentiation: Quantifying Phenotype-Definition Leakage

A four-arm feature ablation study using NHANES 2007-2012 data to separate genuine
predictive signal from phenotype-definition leakage when distinguishing asthma from
COPD. Because spirometry is used to *define* the COPD label, models that also receive
spirometry as input report inflated performance; the ablation quantifies how much.

---

## Repository layout

```
src/        Analysis notebooks (run in numbered order)
data/       NHANES .xpt files, by survey cycle (not tracked - see below)
results/    Figures and exported tables
```

---

## Data

This project uses the **National Health and Nutrition Examination Survey (NHANES)**,
cycles **2007-2008**, **2009-2010**, and **2011-2012**. These three cycles were chosen
because they are the last to carry both spirometry and fractional exhaled nitric oxide
(FeNO) examination components.

Nine components are pulled from each cycle:

| Component | Description |
|-----------|-------------|
| `DEMO` | Demographics |
| `BMX`  | Body measurements |
| `CBC`  | Complete blood count |
| `ENX`  | Exhaled nitric oxide |
| `MCQ`  | Medical conditions |
| `OCQ`  | Occupation |
| `RDQ`  | Respiratory health |
| `SMQ`  | Smoking |
| `SPX`  | Spirometry |

Cycle suffixes are `_E` (2007-2008), `_F` (2009-2010), and `_G` (2011-2012) — for
example, `DEMO_E.XPT`.

### Getting the data

The `.xpt` files are **not included in this repository**. They are U.S. federal
public-domain works and are redistributed by NCHS, not by us. Download them from the
CDC and place them in the matching cycle folder:

```
Dataset/
├── 2007_2008/   DEMO_E.XPT, BMX_E.XPT, CBC_E.XPT, ...
├── 2009_2010/   DEMO_F.XPT, BMX_F.XPT, CBC_F.XPT, ...
└── 2011_2012/   DEMO_G.XPT, BMX_G.XPT, CBC_G.XPT, ...
```

Download page: https://wwwn.cdc.gov/nchs/nhanes/

The loader walks `Dataset/` recursively and matches on filename, so subfolder naming
is flexible — but all 27 files (9 components x 3 cycles) must be present or the
notebook raises `FileNotFoundError` listing what is missing.

### Citing the data

> Centers for Disease Control and Prevention (CDC), National Center for Health
> Statistics (NCHS). *National Health and Nutrition Examination Survey Data,
> 2007-2008, 2009-2010, and 2011-2012.* Hyattsville, MD: U.S. Department of Health
> and Human Services, Centers for Disease Control and Prevention.
> https://wwwn.cdc.gov/nchs/nhanes/

NHANES is a complex, multistage probability sample. Any population-level estimate
derived from it must use the survey design variables (`WTMEC2YR`, `SDMVPSU`,
`SDMVSTRA`); see the NCHS analytic guidelines before reusing this code for
descriptive work.

---

## Setup

Requires [conda](https://docs.conda.io/) (or mamba/micromamba).

```bash
git clone https://github.com/[ORG]/asthma-copd-v2.git
cd asthma-copd-v2

conda env create -f environment.yml
conda activate asthma-copd
```

Then launch the notebooks:

```bash
jupyter lab
```

Run the notebooks in `Code/` in numbered order. The first builds the analytic cohort;
later ones depend on objects it defines.

To record exactly what you ran, export a lock file:

```bash
conda env export --no-builds > environment.lock.yml
```

---

## Contributing

Contributions are welcome. This repository uses the standard fork-and-pull-request
workflow:

1. **Fork** the repository to your own GitHub account.
2. **Clone** your fork locally and create a branch for your change:
   ```bash
   git checkout -b fix/threshold-selection
   ```
3. **Make your changes** and commit them with a clear message explaining *why*, not
   just what.
4. **Push** the branch to your fork:
   ```bash
   git push origin fix/threshold-selection
   ```
5. **Open a pull request** against the `main` branch of this repository, describing
   what you changed and how you verified it.

A few things that make review faster:

- Keep one logical change per pull request.
- If a change affects reported numbers, say which table or figure moves and by how
  much.
- Do not commit `.xpt` data files or regenerated CSV/PNG outputs — `.gitignore`
  covers these.
- Open an issue first for anything large, so we can agree on the approach before you
  spend time on it.

All participants are expected to follow the [Code of Conduct](CODE_OF_CONDUCT.md).

---

## License and citation

Released under [License](LICENSE) — you are free to use and modify.

The license covers the analysis code only, not the NHANES data.

If you use this work in research, please cite the accompanying paper. Citation
metadata is in [`CITATION.cff`](CITATION.cff), and GitHub will generate a formatted
citation from the "Cite this repository" button on the repository page.