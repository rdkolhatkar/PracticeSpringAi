# 🤖 Spring AI — Multi-LLM Provider Setup Guide

A complete step-by-step guide for integrating **Spring AI** with multiple LLM providers:
**Anthropic Claude**, **Google Gemini**, **Mistral AI**, and **Ollama (Free/Local)**.

---

## 📋 Table of Contents

- [Anthropic Claude](#-anthropic-claude)
- [Google Gemini](#-google-gemini)
- [Mistral AI](#-mistral-ai)
- [Ollama (100% Free, Local)](#-ollama-100-free-local)
- [application.yaml — All Providers](#applicationyaml--all-providers-reference)
- [Switching Between Providers](#switching-between-providers)
- [Free Tier Comparison](#free-tier-comparison)
- [Security Best Practices](#security-best-practices)

---

## 🟣 Anthropic Claude

### Step 1 — Create a Free Anthropic Account

1. Go to: **https://console.anthropic.com/login**
2. Click **"Sign up"**
3. Register using:
   - ✅ Email and Password
   - ✅ Google account
4. Verify your email address by clicking the link sent to your inbox
5. You may be asked to verify your **phone number** — enter the OTP received via SMS
6. Complete the short onboarding form (name, use case)
7. You will land on the **Anthropic Console dashboard**

> 💡 **Student Tip:** Anthropic provides **$5 free credits** for new accounts.
> Use `claude-haiku-3-5` — it is the fastest and cheapest Claude model, ideal for practice.

---

### Step 2 — Generate Your Anthropic API Key

1. Go to: **https://console.anthropic.com/settings/keys**
   *(Or from the dashboard: click "API Keys" in the left sidebar)*
2. Click **"+ Create Key"**
3. Enter a name for your key, e.g., `spring-ai-claude-practice`
4. Click **"Create Key"**
5. Your API key will be shown **only once** — it looks like:
   ```
   sk-ant-api03-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
   ```
6. ⚠️ **Copy and save this key immediately.** You cannot view it again after closing the dialog.
7. Click **"Done"**

---

### Step 3 — Add Anthropic Dependency

#### Gradle (`build.gradle`):
```groovy
dependencies {
    implementation 'org.springframework.ai:spring-ai-anthropic-spring-boot-starter'
}

dependencyManagement {
    imports {
        mavenBom "org.springframework.ai:spring-ai-bom:1.0.0-M1"
    }
}
```

#### Maven (`pom.xml`):
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-anthropic-spring-boot-starter</artifactId>
</dependency>
```

---

### Step 4 — Configure application.yaml for Claude

```yaml
spring:
  application:
    name: PracticeSpringAi
  ai:
    anthropic:
      api-key: ${ANTHROPIC_API_KEY}
      chat:
        options:
          model: claude-haiku-4-5          # Cheapest Claude model for practice
          max-tokens: 1024
          temperature: 0.7
```

#### Set Environment Variable:

**Windows (CMD):**
```cmd
setx ANTHROPIC_API_KEY "sk-ant-api03-your-key-here"
```

**Windows (PowerShell):**
```powershell
$env:ANTHROPIC_API_KEY = "sk-ant-api03-your-key-here"
```

**macOS / Linux:**
```bash
export ANTHROPIC_API_KEY="sk-ant-api03-your-key-here"
```

#### Available Claude Models (for students):

| Model | Best For | Cost |
|-------|----------|------|
| `claude-haiku-4-5` | Fast, cheap, practice | Cheapest |
| `claude-sonnet-4-5` | Balanced quality | Mid-range |
| `claude-opus-4-5` | Most capable | Higher cost |

---

### Step 5 — Sample Controller for Claude

```java
import org.springframework.ai.anthropic.AnthropicChatModel;
import org.springframework.ai.chat.messages.UserMessage;
import org.springframework.ai.chat.prompt.Prompt;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/claude")
public class ClaudeController {

    private final AnthropicChatModel chatModel;

    public ClaudeController(AnthropicChatModel chatModel) {
        this.chatModel = chatModel;
    }

    @GetMapping("/ask")
    public String ask(@RequestParam String message) {
        return chatModel.call(new Prompt(new UserMessage(message)))
                        .getResult()
                        .getOutput()
                        .getContent();
    }
}
```

---

## 🔵 Google Gemini

### Step 1 — Create a Free Google AI Studio Account

1. Go to: **https://aistudio.google.com**
2. Sign in with your **existing Google account** (Gmail)
   - If you don't have one, create at: **https://accounts.google.com/signup**
3. Accept the **Terms of Service**
4. You will land on the **Google AI Studio** dashboard

> 💡 **Student Tip:** Google AI Studio offers a **free tier** with generous rate limits.
> No credit card is required for the free tier!

---

### Step 2 — Generate Your Gemini API Key

1. Inside Google AI Studio, click **"Get API Key"** in the left sidebar
   *(Or go to: **https://aistudio.google.com/app/apikey**)*
2. Click **"+ Create API Key"**
3. Select a **Google Cloud Project**:
   - If you have no project, click **"Create API key in new project"** — Google creates one automatically
4. Your API key will be displayed — it looks like:
   ```
   AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
   ```
5. ⚠️ **Copy this key and store it safely.**
6. Click **"Close"**

---

### Step 3 — Add Gemini Dependency

#### Gradle:
```groovy
dependencies {
    implementation 'org.springframework.ai:spring-ai-vertex-ai-gemini-spring-boot-starter'
}
```

#### Maven:
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-vertex-ai-gemini-spring-boot-starter</artifactId>
</dependency>
```

---

### Step 4 — Configure application.yaml for Gemini

```yaml
spring:
  application:
    name: PracticeSpringAi
  ai:
    vertex:
      ai:
        gemini:
          api-key: ${GEMINI_API_KEY}
          chat:
            options:
              model: gemini-1.5-flash      # Free tier model, fast and capable
              temperature: 0.7
```

#### Set Environment Variable:

**Windows (CMD):**
```cmd
setx GEMINI_API_KEY "AIzaSy-your-key-here"
```

**macOS / Linux:**
```bash
export GEMINI_API_KEY="AIzaSy-your-key-here"
```

#### Available Gemini Models (for students):

| Model | Best For | Cost |
|-------|----------|------|
| `gemini-1.5-flash` | Fast, practice, free tier | Free / Very cheap |
| `gemini-1.5-pro` | Complex tasks | Low cost |
| `gemini-2.0-flash` | Latest, fast | Free / Very cheap |

---

### Step 5 — Sample Controller for Gemini

```java
import org.springframework.ai.vertexai.gemini.VertexAiGeminiChatModel;
import org.springframework.ai.chat.messages.UserMessage;
import org.springframework.ai.chat.prompt.Prompt;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/gemini")
public class GeminiController {

    private final VertexAiGeminiChatModel chatModel;

    public GeminiController(VertexAiGeminiChatModel chatModel) {
        this.chatModel = chatModel;
    }

    @GetMapping("/ask")
    public String ask(@RequestParam String message) {
        return chatModel.call(new Prompt(new UserMessage(message)))
                        .getResult()
                        .getOutput()
                        .getContent();
    }
}
```

---

## 🟠 Mistral AI

### Step 1 — Create a Free Mistral Account

1. Go to: **https://console.mistral.ai**
2. Click **"Sign up"**
3. Register with Email or Google account
4. Verify your email
5. You land on the **La Plateforme dashboard**

> 💡 **Student Tip:** Mistral provides free credits for new accounts. `mistral-small` is extremely
> affordable for practice.

---

### Step 2 — Generate Your Mistral API Key

1. Go to: **https://console.mistral.ai/api-keys**
2. Click **"+ Create new key"**
3. Name your key (e.g., `spring-ai-practice`)
4. Click **"Create"**
5. Copy the key that looks like:
   ```
   xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
   ```
6. ⚠️ **Save it immediately — it won't be shown again.**

---

### Step 3 — Add Mistral Dependency

#### Gradle:
```groovy
dependencies {
    implementation 'org.springframework.ai:spring-ai-mistral-ai-spring-boot-starter'
}
```

#### Maven:
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-mistral-ai-spring-boot-starter</artifactId>
</dependency>
```

---

### Step 4 — Configure application.yaml for Mistral

```yaml
spring:
  application:
    name: PracticeSpringAi
  ai:
    mistralai:
      api-key: ${MISTRAL_API_KEY}
      chat:
        options:
          model: mistral-small-latest      # Cheapest for practice
          temperature: 0.7
```

#### Set Environment Variable:

**Windows (CMD):**
```cmd
setx MISTRAL_API_KEY "your-mistral-key-here"
```

**macOS / Linux:**
```bash
export MISTRAL_API_KEY="your-mistral-key-here"
```

---

## 🟢 Ollama (100% Free, Local)

> 🏆 **Best option for students who want ZERO cost!**
> Ollama runs LLMs 100% locally on your computer — **no API key needed, no internet required, no charges.**

### Step 1 — Install Ollama

1. Go to: **https://ollama.com/download**
2. Download and install for your OS:
   - **Windows:** Download the `.exe` installer and run it
   - **macOS:** Download the `.dmg` file and drag to Applications
   - **Linux:** Run in terminal:
     ```bash
     curl -fsSL https://ollama.com/install.sh | sh
     ```
3. After installation, Ollama runs as a background service at `http://localhost:11434`

---

### Step 2 — Download a Free Model

Open your terminal and run:

```bash
# Lightweight, fast model — great for practice on most laptops
ollama pull llama3.2

# Smaller model for low-memory machines
ollama pull llama3.2:1b

# Google's free model
ollama pull gemma3

# Microsoft's small but capable model
ollama pull phi4-mini
```

> 💡 For most student laptops (8GB RAM), use `llama3.2` or `phi4-mini`

---

### Step 3 — Add Ollama Dependency

#### Gradle:
```groovy
dependencies {
    implementation 'org.springframework.ai:spring-ai-ollama-spring-boot-starter'
}
```

#### Maven:
```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-ollama-spring-boot-starter</artifactId>
</dependency>
```

---

### Step 4 — Configure application.yaml for Ollama

```yaml
spring:
  application:
    name: PracticeSpringAi
  ai:
    ollama:
      base-url: http://localhost:11434      # Default Ollama URL — no API key needed!
      chat:
        options:
          model: llama3.2                   # The model you pulled
          temperature: 0.7
```

No API key required! Ollama runs entirely on your machine.

---

### Step 5 — Sample Controller for Ollama

```java
import org.springframework.ai.ollama.OllamaChatModel;
import org.springframework.ai.chat.messages.UserMessage;
import org.springframework.ai.chat.prompt.Prompt;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/ollama")
public class OllamaController {

    private final OllamaChatModel chatModel;

    public OllamaController(OllamaChatModel chatModel) {
        this.chatModel = chatModel;
    }

    @GetMapping("/ask")
    public String ask(@RequestParam String message) {
        return chatModel.call(new Prompt(new UserMessage(message)))
                        .getResult()
                        .getOutput()
                        .getContent();
    }
}
```

---

## application.yaml — All Providers Reference

Below is a full reference showing all providers in one file.
**Uncomment only the provider you want to use:**

```yaml
spring:
  application:
    name: PracticeSpringAi

  ai:

    # -------------------------------------------------------
    # OPTION 1: Anthropic Claude
    # -------------------------------------------------------
    anthropic:
      api-key: ${ANTHROPIC_API_KEY}
      chat:
        options:
          model: claude-haiku-4-5
          max-tokens: 1024
          temperature: 0.7

    # -------------------------------------------------------
    # OPTION 2: Google Gemini
    # -------------------------------------------------------
    vertex:
      ai:
        gemini:
          api-key: ${GEMINI_API_KEY}
          chat:
            options:
              model: gemini-1.5-flash
              temperature: 0.7

    # -------------------------------------------------------
    # OPTION 3: Mistral AI
    # -------------------------------------------------------
    mistralai:
      api-key: ${MISTRAL_API_KEY}
      chat:
        options:
          model: mistral-small-latest
          temperature: 0.7

    # -------------------------------------------------------
    # OPTION 4: Ollama (Local, No API key required)
    # -------------------------------------------------------
    ollama:
      base-url: http://localhost:11434
      chat:
        options:
          model: llama3.2
          temperature: 0.7
```

---

## Switching Between Providers

To switch between providers in Spring AI, use Spring profiles.

### application.yaml:
```yaml
spring:
  profiles:
    active: ollama   # Change to: claude, gemini, mistral, or ollama
```

### application-claude.yaml:
```yaml
spring:
  ai:
    anthropic:
      api-key: ${ANTHROPIC_API_KEY}
      chat:
        options:
          model: claude-haiku-4-5
```

### application-gemini.yaml:
```yaml
spring:
  ai:
    vertex:
      ai:
        gemini:
          api-key: ${GEMINI_API_KEY}
          chat:
            options:
              model: gemini-1.5-flash
```

### application-ollama.yaml:
```yaml
spring:
  ai:
    ollama:
      base-url: http://localhost:11434
      chat:
        options:
          model: llama3.2
```

---

## Free Tier Comparison

| Provider | Free Credits | Card Required | Rate Limit | Best Free Model |
|----------|-------------|---------------|------------|-----------------|
| **Ollama** | ♾️ Unlimited | ❌ No | None | llama3.2, phi4-mini |
| **Google Gemini** | ✅ Generous free tier | ❌ No | 15 req/min | gemini-1.5-flash |
| **Anthropic Claude** | ~$5 credits | ❌ No | Limited | claude-haiku-4-5 |
| **Mistral AI** | ~€5 credits | ❌ No | Limited | mistral-small |
| **OpenAI** | ~$5 credits | ❌ No | 3 req/min | gpt-4o-mini |

> 🏆 **For students with no budget:** Use **Ollama** — it's 100% free, runs locally, and is perfect
> for learning Spring AI without any API costs.

---

## Security Best Practices

### ✅ Always use environment variables — never hardcode keys

```yaml
# ✅ CORRECT — uses environment variable
spring:
  ai:
    anthropic:
      api-key: ${ANTHROPIC_API_KEY}

# ❌ WRONG — key is exposed in code
spring:
  ai:
    anthropic:
      api-key: sk-ant-api03-abc123...
```

### ✅ Add sensitive files to .gitignore

```gitignore
# .gitignore
.env
*.env
application-local.yaml
application-local.yml
application-secrets.yaml
```

### ✅ Set environment variables per OS

| OS | Command |
|----|---------|
| Windows CMD | `setx VARIABLE_NAME "value"` |
| Windows PowerShell | `$env:VARIABLE_NAME = "value"` |
| macOS / Linux | `export VARIABLE_NAME="value"` |

### ✅ Set usage limits on your dashboard

Always set a **monthly spending cap** on each provider's dashboard to avoid accidental charges:
- Anthropic: https://console.anthropic.com/settings/billing
- Google AI: https://console.cloud.google.com/billing
- Mistral: https://console.mistral.ai/billing

---

## 🔗 Useful Links

| Provider | Console | API Keys | Docs |
|----------|---------|----------|------|
| Anthropic Claude | https://console.anthropic.com | https://console.anthropic.com/settings/keys | https://docs.anthropic.com |
| Google Gemini | https://aistudio.google.com | https://aistudio.google.com/app/apikey | https://ai.google.dev/docs |
| Mistral AI | https://console.mistral.ai | https://console.mistral.ai/api-keys | https://docs.mistral.ai |
| Ollama | https://ollama.com | N/A (local) | https://github.com/ollama/ollama |
| Spring AI | N/A | N/A | https://docs.spring.io/spring-ai/reference/ |

---

*Happy Learning! 🚀 — Built for Spring AI practice and student training*
