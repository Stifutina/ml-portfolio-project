# Support Message Intent Classification

A Python ML portfolio project for automatically routing support chat messages by intent. The input is one user message in Russian, Ukrainian, or a mixture of both; the intended output is one of seven intent labels. Routing messages automatically should reduce manual sorting and speed up handling by the support team or an AI agent backend.

The project specification is in [TASK.md](TASK.md).

## Current status

Initial exploration only: `check.ipynb` creates a pandas DataFrame with four illustrative messages and labels. A full dataset, trained classifier, inference interface, and evaluation results have not been implemented yet. The metrics below are goals, not measured results.

## Intent labels

| Label | Meaning |
| --- | --- |
| `billing_issue` | Payment, invoice, or money refund problems |
| `technical_issue` | Technical errors, bugs, or something not working |
| `delivery_status` | Delivery status or timing questions |
| `return_request` | Product return or exchange requests |
| `request_human` | Explicit complaints or requests to speak to a human |
| `positive_feedback` | Thanks or positive feedback |
| `other` | Everything else |

## Success criteria and data

- Overall accuracy: **at least 85%**.
- Recall for `request_human`: **at least 90%**. Missing a complaint or a request for a human is especially costly; unnecessary escalation is preferable to missing one.
- Planned data: a public support dataset as a starting point, combined with manually written and LLM-generated Russian and Ukrainian examples, initially around **100–150 examples per class** (700–1,050 total).

The notebook uses `message` for the input text and `intent` for the target label. Before training, define labeling rules for overlapping intents, record data sources and licenses, and remove personal information. Keep duplicate or closely related synthetic examples in the same split to avoid leakage. Evaluate on a held-out test set and report per-class precision, recall, F1, and a confusion matrix alongside the target metrics.

## Local setup

The existing notebook records Python **3.14.3**. Use that version to match the recorded environment; compatibility with other versions has not been verified. Dependencies are pinned in [requirements.txt](requirements.txt), including pandas, NumPy, scikit-learn, and Jupyter.

Run these commands from the repository root on macOS/Linux:

```bash
python3.14 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

On Windows PowerShell, create and activate the environment with:

```powershell
py -3.14 -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Then run the same `python -m pip` commands. The dependency snapshot includes platform-specific packages such as `appnope`; installation on Windows or Linux may require platform markers or an adjusted dependency file. Cross-platform installation has not been verified.

## VS Code and notebooks

1. Open the repository folder in VS Code and install Microsoft's **Python** and **Jupyter** extensions.
2. Run **Python: Select Interpreter** from the Command Palette and choose the interpreter in `.venv`.
3. Open `check.ipynb`, click **Select Kernel**, and select the same `.venv` environment.
4. Run all cells. The current example should print four messages and report four unique classes.

Alternatively, launch Jupyter from the activated environment:

```bash
python -m jupyter lab
```

## Repository layout

```text
.
├── TASK.md           # Business problem, classes, and success targets
├── README.md         # Project overview and setup
├── requirements.txt  # Pinned Python dependencies
├── check.ipynb       # Initial labeled-message example
└── .gitignore        # Local environment and generated-file exclusions
```

For future work, use `data/` for local datasets, `models/` or `checkpoints/` for trained artifacts, and `outputs/` or `runs/` for generated experiment outputs. These directories are ignored by Git. Keep code, notebooks, data preparation instructions, and curated evaluation summaries under version control. Record random seeds and dataset versions when adding training.

The `.gitignore` excludes local VS Code settings while allowing shared extension recommendations, tasks, and launch configurations. Notebook files remain tracked; review their outputs before committing to avoid including private messages or unnecessarily large results.
