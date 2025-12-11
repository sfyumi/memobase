# 使用 uv 评测 locomo

curl -LsSf https://astral.sh/uv/install.sh | sh
source $HOME/.local/bin/env

cd docs/experiments/locomo-benchmark

mkdir dataset
cd dataset
wget https://raw.githubusercontent.com/snap-research/locomo/refs/heads/main/data/locomo10.json

cd ..
uv venv
source .venv/bin/activate
uv pip install -e .
uv pip install -r requirements.txt

uv run run_experiments.py --technique_type memobase --method add --output_folder results/
uv run run_experiments.py --technique_type memobase --method search --output_folder results/
uv run evals.py --input_file results.json --output_file evals.json
uv run generate_scores.py --input_path="evals.json"