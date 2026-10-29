---
layout: default
title: Home
---


<div class="hero" markdown="1">
<div class="hero-text" markdown="1">

# Robot autonomy on real hardware

Senior at WPI, double-majoring in **Robotics Engineering** and **Computer Science** (two B.S. degrees, May 2027). I build robot autonomy stacks — **perception, planning, and control** on physical robots — and the software tooling around them, from ROS 2 services to LLM agent pipelines.

**Currently:** Research Assistant at WPI's [ELPIS Lab](https://github.com/elpis-lab) (UR10 manipulation) and completing a Saint-Gobain–sponsored MQP automating Mayer-rod coating on a UR10.

</div>
<figure class="hero-media">
<a href="/projects#bomberman"><img src="/assets/bomberman_variant5.gif" alt="Approximate Q-learning agent escaping a Bomberman level with two monsters"></a>
<figcaption>Q-learning agent bombing through walls past an aggressive monster — 94–100% win rate across five variants. <a href="/projects#bomberman">Project →</a></figcaption>
</figure>
</div>

## Highlights
- **Runtime placement selection for a UR10** (ELPIS Lab): RealSense depth → HDBSCAN scene clustering → collision-free (x, y, z) placements, served over ROS 2 and used in the lab's ManipulationNet pick-and-place system.
- **Autonomous frontier exploration on a TurtleBot3** (ROS 2 Jazzy): frontier selection, risk-aware A\*, pure-pursuit tracking, and AMCL relocalization — our own planner and controller, no Nav2 stack — on physical hardware.
- **Approximate Q-learning Bomberman agent** (AI for Robotics): 94–100% win rate across five monster variants on 200 held-out seeds, with an exact one-tick simulator and a learned linear Q-function.
- **Agentic LLM assistant for enterprise logistics** (Argano / Netlogistik): migrated a single-shot chatbot to a multi-step tool-use agent with RAG on Azure AI Search and an LLM-as-a-Judge eval pipeline.

## Experience
- **Software Engineering Intern — Argano, Netlogistik SaaS team** (May–Aug 2026, remote). MCP, Node.js, SQL, Snowflake, Azure AI Search.
- **Research Assistant — ELPIS Lab, WPI** (Sep 2024–present). ROS 2, Python, RealSense, HDBSCAN, UR10.
- **Apprentice — Horizon Surgical Systems** (Oct 2025, Santa Monica, CA). Reproduced reliability flaws from cataract-extraction trials on the Polaris™ robotic system and helped test trajectory regressions across automated surgical steps.

**Quick links:**
- [Projects](/projects)
- [Resume](/resume) · [Download PDF](/Jeronimo_Ruiz_Fernandez_Resume.pdf)
- [GitHub](https://github.com/jeroruizfe) · [LinkedIn](https://www.linkedin.com/in/jeronimo-ruiz-fernandez)

*Email:* jeronimoruizfernandez@gmail.com
