# Transformer

## 1. Model Description and Parameter Justification

### Model Architecture

In this assignment, I implemented a **Transformer** based **Large Language Model (LLM)**. The model is trained on the Shakespeare dataset for autoregressive next-token prediction.

The architecture consists of:

- Token Embedding Layer
- Sinusoidal Positional Encoding
- 6 Transformer Blocks
  - Multi-Head Self-Attention (6 heads)
  - Feed Forward Network
  - Residual Connections
  - Layer Normalization
- Final Linear Projection to vocabulary space

The model performs causal self-attention using a lower triangular mask to ensure autoregressive behavior.

------

### Key Hyperparameters and Justification

| Parameter        | Value | Justification                                                |
| ---------------- | ----- | ------------------------------------------------------------ |
| `context_length` | 256   | Allows modeling medium-range dependencies without excessive memory cost |
| `n_layer`        | 6     | Provides sufficient depth for representation learning while keeping training time manageable |
| `n_head`         | 6     | Enables multi-view attention over token relationships        |
| `n_embd`         | 384   | Balanced embedding size for expressiveness vs computation    |
| `dropout`        | 0.2   | Reduces overfitting on small dataset                         |
| `learning_rate`  | 1e-3  | Standard starting point for AdamW optimizer                  |
| `batch_size`     | 64    | Stable gradient estimation while fitting in GPU memory       |

The attention dimension is computed as:
$$
\text{attn-dim} = \frac{n_{embd}}{n_{head}}
$$
ensuring equal dimensional allocation per attention head.

The positional encoding follows the sinusoidal formulation:
$$
PE(pos,2i) = \sin\left(\frac{pos}{10000^{2i/d_{model}}}\right)
$$

$$
PE(pos,2i+1) = \cos\left(\frac{pos}{10000^{2i/d_{model}}}\right)
$$
which allows the model to generalize to unseen sequence lengths.

------

## 2. Model Evaluation

### Training Behavior

During training:

- Training loss steadily decreased across iterations.
- Evaluation loss tracked training loss, indicating stable optimization.

The use of:

- Gradient clipping
- Dropout
- Layer normalization
  helped stabilize training.

------

### Generated Text Quality

The trained model was able to generate Shakespeare-like text when prompted with:

```
ROMEO:
```

The generated sequences exhibited:

- Correct structure of dialogue format
- Shakespearean vocabulary patterns
- Coherent sentence beginnings

Example characteristics observed:

- Dramatic tone imitation
- Character name formatting preserved
- Local syntactic coherence

However:

- Long-range consistency remains limited

These limitations are expected due to:

- Small dataset size
- Limited model scale
- Relatively short training duration

------

### Model Size

Total trainable parameters:
$$
\text{Total Parameters} = \sum p.numel()
$$
The model remains lightweight compared to large-scale language models, making it feasible to train on a local GPU within minutes.

------

## 3. Reflection and Takeaways

This lab provided hands-on insight into the internal mechanics of Transformer-based language models. Implementing multi-head attention manually clarified how parallel attention heads capture different relational aspects of the sequence.

Key takeaways:

- Self-attention enables flexible dependency modeling without recurrence.
- Multi-head attention increases representational capacity.
- Positional encoding is essential for sequence awareness.
- Training stability strongly depends on normalization and dropout.
- Even relatively small Transformers can produce convincing stylistic text.

Overall, this exercise deepened my understanding of Transformer architecture and the practical considerations involved in training generative language models.