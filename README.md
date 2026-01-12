# 🔍 Open-Reasoning: A Tool-Augmented Deep Research Agent for Real-World Information Matching

## 🚀 Project Tagline
Empowering AI with human-like deep research capabilities, tool-augmented reasoning, and precise real-world information matching.

## 📖 Overview
`Open-Reasoning` is an ambitious **open-source AI research agent** designed to conduct deep, comprehensive investigations into real-world topics. Inspired by the sophisticated methodologies of **OpenAI Deep Research** and **Google Gemini Deep Research**, this agent harnesses the power of cutting-edge **Large Language Models (LLMs)**, dynamic **tool augmentation**, and advanced **iterative reasoning** to efficiently gather, synthesize, and present information.

This project goes beyond traditional search by actively planning research, executing searches with diverse strategies, processing findings, building and refining a **knowledge base**, and continually learning through multiple iterations. The ultimate goal is to generate detailed research reports, identify critical insights, and pinpoint follow-up questions, mirroring the depth of human expert research.

## ✨ Key Features

-   **🔄 Iterative Reasoning Strategy**: Employs a multi-step research loop to assess current knowledge, decide optimal next search actions, execute searches, and continuously update a persistent knowledge state. This cycle continues until a confident answer is derived or maximum iterations are met.
-   **🛠️ Dynamic Tool Augmentation**: Seamlessly integrates with external tools and APIs. It converts OpenAPI specifications into Gemini-compatible schemas for robust function calling, enabling the agent to interact with the digital world and extract specific information.
-   **🔍 Multiple Search Strategies**: Supports diverse approaches for **real-world information matching**, including:
    -   `SourceBasedSearchStrategy`: Focuses on extracting precise information directly from identified sources.
    -   `iterdrag`, `standard`, `rapid`, `parallel`: Configurable strategies for varying research depths, speeds, and concurrency.
-   **🧠 Knowledge State Management**: Maintains a comprehensive `KnowledgeState` throughout the research journey, meticulously tracking key facts, candidate answers (with confidence scores), identified uncertainties, and the complete search history.
-   **📝 Progressive Synthesis & Reporting**: Generates interim findings and culminates in a final, synthesized research report. This includes supporting facts, nuanced insights, and clearly identified follow-up research questions.
-   **🌐 Model Agnostic Design**: Engineered to be flexible, supporting various **LLM** providers, with specific integrations already in place for **Google Gemini** and **OpenAI** models.
-   **⚙️ Hyperparameter Optimization (via Optuna)**: Incorporates an `OptunaOptimizer` to systematically fine-tune critical research parameters (e.g., iterations, questions per iteration, search strategy), maximizing both research quality and operational efficiency.
-   **📊 Detailed Logging & Callbacks**: Provides extensive logging and progress callbacks, enabling real-time monitoring, debugging, and seamless integration into interactive user interfaces.

## 💡 How It Works (High-Level) 

At its core, `Open-Reasoning` operates on an `IterativeReasoningStrategy`, executing the following cycle when presented with a query:

1.  **Initial Analysis**: The agent first processes the original query, sets up the initial **knowledge base**, and prepares for the research.
2.  **Decision Making (`_decide_next_search`)**: Leveraging its current knowledge, identified uncertainties, and candidate answers, the **LLM** intelligently determines the most effective next search query or action. This decision factors in research progress, confidence levels, and the ongoing need for new information.
3.  **Execution (`_execute_search`)**: The selected search query is executed using an underlying search mechanism (e.g., web search, database query), potentially employing different strategies to retrieve highly relevant **real-world information**.
4.  **Knowledge Update (`_update_knowledge`)**: The results from each search are thoroughly analyzed by the **LLM** to extract new key facts, identify emergent uncertainties, propose refined candidate answers, and resolve existing unknowns. This process incrementally builds and strengthens the agent's **knowledge base**.
5.  **Answer Assessment (`_assess_answer`)**: The agent continuously evaluates the confidence in its candidate answers, considering the convergence of findings, the volume of supporting facts, and the reduction of uncertainties.
6.  **Iteration & Synthesis**: This cycle repeats until a predefined confidence threshold is achieved, maximum iterations are reached, or no further productive searches can be identified. Finally, a comprehensive and well-structured research report is synthesized from all accumulated knowledge.

