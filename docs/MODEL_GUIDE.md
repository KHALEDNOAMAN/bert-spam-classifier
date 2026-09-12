# BERT Spam Classifier - Model Guide

## Architecture Comparison
| Model | Accuracy | F1-Score | Training Time |
|-------|----------|---------|--------------|
| Naive Bayes (baseline) | ~96% | ~0.93 | < 1 min |
| BERT frozen features | ~97% | ~0.95 | ~10 min |
| BERT fine-tuned | ~98.5% | ~0.97 | ~30 min |

## How BERT Fine-Tuning Works
```
Input SMS --> BERT Tokenizer --> [CLS] token --> Classification Head --> Spam/Ham
                                    |
                         768-dim embedding
                                    |
                          Linear(768, 2)
```

## Key Decisions
- **Max length**: 128 tokens (SMS are short)
- **Learning rate**: 2e-5 (standard for BERT fine-tuning)
- **Epochs**: 3 (avoid overfitting on small dataset)
- **Batch size**: 16

## Evaluation Metrics
| Metric | What It Measures |
|--------|-----------------|
| Accuracy | Overall correctness |
| Precision | Of predicted spam, how many are actually spam |
| Recall | Of actual spam, how many did we catch |
| F1-Score | Balance of precision and recall |

## Running Inference
```python
from classifier import predict
result = predict("Congratulations! You won a free iPhone!")
# Output: {"label": "spam", "confidence": 0.99}
```

## Honest Findings
This project documents both successes and failures:
- Dataset distribution shift with modern messages
- Overfitting risks on small datasets
- When simpler models (Naive Bayes) might be enough