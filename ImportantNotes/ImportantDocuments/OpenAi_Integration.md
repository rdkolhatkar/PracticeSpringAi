# 🤖 PracticeSpringAI – OpenAI Integration Guide

This guide will help you:

* Create an OpenAI API key (free/credits-based)
* Configure it in your Spring Boot application
* Run your Spring AI project successfully

---

## 🚀 1. Create OpenAI API Key (Step-by-Step)

### Step 1: Create an OpenAI Account

1. Go to: https://platform.openai.com/signup
2. Sign up using:

    * Email OR
    * Google / Microsoft account

---

### Step 2: Verify Your Account

* Verify your email
* Add phone number (required for API usage)

---

### Step 3: Access API Keys Page

1. Go to: https://platform.openai.com/api-keys
2. Click on **"Create new secret key"**

---

### Step 4: Generate API Key

* Give it a name (e.g., `spring-ai-key`)
* Click **Create**
* Copy the key immediately ⚠️ (you won’t see it again)

Example:

```
sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

---

## 💡 Free Usage for Students

* OpenAI usually provides **free credits (trial-based)** for new users (subject to change)
* After credits are exhausted, usage becomes **paid**
* You can:

    * Set **usage limits** in billing
    * Monitor usage here: https://platform.openai.com/usage

---

## 🔐 2. Add API Key in Spring Boot

### ❌ NOT Recommended (Hardcoding)

```yaml
spring:
  ai:
    openai:
      api-key: sk-xxxxxxxxxxxxxxxx
```

---

### ✅ Recommended (Secure Approach using Environment Variable)

#### Step 1: Set Environment Variable

### Windows (PowerShell)

```powershell
setx OPENAI_API_KEY "sk-xxxxxxxxxxxxxxxx"
```

### Mac/Linux

```bash
export OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxx
```

---

#### Step 2: Use in `application.yaml`

```yaml
spring:
  application:
    name: PracticeSpringAi

  ai:
    openai:
      api-key: ${OPENAI_API_KEY}
```

---

## ⚙️ 3. Add Required Dependencies (Gradle)

```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.ai:spring-ai-openai-spring-boot-starter'
}
```

---

## 🧠 4. Example Usage (Service Layer)

```java
import org.springframework.ai.chat.client.ChatClient;
import org.springframework.stereotype.Service;

@Service
public class AiService {

    private final ChatClient chatClient;

    public AiService(ChatClient.Builder builder) {
        this.chatClient = builder.build();
    }

    public String askAi(String prompt) {
        return chatClient.prompt(prompt).call().content();
    }
}
```

---

## 🌐 5. Controller Example

```java
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/ai")
public class AiController {

    private final AiService aiService;

    public AiController(AiService aiService) {
        this.aiService = aiService;
    }

    @GetMapping("/ask")
    public String ask(@RequestParam String question) {
        return aiService.askAi(question);
    }
}
```

---

## ▶️ 6. Run the Application

```bash
./gradlew bootRun
```

Test API:

```
http://localhost:8080/ai/ask?question=What is Spring AI?
```

---

## ⚠️ Best Practices

* ✅ Never commit API keys to GitHub
* ✅ Use `.env` or environment variables
* ✅ Add to `.gitignore`:

```
.env
```

* ✅ Set billing limits to avoid unexpected charges

---

## 📌 Troubleshooting

| Issue            | Solution              |
| ---------------- | --------------------- |
| Invalid API Key  | Regenerate key        |
| 401 Unauthorized | Check env variable    |
| API not working  | Verify dependencies   |
| No response      | Check internet / logs |

---

## 🎯 Summary

* Create API key from OpenAI platform
* Store it securely using environment variables
* Configure in `application.yaml`
* Use Spring AI to interact with OpenAI models

---

Happy Coding 🚀
