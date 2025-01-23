# Vellum demo of how to call a workflow in python.

Pretty straight forward. Little bit fiddly.
Original code block was from Vellum's post-deploy "here's the code" block.
Rest is just making sure it works.

- test it in the GUI properly first. Inputs, outputs, feel confident.
- point Cursor / LLM at spec story (Cursor chat history) if you want to load how I got there into context (more likely to get it right).




# 0-shot LLM. I didn't write the below.

A Python script that uses Vellum.ai to classify text content for harmful content. The script processes multiple texts in parallel and displays results in a formatted table.

## Features

- Parallel processing of multiple text inputs
- Rich console output with formatted tables
- Debug mode for API request inspection
- Error handling and graceful failure reporting

## Prerequisites

- Python 3.8+
- Vellum API key (get one at https://app.vellum.ai/api-keys#keys)

## Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/vellum-content-classifier.git
cd vellum-content-classifier
```

2. Create and activate a virtual environment:
```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

4. Set up your Vellum API key:
```bash
export VELLUM_API_KEY='your-api-key-here'
```

## Usage

Run the script:
```bash
python quick-workflow-call.py
```

The script will:
1. Process a set of predefined test cases
2. Display results in a formatted table
3. Show any errors that occurred during processing

## Configuration

- `DEBUG_PRINT_MODE`: Set to `True` in the script to see detailed API requests
- `TEST_CASES`: Modify the list in the script to test different text inputs

## Output Example

```
🔍 Running Vellum Classification Tests in Parallel

┌────────────────────────┬───────────────────┐
│ Input Text            │ Classification     │
├────────────────────────┼───────────────────┤
│ Example harmful text...│ HARMFUL           │
│ Example safe text...   │ NOT HARMFUL       │
└────────────────────────┴───────────────────┘
```

## License

MIT License - see LICENSE file for details
