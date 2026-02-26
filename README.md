# AutoSDT

## Project Information

**Project Name:** AutoSDT  
**Tags:** Foundation-AI  
**License:** MIT  

AutoSDT is an automated pipeline for constructing data-driven scientific discovery coding tasks from real-world repositories. It serves as a scalable foundation for training and evaluating AI co-scientists in computational science domains.

<p align="center">
[Website](https://osu-nlp-group.github.io/AutoSDT/) •
[Paper](https://arxiv.org/abs/2506.08140) •
[Dataset](https://huggingface.co/datasets/osunlp/AutoSDT-5K)
</p>

---

# Explanation

## Why AutoSDT?

Building reliable AI co-scientists requires high-quality, ecologically valid training tasks. Existing benchmarks are limited in scale and diversity.

AutoSDT addresses this by automatically mining, adapting, and packaging scientific workflows from real-world repositories into executable coding tasks.

## System Architecture

AutoSDT consists of three modules:

1. **AutoSDT-Search**
2. **AutoSDT-Select**
3. **AutoSDT-Adapt**

These modules collectively generate AutoSDT-5K, a dataset of 5,404 scientific coding tasks across four disciplines.

---

# Tutorials

## Quick Start

### Installation

```bash
git clone https://github.com/OSU-NLP-Group/AutoSDT
cd AutoSDT
pip install -r requirements.txt

Configure Azure Endpoint

export AZURE_OPENAI_KEY=YOUR_AZURE_API_KEY
export AZURE_ENDPOINT=YOUR_AZURE_ENDPOINT
export AZURE_API_VERSION=YOUR_AZURE_API_VERSION

Run the Full Pipeline

cd autosdt/scripts
bash run_search.sh
bash run_crawl_files.sh
bash run_scientific_task_verify.sh
bash run_locate_dependencies.sh
bash run_prepare_env.sh
bash run_adapt_code.sh
bash run_generate_instruction.sh

Convert output:

python convert_data_to_alpaca_format.py


⸻

How-To Guides

How to Add a New Discipline
	1.	Modify base_keywords in search config.
	2.	Update discipline-specific filters.
	3.	Re-run AutoSDT-Search and downstream modules.

How to Fine-Tune Models

We use LLaMA-Factory￼.

YAML configs are in models/.

How to Evaluate on DiscoveryBench

python evaluate_with_llm_engine.py
python cal_eval_avg.py


⸻

Reference

Dataset Statistics
	•	5,404 tasks
	•	4 scientific disciplines
	•	756 unique Python packages

External Links
	•	ScienceAgentBench
	•	DiscoveryBench
	•	LLaMA-Factory
	•	vLLM

Command Reference

Script	Purpose
run_search.sh	Repository search
run_crawl_files.sh	Crawl python files
run_adapt_code.sh	Adapt program


⸻

Issue Reporting

Please open a GitHub Issue and provide:
	•	Environment details
	•	Error logs
	•	Minimal reproducible example

Contact:
Yifei Li (li.14042@osu.edu)

⸻

Acknowledgements

This work is supported in part by the National Science Foundation (NSF) AI Institute for Intelligent Cyberinfrastructure with Computational Learning in the Environment (ICICLE), award OAC 2112606.

⸻

License

This project is licensed under the MIT License.

⸻

Citation
Please cite our paper (and star our repo!) if you use our data, models, or code.

```bibtex
@misc{li2025autosdtscalingdatadrivendiscovery,
      title={AutoSDT: Scaling Data-Driven Discovery Tasks Toward Open Co-Scientists}, 
      author={Yifei Li and Hanane Nour Moussa and Ziru Chen and Shijie Chen and Botao Yu and Mingyi Xue and Benjamin Burns and Tzu-Yao Chiu and Vishal Dey and Zitong Lu and Chen Wei and Qianheng Zhang and Tianyu Zhang and Song Gao and Xuhui Huang and Xia Ning and Nesreen K. Ahmed and Ali Payani and Huan Sun},
      year={2025},
      eprint={2506.08140},
      archivePrefix={arXiv},
      primaryClass={cs.LG},
      url={https://arxiv.org/abs/2506.08140}, 
}
```

## Acknowledgements

This work is supported in part by the National Science Foundation (NSF) funded AI Institute for Intelligent Cyberinfrastructure with Computational Learning in the Environment (ICICLE) under award OAC 2112606.
