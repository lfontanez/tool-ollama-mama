# 🦙 Ollama Context Window Variant Generator

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Bash](https://img.shields.io/badge/Language-Bash-green.svg)](https://www.gnu.org/software/bash/)
[![Ollama](https://img.shields.io/badge/Requires-Ollama-blue.svg)](https://ollama.ai/)

> **Automatically generate Ollama model variants with different context window sizes for optimal performance across various use cases.**

## 📖 Description

The Ollama Context Window Variant Generator (`ollama-mama`) is a powerful bash script that scans your existing Ollama models and automatically creates variants with different context window sizes. This allows you to optimize model performance for different scenarios - from quick responses with smaller contexts to comprehensive analysis with larger contexts.

### ✨ Key Features

- 🔍 **Smart Model Detection** - Automatically scans and identifies base models vs. derived models
- 📏 **Progressive Context Scaling** - Creates variants starting from 8K, doubling up to the model's maximum context
- 🛡️ **Robust Error Handling** - Comprehensive edge case handling and graceful error recovery
- 📁 **Organized Output** - Generates clean Modelfiles in a structured directory
- 🔧 **Flexible Configuration** - Customizable output directory and starting context size
- 🏷️ **Namespace Support** - Handles complex model names with slashes (e.g., `namespace/model`)
- 🔄 **Version Compatibility** - Works with both newer and older Ollama versions
- 💬 **Interactive Mode** - User-friendly prompts for overwrite decisions

## 🚀 Quick Start

### Prerequisites

- **Ollama** installed and running ([Installation Guide](https://ollama.ai/))
- **Bash** shell (Linux/macOS/WSL)
- At least one Ollama model downloaded

### Installation

1. **Download the script:**
   ```bash
   curl -O https://raw.githubusercontent.com/yourusername/ollama-mama/main/ollama-mama
   ```

2. **Make it executable:**
   ```bash
   chmod +x ollama-mama
   ```

3. **Run the script:**
   ```bash
   ./ollama-mama
   ```

## 📋 Usage

### Basic Usage

Simply run the script and follow the interactive prompts:

```bash
./ollama-mama
```

The script will:
1. Scan your existing Ollama models
2. Ask if you want to overwrite existing variants
3. Generate context window variants for each base model
4. Create Modelfiles and register new models with Ollama

### Example Output

```
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

## ⚙️ Configuration

### Environment Variables

You can customize the script behavior by modifying these variables at the top of the script:

```bash
# Default output directory for Modelfiles
OUTPUT_DIR="$HOME/dev/ollama/Modelfiles"

# Starting context window size (in tokens)
START_CONTEXT=8192
```

### Generated Model Naming Convention

Models are named following this pattern:
```
{original-model}:{parameters}-{context}k
```

Examples:
- `llama2:7b-8k` (8,192 tokens)
- `llama2:7b-16k` (16,384 tokens)
- `codellama:13b-32k` (32,768 tokens)

### Modelfile Naming Convention

Modelfiles are saved with this pattern:
```
my-{model-name}-{parameters}-{context}k.modelfile
```

## 🎯 Use Cases

### 📝 Content Creation
- **8K context**: Quick responses, code snippets
- **16K context**: Medium articles, documentation
- **32K+ context**: Long-form content, comprehensive analysis

### 💻 Development
- **8K context**: Code completion, quick debugging
- **16K context**: Code review, refactoring suggestions
- **32K+ context**: Architecture analysis, large codebase understanding

### 📊 Data Analysis
- **8K context**: Quick data insights
- **16K context**: Report generation
- **32K+ context**: Comprehensive data analysis, large dataset processing

## 🔧 Troubleshooting

### Common Issues

#### "No Ollama models found"
```bash
# Check if Ollama is running
ollama list

# If empty, download a model first
ollama pull llama2:7b
```

#### "Could not determine parameter size or max context"
This usually happens with older Ollama versions or custom models. The script will skip these models and continue with others.

#### Permission denied errors
```bash
# Ensure the script is executable
chmod +x ollama-mama

# Check if output directory is writable
ls -la ~/dev/ollama/
```

#### Model creation fails
- Ensure you have enough disk space
- Check that Ollama service is running: `ollama serve`
- Verify the base model exists: `ollama list`

### Debug Mode

For verbose output, you can modify the script to add debug information:
```bash
# Add this line after the shebang for debug mode
set -x
```

## 🤝 Contributing

We welcome contributions! Here's how you can help:

### Reporting Issues
- Use the [GitHub Issues](https://github.com/yourusername/ollama-mama/issues) page
- Include your Ollama version: `ollama --version`
- Provide the full error output
- Mention your operating system

### Feature Requests
- Check existing issues first
- Clearly describe the use case
- Provide examples if possible

### Pull Requests
1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Make your changes
4. Test thoroughly
5. Submit a pull request with a clear description

### Development Setup
```bash
git clone https://github.com/yourusername/ollama-mama.git
cd ollama-mama
chmod +x ollama-mama
./ollama-mama
```

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Ollama](https://ollama.ai/) team for creating an amazing local LLM platform
- The open-source community for inspiration and feedback
- Contributors who help improve this tool

## 📞 Support

- 📖 **Documentation**: Check this README and inline script comments
- 🐛 **Bug Reports**: [GitHub Issues](https://github.com/yourusername/ollama-mama/issues)
- 💬 **Discussions**: [GitHub Discussions](https://github.com/yourusername/ollama-mama/discussions)
- 📧 **Contact**: [your.email@example.com](mailto:your.email@example.com)

---

<div align="center">

**Made with ❤️ for the Ollama community**

[⭐ Star this repo](https://github.com/yourusername/ollama-mama) • [🐛 Report Bug](https://github.com/yourusername/ollama-mama/issues) • [✨ Request Feature](https://github.com/yourusername/ollama-mama/issues)

</div>
