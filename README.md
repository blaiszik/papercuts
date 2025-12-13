# Papercuts

Standalone tools that solve real friction in AI development and scientific computing. Each papercut is:
- A self-contained project, not a feature request to a large codebase
- Buildable in an afternoon by a skilled developer with AI assistance
- Focused on a specific, repeated frustration

**Want to add a papercut?** [Open an issue](../../issues/new) describing the friction and what a solution might look like.

**Want to solve one?** Pick a papercut, build it, and submit a PR linking to your tool.

---

## Quick Reference

| # | Name | Type | Domain |
|---|------|------|--------|
| 1 | checkpoint-bundle | CLI | ML |
| 2 | tokcount | CLI | LLM |
| 3 | nan-sentinel | CLI/Daemon | ML |
| 4 | unitcalc | CLI | Physics/Chemistry |
| 5 | papenv | CLI | ML |
| 6 | crystconv | CLI | Materials |
| 7 | sqwatch | TUI | HPC |
| 8 | promptgit | CLI | LLM |
| 9 | embviz | CLI/Web | ML |
| 10 | constaudit | CLI | Physics |
| 11 | hdf-diff | CLI | Scientific Data |
| 12 | lablog | CLI | Lab |
| 13 | molsim | CLI | Chemistry |
| 14 | xrdmatch | CLI | Materials |
| 15 | vasp2qe | CLI | Materials/DFT |
| 16 | plotextract | CLI | Scientific Data |
| 17 | synthparse | CLI | Chemistry |
| 18 | jupyckpt | Jupyter Extension | ML |
| 19 | chemstock | CLI | Lab |
| 20 | ragchunk | CLI | LLM |
| 21 | arxiv-plus | Browser Extension | ML |
| 22 | gpu-fleet | Desktop App | ML/HPC |
| 23 | run-compare | Web App | ML |
| 24 | slurm-insights | Web Dashboard | HPC |
| 25 | structure-view | Web App | Materials |
| 26 | tensor-shapes | VS Code Extension | ML |
| 27 | hpc-skeleton | Code Generator | HPC |
| 28 | queue-bot | Slack/Discord Bot | HPC |
| 29 | mol-hover | Browser Extension | Chemistry |
| 30 | spec-annotate | Web App | Chemistry |
| 31 | grove | CLI | Dev Tooling |
| 32 | cite-fetch | CLI | Scientific Writing |
| 33 | env-diff | CLI | ML/Python |
| 34 | traceback-buddy | Web App | Python |
| 35 | notebook-clean | CLI | ML/Python |
| 36 | param-sweep | CLI/Web | ML |

---

## The Papercuts

### 1. checkpoint-bundle

**The problem:** You save a PyTorch/JAX checkpoint, but six months later you can't reproduce the results. The checkpoint has weights but not the code version, config, random seeds, or environment that created it.

**The solution:** A CLI that wraps your training script and creates reproducibility bundles. `checkpoint-bundle run train.py` captures the git hash, full config, pip freeze, hardware info, and random seeds alongside every checkpoint. `checkpoint-bundle reproduce ./checkpoint-bundle-20240115/` recreates the environment and resumes exactly.

---

### 2. tokcount

**The problem:** Estimating LLM costs requires running each provider's tokenizer separately. "How many tokens is this document for Claude vs GPT-4 vs Gemini?" requires three different tools.

**The solution:** A universal token counter. `tokcount document.txt` shows token counts and estimated costs across all major providers in one table. Supports stdin, files, and directories. `tokcount --watch ./prompts/` monitors a directory and alerts when prompts exceed thresholds.

---

### 3. nan-sentinel

**The problem:** Training runs for 8 hours, then you wake up to find loss went NaN at hour 2 and wasted 6 hours of GPU time. There's no built-in tripwire.

**The solution:** A lightweight daemon that watches training logs. `nan-sentinel --log train.log --notify slack` monitors for NaN, loss explosions, or training stalls. Sends alerts via Slack, email, or SMS. Works with any framework—just watches stdout/files for patterns.

---

### 4. unitcalc

**The problem:** Scientific code silently mixes units. You add a value in eV to one in Hartree and get nonsense. Bugs hide for months.

