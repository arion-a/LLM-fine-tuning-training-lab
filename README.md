# LLM Finetune Lab

Fine-tunes an open instruction-tuned language model to extract structured
fields — parties, obligations, deadlines, governing law — from contract
clauses. It's the same class of task [LexiBridge](https://github.com/arion-a/lexibridge)
draws on for legal research and drafting, but trained into the model rather
than retrieved and prompted at inference time.

## Status

Planned — training script, dataset, and eval harness in progress.

## Plan

- **Model**: Qwen2.5-7B-Instruct, loaded in 4-bit (QLoRA).
- **Task**: structured extraction from a contract clause to JSON (party,
  obligation, deadline, governing-law fields), scored by exact-match/F1
  against labeled held-out clauses.
- **Data**: [CUAD](https://www.atticusprojectai.org/cuad) (Contract
  Understanding Atticus Dataset) as a base, filtered and cleaned to the
  extraction task, with a documented train/val/test split and leakage
  checks.
- **Method**: QLoRA supervised fine-tuning (`peft` + `bitsandbytes` +
  `trl`'s `SFTTrainer`).
- **Infra**: single-GPU training (RunPod).
- **Eval**: zero-shot baseline vs. the fine-tuned checkpoint on the same
  held-out clauses, reported as a metric delta with sample-size caveats —
  not a qualitative comparison.

## Why this exists

Most public legal-AI tooling is prompting or RAG over a frozen model (see
LexiBridge). This project is the complementary piece: actually training a
model on the task, so the two approaches can eventually be compared or
combined.

**Disclaimer**: for research purposes; not legal advice, and any
downstream use should involve a licensed attorney's review.
