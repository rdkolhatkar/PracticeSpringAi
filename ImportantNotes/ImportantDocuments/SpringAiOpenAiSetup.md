# 🤖 Spring AI — OpenAI Integration Guide

A complete step-by-step guide for setting up **Spring AI** with **OpenAI** in a Spring Boot project.
Includes how to create a **free OpenAI API key** for training and practice purposes.

---

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [Step 1 — Create a Free OpenAI Account](#step-1--create-a-free-openai-account)
- [Step 2 — Generate Your OpenAI API Key](#step-2--generate-your-openai-api-key)
- [Step 3 — Spring Boot Project Setup](#step-3--spring-boot-project-setup)
- [Step 4 — Add Dependencies](#step-4--add-dependencies)
- [Step 5 — Configure application.yaml](#step-5--configure-applicationyaml)
- [Step 6 — Keeping Your API Key Secure](#step-6--keeping-your-api-key-secure)
- [Step 7 — Verify Your Setup](#step-7--verify-your-setup)
- [Free Tier Limits (OpenAI)](#free-tier-limits-openai)
- [Troubleshooting](#troubleshooting)

---

## Prerequisites

Before you begin, make sure you have the following installed:

| Tool | Version |
|------|---------|
| Java | 17 or higher |
| Spring Boot | 3.2.x or higher |
| Gradle or Maven | Latest stable |
| IDE | IntelliJ IDEA / VS Code / Eclipse |
| Internet Browser | For OpenAI platform access |

---

## Step 1 — Create a Free OpenAI Account

1. Open your browser and go to: **https://platform.openai.com/signup**

2. Click **"Sign up"**

3. Choose one of the following registration options:
   - ✅ Sign up with **Email and Password**
   - ✅ Sign up with **Google account**
   - ✅ Sign up with **Microsoft account**

4. If you chose Email and Password:
   - Enter your **email address**
   - Create a **strong password**
   - Click **"Continue"**
   - Check your inbox and click the **verification link** sent to your email

5. After verifying, you will be asked to enter your:
   - **First name** and **Last name**
   - **Date of birth**
   - Click **"Continue"**

6. You may be asked to verify your **phone number**:
   - Enter your country code and phone number
   - Click **"Send code"**
   - Enter the **6-digit OTP** received on your phone
   - Click **"Verify"**

7. You now have an OpenAI account. You will land on the **OpenAI Platform dashboard**.

> 💡 **Student Tip:** OpenAI provides **$5 free credits** for new accounts (valid for a limited time).
> This is enough for hundreds of API calls during practice and learning.

---

## Step 2 — Generate Your OpenAI API Key

1. Go to: **https://platform.openai.com/api-keys**
   *(Or from the dashboard: click your profile icon → "API keys")*

2. Click the **"+ Create new secret key"** button

3. A dialog box will appear:
   - Enter a **name** for your key, e.g., `spring-ai-practice`
   - Leave the **Project** as default (or select one if you have multiple)
   - Click **"Create secret key"**

4. Your API key will be displayed **only once** — it looks like this:
   ```
   sk-proj-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
   ```

5. ⚠️ **IMPORTANT:**
   - **Copy this key immediately** and store it in a safe place (e.g., Notepad, password manager)
   - Once you close this dialog, **you cannot view the key again**
   - If you lose it, you must **delete and create a new one**

6. Click **"Done"**

---

## Step 3 — Spring Boot Project Setup

### Using Spring Initializr (https://start.spring.io)

1. Go to **https://start.spring.io**
2. Fill in the project details:

| Field | Value |
|-------|-------|
| Project | Gradle - Groovy *(or Maven)* |
| Language | Java |
| Spring Boot | 3.2.x or higher |
| Group | com.yourname |
| Artifact | PracticeSpringAi |
| Packaging | Jar |
| Java | 17 |

3. Under **Dependencies**, add:
   - ✅ `Spring Web`
   - ✅ `OpenAI` *(under AI section)*

4. Click **"Generate"**, download and extract the ZIP, then open it in your IDE.

---

## Step 4 — Add Dependencies

### For Gradle (`build.gradle`)

```groovy
plugins {
    id 'org.springframework.boot' version '3.2.5'
    id 'io.spring.dependency-management' version '1.1.4'
    id 'java'
}

group = 'com.yourname'
version = '0.0.1-SNAPSHOT'

java {
    sourceCompatibility = '17'
}

repositories {
    mavenCentral()
    maven { url 'https://repo.spring.io/milestone' }
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.ai:spring-ai-openai-spring-boot-starter'

    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

dependencyManagement {
    imports {
        mavenBom "org.springframework.ai:spring-ai-bom:1.0.0-M1"
    }
}
```

### For Maven (`pom.xml`)

```xml
<properties>
    <java.version>17</java.version>
    <spring-ai.version>1.0.0-M1</spring-ai.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.ai</groupId>
        <artifactId>spring-ai-openai-spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>

<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.ai</groupId>
            <artifactId>spring-ai-bom</artifactId>
            <version>${spring-ai.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

<repositories>
    <repository>
        <id>spring-milestones</id>
        <name>Spring Milestones</name>
        <url>https://repo.spring.io/milestone</url>
    </repository>
</repositories>
```

---

## Step 5 — Configure application.yaml

Your `application.yaml` (located at `src/main/resources/application.yaml`) should look like this:

```yaml
spring:
  application:
    name: PracticeSpringAi
  ai:
    openai:
      api-key: ${OPENAI_API_KEY}        # Recommended: use environment variable
      chat:
        options:
          model: gpt-4o-mini            # Cheapest model, great for practice
          temperature: 0.7
      embedding:
        options:
          model: text-embedding-ada-002
```

> 🔑 Replace `${OPENAI_API_KEY}` with your actual key ONLY for quick local testing.
> See **Step 6** below for the secure approach.

### Available Models (cost-effective for students)

| Model | Best For | Cost |
|-------|----------|------|
| `gpt-4o-mini` | Chat, Q&A, practice | Very cheap |
| `gpt-3.5-turbo` | General chat | Cheap |
| `gpt-4o` | Advanced tasks | Higher cost |
| `text-embedding-ada-002` | Embeddings | Very cheap |

---

## Step 6 — Keeping Your API Key Secure

### ✅ Recommended: Use Environment Variables

**Never hardcode your API key directly in `application.yaml`.**

#### On Windows (Command Prompt):
```cmd
setx OPENAI_API_KEY "sk-proj-your-key-here"
```

#### On Windows (PowerShell):
```powershell
$env:OPENAI_API_KEY = "sk-proj-your-key-here"
```

#### On macOS / Linux:
```bash
export OPENAI_API_KEY="sk-proj-your-key-here"
```
Add this line to your `~/.bashrc` or `~/.zshrc` to make it permanent.

#### In IntelliJ IDEA:
1. Go to **Run → Edit Configurations**
2. Select your Spring Boot run configuration
3. Click **"Modify options" → "Environment variables"**
4. Add: `OPENAI_API_KEY=sk-proj-your-key-here`

Then in your `application.yaml`:
```yaml
spring:
  ai:
    openai:
      api-key: ${OPENAI_API_KEY}
```

### ✅ Alternative: Use a `.env` file (with dotenv library)

Create a file named `.env` in the project root:
```
OPENAI_API_KEY=sk-proj-your-key-here
```

**Add `.env` to your `.gitignore` immediately:**
```
# .gitignore
.env
application-local.yaml
```

---

## Step 7 — Verify Your Setup

Create a simple controller to test your Spring AI integration:

```java
package com.yourname.PracticeSpringAi.controller;

import org.springframework.ai.chat.client.ChatClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class AiController {

    private final ChatClient chatClient;

    public AiController(ChatClient.Builder builder) {
        this.chatClient = builder.build();
    }

    @GetMapping("/ask")
    public String ask(@RequestParam(defaultValue = "Tell me a fun fact") String message) {
        return chatClient.prompt()
                .user(message)
                .call()
                .content();
    }
}
```

Run the application and test:
```
GET http://localhost:8080/ask?message=What is Spring AI?
```

---

## Free Tier Limits (OpenAI)

| Resource | Free Tier |
|----------|-----------|
| Free Credits | ~$5 USD (new accounts) |
| Credit Expiry | Check your account dashboard |
| Rate Limits | 3 requests/min on free tier |
| Models Available | gpt-4o-mini, gpt-3.5-turbo, embeddings |

> 💡 **Student Tips:**
> - Use `gpt-4o-mini` — it's the cheapest and fastest model for practice
> - Always set a **usage limit** in your OpenAI dashboard to avoid surprise charges
> - Go to: **https://platform.openai.com/settings/organization/limits** to set a monthly cap

---

## Troubleshooting

| Error | Cause | Fix |
|-------|-------|-----|
| `401 Unauthorized` | Invalid or missing API key | Double-check the key in your config |
| `429 Too Many Requests` | Rate limit exceeded | Wait a moment and retry |
| `insufficient_quota` | Free credits exhausted | Add billing or create a new account |
| `model_not_found` | Wrong model name | Use `gpt-4o-mini` or `gpt-3.5-turbo` |
| `Could not resolve spring-ai` | Missing milestone repo | Add Spring Milestones repository to build file |

---

## 📁 Project Structure

```
PracticeSpringAi/
├── src/
│   ├── main/
│   │   ├── java/com/yourname/PracticeSpringAi/
│   │   │   ├── PracticeSpringAiApplication.java
│   │   │   └── controller/
│   │   │       └── AiController.java
│   │   └── resources/
│   │       └── application.yaml
│   └── test/
├── .gitignore
├── build.gradle
└── README.md
```

---

## 🔗 Useful Links

- OpenAI Platform: https://platform.openai.com
- OpenAI API Keys: https://platform.openai.com/api-keys
- OpenAI Usage Dashboard: https://platform.openai.com/usage
- Spring AI Docs: https://docs.spring.io/spring-ai/reference/
- Spring Initializr: https://start.spring.io

---

*Happy Learning! 🚀 — Built for Spring AI practice and student training*
