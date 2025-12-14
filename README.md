This repository contains a real-time evaluation pipeline to assess the reliability of LLM responses using retrieved context.
The system evaluates responses for relevance, hallucination risk, and latency, with a strong focus on scalability and cost efficiency.

# Local Setup Instructions
1. Environment Setup
python -m venv venv
source venv/bin/activate        # macOS / Linux
venv\Scripts\activate           # Windows
2. Install Dependencies
pip install -r requirements.txt
3. Run the Evaluation
python llm_evaluation_pipeline.py

# Evaluation Pipeline Architecture

- Conversation JSON + Context Vectors JSON
- Input Normalization Layer
- Embedding Generation Layer (User Query, AI Response, Context)
- Similarity Scoring
   - Relevance: User ↔ AI Response
   - Grounding: AI Response ↔ Context
 
- Decision & Metrics Layer
   - Relevance Score
   -  Hallucination Score
   -  latency Measurement
   -   PASS / FAIL Decision
   
# Design Rationale
Why embedding-based evaluation?
Embeddings provide fast, deterministic similarity scoring

No recursive dependency on LLMs for evaluation

Avoids the cost and latency of “LLM-as-a-judge” approaches

Why not rule-based or keyword matching?
Keyword rules fail on paraphrasing

Embeddings capture semantic meaning, not surface text

Why not use an LLM for factual checking?
High cost at scale

Unpredictable latency

Difficult to enforce consistent scoring

This design balances accuracy, speed, and operational simplicity.

# Scalability, Latency & Cost Optimization
The pipeline is designed for millions of daily evaluations:

No external API calls during evaluation

Lightweight embedding model with low inference cost

Stateless execution → horizontal scaling

Supports batching and embedding caching

Single-pass similarity computations

Evaluation latency typically under a few hundred milliseconds

This ensures minimal overhead in real-time production systems.

# Summary
This solution provides a practical, scalable, and cost-efficient approach to evaluating LLM responses in real time, ensuring relevance and factual grounding without introducing significant latency or operational complexity.

