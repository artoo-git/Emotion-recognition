# Emotion Recognition

Notebooks are automatically stripped of outputs before committing via `nbstripout`. When cloning this repo, run the steps below to enable the same behaviour locally.

---

### 1. Install `nbstripout`

```bash
pip install nbstripout
```

---

### 2. Enable the Git filter

From the project root:

```bash
nbstripout --install
```

This registers a Git filter that strips outputs from `*.ipynb` files on every commit. The `.gitattributes` file that routes notebooks through this filter is already tracked in the repo.

---

### 3. Workflow

- Edit and save notebooks as usual.
- On commit, outputs are stripped automatically — no extra steps needed.
- The committed notebook is clean and lightweight; your local working copy is unaffected.

---

### 4. Sync with remote

```bash
git add .
git commit -m "Your message"
git push
```

---

**Alternative (discouraged):** clear outputs manually in JupyterLab/VS Code via `Command Palette` → "Notebook: Clear All Outputs" → Save.