**The solution:** A unit-aware calculator CLI. `unitcalc "5 eV + 0.1 Hartree in kcal/mol"` does the conversion. Refuses to add incompatible units. Knows physical constants with proper units. Can lint Python files to find suspicious dimensionless arithmetic: `unitcalc lint simulation.py`.

---

### 5. papenv

**The problem:** Paper code repos have broken dependencies. `pip install -r requirements.txt` fails because packages are unpinned or yanked. Getting a 2-year-old paper's code running takes hours.

**The solution:** A tool that creates working environments from paper repos. `papenv https://github.com/author/paper-code` analyzes the repo, resolves version conflicts using historical PyPI data, and generates a working Dockerfile or conda environment. Maintains a community database of known-working configurations.

---

### 6. crystconv

**The problem:** Converting between CIF, POSCAR, XYZ, and other crystal structure formats silently drops metadata. You lose symmetry information, occupancies, or bibliographic data without warning.

**The solution:** A structure converter that warns loudly. `crystconv input.cif -o output.poscar` converts but prints exactly what information will be lost. `--strict` mode refuses lossy conversions. `crystconv diff a.cif b.cif` compares structures accounting for equivalent representations.

---

### 7. sqwatch

**The problem:** You submit 500 Slurm jobs and have no good way to monitor them. Which failed? Why? `squeue` and `sacct` output is dense and hard to parse.

**The solution:** A TUI dashboard for Slurm. `sqwatch` shows your jobs in a navigable interface. Filter by status, drill into failed jobs to see error logs, resubmit failed jobs with one keypress. `sqwatch --notify discord` alerts you when jobs complete or fail.

---

### 8. promptgit

**The problem:** Prompt engineering involves lots of iteration, but prompts are scattered across codebases with no version history. "What prompt were we using last week?" requires git archaeology.

**The solution:** A prompt versioning system. `promptgit init` creates a prompt repository. `promptgit save summarizer "Summarize the following..."` versions your prompts. `promptgit diff summarizer` shows how a prompt evolved. `promptgit use summarizer@v3` in your code loads a specific version. Tracks which prompt versions were used in production.

---

### 9. embviz

**The problem:** You generated embeddings but have no quick way to sanity-check them. Are similar items actually close? Are there obvious clusters? Checking requires writing matplotlib code every time.

**The solution:** An instant embedding visualizer. `embviz embeddings.npy --labels labels.txt` opens an interactive UMAP/t-SNE visualization. Hover for labels, search for items, see nearest neighbors. `embviz compare old.npy new.npy` shows how your embedding space changed. Works in terminal (sixel) or browser.

---

### 10. constaudit

**The problem:** Scientific code has hardcoded physical constants with unknown provenance. Is that `6.626e-34` from CODATA 2018? A textbook? Who knows.

