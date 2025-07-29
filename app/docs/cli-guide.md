# CTINexus Command Line Interface

CTINexus provides a powerful command line interface (CLI) for processing threat intelligence reports without the need for a graphical interface. This is ideal for automation, batch processing, and integration into existing security workflows.

## Quick Start

### Basic Usage

```bash
# Process text directly
python app.py --text "Your threat intelligence text here"

# Process a file
python app.py --input-file report.txt

# Use specific models
python app.py --input-file report.txt --model gpt-4o --embedding-model text-embedding-3-large
```

## Command Line Options

### Input Options

| Option | Short | Description |
|--------|-------|-------------|
| `--text TEXT` | `-t` | Input threat intelligence text to process directly |
| `--input-file FILE` | `-i` | Path to `.txt`, `.md`, or `.pdf` file |
| `--url URL` |  | Fetch text from a remote URL |

**Note**: `--text`, `--input-file`, and `--url` are mutually exclusive - use only one.

### Model Selection

| Option | Description | Example |
|--------|-------------|---------|
| `--provider` | AI provider to use (auto-detected if not specified) | `--provider OpenAI` |
| `--model` | Model to use for all text processing steps | `--model gpt-4o` |
| `--embedding-model` | Embedding model for entity alignment | `--embedding-model text-embedding-3-large` |

#### Provider-Specific Models

**OpenAI Models:**
- `o4-mini`, `o3-mini`, `o3`, `o3-pro` (Reasoning models)
- `gpt-4.1`, `gpt-4o`, `gpt-4`, `gpt-4-turbo` (GPT models)
- `gpt-4.1-mini`, `gpt-4o-mini`, `gpt-4.1-nano` (Smaller models)

**Gemini Models:**
- `gemini-2.5-flash-lite`, `gemini-2.0-flash`, `gemini-2.0-flash-lite`

**AWS Models:**
- `anthropic.claude-3-7-sonnet`, `anthropic.claude-3-5-sonnet`
- `anthropic.claude-3-5-haiku`, `anthropic.claude-3-haiku`
- `amazon.nova-micro-v1:0`, `amazon.nova-lite-v1:0`, `amazon.nova-pro-v1:0`
- `deepseek.r1-v1:0`, `mistral.pixtral-large-2502-v1:0`
- Meta Llama models: `meta.llama3-1-8b-instruct-v1:0`, etc.

**Claude Models:**
- `claude-3-opus-20240229`, `claude-3-sonnet-20240229`, `claude-3-haiku-20240307`

**Perplexity Models:**
- `pplx-70b-online`, `pplx-7b-online`

**Ollama Models:**
- `llama2`, `mixtral`

### Fine-Grained Model Control

The default models can be overridden with specific models for specific tasks in the pipeline:

| Option | Description |
|--------|-------------|
| `--ie-model` | Override model for Intelligence Extraction |
| `--et-model` | Override model for Entity Tagging |
| `--ea-model` | Override embedding model for Entity Alignment |
| `--lp-model` | Override model for Link Prediction |

### Pipeline Configuration

| Option | Default | Description |
|--------|---------|-------------|
| `--similarity-threshold` | 0.6 | Similarity threshold for entity alignment (0.0-1.0) |

### Output Options

| Option | Short | Description |
|--------|-------|-------------|
| `--output FILE` | `-o` | Output file path (default: organized in app/output/ directory) |

**Default output behavior:**
- For a file input: `app/output/<input_file>_output.json`
- For a text input: `app/output/output.json`
- Custom output: Uses the exact path provided by user

## Usage Examples

### Basic Processing

```bash
# Process a threat report file with default models
python app.py --input-file threat_report.txt

# Process text directly
python app.py --text "APT29 used PowerShell to download additional malware from 192.168.1.100"
```

### Provider and Model Selection

```bash
# Use OpenAI with specific models
python app.py -i report.txt --provider OpenAI --model gpt-4o --embedding-model text-embedding-3-large

# Use Gemini with default models
python app.py -i report.txt --provider Gemini

# Use AWS Claude
python app.py -i report.txt --provider AWS --model anthropic.claude-3-5-sonnet

# Use Claude API directly
python app.py -i report.txt --provider Claude --model claude-3-sonnet-20240229

# Use Perplexity
python app.py -i report.txt --provider Perplexity --model pplx-7b-online

# Use Ollama local models
python app.py -i report.txt --provider Ollama --model llama2
```

### Advanced Model Configuration

```bash
# Use different models for each pipeline step
python app.py -i report.txt \
  --ie-model gpt-4o \
  --et-model gpt-4o-mini \
  --ea-model text-embedding-3-large \
  --lp-model gpt-4o-mini

# Adjust similarity threshold for stricter entity alignment
python app.py -i report.txt --model gpt-4o --similarity-threshold 0.8
```

### Output Control

```bash
# Use organized default output (saves to app/output/)
python app.py -i report.txt --model gpt-4o
# Creates: app/output/report_output.json

# Specify custom output file
python app.py -i report.txt --output analysis_results.json
# Creates: analysis_results.json (in current directory)
```

## API Configuration

Before using the CLI, ensure your API keys are configured in the `.env` file:

```bash
# Copy the example file
cp .env.example .env

# Edit with your API keys
OPENAI_API_KEY=your_openai_api_key_here
GEMINI_API_KEY=your_gemini_api_key_here
AWS_ACCESS_KEY_ID=your_aws_access_key
AWS_SECRET_ACCESS_KEY=your_aws_secret_key
AWS_REGION=us-east-1
```

You only need to configure one provider, but multiple providers can be set up for flexibility.

### Getting Help

```bash
# Show all available options
python app.py --help
```
