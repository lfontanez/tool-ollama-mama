# 🦙 Ollama Context Window Variant Generator

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Shell Script](https://img.shields.io/badge/Shell-Bash-green.svg)](https://www.gnu.org/software/bash/)
[![Ollama](https://img.shields.io/badge/Ollama-Compatible-blue.svg)](https://ollama.ai/)

A robust bash script that automatically generates Ollama model variants with different context window sizes. Perfect for testing and optimizing model performance across various context lengths.

## ✨ Features

- 🔄 **Automatic Variant Generation**: Creates model variants with context sizes from 8K up to the model's maximum capacity
- 🗑️ **Cleanup Management**: Delete generated model variants and their Modelfiles with confirmation
- 🎯 **Selective Base Model Deletion**: Remove specific base models and all their generated variants
- 🛡️ **Edge Case Handling**: Properly handles namespace/model names (e.g., `microsoft/DialoGPT`)
- 🔧 **Backward Compatibility**: Works with older Ollama versions by parsing plain text output
- 📁 **Smart File Management**: Sanitizes model names for valid filenames
- ⚡ **Batch Processing**: Processes all available models in one run
- 🎯 **Selective Overwrite**: Choose whether to overwrite existing models/files
- 📊 **Progress Tracking**: Clear progress indicators and status messages
- 🚫 **Error Prevention**: Prevents silent exits with comprehensive error handling

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

The script now uses a command-based structure:

```bash
./ollama-mama ctx [--create|--delete|--delete-base <model>|--create-base <model>]
```

### Create Model Variants

Generate context window variants for all base models:

```bash
./ollama-mama ctx --create
```

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

### Example Output

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
