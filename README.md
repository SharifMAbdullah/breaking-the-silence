## 📋 Project Overview

This repository contains code and experiments from the paper **"Breaking the Silence: A Dataset and Benchmark for Bangla Text-to-Gloss Translation"** ([arXiv:2504.02293](https://arxiv.org/abs/2504.02293)).

### What is This Project About?

**Sign language** is used by deaf people to communicate. **Bangla Sign Language (BdSL)** is the sign language used in Bangladesh. The problem is that sign language is very different from written Bangla—yit can't capture the grammatical nuances of Bangla.

**Gloss** is a simplified, middle-ground representation. It's not full Bangla, but it's the simplified representation of Sign Language. 
It wokrs as a bridge between sign language and written text. For example:

The Bangla sentence: `আমি বৃষ্টিতে ছাতা নিয়ে বাইরে যাই`    
The gloss would be: `আমি বৃষ্টি ছাতা নিয়ে আসা বাইরে যাওয়া`

This project:
1. **Generates synthetic gloss** (artificial gloss data) using NVIDIA NeMo Data Designer
2. **Combines synthetic gloss with human-written gloss** to create a larger training dataset
3. **Fine-tunes language models** on this combined data to improve their ability to translate sign language to Bangla
4. **Compares different models** (both open-source and commercial) to see which performs best

### Why Synthetic Data?

Human-annotated data is **expensive and time-consuming** to create. We use AI (NVIDIA NeMo) to generate synthetic gloss automatically, then combine it with real human data to:
- Increase total training data size
- Improve model performance
- Reduce annotation costs

---


## 🎯 Key Results

After testing and evaluation, here's how different models performed:

| Model | BLEU | ChRF | Human Evaluation |
|-------|------| -----| -----------------| 
| GPT-5.4 | 39.26 | 73.75 | 67.76% |
| Qwen-3 | 30.82 | 70.16 | 72.27% |
| phi-4 | 2.26 | 26.95 | 0% |
| mBART | 34.81 | 70.81 | 63.76% |
| MarianMT | 7.11 | 44.60 | 2.1% |

**What this means:** Our approach successfully improved open-source models to compete with expensive commercial APIs!

### Evaluation Metrics

The three main metrics to measure translation quality are:

- **BLEU Score**: Measures how similar generated text is to reference translations (0-100, higher is better)
- **ChrF Score**: Character-level metric, more forgiving than BLEU (0-100, higher is better)
- **Human Evaluation**: Real people judge translation quality on fluency and meaning

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

## Advices for Running the Codes

Though you can use jupyter to run the notebooks, we recommend using
Google Colab to replicate our results, specifically the `2026-02-23` version
because it comes with the necessary packages that ensures the code will run without bugs. 
You can use a past runtime version by selecting it from the Runtime -> Change runtime type dialog, 
with the "Runtime Version" dropdown.
Version specific packages have been mentioned inside the `pip install` command.
Most of the notebooks contain finetuning and evaluation code together, 
so you can see the performance of the model in a single run. 


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

## 🐛 Troubleshooting

### Issue: "CUDA out of memory"
- **Solution**: Reduce `batch_size` in the fine-tuning notebooks

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

If you use this code or find anything from the paper useful for your research, 
please cite the paper:

```bibtex
@misc{abdullah2026breakingsilencedatasetbenchmark,
      title={Breaking the Silence: A Dataset and Benchmark for Bangla Text-to-Gloss Translation}, 
      author={Sharif Mohammad Abdullah and Abhijit Paul and Shubhashis Roy Dipta and Zarif Masud and Shebuti Rayana and Ahmedul Kabir},
      year={2026},
      eprint={2504.02293},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={https://arxiv.org/abs/2504.02293}, 
}
```

---
