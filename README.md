# Google-Colab-Notebook-Cleaner
# 🧼 Google Colab Notebook Cleaner

A simple and effective tool to clean up Google Colab notebooks that fail to render on GitHub due to corrupted or incomplete widget metadata.

---

## 📌 Problem

When you save a notebook from **Google Colab** and try to upload it to **GitHub**, you may encounter this error:

> **Invalid Notebook**  
> "There was an error rendering your Notebook: the `'state'` key is missing from `metadata.widgets`..."

This happens because Colab sometimes saves internal widget metadata that **GitHub cannot process**.

---

## ✅ Solution

This tool removes the problematic metadata (`metadata.widgets`) from your notebook, making it clean and fully renderable on GitHub.

---

## ⚙️ How It Works

1. **Upload** your `.ipynb` file.
2. It **removes** the `metadata.widgets` section.
3. A **cleaned version** is generated.
4. You can **download and upload** the cleaned notebook to GitHub.

---

## 🚀 Usage (Run in Google Colab)

Just paste the following code into a Colab notebook cell and run it:

```python
import json
from google.colab import files
from pathlib import Path

# Step 1: Upload the broken notebook
uploaded = files.upload()
notebook_filename = next(iter(uploaded))  # get uploaded filename

# Step 2: Clean the notebook's metadata
def clean_metadata(notebook_path):
    path = Path(notebook_path)
    with open(path, "r", encoding="utf-8") as f:
        data = json.load(f)

    if "widgets" in data.get("metadata", {}):
        print("🧹 Removing metadata.widgets...")
        del data["metadata"]["widgets"]

    cleaned_path = path.with_name(path.stem + "_CLEAN.ipynb")
    with open(cleaned_path, "w", encoding="utf-8") as f:
        json.dump(data, f, indent=1)

    print(f"✅ Cleaned notebook saved as: {cleaned_path}")
    return cleaned_path

cleaned_file = clean_metadata(notebook_filename)

# Step 3: Download the cleaned notebook
files.download(str(cleaned_file))
