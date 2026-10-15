# Local OpenCode API Gateway

An incredibly fast, OpenAI-compatible local API gateway optimized for coding and reasoning tasks. Built to run with zero performance overhead inside Docker using the **SGLang** inference engine, utilizing bare-metal NVIDIA GPU acceleration.

## Prerequisites

- **OS:** Linux (Ubuntu 22.04+ recommended)
- **Hardware:** NVIDIA GPU with modern drivers (e.g., ASUS GX10, H100, A100, RTX 4090)
- **Software:** Docker, Docker Compose, and the [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

## Quick Start

1. **Clone and Enter Repository:**
   ```bash
   git clone <your-repo-url> opencode-api
   cd opencode-api
   ```

2. **Configure Environment:**
   Copy the example environment file and fill out variables if running gated models.
   ```bash
   cp .env.example .env
   ```

3. **Launch the Server:**
   ```bash
   docker compose up -d
   ```
   *Note: The initial run will download the model weights (approx. 30GB+ for Qwen 32B), which may take several minutes depending on your internet connection.*

4. **Verify Logs:**
   Ensure the server boots and loads weights cleanly into your VRAM:
   ```bash
   docker logs -f sglang-opencode-server
   ```
   Look for: `The server is fired up and listening at http://0.0.0.0:8000`

## Editor Integration

### Continue (VS Code / JetBrains)
Add this to your `~/.continue/config.json`:
```json
{
  "models": [
    {
      "title": "Local OpenCode Engine",
      "provider": "openai",
      "model": "qwen/Qwen2.5-32B-Instruct",
      "apiBase": "http://localhost:8000/v1"
    }
  ],
  "tabAutocompleteModel": {
    "title": "Local Autocomplete",
    "provider": "openai",
    "model": "qwen/Qwen2.5-32B-Instruct",
    "apiBase": "http://localhost:8000/v1"
  }
}
```

### Aider
Run aider directly against your local container:
```bash
aider --openai-api-base http://localhost:8000/v1 --openai-api-key none --model openai/qwen/Qwen2.5-32B-Instruct
```

## Changing Models
To switch to a different model (e.g., a smaller model or a larger dense architecture):
1. Edit the `MODEL_PATH` variable in your `.env` file.
2. Restart the architecture: `docker compose down && docker compose up -d`
