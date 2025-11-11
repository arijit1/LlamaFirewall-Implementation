# 🧱 LlamaFirewall: A Multi-Layered Safety System for LLM Workflows

This project is an **open, reproducible implementation of key ideas from the paper**  
📄 [*LlamaFirewall: An Open-Source Guardrail System for Building Secure AI Agents*](https://arxiv.org/pdf/2505.03574)

It demonstrates how **PromptGuard**, **AlignmentCheck**, and **CodeShield** can be combined to form a robust firewall that protects language-model workflows from unsafe prompts, harmful generations, and insecure code.

---

### 🧩 Demonstration Paper

This notebook specifically analyzes and demonstrates the following paper:

LlamaFirewall: An Open-Source Guardrail System for Building Secure AI Agents
arXiv:2505.03574 (2025)
The paper introduces PromptGuard 2, AlignmentCheck, and CodeShield as modular components for securing AI agents against prompt injection, reasoning drift, and insecure code generation.

By implementing these ideas inside a live LLM notebook, this project bridges theory and practice, providing a working demonstration of multi-layered LLM security.

## 🧩 Overview

LlamaFirewall introduces a **layered defense system** for Large Language Models (LLMs), targeting three critical areas:

| Layer | Component | Function |
|-------|------------|-----------|
| 1️⃣ | **PromptGuard** | Classifies incoming prompts into risk categories (block / caution / ok) to detect jailbreaks and policy violations. |
| 2️⃣ | **AlignmentCheck** | Audits generated answers for hallucinations, unsafe reasoning, or policy breaches, offering safe rewrites when needed. |
| 3️⃣ | **CodeShield** | Scans generated code for dangerous patterns (e.g. `rm -rf`, `subprocess.shell=True`, `curl | sh`) and removes or blocks them. |

This notebook integrates all three layers into a single callable interface:

```python
result = firewalled_chat("Summarize the paper in 5 bullets.")
print(result["answer"])
print(result["safety_report"])

# ARCHITECTURE

 User Query
   ↓
[PromptGuard] → classify → {ok, caution, block}
   ↓ (if ok)
[Retriever] → fetch paper context
   ↓
[LLM Draft] → initial answer
   ↓
[CodeShield] → static scan of code blocks
   ↓
[AlignmentCheck] → audit reasoning and apply safe rewrites
   ↓
Final Answer + Safety Report

```
## KEY RESULTS
### === Manual LlamaFirewall tests ===

1) Benign summary status: ok
→ Model safely summarized LlamaFirewall’s core contributions
Safety report keys: ['prompt_guard', 'code_shield', 'alignment_check']

2) Code allowed status: ok
→ Generated safe sandboxed Python; CodeShield found 0 issues.

3) Code disallowed status: ok
→ Auto-redacted when allow_code=False.

4) Malicious placeholder status: blocked
→ PromptGuard flagged explicit_malware_writing and refused.

## Citation

If you use this prototype, please cite:
```
@article{llamafirewall2025,
  title={LlamaFirewall: An Open-Source Guardrail System for Building Secure AI Agents},
  author={Anonymous et al.},
  journal={arXiv preprint arXiv:2505.03574},
  year={2025}
}
```

## 🧾 License

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
