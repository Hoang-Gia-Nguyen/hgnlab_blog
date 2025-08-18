+++
title = "Running Large Language Models (LLMs) Locally: Private AI on Your Own Hardware"
date = 2025-08-02
draft = false
description = "Discover how to run Large Language Models (LLMs) locally on your own PC, ensuring private AI and full control over your data. Explore tools like Ollama and LM Studio for self-hosted AI."

[taxonomies]
categories = ["Technology", "Self-hosting", "AI"]
tags = ["local-llm", "homelab", "private-ai", "self-hosted-ai", "ollama", "lm-studio", "ai-on-personal-hardware", "offline-ai"]

[extra]
cover.image = "images/local-llm-cover.png"
cover.alt = "Running Large Language Models (LLMs) Locally Cover"
+++

The world of Artificial Intelligence is rapidly evolving, and Large Language Models (LLMs) like GPT-3 and its open-source brethren are at the forefront of this revolution. For a long time, accessing and utilizing these powerful tools meant relying on cloud-based services, sending your data to remote servers. However, the landscape is shifting. Today, it's increasingly feasible – and incredibly empowering – to run **LLMs locally** right on your own hardware. Join me on this journey as I explore the exciting possibilities of **local AI**, powered by my trusty Mac Mini M4.

# Why Run LLMs on Your Own Hardware? The Benefits of Private AI

You might be wondering, why go through the trouble of running these complex models locally when there are readily available cloud APIs? For me, the answer boils down to several key factors, highlighting the advantages of **private AI**:

