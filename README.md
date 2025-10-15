# 🦙 Ollama Context Window Variant Generator

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Shell Script](https://img.shields.io/badge/Shell-Bash-green.svg)](https://www.gnu.org/software/bash/)
[![Ollama](https://img.shields.io/badge/Ollama-Compatible-blue.svg)](https://ollama.ai/)

A robust bash script that automatically generates Ollama model variants with different context window sizes. Perfect for testing and optimizing model performance across various context lengths.

## ✨ Features

- 🔄 **Automatic Variant Generation**: Creates model variants with context sizes from 8K up to the model's maximum capacity
- 🗑️ **Cleanup Management**: Delete generated model variants and their Modelfiles with confirmation
- 🎯 **Selective Base Model Deletion**: Remove specific base models and all their generated variants
- 📊 **Context Window Leaderboard**: List and rank base models by their maximum context window size with memory usage estimates
- 🛡️ **Edge Case Handling**: Properly handles namespace/model names (e.g., `microsoft/DialoGPT`)
- 🔧 **Backward Compatibility**: Works with older Ollama versions by parsing plain text output
- 📁 **Smart File Management**: Sanitizes model names for valid filenames
- ⚡ **Batch Processing**: Processes all available models in one run
- 🎯 **Selective Overwrite**: Choose whether to overwrite existing models/files
- 📊 **Progress Tracking**: Clear progress indicators and status messages with dynamic column formatting
- 🚫 **Error Prevention**: Prevents silent exits with comprehensive error handling
- 🏷️ **Naming Convention Enforcement**: Ensures all models follow proper Ollama naming standards
- 🔄 **Auto-Rename Functionality**: Automatically fixes models with problematic naming patterns

## 🚀 Quick Start

### Prerequisites

- [Ollama](https://ollama.ai/) installed and running
- Bash shell (Linux/macOS/WSL)
- At least one Ollama model downloaded

### Installation

1. Clone this repository:
```bash
git clone https://github.com/lfontanez/tool-ollama-mama.git
cd tool-ollama-mama
```

2. Make the script executable:
```bash
chmod +x ollama-mama
```

3. Run the script:
```bash
./ollama-mama ctx --create
```

## 📖 Usage

### Command Structure

The script uses a command-based structure with naming convention enforcement:

```bash
./ollama-mama ctx [--create|--delete|--delete-base <model>|--create-base <model>|--list [filter]]
./ollama-mama model [--autorename]
```

### ⚠️ Important: Naming Convention Requirements

**All models must follow Ollama naming conventions before context variants can be created:**

- ✅ **Correct Format**: `[MODEL_NAME]:[SIZE]-[TYPE]-[QUANTIZATION]`
- ✅ **Examples**: `llama3.1:8b-instruct-q4_K_M`, `qwen2.5:14b`, `microsoft/DialoGPT:medium`
- ❌ **Incorrect**: `model-6.7B-instruct:Q4_K_M` (size in wrong place)

**Workflow**: Run `./ollama-mama model --autorename` first to fix any non-conforming models, then use `ctx` commands.

### Fix Model Naming (Required First Step)

Before creating context variants, ensure all models follow proper naming conventions:

```bash
./ollama-mama model --autorename
```

This command will:
- Scan for models with problematic naming patterns
- Propose corrections (e.g., `model-6.7B-instruct:Q4_K_M` → `model-instruct-Q4_K_M:6.7B`)
- Create properly named models
- Optionally remove the original incorrectly named models

### Create Model Variants

Generate context window variants for all conforming base models:

```bash
./ollama-mama ctx --create
```

**Note**: This command will reject non-conforming models and suggest running `--autorename` first.

### Create Variants for Specific Model

Generate context window variants for a single base model:

```bash
./ollama-mama ctx --create-base llama2:7b
./ollama-mama ctx --create-base microsoft/DialoGPT:medium
./ollama-mama ctx --create-base fredrezones55/unsloth-deepseek-r1:8b
```

### Delete All Model Variants

Remove all generated model variants and their Modelfiles:

```bash
./ollama-mama ctx --delete
```

### Delete Specific Base Model

Remove a specific base model and all its generated variants:

```bash
./ollama-mama ctx --delete-base llama2:7b
./ollama-mama ctx --delete-base microsoft/DialoGPT:medium
./ollama-mama ctx --delete-base fredrezones55/unsloth-deepseek-r1:8b
```

### List Base Models

Display a context window leaderboard of all base models:

```bash
./ollama-mama ctx --list
```

Filter models by name:

```bash
./ollama-mama ctx --list llama
./ollama-mama ctx --list qwen
./ollama-mama ctx --list microsoft
```

**Create Command** (`ctx --create`) will:
1. Scan all available Ollama models
2. Ask if you want to overwrite existing variants
3. Generate context window variants for each base model
4. Create Modelfiles and register new models with Ollama

**Create Base Command** (`ctx --create-base <model>`) will:
1. Validate the specified model exists and is a base model
2. Ask if you want to overwrite existing variants
3. Generate context window variants for only that model
4. Create Modelfiles and register new models with Ollama

**Delete Command** (`ctx --delete`) will:
1. Identify all generated model variants (pattern: `{model}:{params}-{context}k`)
2. List corresponding Modelfiles to be removed
3. Ask for confirmation before deletion
4. Remove both Ollama models and Modelfiles

**Delete Base Command** (`ctx --delete-base <model>`) will:
1. Find the specified base model and all its variants
2. List all models and Modelfiles to be removed
3. Ask for confirmation before deletion
4. Remove the base model, all variants, and corresponding Modelfiles
5. Perform proper cleanup of Ollama blobs and registry entries

**List Command** (`ctx --list [filter]`) will:
1. Scan all available Ollama models and identify base models
2. Extract context window information and parameter sizes
3. Calculate estimated memory usage for context storage
4. Display models in a ranked table sorted by maximum context window (descending)
5. Apply optional name filter to show only matching models
6. Exclude generated variants and derived models from the listing

### Example Output

**List Command:**
```
$ ./ollama-mama ctx --list
OLLAMA BASE MODELS - CONTEXT WINDOW LEADERBOARD
================================================

Rank | Model Name                              | Parameters | Max Context | Context Mem
-----|----------------------------------------|------------|-------------|-------------
1    | qwen2.5:72b                            | 72.7B      | 131,072     | ~256 MB
2    | llama3.1:70b                           | 70.6B      | 131,072     | ~256 MB
3    | qwen2.5:32b                            | 32.8B      | 131,072     | ~256 MB
4    | llama3.1:8b                            | 8.03B      | 131,072     | ~256 MB
5    | qwen2.5:14b                            | 14.8B      | 32,768      | ~64 MB
6    | llama2:13b                             | 13.0B      | 4,096       | ~8 MB
7    | llama2:7b                              | 6.74B      | 4,096       | ~8 MB

Found 7 base model(s).
```

**List Command with Filter:**
```
$ ./ollama-mama ctx --list llama
OLLAMA BASE MODELS - CONTEXT WINDOW LEADERBOARD
================================================
Filter: Models containing 'llama'

Rank | Model Name                              | Parameters | Max Context | Context Mem
-----|----------------------------------------|------------|-------------|-------------
1    | llama3.1:70b                           | 70.6B      | 131,072     | ~256 MB
2    | llama3.1:8b                            | 8.03B      | 131,072     | ~256 MB
3    | llama2:13b                             | 13.0B      | 4,096       | ~8 MB
4    | llama2:7b                              | 6.74B      | 4,096       | ~8 MB

Found 4 base model(s).
Use './ollama-mama ctx --list' to see all base models.
```

**Create Command:**
```
$ ./ollama-mama ctx --create
INFO: Modelfiles will be saved in '/home/user/dev/ollama/Modelfiles'
Do you want to overwrite existing Modelfiles and recreate models? (y/N) n
INFO: Overwrite disabled. Existing models/files will be skipped.
--- Starting Model Scan ---
---
Processing Tag: llama2:7b
INFO: 'llama2:7b' identified as a base model.
INFO: Found Model: llama2 | Parameters: 7B | Max Context: 4096 tokens
 -> Preparing variant: llama2:7b-8k
    CREATED Modelfile: /home/user/dev/ollama/Modelfiles/my-llama2-7b-8k.modelfile
    CREATING Ollama model 'llama2:7b-8k'...
    SUCCESS: Created 'llama2:7b-8k'.
```

**Create Base Command:**
```
$ ./ollama-mama ctx --create-base llama2:7b
--- Starting Base Model Variant Creation ---
Target base model: llama2:7b
INFO: Modelfiles will be saved in '/home/user/dev/ollama/Modelfiles'
INFO: 'llama2:7b' validated as a base model.
Do you want to overwrite existing Modelfiles and recreate models? (y/N) y
INFO: Overwrite enabled. Existing files will be replaced.
---
Processing Tag: llama2:7b
INFO: Found Model: llama2 | Parameters: 7B | Max Context: 4096 tokens
 -> Preparing variant: llama2:7b-8k
    CREATED Modelfile: /home/user/dev/ollama/Modelfiles/my-llama2-7b-8k.modelfile
    CREATING Ollama model 'llama2:7b-8k'...
    SUCCESS: Created 'llama2:7b-8k'.
---
Base model variant creation completed.
```

**Delete Command:**
```
$ ./ollama-mama ctx --delete
--- Starting Model Deletion ---
Found 5 generated model variant(s):
  - llama2:7b-8k
  - llama2:7b-16k
  - llama2:7b-32k
  - qwen2.5:14b-8k
  - qwen2.5:14b-16k

Found 5 corresponding Modelfile(s):
  - my-llama2-7b-8k.modelfile
  - my-llama2-7b-16k.modelfile
  - my-llama2-7b-32k.modelfile
  - my-qwen2.5-14b-8k.modelfile
  - my-qwen2.5-14b-16k.modelfile

This will delete:
  - 5 Ollama model variant(s)
  - 5 Modelfile(s)

Do you want to proceed? (y/N)
```

**Delete Base Command:**
```
$ ./ollama-mama ctx --delete-base llama2:7b
--- Starting Base Model Deletion ---
Target base model: llama2:7b
Found 4 model(s) to delete:
  - llama2:7b (base model)
  - llama2:7b-8k (variant)
  - llama2:7b-16k (variant)
  - llama2:7b-32k (variant)

Found 3 corresponding Modelfile(s):
  - my-llama2-7b-8k.modelfile
  - my-llama2-7b-16k.modelfile
  - my-llama2-7b-32k.modelfile

This will delete:
  - 4 Ollama model(s) (including base model and variants)
  - 3 Modelfile(s)

Are you sure you want to proceed? (y/N)
```

This command is particularly useful when you want to completely remove a model family from your system, including the original base model and all context window variants that were generated from it.

**Auto-Rename Command:**
```
$ ./ollama-mama model --autorename
--- Starting Model Auto-Rename ---
Scanning for models with problematic naming patterns...

Found 2 model(s) with problematic naming patterns:

Proposed Transformations:
=========================
  hf.co/TheBloke/deepseek-coder-6.7B-instruct-GGUF:Q4_K_M
    -> hf.co/TheBloke/deepseek-coder-instruct-GGUF-Q4_K_M:7B

  microsoft/DialoGPT-30B-base:Q8_0
    -> microsoft/DialoGPT-base-Q8_0:30B

This will:
  - Create 2 new model(s) with corrected names
  - Generate corresponding Modelfiles

Do you want to proceed with the transformations? (y/N) y

Do you want to remove the original models after successful rename? (y/N) y
INFO: Original models will be removed after successful transformation.

INFO: Modelfiles will be saved in '/home/user/dev/ollama/Modelfiles'

--- Processing Transformations ---
Processing: hf.co/TheBloke/deepseek-coder-6.7B-instruct-GGUF:Q4_K_M -> hf.co/TheBloke/deepseek-coder-instruct-GGUF-Q4_K_M:7B
  CREATED Modelfile: /home/user/dev/ollama/Modelfiles/autorename-hf.co-TheBloke-deepseek-coder-instruct-GGUF-Q4_K_M-7B.modelfile
  CREATING Ollama model 'hf.co/TheBloke/deepseek-coder-instruct-GGUF-Q4_K_M:7B'...
  SUCCESS: Created 'hf.co/TheBloke/deepseek-coder-instruct-GGUF-Q4_K_M:7B'

--- Auto-Rename Summary ---
Successfully transformed: 2 model(s)
Failed transformations: 0

Successfully renamed models:
  hf.co/TheBloke/deepseek-coder-6.7B-instruct-GGUF:Q4_K_M -> hf.co/TheBloke/deepseek-coder-instruct-GGUF-Q4_K_M:7B
  microsoft/DialoGPT-30B-base:Q8_0 -> microsoft/DialoGPT-base-Q8_0:30B

Auto-rename completed.
```

## ⚙️ Configuration

### Default Settings

- **Output Directory**: `$HOME/dev/ollama/Modelfiles`
- **Starting Context**: 8192 tokens (8K)
- **Context Progression**: Doubles each iteration (8K → 16K → 32K → ...)

### Customization

Edit the script to modify these variables:

```bash
# Change output directory
OUTPUT_DIR="$HOME/my-custom-path/Modelfiles"

# Change starting context size
START_CONTEXT=4096  # Start at 4K instead of 8K
```

## 📁 Generated Files

### Modelfile Structure

Each generated Modelfile follows this format:

```dockerfile
# Modelfile for llama2:7b-16k
# Generated by script on Thu Oct 10 14:30:25 EDT 2025

FROM llama2:7b

# Set the new context window size
PARAMETER num_ctx 16384
```

### File Naming Convention

- **Modelfiles**: `my-{model-name}-{params}-{context}.modelfile`
- **Ollama Tags**: `{original-name}:{params}-{context}`

Examples:
- `my-llama2-7b-8k.modelfile` → `llama2:7b-8k`
- `my-microsoft-DialoGPT-medium-16k.modelfile` → `microsoft/DialoGPT:medium-16k`

## 🔧 Troubleshooting

### Common Issues

**Q: Script says "No Ollama models found"**
```bash
# Check if Ollama is running and has models
ollama list
```

**Q: Permission denied when creating files**
```bash
# Ensure output directory is writable
mkdir -p "$HOME/dev/ollama/Modelfiles"
chmod 755 "$HOME/dev/ollama/Modelfiles"
```

**Q: Model creation fails**
```bash
# Check Ollama service status
ollama serve  # Run in another terminal if needed
```

**Q: Git errors during execution**
```bash
# Initialize git repository if needed
git init
git add .
git commit -m "Initial commit"
```

### Debug Mode

For verbose output, run with bash debug mode:
```bash
bash -x ./ollama-mama ctx --create
bash -x ./ollama-mama ctx --delete
```

### Usage Help

Display available commands:
```bash
./ollama-mama
# Shows: Usage: ./ollama-mama ctx [--create|--delete|--delete-base <model>]
```

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Development Guidelines

- Follow existing code style and conventions
- Add comments for complex logic
- Test with multiple Ollama versions
- Update documentation for new features

## 📋 Roadmap

- [ ] Support for custom context size ranges
- [ ] JSON configuration file support
- [ ] Model performance benchmarking
- [ ] Integration with Ollama model registry
- [ ] GUI version for non-technical users

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Ollama](https://ollama.ai/) team for the amazing local LLM platform
- Community contributors and testers
- Everyone who provided feedback and bug reports

## 📞 Support

- 🐛 **Bug Reports**: [GitHub Issues](https://github.com/lfontanez/tool-ollama-mama/issues)
- 💡 **Feature Requests**: [GitHub Discussions](https://github.com/lfontanez/tool-ollama-mama/discussions)
- 📧 **Contact**: [Your Email](mailto:your.email@example.com)

---

<div align="center">
  <strong>Made with ❤️ for the Ollama community</strong>
</div>
