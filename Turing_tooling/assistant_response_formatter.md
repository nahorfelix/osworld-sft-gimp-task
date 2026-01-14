# Assistant Response Formatter

## Overview

The `assistant_response_formatter.py` script restructures assistant reasoning in Jupyter notebooks by using an LLM (Google Gemini) to generate detailed, step-by-step explanations for automation tool calls.

## Purpose

This tool processes notebooks containing automation trajectories and replaces brief assistant responses with comprehensive explanations that describe:
- The specific action being taken
- The immediate purpose of that action
- How it contributes to the overall goal
- The efficiency or benefit of the step

## Requirements

- Python 3.x
- `langchain-google-genai` package
- `python-dotenv` package
- Google API Key (set as `GOOGLE_API_KEY` environment variable)

## Installation

```bash
pip install langchain-google-genai python-dotenv
```

## Setup

1. Create a `.env` file in the project root or set the environment variable:
   ```bash
   export GOOGLE_API_KEY="your-api-key-here"
   ```

2. Or add to `.env` file:
   ```
   GOOGLE_API_KEY=your-api-key-here
   ```

## Usage

### Process a single notebook:
```bash
python assistant_response_formatter.py path/to/notebook.ipynb
```

### Process all notebooks in a directory:
```bash
python assistant_response_formatter.py path/to/directory/
```

## Notebook Format

The script expects notebooks with the following structure:

- **User instruction cells**: Markdown cells starting with `**[user]**`
- **Tool call cells**: Markdown cells starting with `**[tool_call]**` containing JSON-formatted tool call data
- **Assistant cells**: Markdown cells starting with `**[assistant]**` that will be replaced with generated explanations

### Example Notebook Structure:

```markdown
**[user]**
Open the file manager and create a new folder.

**[tool_call]**
```json
{
  "tool_name": "press_key",
  "arguments": {"key": "super"}
}
```

**[assistant]**
[Original brief response - will be replaced]
```

## How It Works

1. **Parse Notebook**: Extracts user instructions, tool calls, and assistant cells
2. **Generate Prompt**: Creates a detailed prompt for the LLM with all tool calls
3. **Call LLM**: Uses Google Gemini to generate comprehensive explanations
4. **Update Notebook**: Replaces assistant cell content with new explanations
5. **Preserve DONE**: Keeps the final "DONE" cell unchanged

## Output

The script will:
- Print processing status for each notebook
- Warn if the number of generated explanations doesn't match assistant cells
- Update the notebook file in-place with new reasoning
- Print success/error messages

## Notes

- The script processes notebooks in-place (modifies the original file)
- Make backups before running on important notebooks
- The LLM generates explanations without including the actual command/action text
- Each explanation is a single comprehensive paragraph
- The last assistant cell (usually "DONE") is preserved as-is

## Error Handling

The script handles:
- Invalid JSON in tool calls (prints warning, continues)
- Missing user instructions or tool calls (skips file)
- API errors (prints error message, continues)
- File I/O errors (prints error message)