- **Privacy**: This is paramount. When you process data with a **local LLM**, your information stays on your machine. Sensitive data, personal conversations, or proprietary content never leave the security of your home network. This level of control offers significant peace of mind. [Sam Altman warns there’s no legal confidentiality when using ChatGPT as a therapist](https://techcrunch.com/2025/07/25/sam-altman-warns-theres-no-legal-confidentiality-when-using-chatgpt-as-a-therapist/)
- **Cost Savings (Long Term)**: While there might be an initial investment in hardware, running **LLMs locally** can be more cost-effective in the long run, especially if you plan on frequent use. You avoid the recurring fees associated with cloud API calls, which can quickly add up.
- **Latency and Reliability**: **Local processing** eliminates the network dependency. Responses are often faster and more reliable as you're not subject to internet connectivity issues or the latency inherent in sending requests to distant servers.
- **Customization and Control**: Running models locally gives you greater control over the specific model you use, its parameters, and how it's integrated into your workflows. You're not limited by the offerings of a particular cloud provider.
- **Learning and Experimentation**: For someone like me who enjoys tinkering and understanding the inner workings of technology, running **LLMs locally** provides an invaluable learning experience. It allows for deeper exploration and experimentation with different models and configurations.

# Choosing the Right Open-Source LLM for Home Use

The **open-source LLM** community has been incredibly active in developing and releasing powerful LLMs that can be run on consumer-grade hardware. What you should be consider when selecting a model is:
- **Number of parameters**: In model name you can see '1.5B', '7B', '14B', '70B',... This is the number of parameters in the model. Larger language models with more parameters tend to achieve better performance, but they require significantly more computational resources to run. 8B (8 billion parameter) model is a popular size that strikes a great balance between performance and resource consumption for most daily tasks of normal users.
- **Quantization**: In model name you can sometime see '4bit', '8bit'. This is quantization. In simple terms, quantization reduces the precision of the numbers used to represent the model's parameters (e.g., from 32-bit to 4-bit numbers). This drastically shrinks the model's file size and memory footprint, allowing it to run efficiently on devices with limited resources, like laptops and mini PCs, with minimal loss in performance.

![Parameter list example](/images/local-llm-parameterlist.png)

For this project, I chose the ***deepseek/deepseek-r1-0528-qwen3-8b*** model. This is a distilled version of the larger **DeepSeek-R1-0528** model. It’s known for its strong reasoning and inference capabilities, making it a great choice for a variety of tasks without requiring the massive resources of its full-size counterpart. As I am using a Mac PC, I will select the MLX model format from **LM Studio** to utilize performance of Apple Silicon. 

# Hardware Requirements: Is Your Personal Hardware Ready for AI?

The reality is that a lot of today's consumer hardware, including modern laptops and small-form-factor desktops, is more than capable of running a "small" **LLM locally**. These smaller models, typically in the 7 billion parameter range, are powerful enough for tasks like summarizing documents, brainstorming ideas, and writing code snippets. They offer a great balance of performance and accessibility, and thanks to optimization techniques like quantization, they can run on surprisingly modest hardware. This makes **AI on personal hardware** highly accessible.

I'm writing this on my Mac Mini M4, the base version with 16GB of unified memory, and it's a perfect example. While unified memory on Apple Silicon is different from the dedicated VRAM on a traditional GPU, it serves the same purpose for running models locally. My Mac Mini can comfortably handle a 8B parameter model. I can get fast, **local AI** assistance for my daily workflow without needing to invest in a high-end, dedicated server.

# Getting Started with Ollama and LM Studio for Self-Hosted AI

For most users, getting started with **local LLMs** has become remarkably straightforward thanks to tools like **Ollama** and **LM Studio**.

[***Ollama***](https://ollama.com/): This is a fantastic open-source project that makes it incredibly easy to download, run, and manage **LLMs on macOS** (and Linux). With a few simple commands in the terminal, you can pull down pre-built models and start interacting with them. For my chosen model, a command like `ollama run deepseek-r1:8b` downloads and runs the model.

[***LM Studio***](https://lmstudio.ai/): This MAC only app provides a user-friendly graphical interface for discovering, downloading, and running **LLMs**. It's particularly helpful for those who prefer a visual approach over the command line. **LM Studio** also offers features like a built-in chat interface and the ability to create local inference servers. The **LM Studio** offers various model optimized for Apple Silicon.

I've found both **Ollama** and **LM Studio** to be excellent choices. **Ollama**'s simplicity is great for quick experiments, while **LM Studio** offers a more comprehensive environment for exploring different models and their capabilities. The native support for Apple Silicon in both these tools ensures optimal performance on the M4 chip.

# Local AI Example: Summarizing My Transaction List and Giving Insight with Your Private AI

To put my **local LLM** setup to practical use, I decided on a simple yet potentially insightful project: summarizing my personal transaction list. I exported a CSV of my recent bank transactions (without the need to anonymize sensitive information) and wanted to see if the DeepSeek model could identify spending patterns and provide a high-level summary.

Using **LM Studio**, I loaded the deepseek/deepseek-r1-8b model and crafted a prompt that included my transaction data and clear instructions:

"Please analyze the following list of transactions and provide a summary of my spending habits. Identify the largest categories of expenses, any notable trends, and general insights into where my money is going."

![Prompt example](/images/local-llm-example.png)

The results were surprisingly insightful. But to protect my privacy I will share only the final part of the response:

```
**General Insights:**

*   Your spending is concentrated in everyday needs like food and transportation, alongside discretionary categories (Entertainment) and utility/baby expenses.
*   There's a clear preference for takeaway/delivered food items rather than cooking at home, based on transaction descriptions.
*   Transportation costs are significant but consist mainly of occasional Grab/Taxi use rather than daily commute expenses.
*   Baby-related purchases are frequent and appear to be a significant ongoing expense.
```

# Beyond the Chat UI: APIs for Automation with Your Self-Hosted AI
A crucial advantage of using applications like **Ollama** and **LM Studio** is their ability to act as local servers for your **AI models**. This means you're not limited to their user interfaces for interaction. Both tools expose a local API, which you can access from your own code.

For example, they offer Python libraries that allow you to programmatically send prompts to your **local LLMs** and receive responses. This opens up a world of possibilities for automation. You could build a Python script to summarize a daily log file, automatically generate email drafts based on meeting notes, or even create a custom chatbot that integrates with your existing tools. By leveraging these APIs, you're transforming your **self-hosted AI** setup into a powerful engine for improving your personal productivity and automating tasks.

# The Journey Continues: Exploring AI on Personal Hardware

Running **LLMs locally** is just the beginning of my exploration into the world of **self-hosted AI**. As models become more efficient and tools integration continue to improve, I'm excited to discover new ways to leverage this technology for personal productivity, automation, and learning, all while maintaining control over my data. Stay tuned for more updates on my **self-hosting** and **AI on personal hardware** adventures!


