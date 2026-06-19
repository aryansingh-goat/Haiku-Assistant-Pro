# Haiku Assistant Pro

> *Turn a handful of keywords into vivid, concise haikus.*

**Haiku Assistant Pro** is a fine-tuned version of [SmolLM2-135M-Instruct](https://huggingface.co/HuggingFaceTB/SmolLM2-135M-Instruct), specialized in transforming keyword categories into evocative haiku poetry. Trained on 3,814 curated examples, it captures imagery, atmosphere, and brevity—whether your theme is nature, technology, urban life, or science fiction.

<p align="center">
  <img src="https://img.shields.io/badge/parameters-135M-green" alt="Parameters">
  <img src="https://img.shields.io/badge/license-Apache%202.0-blue" alt="License">
</p>

---

## 📦 Installation

```bash
pip install transformers torch
```

*If you want to use the model directly from the Hugging Face Hub (once uploaded), you can load it by name. The local merged checkpoint is also supported.*

---

## 🚀 Quick Start

```python
from transformers import AutoTokenizer, AutoModelForCausalLM

# Local merged checkpoint (FP32)
model_path = "./haiku_full_fp32"

tokenizer = AutoTokenizer.from_pretrained(model_path)
model = AutoModelForCausalLM.from_pretrained(model_path)

prompt = "Categories: mountain, snow, deer\nWrite a haiku."
inputs = tokenizer(prompt, return_tensors="pt")

outputs = model.generate(
    **inputs,
    max_new_tokens=30,
    do_sample=True,
    temperature=0.2,
    repetition_penalty=1.1,
)

generated = outputs[0][inputs["input_ids"].shape[1]:]
print(tokenizer.decode(generated, skip_special_tokens=True))
```

> 💡 **No adapter to merge** – this release is the full FP32 model with LoRA weights already merged into the base.

---

## 🧠 Model Details

| Item | Detail |
|------|--------|
| **Model Name** | Haiku Assistant Pro |
| **Base Model** | HuggingFaceTB/SmolLM2-135M-Instruct |
| **Fine‑tuning Method** | LoRA (merged into base) |
| **Training Examples** | 3,814 curated haikus |
| **Precision** | FP32 (full weights) |
| **Task** | Generate a haiku from three category keywords |

---

## 📝 Prompt Format

The model expects a simple, structured prompt:

```
Categories: <word1>, <word2>, <word3>
Write a haiku.
```

Example input → output:

```
Categories: moon, river, silence
Write a haiku.
```

```
Moonlight on the river
Silence in the still water
The world waits
```

---

## ⚙️ Recommended Generation Settings

For consistent, atmospheric results:

```python
outputs = model.generate(
    **inputs,
    max_new_tokens=30,          # allow up to 50 for longer lines
    do_sample=True,
    temperature=0.1,            # range 0.1 – 0.3
    repetition_penalty=1.1,
)
```

- **temperature** – lower values (0.1–0.3) keep the output coherent and style-consistent.
- **repetition_penalty** – 1.1 helps avoid repeated words or phrasing.
- **max_new_tokens** – 30 is usually enough; increase to 50 if you want room for richer imagery.

Higher temperatures may introduce more creative, but occasionally abstract or fragmented, results.

---

## 🌿 Example Outputs

### Nature

**Categories: mountain, snow, deer**
```
Deer on the snow-capped peak
Snow speaks of winter's slow march
The forest sings with tears
```

**Categories: bridge, fog, heron**
```
Fog on the bridge
Heron stirs with its slow breath
The water swallows him
```

**Categories: shadow, fox, grass**
```
Fox in the grass
Shadow of its tail is cold
The world lies still
```

### Urban

**Categories: city, traffic, kids**
```
City streets slow down
Kids laugh and run in the rain
Roads are still alive
```

### Technology

**Categories: computer, code, bug**
```
Computer code crackles
Bug in the code's code-block
The world is still waiting
```

**Categories: robot, factory, steel**
```
Robot in the factory
Steel is its own lifeblood
The machine's heart beats
```

### Science Fiction

**Categories: spaceship, moon, silence**
```
Spacecraft on the moon's surface
Silence is like a slow breath
The ship holds its head
```

---

## 📋 Training Summary

- **Valid examples**: 3,814 (7 invalid examples removed during cleaning)
- **Fine‑tuning**: LoRA (low‑rank adaptation)
- **Base model parameters**: 135M
- **Final release format**: Merged FP32 checkpoint (ready to use)

---

## ⚠️ Limitations

- Does **not** enforce a strict 5‑7‑5 syllable count – it trades formal meter for imagery and atmosphere.
- Occasionally produces abstract or metaphor‑heavy endings.
- May misinterpret abbreviations or highly specific technical terms.
- Works best when categories are descriptive, concrete words (e.g., “mountain, snow, deer” rather than “z8, x_ray, gigabyte”).

---

## 📄 License

This model is derived from SmolLM2-135M-Instruct and is released under the same [Apache 2.0 license](https://www.apache.org/licenses/LICENSE-2.0). Please refer to the original model’s license terms for full details.

---

## 🙏 Acknowledgements

- Base model: [HuggingFaceTB/SmolLM2-135M-Instruct](https://huggingface.co/HuggingFaceTB/SmolLM2-135M-Instruct)
- Fine‑tuning and dataset curation by the creator me.

---

*Enjoy turning keywords into tiny, vivid worlds.*
