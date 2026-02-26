# AutoSDT

Scaling Data-Driven Discovery Tasks Toward Open Co-Scientists.

## Project Description
AutoSDT is an automatic pipeline for collecting high-quality coding tasks from real-world, data-driven scientific discovery workflows. It uses large language models (LLMs) to search repositories, identify ecologically valid scientific tasks, and synthesize executable task instructions and solutions.

<p align="center">
[<a href="https://osu-nlp-group.github.io/AutoSDT/">Website</a>] •
[<a href="https://arxiv.org/abs/2506.08140">Paper</a>] •
[<a href="https://huggingface.co/datasets/osunlp/AutoSDT-5K">Dataset</a>] •
[<a href="https://x.com/YifeiLiPKU/status/1932962920134046022">Twitter</a>]
</p>

Our AutoSDT collects data-driven discovery tasks in three steps: (1) AutoSDT-Search generates a list of keywords for each discipline and searches for relevant repositories. (2) AutoSDT-Select identifies programs that represent data-driven discovery tasks and extracts their execution dependency folders. (3) AutoSDT-Adapt modifies the selected programs to be independently executable and generates their corresponding task instructions.

We construct AutoSDT-5K, a dataset of 5,404 scientific coding tasks spanning four scientific disciplines (bioinformatics, computational chemistry, geographical information science, and psychology and cognitive neuroscience) and using 756 unique Python packages.

After fine-tuning Qwen2.5-Coder-32B-Instruct on AutoSDT-5K, the model reaches GPT-4o-level performance on ScienceAgentBench with a success rate of 7.8%, doubling the base model performance. It also improves the hypothesis matching score by 17.4% relatively on DiscoveryBench.

