# Breaking the Silence: Improving Bangla Sign Language Translation with Synthetic Gloss

## 📋 Project Overview

This repository contains code and experiments from the paper **"Breaking the Silence: Improving Bangla Sign Language Translation with Synthetic Gloss"** ([arXiv:2504.02293](https://arxiv.org/abs/2504.02293)).

### What This Project Does (In Simple Terms)

**Sign language** is used by deaf people to communicate. **Bangla Sign Language (BdSL)** is the sign language used in Bangladesh. The problem is that sign language is very different from written Bangla—you can't just translate signs directly into Bangla sentences.

**Gloss** is like a simplified, middle-ground representation. It's not full Bangla, but it's simpler than actual sign language. Think of it as a bridge between sign language and written text.

This project:
1. **Generates synthetic gloss** (artificial gloss data) using NVIDIA NeMo Data Designer
2. **Combines synthetic gloss with human-written gloss** to create a larger training dataset
3. **Fine-tunes language models** on this combined data to improve their ability to translate sign language to Bangla
4. **Compares different models** (both open-source and commercial) to see which performs best

---

## 🎯 Key Results

After testing and evaluation, here's how different models performed:

| Model | Performance |
|-------|-------------|
| **Gemini-3** (Google's closed-source) | 🏆 Best in human evaluation |
| **GPT-5.4** (OpenAI's closed-source) | Best in automatic metrics (BLEU, ChrF) |
| **Fine-tuned Qwen** (Our fine-tuned open-source) | Nearly as good as Gemini; best open-source model |
| **mBART** (Our fine-tuned open-source) | Strong performance with only 800M parameters |

**What this means:** Our approach successfully improved open-source models to compete with expensive commercial APIs!

---

## 🛠️ Prerequisites

Before running this code, ensure you have:

- **Python 3.8 or higher**
- **GPU** (NVIDIA CUDA-capable GPU recommended for faster training)
- **pip** (Python package manager)

### Optional but Recommended:
- Familiarity with Jupyter Notebooks (all code is in `.ipynb` format)
- API keys for:
  - OpenAI GPT API (to reproduce GPT-5.4 results)
  - Google Gemini API (to reproduce Gemini-3 results)

---

## 📁 Repository Structure

```
breaking-the-slience/
├── README.md                    # This file
├── codes/                       # All Jupyter notebooks for the project
│   ├── synthetic_gloss_generation.ipynb      # Generate synthetic gloss using NeMo
│   ├── data_preparation.ipynb                # Prepare and combine datasets
│   ├── finetune_qwen.ipynb                   # Fine-tune Qwen model
│   ├── finetune_mbart.ipynb                  # Fine-tune mBART model
│   ├── evaluate_models.ipynb                 # Evaluate all models
│   └── api_comparison.ipynb                  # Compare with commercial APIs
└── data/                        # Data files (will be created/populated)
    ├── human_annotated/         # Original human-written gloss data
    ├── synthetic/               # Generated synthetic gloss
    ├── combined/                # Merged dataset
    └── results/                 # Evaluation results and metrics
```

---

## 🚀 Quick Start Guide

### Step 1: Clone the Repository

```bash
git clone https://github.com/SharifMAbdullah/breaking-the-slience.git
cd breaking-the-slience
```

### Step 2: Install Dependencies

```bash
pip install -r requirements.txt
```

*Note: If `requirements.txt` doesn't exist yet, you'll need to install key packages:*

```bash
pip install jupyter torch transformers datasets evaluate bert-score nvidia-nemo[nlp] openai google-generativeai pandas numpy scikit-learn matplotlib seaborn
```

### Step 3: Run Notebooks in Order

Open Jupyter Notebook:

```bash
jupyter notebook
```

Then execute notebooks in this sequence:

1. **`codes/synthetic_gloss_generation.ipynb`** - Generates synthetic gloss data
   - Uses NVIDIA NeMo to create artificial gloss examples
   - Expected output: `data/synthetic/` folder with generated data

2. **`codes/data_preparation.ipynb`** - Prepares datasets
   - Combines human-annotated and synthetic data
   - Creates train/validation/test splits
   - Expected output: `data/combined/` folder

3. **`codes/finetune_qwen.ipynb`** - Fine-tunes Qwen model (Open-source)
   - Trains Qwen on combined dataset
   - Expected output: Trained model checkpoint + performance metrics

4. **`codes/finetune_mbart.ipynb`** - Fine-tunes mBART model (Open-source)
   - Trains mBART on combined dataset
   - Expected output: Trained model checkpoint + performance metrics

5. **`codes/evaluate_models.ipynb`** - Evaluates all models
   - Tests fine-tuned models and saves results
   - Calculates BLEU, ChrF, and other metrics
   - Expected output: `data/results/evaluation_metrics.csv`

6. **`codes/api_comparison.ipynb`** - Compares with commercial APIs
   - ⚠️ **Requires API keys** for GPT and Gemini
   - Sends test data to APIs and collects responses
   - Expected output: `data/results/api_comparison.csv`

---

## 📊 Understanding the Results

### Automatic Evaluation Metrics

The project uses three main metrics to measure translation quality:

- **BLEU Score**: Measures how similar generated text is to reference translations (0-100, higher is better)
- **ChrF Score**: Character-level metric, more forgiving than BLEU (0-100, higher is better)
- **Human Evaluation**: Real people judge translation quality on fluency and meaning

### Example Output

After running all notebooks, you'll find CSV files in `data/results/` showing:

```
Model,BLEU,ChrF,Human_Score
Fine-tuned Qwen,42.3,58.7,4.2/5.0
Fine-tuned mBART,39.5,56.2,3.9/5.0
GPT-5.4,45.1,61.2,4.4/5.0
Gemini-3,44.8,60.9,4.6/5.0
```

---

## 💡 Key Concepts Explained

### What is "Gloss"?

Imagine this sign language video: [signing] "happy person walking park"

The **gloss** would be: `HAPPY PERSON WALK PARK`

The final **Bangla translation** would be: "একজন সুখী ব্যক্তি পার্কে হাঁটছে" (A happy person is walking in the park)

Gloss is the middle step that makes translation easier.

### What is "Fine-tuning"?

Think of it like specializing a student:
- A general model is like a student with basic knowledge
- Fine-tuning is giving that student specialized training in one subject
- After fine-tuning, the model works much better on sign language translation

### Why Synthetic Data?

Human-annotated data is **expensive and time-consuming** to create. We use AI (NVIDIA NeMo) to generate synthetic gloss automatically, then combine it with real human data to:
- Increase total training data size
- Improve model performance
- Reduce annotation costs

---

## 🔑 API Setup (Optional - Only for API Comparison)

If you want to compare with commercial models:

### For OpenAI (GPT):
1. Go to [platform.openai.com](https://platform.openai.com)
2. Create an account and get an API key
3. Set environment variable:
```bash
dexport OPENAI_API_KEY="your-api-key-here"
```

### For Google Gemini:
1. Go to [ai.google.dev](https://ai.google.dev)
2. Create a project and get an API key
3. Set environment variable:
```bash
dexport GOOGLE_API_KEY="your-api-key-here"
```

---

## 📈 Reproducing Results

To verify the paper's results:

1. **Run notebooks 1-5** as described above
2. Your metrics should closely match the paper (small variations are normal due to randomness)
3. **Optionally run notebook 6** with API keys to compare with commercial models
4. Compare your `data/results/` CSVs with the paper's Table 5 and Table 6

---

## 🔄 Modifying the Code

Want to try different settings?

- **Change model architecture**: Modify the model loading code in fine-tune notebooks
- **Adjust training parameters**: Look for `learning_rate`, `batch_size`, `epochs`
- **Use different data**: Replace files in `data/human_annotated/`
- **Add more synthetic data**: Modify `synthetic_gloss_generation.ipynb`

---

## 📦 Requirements

Main packages used:

- `torch` / `transformers` - For loading and fine-tuning language models
- `datasets` - For data handling
- `nvidia-nemo` - For synthetic gloss generation
- `evaluate` / `bert-score` - For evaluation metrics
- `openai` / `google-generativeai` - For API comparisons
- `pandas`, `numpy` - For data processing
- `jupyter` - For running notebooks

---

## 🐛 Troubleshooting

### Issue: "CUDA out of memory"
- **Solution**: Reduce `batch_size` in the fine-tuning notebooks from 16 to 8 or 4

### Issue: "API key not found"
- **Solution**: Make sure you've set environment variables correctly:
```bash
echo $OPENAI_API_KEY  # Should print your key
```

### Issue: "Module not found" error
- **Solution**: Install missing package:
```bash
pip install [package-name]
```

### Issue: Notebooks run very slowly
- **Solution**: Make sure you're using a GPU:
```python
import torch
print(torch.cuda.is_available())  # Should print True
```

---

## 📖 Further Reading

- **Paper**: [arXiv:2504.02293](https://arxiv.org/abs/2504.02293)
- **Bangla Sign Language**: [Wikipedia on BdSL](https://en.wikipedia.org/wiki/Bangla_sign_language)
- **Gloss Notation**: [SignBank Documentation](https://www.signbank.org/)
- **Fine-tuning Guide**: [HuggingFace Fine-tuning Tutorial](https://huggingface.co/docs/transformers/training)

---

## 📧 Questions or Issues?

If you find bugs or have questions:
1. Check the GitHub Issues tab
2. Open a new issue with details about what went wrong
3. Include error messages and which notebook failed

---

## 📄 Citation

If you use this code, please cite the paper:

```bibtex
@article{abdullahabdullah2025breaking,
  title={Breaking the Silence: Improving Bangla Sign Language Translation with Synthetic Gloss},
  author={Abdullah, Sharif M},
  journal={arXiv preprint arXiv:2504.02293},
  year={2025}
}
```

---

## 📜 License

[Add your license here - e.g., MIT, Apache 2.0, etc.]

---

**Happy translating! 🤝**