## 📦 Installation

This project is primarily built with Python. To get started with your **AI research agent**:

```bash
git clone https://github.com/your-username/open-reasoning.git
cd open-reasoning
pip install -r requirements.txt
```
*(Note: `requirements.txt` is hypothetical; you'll need to create one based on actual dependencies like `google-generativeai`, `openai`, `optuna`, `torch`, `numpy`, etc.)*

## 🚀 Usage

Here’s a basic example demonstrating how to leverage the `Open-Reasoning` agent for **deep research**:

```python
import os
from your_project_path.strategies.iterative_reasoning import IterativeReasoningStrategy
from your_project_path.models.gemini_model import GeminiModel
from your_project_path.search.your_search_engine import YourSearchEngine # Replace with your actual search engine integration

# Initialize your LLM (e.g., Gemini)
# Ensure GEMINI_API_KEY is set in your environment variables for secure access
model = GeminiModel(model_name="gemini-1.5-flash")

# Initialize your search engine – crucial for real-world information matching
# This would typically be an abstraction over Google Search, Brave Search, etc.
search_engine = YourSearchEngine() 

# Define a challenging research query for the AI agent
query = "What are the latest breakthroughs in AI for drug discovery and what are their implications for future pharmaceutical development?"

# (Optional) Define a progress callback for real-time updates during the deep research process
def my_progress_callback(message, progress, data):
    print(f"[{progress}%] {message} - Data: {data}")

# Initialize the IterativeReasoningStrategy – the core of our AI research agent
research_agent = IterativeReasoningStrategy(
    model=model,
    search=search_engine,
    all_links_of_system=[], # Placeholder: To be populated by the agent during research
    max_iterations=10,
    confidence_threshold=0.9,
    search_iterations_per_round=1,
    questions_per_search=10,
)

# Set the progress callback for transparency
research_agent.set_progress_callback(my_progress_callback)

print(f"Starting deep research with the Open-Reasoning AI agent for query: '{query}'")
# Execute the deep research
analysis_results = research_agent.analyze_topic(query)

print("\n--- 📝 Research Report Summary ---")
print(analysis_results["formatted_findings"])

print("\n--- ✅ Final Answer from AI Agent ---")
print(analysis_results["current_knowledge"])

print("\n--- 📊 Knowledge State Overview ---")
print(f"Key Facts: {analysis_results['knowledge_state']['key_facts']}")
print(f"Uncertainties: {analysis_results['knowledge_state']['uncertainties']}")
print(f"Candidate Answers: {analysis_results['knowledge_state']['candidate_answers']}")
print(f"Final Confidence: {analysis_results['knowledge_state']['confidence']:.2%}")
```

## 🛠️ Configuration

Key configurable parameters for tailoring your **AI research agent**:

-   **LLM Model**: Specify `model_name` and `provider` (e.g., `GeminiModel`, `OpenAIProvider`) to choose your preferred **Large Language Model**.
-   **Search Engine**: Integrate your preferred search backend for optimal **real-world information matching**.
-   **Strategy Parameters**: Fine-grained control over the **iterative reasoning** process with `max_iterations`, `confidence_threshold`, `search_iterations_per_round`, and `questions_per_search`.
-   **Optimization**: Utilize `OptunaOptimizer` to systematically discover the best combination of these parameters for your specific **deep research** use cases, enhancing both quality and efficiency.

## 👋 Contributing

We wholeheartedly welcome contributions from the community to enhance this **open-source AI research agent**! Whether you're adding new search strategies, integrating more **LLM** providers, improving the **iterative reasoning** process, or enhancing report generation, your input is incredibly valuable. Please refer to our `CONTRIBUTING.md` (to be created) for detailed guidelines.

## ⚖️ License

This **AI research agent** project is licensed under the MIT License. See the `LICENSE` file for full details.

## 🙏 Acknowledgements

`Open-Reasoning` draws significant inspiration from the pioneering work in AI research and advanced reasoning by:

-   **OpenAI Deep Research**: For their groundbreaking advancements in autonomous agents and complex problem-solving.
-   **Google Gemini Deep Research**: For their innovative approaches to multimodal reasoning and robust tool integration.

We are building upon these foundational concepts, striving to provide an accessible, extensible, and powerful **open-source** platform for the next generation of **deep research AI agents**.