## Table-of-Contents
- [Overview](#overview)
- [Installation](#installation)
- [AutoSDT Pipeline](#autosdt-pipeline)
- [Tutorials](#tutorials)
- [How-To Guides](#how-to-guides)
- [Explanation](#explanation)
- [Training and Inference](#training-and-inference)
- [References](#references)
- [Issue Reporting](#issue-reporting)
- [Acknowledgements](#acknowledgements)
- [Contact](#contact)
- [Disclaimer](#disclaimer)
- [License](#license)
- [Citation](#citation)

## Overview
Despite long-standing efforts in accelerating scientific discovery with AI, building reliable AI co-scientists remains challenging due to the lack of high-quality data for training and evaluation. AutoSDT addresses this data scarcity problem through an automatic data collection and adaptation pipeline.

## Installation
Clone this repository and install the required packages:
```
git clone https://github.com/OSU-NLP-Group/AutoSDT
cd AutoSDT
pip install -r requirements.txt
```

## AutoSDT Pipeline
### Configure Azure endpoint and API key
```
vim ~/.bashrc
export AZURE_OPENAI_KEY=YOUR_AZURE_API_KEY
export AZURE_ENDPOINT=YOUR_AZURE_ENDPOINT
export AZURE_API_VERSION=YOUR_AZURE_API_VERSION
source ~/.bashrc
```

### AutoSDT-Search: Search for research related repositories
```
cd autosdt/scripts
bash run_search.sh
```

Specify discipline keywords in the `base_keywords` argument.

### AutoSDT-Select: Crawl python files, verify tasks, and prepare workspaces
```
bash run_crawl_files.sh
bash run_scientific_task_verify.sh
bash run_locate_dependencies.sh
bash run_prepare_env.sh
```

### AutoSDT-Adapt: Adapt programs and generate task instructions
```
bash run_adapt_code.sh
bash run_generate_instruction.sh
```

After the above steps, you should obtain a `final_combined_training_data.jsonl` containing generated instructions and code. Then run:
```
python convert_data_to_alpaca_format.py
```

## Tutorials
### Quickstart Tutorial
1. Install dependencies with `pip install -r requirements.txt`.
2. Configure Azure/OpenAI environment variables.
3. Run Search -> Select -> Adapt pipeline scripts in order.
4. Verify `final_combined_training_data.jsonl` is generated.
5. Convert data format with `python convert_data_to_alpaca_format.py`.

### Prerequisites
- Python 3.10+
- Required Python packages from `requirements.txt`
- Valid API credentials for the configured LLM provider

### Expected End Result
You will get a structured training dataset of scientific coding tasks that can be used directly for supervised fine-tuning.

## How-To Guides
### How to run only one pipeline stage
- Search only: run `bash run_search.sh`
- Select only: run `bash run_crawl_files.sh`, `bash run_scientific_task_verify.sh`, `bash run_locate_dependencies.sh`, `bash run_prepare_env.sh`
- Adapt only: run `bash run_adapt_code.sh`, `bash run_generate_instruction.sh`

### How to fine-tune models
1. Prepare Alpaca-format training data.
2. Use LLaMA-Factory configs in `models/`.
3. Launch supervised fine-tuning with your selected config.

### Troubleshooting
- If API requests fail, verify endpoint, key, and API version.
- If scripts fail due to missing dependencies, reinstall via `pip install -r requirements.txt`.
- If outputs are empty, check logs and intermediate files under each pipeline stage.

## Explanation
AutoSDT is designed to maximize ecological validity of scientific programming tasks while minimizing manual curation cost.

- AutoSDT-Search improves repository recall by discipline-specific keyword generation.
- AutoSDT-Select improves data quality by verifying scientific-task relevance and isolating executable dependencies.
- AutoSDT-Adapt improves usability by converting fragmented code into standalone tasks with instructions.

This design supports both training (task-solution supervision) and evaluation (realistic scientific coding benchmarks).

## Training and Inference
### Supervised Fine-tuning
We use the [LLaMA-Factory](https://github.com/hiyouga/LLaMA-Factory) library for SFT. Example config files are provided in the `models/` folder.

### Inference and Evaluation
For ScienceAgentBench, follow its original repository instructions.

For DiscoveryBench, first start an LLM engine at localhost using [vllm](https://docs.vllm.ai/en/latest/), then run:
```
python evaluate_with_llm_engine.py
```

Then compute final metrics:
```
python cal_eval_avg.py
```

## References
- AutoSDT Project Website: https://osu-nlp-group.github.io/AutoSDT/
- AutoSDT Paper (arXiv): https://arxiv.org/abs/2506.08140
- AutoSDT-5K Dataset (Hugging Face): https://huggingface.co/datasets/osunlp/AutoSDT-5K
- LLaMA-Factory: https://github.com/hiyouga/LLaMA-Factory
- vLLM Documentation: https://docs.vllm.ai/en/latest/

## Issue Reporting
If you encounter any issues, please report through one of the following channels:
- GitHub Issues: https://github.com/OSU-NLP-Group/AutoSDT/issues
- Email: li.14042@osu.edu

When reporting, include your environment, executed command, error logs, and reproduction steps.

## Acknowledgements
Please include other funding sources as needed.

National Science Foundation (NSF) funded AI institute for Intelligent Cyberinfrastructure with Computational Learning in the Environment (ICICLE) (OAC 2112606).

## Contact
Yifei Li (li.14042@osu.edu), Hanane Nour Moussa (moussa.45@osu.edu), Huan Sun (sun.397@osu.edu), The Ohio State University

## Disclaimer
AutoSDT creates tasks based on open-source code and data, and we respect creators' ownership and intellectual property. We made best efforts to ensure included repositories have permissive licenses allowing academic use.

We ensure all 1325 repositories composing final tasks in AutoSDT-5K allow academic use, including MIT, GNU, Apache, BSD, CC, Boost, Public Domain, ISC, Eclipse, PolyForm, Mulan, and custom licenses. We manually reviewed repositories with custom licenses to confirm academic and non-commercial use permissions.

## License
Code under this repo is licensed under MIT License.

Note: If your GitHub repository metadata currently shows GPL-3.0 while this README states MIT, please reconcile the mismatch by updating the repository license settings and the root `LICENSE` file to the intended single license.

## Citation
Please cite our paper (and star our repo) if you use our data, models, or code.

@misc{li2025autosdtscalingdatadrivendiscovery,
      title={AutoSDT: Scaling Data-Driven Discovery Tasks Toward Open Co-Scientists},
      author={Yifei Li and Hanane Nour Moussa and Ziru Chen and Shijie Chen and Botao Yu and Mingyi Xue and Benjamin Burns and Tzu-Yao Chiu and Vishal Dey and Zitong Lu and Chen Wei and Qianheng Zhang and Tianyu Zhang and Song Gao and Xuhui Huang and Xia Ning and Nesreen K. Ahmed and Ali Payani and Huan Sun},
      year={2025},
      eprint={2506.08140},
      archivePrefix={arXiv},
      primaryClass={cs.LG},
      url={https://arxiv.org/abs/2506.08140}
}


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
