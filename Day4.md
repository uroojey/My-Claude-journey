"""
Day 4 — Chain-of-Thought Reasoning
Career Roadmap Generator using CoT

This is the script that generated the 12-month AI career roadmap
featured in the Day 4 LinkedIn post.

The prompt uses structured CoT to force step-by-step reasoning
before producing the final roadmap output.
"""

import anthropic

client = anthropic.Anthropic()
MODEL = "claude-sonnet-4-6"


COT_SYSTEM_PROMPT = """
You are an Elite AI Career Strategist with 10+ years of experience 
helping students and professionals break into high-paying Data and AI roles.

You use Chain-of-Thought reasoning: you always think through problems 
step by step before producing your final output. Your reasoning is 
visible, structured, and logically sound.
""".strip()


def build_roadmap_prompt(situation: str, skills: list, goal: str, timeline: str) -> str:
    skills_str = ", ".join(skills)
    return f"""
I need a personalised AI career roadmap. Here is my situation:

- Current situation: {situation}
- Skills I have: {skills_str}
- Target goal: {goal}
- Timeline: {timeline}

Before creating the roadmap, think step by step through the following:

STEP 1 — ANALYSE CURRENT POSITION
What is strong about this person's current profile? 
What is the overall starting level?

STEP 2 — IDENTIFY SKILL GAPS
What skills are missing for the target role?
Rank them by priority: Critical / Important / Nice-to-have.

STEP 3 — FIND THE FASTEST PATH
Given the timeline and existing skills, what is the most efficient 
route to the goal? What should be learned first vs. later?

STEP 4 — LEARNING PRIORITIES
List the top 5 skills to learn in priority order with recommended resources.

STEP 5 — PORTFOLIO PROJECTS
Suggest 5 portfolio projects that demonstrate the right skills to employers,
with estimated timelines for each.

STEP 6 — NETWORKING STRATEGY
What are the 3–5 highest-leverage networking actions for this person?

STEP 7 — MONTHLY MILESTONES
Create clear, measurable milestones for each phase of the {timeline} timeline.

STEP 8 — IMMEDIATE NEXT ACTIONS
What are the 4 most important things to do THIS WEEK to start?

---

After completing all 8 reasoning steps, produce a clean, structured 
FINAL ROADMAP that synthesises your thinking into an actionable plan.
""".strip()


def generate_roadmap(
    situation: str = "Student",
    skills: list = None,
    goal: str = "High-paying Data/AI job",
    timeline: str = "12 months"
) -> str:
    if skills is None:
        skills = ["Python", "SQL", "Power BI", "Data Analysis", "Math", "Statistics"]

    prompt = build_roadmap_prompt(situation, skills, goal, timeline)

    print("🧠 Generating roadmap using Chain-of-Thought reasoning...")
    print(f"   Situation: {situation}")
    print(f"   Skills: {', '.join(skills)}")
    print(f"   Goal: {goal}")
    print(f"   Timeline: {timeline}")
    print()

    message = client.messages.create(
        model=MODEL,
        max_tokens=4096,
        system=COT_SYSTEM_PROMPT,
        messages=[{"role": "user", "content": prompt}]
    )

    return message.content[0].text


def save_roadmap(content: str, filename: str = "roadmap_output.md") -> None:
    with open(filename, "w") as f:
        f.write("# AI Career Roadmap — Generated with Chain-of-Thought Reasoning\n\n")
        f.write(f"> Generated using Claude ({MODEL}) with structured CoT prompting\n\n")
        f.write("---\n\n")
        f.write(content)
    print(f"✅ Roadmap saved to: {filename}")

![Alternative description text](assets/dashboard-screenshot.png)

# ── Demo ──────────────────────────────────────────────────────────────────────

if __name__ == "__main__":

    # Configuration — edit these to personalise
    config = {
        "situation": "Student",
        "skills": ["Python", "SQL", "Power BI", "Data Analysis", "Math", "Statistics"],
        "goal": "High-paying Data/AI job",
        "timeline": "12 months"
    }

    roadmap = generate_roadmap(**config)

    print("=" * 60)
    print(roadmap)
    print("=" * 60)

    # Optionally save to file
    save_roadmap(roadmap, "my_ai_roadmap.md")
