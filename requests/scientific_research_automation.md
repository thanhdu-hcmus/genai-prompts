I need to design and build a multi-agent + RAG pipeline system to automate the scientific research workflow.

Input: a keyword (e.g., "RIS beamforming optimization")
Output: literature review, research gap analysis, idea proposals, and code suggestions.

The system should support the following steps:
- Search for all relevant research papers based on the input keyword, with configurable time ranges (e.g., from year X to year Y), and store them in a structured and well-organized manner for future management.
- Read, understand, and analyze all collected papers to grasp the core concepts and current state of the research topic.
- Generate potential research directions, evaluate their feasibility, estimate workload, prioritize them, and suggest suitable journals or conferences for - submission upon completion.
- Search for all related code resources available online, store them, and generate simulation code based on the selected proposal (chosen by the user).
- Draft a research manuscript based on the proposed idea, simulation results, and theoretical insights from existing literature.
- Provide a centralized configuration management system for all components.

Cost optimization is a priority:
- Use deterministic or algorithmic approaches wherever possible instead of AI to reduce costs.
- Prioritize high-quality, reliable open-source and free tools.
- Prefer local AI agents by default, with the ability to switch to remote AI agents (e.g., Claude, Gemini) only when necessary.