# mutabench
A benchmark evaluating LLM mutagenicity prediction on the Ames test dataset.

**Part 1 of a series.** Can frontier LLMs screen chemical compounds for 
mutagenicity? We tested Claude, GPT-4o, DeepSeek, and Llama3 on 30 molecules 
from the Mendeley Ames dataset and measured false negative rate, false positive 
rate, accuracy, and confidence calibration.

## Results (Run 1)

| Model    | Accuracy | False Negative Rate | Confidently Wrong |
|----------|----------|--------------------|--------------------|
| Claude   | 73.3%    | 26.7%              | 6                  |
| GPT-4o   | 63.3%    | 33.3%              | 9                  |
| DeepSeek | 66.7%    | 40.0%              | 10                 |
| Llama3   | 70.0%    | 26.7%              | 9                  |

False negative rate = missed mutagens. Confidently wrong = confidence ≥ 7, incorrect.

## Key Finding

DeepSeek rated Diammonium Tetrachloroplatinate (a known platinum mutagen) 
as non-mutagenic with confidence 10/10. All models failed on metal complexes 
and structurally ambiguous organics — systematic blind spots, not random noise.

## Series

| Part | Status | Topic |
|------|--------|-------|
| 1 | ✅ Published | Baseline benchmark of 4 models, 30 molecules |
| 2 | 🔜 Coming | Consistency testing: does the verdict flip on repeat runs? |
| 3 | 🔜 Coming | RAG grounding, can retrieval fix the blind spots? |

## Dataset

[Mendeley Ames Mutagenicity Dataset](https://data.mendeley.com/datasets/ktc6gbfsbh/2)  
30 molecules sampled: 15 mutagens, 15 non-mutagens, balanced across structural classes.

## Prompt
```
You are a computational chemist specializing in mutagenicity prediction.
Assess the Ames mutagenicity of the following molecule.
SMILES: {smiles}
Respond in exactly this format:
Verdict: [mutagenic / non-mutagenic]
Confidence: [1-10]
Reasoning: [identify specific structural features driving your assessment —
functional groups, electrophilicity, known toxicophores]
```

## Models Tested

- Claude (claude-opus-4-5) via Anthropic API
- GPT-4o via OpenAI API
- DeepSeek (deepseek-chat) via DeepSeek API
- Llama3 (llama-3.3-70b-versatile) via Groq API

## Usage

```bash
pip install -r requirements.txt
```

Set your API keys as environment variables:
```bash
export ANTHROPIC_API_KEY=...
export OPENAI_API_KEY=...
export DEEPSEEK_API_KEY=...
export GROQ_API_KEY=...
```

Then run `notebooks/mutabench_run1.ipynb`.
