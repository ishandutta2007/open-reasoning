# 🔍 Open-Reasoning: A Tool-Augmented Deep Research Agent

## Project Tagline
Empowering AI with human-like deep research capabilities through tool-augmented, iterative reasoning.

## Overview
`Open-Reasoning` is an ambitious open-source project aimed at building an advanced AI research agent capable of performing deep, comprehensive investigations into real-world topics. Inspired by the sophisticated research methodologies demonstrated by systems like OpenAI Deep Research and Google Gemini Deep Research, this agent leverages cutting-edge Large Language Models (LLMs) combined with dynamic tool augmentation and iterative reasoning to gather, synthesize, and present information.

Unlike traditional search methods, `Open-Reasoning` doesn't just retrieve documents; it actively plans research, executes searches using various strategies, processes findings, updates its knowledge base, and refines its understanding through multiple iterations, ultimately generating detailed reports and identifying follow-up questions.

## Features

-   **Iterative Reasoning Strategy**: Employs a multi-step research loop to assess current knowledge, decide on the next best search action, execute searches, and continuously update a persistent knowledge state until a confident answer is found or maximum iterations are reached.
-   **Dynamic Tool Augmentation**: Integrates seamlessly with various external tools and APIs, converting OpenAPI specifications into Gemini-compatible schemas for robust function calling and information extraction.
-   **Multiple Search Strategies**: Supports diverse search approaches, including:
    -   `SourceBasedSearchStrategy`: Focuses on extracting information directly from identified sources.
    -   `iterdrag`, `standard`, `rapid`, `parallel`: Configurable strategies for different research depths and speeds.
-   **Knowledge State Management**: Maintains a comprehensive `KnowledgeState` throughout the research process, tracking key facts, candidate answers with confidence scores, uncertainties, and search history.
-   **Progressive Synthesis**: Generates interim findings and a final, synthesized report, complete with supporting facts and identified follow-up questions.
-   **Model Agnostic Design**: Designed to work with various LLMs, including specific integrations for Google Gemini and OpenAI models.
-   **Hyperparameter Optimization (via Optuna)**: Includes an `OptunaOptimizer` to systematically fine-tune research parameters (e.g., iterations, questions per iteration, search strategy) to maximize research quality and efficiency.
-   **Detailed Logging & Callbacks**: Provides extensive logging and progress callbacks for real-time monitoring and integration into interactive interfaces.

## How It Works (High-Level)

The core of `Open-Reasoning` is its `IterativeReasoningStrategy`. When posed with a query, the agent performs the following cycle:

1.  **Initial Analysis**: The agent first processes the original query and initializes its knowledge state.
2.  **Decision Making (`_decide_next_search`)**: Based on its current knowledge, identified uncertainties, and candidate answers, the LLM determines the most effective next search query or action. It considers factors like progress, confidence, and the need for new information.
3.  **Execution (`_execute_search`)**: The chosen search query is executed using an underlying search mechanism (e.g., web search, database query), potentially employing different strategies to retrieve relevant information.
4.  **Knowledge Update (`_update_knowledge`)**: The results from the search are analyzed by the LLM to extract new key facts, identify new uncertainties, propose candidate answers, and resolve existing uncertainties, incrementally building a richer knowledge base.
5.  **Answer Assessment (`_assess_answer`)**: The agent continuously evaluates its confidence in its candidate answers, considering the spread of confidence among candidates, the number of supporting facts, and remaining uncertainties.
6.  **Iteration & Synthesis**: This cycle repeats until a predefined confidence threshold is met, maximum iterations are reached, or no further productive searches can be identified. Finally, a comprehensive report is synthesized based on all accumulated knowledge.

## Installation

This project is built with Python. To get started, clone the repository and install the necessary dependencies:

```bash
git clone https://github.com/your-username/open-reasoning.git
cd open-reasoning
pip install -r requirements.txt
```
*(Note: `requirements.txt` is hypothetical; you'll need to create one based on actual dependencies like `google-generativeai`, `openai`, `optuna`, `torch`, `numpy`, etc.)*

## Usage

A basic example of how to use the `Open-Reasoning` agent:

```python
import os
from your_project_path.strategies.iterative_reasoning import IterativeReasoningStrategy
from your_project_path.models.gemini_model import GeminiModel
from your_project_path.search.your_search_engine import YourSearchEngine # Replace with your actual search engine integration

# Initialize your LLM (e.g., Gemini)
# Ensure GEMINI_API_KEY is set in your environment variables
model = GeminiModel(model_name="gemini-1.5-flash")

# Initialize your search engine
# This would typically be an abstraction over Google Search, Brave Search, etc.
search_engine = YourSearchEngine() 

# Define a research query
query = "What are the latest breakthroughs in AI for drug discovery and what are their implications?"

# (Optional) Define a progress callback for real-time updates
def my_progress_callback(message, progress, data):
    print(f"[{progress}%] {message} - Data: {data}")

# Initialize the IterativeReasoningStrategy
research_agent = IterativeReasoningStrategy(
    model=model,
    search=search_engine,
    all_links_of_system=[], # To be populated during research
    max_iterations=10,
    confidence_threshold=0.9,
    search_iterations_per_round=1,
    questions_per_search=10,
)

# Set the progress callback
research_agent.set_progress_callback(my_progress_callback)

print(f"Starting deep research for query: '{query}'")
# Run the research
analysis_results = research_agent.analyze_topic(query)

print("\n--- Research Report ---")
print(analysis_results["formatted_findings"])

print("\n--- Final Answer ---")
print(analysis_results["current_knowledge"])

print("\n--- Knowledge State Summary ---")
print(f"Key Facts: {analysis_results['knowledge_state']['key_facts']}")
print(f"Uncertainties: {analysis_results['knowledge_state']['uncertainties']}")
print(f"Candidate Answers: {analysis_results['knowledge_state']['candidate_answers']}")
print(f"Final Confidence: {analysis_results['knowledge_state']['confidence']:.2%}")
```

## Configuration

Key configurable parameters include:

-   **LLM Model**: Specify `model_name` and `provider` (e.g., `GeminiModel`, `OpenAIProvider`).
-   **Search Engine**: Integrate your preferred search backend.
-   **Strategy Parameters**: `max_iterations`, `confidence_threshold`, `search_iterations_per_round`, `questions_per_search` for fine-grained control over the reasoning process.
-   **Optimization**: Use `OptunaOptimizer` to explore the best combination of these parameters for your specific use cases.

## Contributing

We welcome contributions from the community! Whether it's adding new search strategies, integrating more LLM providers, improving the reasoning process, or enhancing the report generation, your input is valuable. Please refer to our `CONTRIBUTING.md` (to be created) for detailed guidelines.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Acknowledgements

`Open-Reasoning` draws significant inspiration from the pioneering work in AI research and reasoning by:

-   **OpenAI Deep Research**: For their advancements in autonomous agents and complex problem-solving.
-   **Google Gemini Deep Research**: For their innovative approaches to multimodal reasoning and tool integration.

We are building upon the foundational concepts and aiming to provide an accessible, extensible, and powerful open-source alternative.