# Emotion Recognition

please configure Git sync and clear output before committing large Python notebook documents (such as `.ipynb` files), follow these steps:

---

### 1. **Install `nbstripout`**

This tool automatically strips output from Jupyter notebooks before committing.

```bash
pip install nbstripout
```

---

### 2. **Enable `nbstripout` for Your Git Repo**

Navigate to your project folder and run:

```bash
nbstripout --install
```

This sets up a Git filter so that output is cleared from notebooks on commit.

---

### 3. **Workflow**

- Edit and save your notebook as usual.
- When you commit, outputs will be automatically stripped.
- The notebook in your Git repo will be clean and lightweight.

---

### 5. **Sync with Remote**

Use standard Git commands terminal:

```bash
git add .
git commit -m "Your message"
git push
```

---

**alternative (discouraged):**  
You can also clear outputs manually in JupyterLab/VS Code:  
`Command Palette` → “Notebook: Clear All Outputs” → Save.

---

