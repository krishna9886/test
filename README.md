# TrueBoard (Team AutoVault Intelligence) 🚀

TrueBoard is a 7-Layer Behavior-Aware Contextual Ranking Engine built for the India Runs Data & AI Challenge.

## Links
- **Interactive Sandbox:** [https://huggingface.co/spaces/vatsal-ag/trueboard](https://huggingface.co/spaces/vatsal-ag/trueboard)
- **Landing Page:** [https://vatsal-ag.github.io/Redrob-Test/](https://vatsal-ag.github.io/Redrob-Test/)

## 🧠 Core Features & Architecture Explained

Our algorithm does not blindly match keywords. It treats candidate profiles as chronological timelines and uses a 7-layer pipeline to score talent based on authenticity, behavior, and career structure.

Here is a breakdown of every feature our engine evaluates, along with examples:

### 1. Title Semantic Gate
**What it does:** Prevents candidates from applying to roles they are vastly underqualified (or overqualified) for by applying mathematical penalties to semantic distance if their job title history misaligns with the target JD.
* **Example:** If a candidate with a history of "Junior HR Assistant" applies for a "VP of Human Resources" JD, they receive a massive penalty multiplier (e.g., `0.20x`) ensuring they sink to the bottom, regardless of how many keywords they copied into their resume.

### 2. Skill Authenticity Verifier
**What it does:** Fights keyword stuffing. Instead of just looking at a standalone "skills" array, the engine scans the candidate's actual narrative work history to see if the claimed skills were used in context.
* **Example:** If a candidate lists "Machine Learning" in their skills array, but the text of their past 3 jobs only talks about "Data Entry", their Skill Authenticity Score drops to `0%`, killing their rank.

### 3. Behavioral Trust Multiplier
**What it does:** Uses live platform signals from the Redrob network to dynamically boost candidates who are highly likely to accept an interview.
* **Example:** A candidate whose `last_active_date` was 3 days ago, has an `open_to_work` flag, and an 85% `recruiter_response_rate` gets a huge `1.2x` boost over a candidate who hasn't logged in for 6 months.

### 4. Proctored Assessment Bonus
**What it does:** Leverages Redrob's verified skill assessment scores to boost candidates who have proven their abilities over those who just self-report.
* **Example:** If the JD asks for "React", and a candidate scored a 92/100 on the Redrob React Assessment, they receive up to a `1.45x` multiplier.

### 5. Missing Skills & Team Complementarity (Puzzle Piece)
**What it does:** Evaluates what required skills a candidate has that the *existing team currently lacks*.
* **Example:** If you input that your current team knows "Python" and "SQL", but the JD requires "GraphQL", the engine applies a specific "Complementarity Boost" to candidates who bring exactly "GraphQL" to fill the team's blind spot.

### 6. Career Personas & Startup DNA
**What it does:** Analyzes the chronological structure of their career jumps to assign them a psychological/career persona.
* **Example:** A candidate who has held 4 roles in 3 years with under 5 years of total experience is tagged as a **"Startup Scrapper"**. A candidate who has stayed at the same company for 6+ years is tagged as a **"Loyalist"**.

### 7. Title Inflation Detector
**What it does:** Checks if high-level executive titles actually make sense given their total years of experience.
* **Example:** A candidate holding the title "Chief Technology Officer" but who only has 3 total years of professional experience is flagged as `is_title_inflated`, preventing them from being falsely matched to enterprise-level CTO roles.

### 8. Silent Carrier (Glue Work)
**What it does:** Identifies Individual Contributors (non-managers) who do the heavy lifting for the team in terms of culture, mentoring, and organization.
* **Example:** A Software Engineer whose resume frequently uses words like "mentored", "agile", "cross-functional", and "facilitated" is flagged as a `Team Multiplier`, making them highly valuable.

### 9. AI Outreach Drafter
**What it does:** Automatically drafts a highly personalized, psychological email hook to the candidate based on their specific career persona.
* **Example:** For a "Loyalist" candidate, it drafts: *"I noticed you've built an impressive track record of loyalty at [Company]. I know you might not be actively looking, but I'd love to chat if you're open to a new challenge."*

---

## 🚀 How to Run the Full 100k Dataset Locally

Because GitHub blocks files over 100MB, the 487MB `candidates.jsonl` database and our 153MB pre-computed `embeddings_cache.npy` are not included in this repo.

To run the full 100,000 candidate ranking on your machine:
1. Place the official `[PUB] India_runs_data_and_ai_challenge` folder into the root of this repository.
2. Run `python app.py` (to start the web server) OR `python run_pipeline.py` (to generate the final CSV).
*(Our engine is smart enough to detect the missing cache and will automatically generate it in the background if the dataset is present!)*

## 📦 How to generate `team_submission.csv` (For Judges)

1. Ensure the `[PUB] India_runs_data_and_ai_challenge` folder is in the root directory.
2. Ensure you have installed the requirements: `pip install -r requirements.txt`
3. Run the offline pipeline:
```bash
python run_pipeline.py
```
This will automatically generate a perfectly valid, monotone-checked `team_submission.csv` in the root directory.

## ☁️ Cloud Sandbox Deployment
We have provided a `Dockerfile` and `requirements.txt` perfectly configured for a **HuggingFace Docker Space**. 
To deploy your own instance:
1. Create a new Docker Space on HuggingFace.
2. Upload the files in this repository.
3. The Space will automatically build and host the interactive web UI on port 7860!