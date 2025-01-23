# Vellum Content Safety Classifier

A Python script that uses Vellum.ai to classify text content for harmful content. The script processes multiple texts in parallel and displays results in a formatted table.

## Features

- Parallel processing of multiple text inputs
- Rich console output with formatted tables
- Debug mode for API request inspection
- Error handling and graceful failure reporting
- Environment variable management with python-dotenv

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

4. Set up your environment variables:
   - Copy the sample .env file: `cp .env.sample .env`
   - Edit .env and add your Vellum API key:
```bash
VELLUM_API_KEY='your-api-key-here'
```

## Usage

Run the script:
```bash
python quick-workflow-call.py
```

The script will:
1. Verify environment variables are properly set
2. Load environment variables from .env
3. Process a set of predefined test cases
4. Display results in a formatted table
5. Show any errors that occurred during processing

## Configuration

- `.env`: Contains your Vellum API key (copy from .env.sample)
- `DEBUG_PRINT_MODE`: Set to `True` in the script to see detailed API requests
- `TEST_CASES`: Modify the list in the script to test different text inputs

## Error Handling

The script includes robust error handling for:
- Missing or invalid API keys
- API request failures
- Invalid environment configuration

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
