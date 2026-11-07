
# Arete-27B-Abliterated-Solotov (GGUF Q6_K)

An abliterated (refusal-direction removed) GGUF quant of [NousResearch/Arete-27B](https://huggingface.co/NousResearch/Arete-27B), which is itself a post-train of [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B).

**Weights (GGUF, 22 GB):** https://huggingface.co/Solotovfahl/Arete-27B-Abliterated-Solotov

This repo holds the docs, Ollama Modelfile and license only; no weights.

## Credits
- **Nous Research** for Arete-27B (Apache-2.0).
- **Alibaba Qwen team** for Qwen3.8-27B (Apache-2.0).
- Abliteration recipe follows the approach used by huihui-ai's Qwen3.8-27B abliterated model.

## Modifications (Apache-2.0 §4 notice)
The weights of Arete-27B were modified by a static abliteration transform. For each decoder layer 18–51 (inclusive), the output projections were left-projected to remove a unit "refusal direction" `d` in the 5120-dim hidden space:

```
W' = W - d (dᵀ W)
```

applied to `mlp.down_proj.weight`, `self_attn.o_proj.weight` (full-attention layers) and `linear_attn.out_proj.weight` (linear-attention layers). The first 18 and last 12 layers are untouched. The result was converted to GGUF f16 and quantized to **Q6_K** (~22.1 GB).

## ⚠️ Disclaimer
This model is **uncensored**: its safety refusals have been largely removed. It may produce harmful, offensive, or inaccurate content. You are solely responsible for how you use it and for complying with applicable laws. Not intended for deployment to end users without your own safeguards. Provided "AS IS" without warranty.

## Performance (tested)
- RTX 5090 (32 GB), fully on GPU: **~57 tok/s**
- Context: **64k** (num_ctx 65536)
- Chat and tool calling verified with Ollama

## Ollama
```
FROM ./Arete-27B-Abliterated-Solotov-Q6_K.gguf
RENDERER qwen3.5
PARSER qwen3.5
PARAMETER temperature 1
PARAMETER top_k 20
PARAMETER top_p 0.95
PARAMETER min_p 0
PARAMETER num_ctx 65536
```
```
ollama create arete-27b-abliterated -f Modelfile
ollama run arete-27b-abliterated
```

## License
Apache-2.0, same as the base model. See `LICENSE`.
