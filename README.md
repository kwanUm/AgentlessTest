# 🛠️ AgentlessTest

## Setup

Clone the repository and set up the environment:

```shell
git clone https://github.com/kwanum/AgentlessTest.git
cd AgentlessTest

conda create -n agentless python=3.11 
conda activate agentless
pip install -r requirements.txt

export OPENAI_API_KEY={key_here}
```

# Generate Task Instances

Before running localization, generate the task instances from the SWE-bench repository:

```shell
git clone https://github.com/princeton-nlp/SWE-bench.git
cd SWE-bench

# Install deps per SWE-bench repo instructions

# Run the task generation script
python swebench/collect/get_tasks_pipeline.py --repos "pallets/flask" \
                                              --path_prs "swebench/collect/prs/" \
                                              --path_tasks "swebench/collect/tasks/"

# Ensure the output path in the following steps matches the one used here
```

# Localization

Run the localization script for unit testing:

```shell
mkdir results/location # Folder to store results
python agentless_test/fl/localize.py --file_level --related_level --fine_grain_line_level \
                                     --output_folder results/location --top_n 8 --compress \
                                     --context_window 10 --tasks_path /path/to/tasks.jsonl

```


/path/to/tasks.jsonl should be one output file from --path_tasks output at previous step

# Write tests

To run the script to write the tests, execute the following command in your terminal:

```shell
mkdir -p results/repair # Ensure the directory exists
python agentless_test/repair/repair.py --loc_file results/location/loc_outputs.jsonl \
                                       --output_folder results/repair --loc_interval \
                                       --top_n 8 --context_window 10 --max_samples 10 \
                                       --cot --diff_format --gen_and_process
```
