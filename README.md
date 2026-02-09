# Cost-Optimized Oh-My-OpenCode Configurations

**Multiple profiles available**: Choose the balance between cost savings and quality that works for you.

---

## 📦 Prerequisites & Installation

### System Requirements

- **Operating System**: macOS, Linux, or Windows (WSL recommended)
- **Terminal**: Modern terminal emulator (WezTerm, Alacritty, Ghostty, Kitty, or iTerm2)
- **Node.js**: 18+ (for npm/bun installation methods)
- **API Keys**: At least one LLM provider (see [Provider Setup](#provider-setup))

---

### Step 1: Install OpenCode

OpenCode is the AI coding agent that oh-my-opencode extends. You must install it first.

#### Install Script

```bash
curl -fsSL https://opencode.ai/install | bash
```

**Verify installation:**
```bash
opencode --version
# Expected: 1.1.53 or higher
```

---

### Step 2: Install Oh-My-OpenCode Plugin

#### Interactive Installer (Recommended)

```bash
Install and configure oh-my-opencode by following the instructions here:
https://raw.githubusercontent.com/code-yeongyu/oh-my-opencode/refs/heads/master/docs/guide/installation.md
```

---

### Step 3: Configure API Providers

You need at least one API provider configured. The cost-optimized configs in this repo use multiple providers for best savings.

#### Required for Cost-Optimized Configs

| Provider | Used For | Cost | Setup |
|----------|----------|------|-------|
| **Anthropic** | Haiku, Sonnet | $$$ | [Get API Key](https://console.anthropic.com/) |
| **OpenCode Free Tier** | GLM-4.7, Kimi, MiniMax | FREE | [Sign up](https://opencode.ai/) |
| **DeepSeek** | Chat, Reasoner | $ | [Get API Key](https://platform.deepseek.com/) |
| **Alibaba Cloud** | Qwen models | $ | [Sign up](https://www.alibabacloud.com/) |
| **Google** | Gemini | $$ | [Get API Key](https://aistudio.google.com/) |

#### Configure API Keys

**DO NOT manually edit `~/.config/opencode/opencode.json`** to add API keys. Use OpenCode's built-in authentication commands instead:

**Method 1: Interactive Setup (Recommended)**

1. Launch OpenCode:
   ```bash
   opencode
   ```
2. Use the `/connect` command in the TUI:

3. Follow the prompts to authenticate each provider

---

### Step 4: Install This Cost-Optimized Config

Now install the cost-optimized configuration from this repository.

```bash
# Clone to a permanent location
git clone https://github.com/chenhaodev/mini-opencode.git
cd mini-opencode
# Use the prefered configuration
cp .opencode/profiles/balanced.json .opencode/oh-my-opencode.json
```
---

### Step 5: Verify Installation

1. **Check OpenCode is installed:**
   ```bash
   opencode --version
   ```

2. **Check oh-my-opencode is loaded:**
   ```bash
   cd /path/to/your-project
   opencode
   # In the TUI, look for "Oh My OpenCode" in the status or check if Sisyphus agent is available
   ```

3. **Verify config is being used:**
   ```bash
   cat .opencode/oh-my-opencode.json | jq '.agents.sisyphus.model'
   # Should show your selected model (e.g., "anthropic/claude-haiku-4-5")
   ```

4. **Check available models:**
   ```bash
   opencode models | grep -E "(haiku|deepseek|kimi|glm|qwen)"
   ```

---

## 📦 Version Compatibility

| Component | Version | Notes |
|-----------|---------|-------|
| **OpenCode** | 1.1.53+ | Tested and working |
| **Oh-My-OpenCode** | latest | Uses `@latest` channel |
| **Config Schema** | v1 | `oh-my-opencode.schema.json` |

> ⚠️ **Note**: These configurations use models available in OpenCode 1.1.53. If you're using an older version, some models may not be available. Run `opencode models` to check available models in your environment.

---

## 🎯 Quick Start - Choose Your Profile

| Profile | Best For | Cost | Quality | Anthropic Usage |
|---------|----------|------|---------|-----------------|
| **[Balanced](#balanced-default)** ⭐ | Most users | 💰💰 Very Low | ⚡⚡⚡ Good | Minimal |
| **[Ultra-Cheap](#ultra-cheap)** | Maximum savings | 💰 Almost Free | ⚡⚡ Decent | Zero |
| **[Haiku-Only](#haiku-only)** | Anthropic-only setups | 💰💰💰 Low | ⚡⚡⚡⚡ High | High |
| **[DeepSeek-Focused](#deepseek-focused)** | Code-heavy workflows | 💰💰 Very Low | ⚡⚡⚡⚡ High | Zero |
| **[Baseline (bak2)](#baseline-bak2)** | Original baseline | $$$$ | ⚡⚡⚡⚡⚡ Maximum | High |

---

## 📋 Profile Details

### Balanced (Default) ⭐

**Best all-around option** - Mixes quality Anthropic agents with free alternatives.

**Strategy:**
- Critical agents (Sisyphus, Oracle): `claude-haiku-4-5` (quality + speed)
- Planning (Prometheus): `kimi-k2.5-free` (FREE, good at planning)
- Analysis (Metis): `deepseek/deepseek-reasoner` (cheap, excellent reasoning)
- Exploration: `glm-4.7-free` (FREE)
- Heavy reasoning: `deepseek/deepseek-reasoner` (cheap powerhouse)

**Cost:** ~75-85% savings vs global Sonnet config

**Install:**
```bash
# Default - already configured
cp .opencode/profiles/oh-my-opencode.json .opencode/oh-my-opencode.json
```

---

### Ultra-Cheap

**Maximum cost savings** - Uses only FREE and ultra-cheap models.

**Strategy:**
- Main orchestrator: `kimi-k2.5-free` (FREE)
- Planning: `glm-4.7-free` (FREE)
- Analysis: `minimax-m2.1-free` (FREE)
- Reasoning: `deepseek/deepseek-reasoner` (ultra-cheap)
- Exploration: `glm-4.7-free` (FREE)

**Cost:** ~95-99% savings (almost entirely free!)

**Tradeoffs:**
- Slightly slower responses
- May need more retries for complex tasks
- Still highly capable for most work

**Install:**
```bash
cp .opencode/profiles/ultra-cheap.json .opencode/oh-my-opencode.json
```

---

### Haiku-Only

**Anthropic-focused** - Uses only Claude Haiku 4.5 (no free models).

**Strategy:**
- Everything runs on `claude-haiku-4-5`
- Consistent quality across all tasks
- Best for teams already invested in Anthropic

**Cost:** ~85-92% savings vs Sonnet (still significant!)

**Benefits:**
- Predictable behavior
- No need to configure multiple providers
- Fast and reliable

**Install:**
```bash
cp .opencode/profiles/haiku-only.json .opencode/oh-my-opencode.json
```

---

### DeepSeek-Focused

**Code-heavy workflows** - Leverages DeepSeek's excellent coding capabilities.

**Strategy:**
- Main agents: `deepseek/deepseek-chat` (fast, cheap)
- Reasoning tasks: `deepseek/deepseek-reasoner` (powerful thinking model)
- Exploration: `glm-4.7-free` (FREE)
- Writing: `qwen-turbo` (cheap, good at text)

**Cost:** ~90-95% savings

**Best for:**
- Code refactoring
- Architecture decisions
- Algorithm implementation
- Technical problem-solving

**Install:**
```bash
cp .opencode/profiles/deepseek-focused.json .opencode/oh-my-opencode.json
```

---

### Baseline (bak2)

**Original baseline configuration** - Reference configuration with mixed providers.

**Strategy:**
- Sisyphus: `anthropic/claude-sonnet-4-5` (original high-quality setting)
- Prometheus/Metis: `anthropic/claude-sonnet-4-5` with `variant: max`
- Hephaestus/Oracle/Momus: `openai/gpt-5.2` with variants
- Atlas: `opencode/kimi-k2.5-free` (FREE)
- Explore: `anthropic/claude-haiku-4-5` (cost-effective exploration)
- Librarian: `opencode/glm-4.7-free` (FREE research)
- Quick/Low categories: `minimax-cn/MiniMax-M2.1` (cheap)
- Visual/Writing: `google/gemini-3-pro-preview`

**Purpose:**
- Reference point for comparing other profiles
- Shows original cost-optimized baseline before further reductions
- Mix of premium (Sonnet, GPT-5.2) and cheap models
- Good balance for users who want quality but some savings

**Cost:** Moderate savings (~40-50% vs full Sonnet)

**Install:**
```bash
cp .opencode/profiles/baseline-bak2.json .opencode/oh-my-opencode.json
```

---

## 📊 Detailed Comparison

### Agent Assignments by Profile

| Agent | Balanced | Ultra-Cheap | Haiku-Only | DeepSeek | Baseline |
|-------|----------|-------------|------------|----------|----------|
| **sisyphus** | claude-haiku-4-5 | kimi-k2.5-free | claude-haiku-4-5 | deepseek-chat | claude-sonnet-4-5 |
| **prometheus** | kimi-k2.5-free | glm-4.7-free | claude-haiku-4-5 | deepseek-chat | claude-sonnet-4-5 (max) |
| **metis** | deepseek-reasoner | minimax-m2.1-free | claude-haiku-4-5 | deepseek-reasoner | claude-sonnet-4-5 (max) |
| **oracle** | claude-haiku-4-5 | deepseek-reasoner | claude-haiku-4-5 | deepseek-reasoner | gpt-5.2 (high) |
| **explore** | glm-4.7-free | glm-4.7-free | claude-haiku-4-5 | glm-4.7-free | claude-haiku-4-5 |
| **librarian** | glm-4.7-free | glm-4.7-free | claude-haiku-4-5 | glm-4.7-free | glm-4.7-free |
| **atlas** | - | - | - | - | kimi-k2.5-free |
| **hephaestus** | - | - | - | - | gpt-5.2-codex (medium) |
| **momus** | - | - | - | - | gpt-5.2 (medium) |

### Category Assignments by Profile

| Category | Balanced | Ultra-Cheap | Haiku-Only | DeepSeek | Baseline |
|----------|----------|-------------|------------|----------|----------|
| **quick** | glm-4.7-free | glm-4.7-free | claude-haiku-4-5 | glm-4.7-free | minimax-m2.1 |
| **ultrabrain** | deepseek-reasoner | deepseek-reasoner | claude-haiku-4-5 | deepseek-reasoner | gpt-5.2-codex (xhigh) |
| **deep** | kimi-k2.5-free | kimi-k2.5-free | claude-haiku-4-5 | deepseek-reasoner | gpt-5.2-codex (medium) |
| **unspecified-high** | claude-haiku-4-5 | deepseek-reasoner | claude-haiku-4-5 | deepseek-reasoner | claude-sonnet-4-5 |
| **writing** | qwen-turbo | qwen-turbo | claude-haiku-4-5 | qwen-turbo | gemini-3-pro |

---

## 🚀 Quick Installation (If Already Set Up)

If you already have OpenCode and oh-my-opencode installed, here's the quick way to use these configs:

### Clone and Copy

```bash
# Clone this repository
git clone https://github.com/yourusername/mini-opencode.git

# Copy to your project
cp mini-opencode/.opencode/profiles/balanced.json /path/to/your-project/.opencode/oh-my-opencode.json

# Or choose a different profile:
cp mini-opencode/.opencode/profiles/ultra-cheap.json /path/to/your-project/.opencode/oh-my-opencode.json
```

### Direct Download

```bash
mkdir -p .opencode
curl -o .opencode/oh-my-opencode.json \
  https://raw.githubusercontent.com/yourusername/mini-opencode/main/.opencode/profiles/balanced.json
```

**For full installation instructions** (first-time setup), see the [Prerequisites & Installation](#step-1-install-opencode) section above.

---

## 🔧 How It Works

### Configuration Hierarchy

Oh-my-opencode uses this priority order:

1. **Project config** (`.opencode/oh-my-opencode.json`) ← **HIGHEST PRIORITY**
2. User config (`~/.config/opencode/oh-my-opencode.json`)

This means:
- ✅ Your global config stays unchanged
- ✅ Only projects with this `.opencode/` directory use these settings
- ✅ Different projects can use different profiles

### What Gets Overridden

These configs override:
- ✅ Agent model assignments (sisyphus, prometheus, metis, oracle, etc.)
- ✅ Category model assignments (quick, ultrabrain, deep, etc.)
- ✅ Model variants (thinking budgets, temperature, etc.)

These stay from global config:
- ⏩ Disabled hooks
- ⏩ MCP configurations
- ⏩ Permission settings
- ⏩ Background task concurrency

---

## 💰 Cost Breakdown

### Model Pricing Reference

| Model | Provider | Cost | Notes |
|-------|----------|------|-------|
| `claude-sonnet-4-5` | Anthropic | $$$$ | Your current global default (expensive!) |
| `claude-haiku-4-5` | Anthropic | $$ | ~10-20x cheaper than Sonnet |
| `deepseek/deepseek-chat` | DeepSeek | $ | Very cheap, excellent for code |
| `deepseek/deepseek-reasoner` | DeepSeek | $$ | Cheap with reasoning capabilities |
| `kimi-k2.5-free` | OpenCode | FREE | Free tier available |
| `glm-4.7-free` | OpenCode | FREE | Free tier available |
| `minimax-m2.1-free` | OpenCode | FREE | Free tier available |
| `qwen-turbo` | Alibaba | $ | Very cheap, good quality |

### Expected Savings by Profile

| Profile | Anthropic Cost | Total Cost | vs Global Sonnet |
|---------|----------------|------------|------------------|
| Global (Sonnet) | 100% | 100% | Baseline |
| **Baseline (bak2)** | 40-50% | 50-60% | **40-50% savings** |
| **Balanced** | 15-25% | 10-15% | **85-90% savings** |
| **Ultra-Cheap** | 0% | 1-5% | **95-99% savings** |
| **Haiku-Only** | 8-15% | 8-15% | **85-92% savings** |
| **DeepSeek** | 0% | 5-10% | **90-95% savings** |

---

### See Available Models

```bash
opencode models | grep -E "(haiku|deepseek|kimi|glm|qwen|minimax)"
```

---

## 🧪 Verification

After installation:

1. **Check file exists:**
   ```bash
   ls -la .opencode/oh-my-opencode.json
   ```

2. **Validate JSON:**
   ```bash
   cat .opencode/oh-my-opencode.json | jq .
   ```

3. **Verify it loads:**
   - Open your project in OpenCode/Claude
   - Start a session
   - Check which models are being used (shown in agent responses)

---

## ⚖️ When to Use Each Profile

### Use **Balanced** if:
- ✅ You want the best cost/quality ratio
- ✅ You're doing general development work
- ✅ You want minimal Anthropic usage without sacrificing quality

### Use **Ultra-Cheap** if:
- ✅ You're on a tight budget
- ✅ You're doing exploratory work, learning, or experimenting
- ✅ You don't mind occasional lower-quality responses

### Use **Haiku-Only** if:
- ✅ You're already invested in Anthropic
- ✅ You want consistent behavior across all tasks
- ✅ You don't want to manage multiple providers

### Use **DeepSeek-Focused** if:
- ✅ You're doing code-heavy work
- ✅ You need strong reasoning for architecture decisions
- ✅ You want high quality at very low cost

### Use **Baseline (bak2)** if:
- ✅ You want the original reference configuration
- ✅ You need premium quality (Sonnet, GPT-5.2) for critical tasks
- ✅ You want moderate savings without aggressive cost-cutting
- ✅ You're comparing against the original cost-optimized baseline

---

### Config Issues

#### Config not being applied

1. Ensure file is at `.opencode/oh-my-opencode.json` (not in project root)
2. Validate JSON: `cat .opencode/oh-my-opencode.json | jq .`
3. Restart OpenCode/Claude
4. Check you're in the correct project directory
5. Verify config schema is correct:
   ```bash
   cat .opencode/oh-my-opencode.json | jq 'has("$schema")'
   # Should output: true
   ```

#### Model not available error

Some models require specific provider setup:
- `deepseek/*`: Requires DeepSeek API key
- `alibaba-cn/*`: Requires Alibaba Cloud account
- `opencode/*-free`: Requires OpenCode free tier registration

Check which models are available:
```bash
opencode models
```
### Cost Issues

#### Still seeing high costs

1. Verify which agents are actually being used
2. Check if background tasks are using different models
3. Ensure you're in a project with the config file
4. Global config might still apply if local config doesn't override specific agents
5. Check variant settings (max variants use more tokens):
   ```bash
   cat .opencode/oh-my-opencode.json | jq '.agents | to_entries[] | select(.value.variant)'
   ```

#### Unexpected provider charges

**Track usage by provider:**
```bash
# Check which models are being used most
opencode logs | grep -E "(model|provider)"
```

**Disable expensive agents temporarily:**
```json
{
  "agents": {
    "oracle": {
      "disable": true
    }
  }
}
```

### Performance Issues

#### Slow responses

**If using free models:**
- Free tiers may have rate limits
- Consider using paid tiers for critical work
- Ultra-cheap profile may be slower due to free model limitations

**Solutions:**
1. Switch to faster profile (Balanced or Haiku-Only)
2. Check your internet connection
3. Some providers (DeepSeek, Alibaba) may have latency from certain regions

#### Out of memory errors

**If using large context models:**
```bash
# Check context window settings
cat .opencode/oh-my-opencode.json | jq '.agents | to_entries[] | select(.value.model | contains("deepseek-reasoner"))'
```

**Solutions:**
1. Use smaller context models (Haiku instead of Reasoner)
2. Clear conversation history regularly
3. Use `compact` command in OpenCode to reduce context

---

## 🤝 Contributing

Found an even better cost optimization? Submit a PR!

1. Test your configuration thoroughly
2. Document the cost/quality tradeoffs
3. Submit with clear comparison data

---

## 📄 License

MIT - Use freely, modify as needed.

---

## 📚 Additional Resources

- [Oh-My-OpenCode Documentation](https://github.com/code-yeongyu/oh-my-opencode/blob/master/docs/configurations.md)
- [OpenCode Model Comparison](https://opencode.ai/models)
- [DeepSeek API Docs](https://platform.deepseek.com/api-docs/)
- [Kimi Free Tier](https://platform.moonshot.cn/)

---

**Note:** These configurations prioritize cost savings. For mission-critical production code, consider using your global Sonnet/Opus config or manually selecting higher-quality models for specific tasks.