**The solution:** A linter for physical constants. `constaudit scan simulation.py` finds hardcoded constants, identifies what they probably are, and reports which standard values they match (or don't). `constaudit replace simulation.py` rewrites them to use `scipy.constants` with proper citations.

---

### 11. hdf-diff

**The problem:** Scientific data lives in HDF5/NetCDF files, but `git diff` shows nothing useful. You can't easily see what changed between two versions of a dataset.

**The solution:** A semantic diff tool for scientific data formats. `hdf-diff old.h5 new.h5` shows which datasets changed, summarizes numerical differences (mean delta, max delta, shape changes), and highlights structural changes. `git config diff.hdf5.command hdf-diff` integrates with git.

---

### 12. lablog

**The problem:** Lab notebooks are either paper (unsearchable) or heavyweight ELN software (slow, overengineered). Quick notes get lost.

**The solution:** A minimal lab notebook CLI. `lablog "Started synthesis of NiFe-LDH, 80°C, 12h"` timestamps and saves to a local SQLite database. `lablog search "NiFe"` finds entries. `lablog today` shows today's notes. `lablog export --bibtex` for when you write the paper. Syncs via git.

---

### 13. molsim

**The problem:** "Find molecules similar to this one" requires ChemDraw, SciFinder access, or writing RDKit code. Quick similarity searches shouldn't need a full cheminformatics setup.

**The solution:** A molecular similarity search CLI. `molsim "CCO" --db pubchem` searches PubChem for molecules similar to ethanol. Returns SMILES, names, and similarity scores. `molsim cluster molecules.smi` groups a set of molecules by structural similarity. Uses open APIs—no license needed.

---

### 14. xrdmatch

**The problem:** You have XRD peak positions and need to identify the phase. Current tools require expensive software or manual comparison with database PDFs.

**The solution:** A peak-matching CLI using open crystallography databases. `xrdmatch peaks.txt` compares your peaks against COD/AMCSD and ranks possible phases. `xrdmatch --cif sample.cif` simulates the pattern and identifies it. Outputs confidence scores and suggests additional peaks to measure for disambiguation.

---

### 15. vasp2qe

**The problem:** Moving calculations between DFT codes (VASP, Quantum ESPRESSO, CASTEP) requires manually rewriting input files. Each has different conventions and formats.

**The solution:** A DFT input converter. `vasp2qe POSCAR INCAR POTCAR -o pwscf.in` converts VASP inputs to Quantum ESPRESSO format. Handles k-points, pseudopotentials (with mappings), and common INCAR parameters. Warns about settings that don't translate directly. `qe2vasp` goes the other way.

---

### 16. plotextract

**The problem:** Papers show data as figures, not tables. You want to compare your results to published data, but the only source is a PNG of a plot.

**The solution:** A figure data extractor. `plotextract figure.png` uses computer vision to detect axes, extract data points, and output CSV. Interactive mode lets you click to define axis bounds. `plotextract --batch figures/` processes a directory. Handles scatter plots, line plots, and bar charts.

---

### 17. synthparse

**The problem:** Synthesis procedures are buried in paper methods sections as prose. Extracting temperatures, times, and quantities for your own use is manual and error-prone.

**The solution:** A synthesis procedure parser. `synthparse methods.txt` extracts structured data: reagents, quantities, temperatures, durations, and steps. Outputs JSON or a step-by-step protocol. `synthparse --compare a.txt b.txt` highlights differences between procedures. Uses LLMs for parsing, with human-reviewable output.

---

### 18. jupyckpt

**The problem:** Jupyter kernel crashes and you lose hours of computed state. Large DataFrames, trained models, processed data—all gone.

**The solution:** A Jupyter state saver. `%load_ext jupyckpt` in your notebook enables automatic checkpointing. Every N minutes (or on demand with `%checkpoint`), it pickles all variables to disk. After a crash, `%restore` brings back your state. Shows what's checkpointed and how much space it uses.

---

### 19. chemstock

**The problem:** Lab reagent inventory lives in unmaintained spreadsheets. "Do we have sodium hydroxide? Where? Is it expired?" requires asking around.

**The solution:** A simple chemical inventory CLI. `chemstock add "NaOH" --location "Cabinet 3" --amount "500g" --expires 2025-06-01` tracks your stock. `chemstock find "sodium"` searches by name or formula. `chemstock expiring` shows what's expiring soon. `chemstock low` shows what's running out. Stores in git-friendly YAML.

---

### 20. ragchunk

**The problem:** Every RAG project starts with "how should I chunk these documents?" You experiment with sizes, overlap, and splitting strategies from scratch each time.

**The solution:** A smart document chunker with good defaults. `ragchunk document.pdf --strategy semantic` chunks intelligently based on content structure. `ragchunk benchmark corpus/ --query queries.txt` tests different strategies against your actual queries and shows retrieval quality. Outputs to formats all vector DBs accept.

---

### 21. arxiv-plus (Browser Extension)

**The problem:** ArXiv paper pages are bare-bones. You constantly open new tabs to find the code repo, check citations, see if there's a blog post, or find the OpenReview discussion.

**The solution:** A browser extension that enhances arXiv pages. Automatically detects and links to GitHub repos (via Papers With Code API). Shows citation count. Adds one-click BibTeX copy. Links to Semantic Scholar, Connected Papers, and OpenReview if available. Shows if your institution has the published version.

---

### 22. gpu-fleet (Menubar App)

**The problem:** You have access to multiple machines with GPUs (lab servers, cloud instances, your desktop). Checking what's available requires SSH-ing into each one and running `nvidia-smi`.

**The solution:** A lightweight menubar/tray app that monitors GPU availability across machines. Shows memory usage, running processes, and temperature at a glance. Click to SSH directly into a free machine. Alerts when a GPU becomes free. Works over SSH with minimal setup—no daemon required on remote machines.

---

### 23. run-compare (Web App)

**The problem:** Comparing ML experiment runs means flipping between TensorBoard tabs, or exporting CSVs and making plots manually. Side-by-side comparison of specific runs is tedious.

**The solution:** A local web app for experiment comparison. Drag-and-drop TensorBoard logs, W&B exports, or CSV files. Automatically aligns metrics by step/epoch. Interactive overlaid plots with hover comparison. Diff configs between runs. Shareable via static HTML export. No account required—runs entirely local.

---

### 24. slurm-insights (Web Dashboard)

**The problem:** You request 32GB RAM and 8 hours for every job because you don't know what you actually need. Your jobs wait in queue forever because you're over-requesting resources.

**The solution:** A web dashboard that analyzes your Slurm job history. Shows actual vs. requested resources (memory, time, CPUs). Identifies patterns: "Your `train_*.sh` jobs use 12GB avg but request 64GB." Suggests optimal resource requests. Estimates queue time savings. Runs on the cluster, queries `sacct` directly.

---

### 25. structure-view (Web App)

**The problem:** Visualizing crystal structures requires installing VESTA or other desktop software. Quickly checking a CIF file means downloading it, opening an app, and waiting for it to load.

**The solution:** A web-based crystal structure viewer. Drag-and-drop CIF/POSCAR/XYZ files or paste URLs. Interactive 3D visualization with symmetry operations, bond lengths, and coordination environments. Compare two structures side-by-side with automatic alignment. Export publication-ready figures. No installation, works on any device.

---

### 26. tensor-shapes (VS Code Extension)

**The problem:** Tensor shape errors are the most common bugs in ML code. You stare at `x = model(x)` and have no idea what shape `x` is without adding print statements and re-running.

**The solution:** A VS Code extension that shows tensor shapes inline. Runs your code once in debug mode to capture shapes, then displays them as inline annotations: `x = model(x)  # [32, 768]`. Updates as you edit. Hover for full dtype and device info. Works with PyTorch, TensorFlow, JAX, and NumPy.

---

### 27. hpc-skeleton (Code Generator)

**The problem:** Every HPC job needs boilerplate: Slurm headers, environment setup, checkpoint handling, logging configuration, signal handling for preemption. You copy-paste from old scripts and hope it still works.

**The solution:** A project generator for HPC workflows. `hpc-skeleton init --cluster nersc --framework pytorch` creates a project with proper Slurm templates, automatic checkpointing, graceful preemption handling, logging setup, and environment management. Templates for common clusters (NERSC, OLCF, local Slurm). Knows each cluster's quirks.

---

### 28. queue-bot (Slack/Discord Bot)

**The problem:** You submit a job, then compulsively check `squeue` every 10 minutes. You want to know when jobs finish, fail, or finally start running—without manual monitoring.

**The solution:** A bot that watches your Slurm jobs and posts updates. `@queuebot watch 12345678` monitors a job. Posts when it starts, finishes, or fails (with exit code and last 20 lines of stderr). `@queuebot status` shows all your pending/running jobs. Runs on cluster login node, posts to your Slack/Discord.

---

### 29. mol-hover (Browser Extension)

**The problem:** Chemistry papers are full of SMILES strings and IUPAC names. You encounter `CC(=O)Oc1ccccc1C(=O)O` and have no idea what molecule it is without leaving the page.

**The solution:** A browser extension that makes molecules interactive. Hover over any SMILES, InChI, or common chemical name to see a 2D structure popup. Shows molecular weight, formula, and links to PubChem. Works on PubMed, arXiv, ChemRxiv, and any webpage. Highlights detected molecules on the page.

---

### 30. spec-annotate (Web App)

**The problem:** Annotating spectroscopy data (NMR, IR, MS, XRD) for group meetings or papers means screenshotting, importing to PowerPoint, and adding arrows manually. Collaboration means emailing files back and forth.

**The solution:** A web app for collaborative spectrum annotation. Upload common spectroscopy formats (JCAMP-DX, Bruker, CSV). Add peak labels, annotations, and regions interactively. Real-time collaboration like Google Docs. Export annotated figures as SVG/PNG. Share via link for group meeting discussions. Comment threads on specific peaks.

---

### 31. grove

**The problem:** Modern development often requires working on multiple branches simultaneously—reviewing a PR while fixing a bug, comparing implementations, or running parallel AI coding sessions. Git worktrees solve this, but for JS/TS projects they're painful: each worktree needs its own `node_modules`, meaning slow installs, gigabytes of duplication, and losing your editor/AI tool configs.

**The solution:** A git worktree manager with smart dependency handling. `grove create feature-x` creates a worktree with shared `node_modules` (via pnpm store or symlinks when lockfiles match). Automatically copies `.claude/`, `.cursor/`, `.vscode/`, and other tool configs. `grove list` shows all worktrees with their branches and status. `grove serve feature-x --port 3001` runs dev servers on different ports. First-class support for Claude Code, Cursor, and other AI tools.

---

### 32. cite-fetch

**The problem:** You have a DOI and need a citation. BibTeX for your paper, APA for a report, or just a clean text reference. Each format requires a different website or tool, copy-pasting, and manual cleanup.

**The solution:** A citation fetcher that outputs any format. `cite-fetch 10.1038/nature12373` prints a clean citation. `cite-fetch 10.1038/nature12373 --format bibtex` for LaTeX. Supports DOI, arXiv ID, PubMed ID, or URLs. Batch mode: `cite-fetch --file dois.txt --format bibtex > refs.bib`. Cleans up publisher junk automatically.

---

### 33. env-diff

**The problem:** "It works on my machine." You and a collaborator have different Python environments and something breaks. Figuring out which package version differs means manually comparing `pip freeze` outputs.

**The solution:** A tool that diffs Python environments. `env-diff environment.yml other.yml` shows added, removed, and changed packages. `env-diff --remote user@server` compares your local env to a remote machine. `env-diff requirements.txt --current` shows drift from pinned versions. Highlights likely culprits for common errors.

---

### 34. traceback-buddy (Web App)

**The problem:** Python tracebacks are intimidating, especially for complex ML frameworks. You get 50 lines of JAX internals and have no idea what you actually did wrong. Stack Overflow requires translating your error into searchable terms.

**The solution:** A web app that explains Python errors. Paste your traceback, get a plain-English explanation of what went wrong and how to fix it. Uses LLMs with context about common libraries (PyTorch, NumPy, pandas). Shows similar errors from GitHub issues. Highlights the line that's actually your fault vs. library internals. No login required.

---

### 35. notebook-clean

**The problem:** Jupyter notebooks grow into 2000-line monsters. Functions that should be importable modules are trapped in cells. Refactoring is manual, tedious, and breaks execution order.

**The solution:** A notebook refactoring tool. `notebook-clean analyze notebook.ipynb` identifies functions/classes that could be extracted. `notebook-clean extract notebook.ipynb --to src/` pulls them into proper Python modules, adds imports, and updates the notebook. `notebook-clean lint notebook.ipynb` warns about common issues: cells that depend on later cells, undefined variables, unused imports.

---

### 36. param-sweep

**The problem:** Running hyperparameter sweeps means writing scripts to generate all config combinations, naming output directories consistently, and tracking which runs you've completed. You rewrite this boilerplate for every project.

**The solution:** A parameter sweep generator. Define a base config and parameter ranges: `param-sweep config.yaml --vary "lr:[1e-4,1e-3,1e-2]" --vary "batch_size:[32,64,128]"` generates all 9 configs with systematic naming. `param-sweep status sweep/` shows which configs have completed runs. `param-sweep next sweep/` prints the next config to run. Integrates with Slurm: `param-sweep submit sweep/ --template slurm.sh`.

---

## Solved Papercuts

### 31. grove
A git worktree manager with smart dependency handling, designed for AI-assisted development workflows.

**Solution:** [grove](https://github.com/anthropics/grove) by [@anthropics](https://github.com/anthropics)

---

## Contributing

### Adding a Papercut
Open an issue describing:
- The friction (what's painful today)
- The shape of a solution (what would the tool do)
- Why existing tools fall short

### Solving a Papercut
1. Build the tool
2. Open a PR adding a link under the papercut description
3. Move it to "Solved Papercuts" with credit
