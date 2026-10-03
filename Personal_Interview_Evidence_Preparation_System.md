# Personal Interview Evidence & Preparation System

## 1. Product Overview

### Working name
**Personal Interview Evidence & Preparation System**

### One-line definition

A local-first, Notion-style interview preparation system that turns a candidate's **real experience, resume, projects, training, notes, and a specific Job Description (JD)** into **evidence-backed, editable interview preparation**.

### The core idea

This is **not primarily an AI question generator**.

The system is built around:

> **My Experience → Evidence → Job Requirements → Questions → Honest Answer → My Answer → Interview Feedback → Better Preparation**

The most important requirement is that generated answers should naturally connect to what the candidate has actually done.

Example:

**Question:**  
What is AWS EC2?

**Weak generic answer:**
> EC2 is a web service provided by AWS that offers resizable compute capacity in the cloud.

**Desired answer:**
> EC2 is basically a virtual server in AWS. I used EC2 to host my Dockerized Next.js application, so in my project the EC2 instance was where the application and deployment setup were running.

The second answer is better for interview preparation because it is:
- technically understandable
- natural to speak
- connected to real experience
- easier to defend when the interviewer asks follow-up questions

---

# 2. The Problem

Freshers often have a strange interview problem:

They may have actually worked on projects and learned technologies, but they struggle to convert that experience into conversational interview answers.

At the same time, generic AI interview tools can generate answers that:
- sound robotic
- contain terminology the candidate cannot explain
- exaggerate experience
- connect technologies to projects that were never used
- encourage memorization instead of understanding
- hide the difference between real experience and theoretical knowledge

The system should solve a more specific problem:

> **Help a candidate prepare answers they can genuinely explain and defend using their own experience.**

---

# 3. Product Philosophy

The product follows these rules.

## Rule 1 — Evidence before generation

The system should first understand the candidate's available evidence.

Sources may include:
- Resume
- Projects
- Training
- Skills
- Certifications
- Personal notes
- Previous interview questions
- User-added experience

Only then should it generate personalized answers.

---

## Rule 2 — Never invent experience

If the candidate has not used something, the system must not pretend they have.

Example:

Candidate evidence:
- Kubernetes basics
- Docker
- AWS EC2

Question:
> Have you managed Kubernetes clusters in production?

The system should produce something like:

> I have basic knowledge of Kubernetes, but I haven't managed a production Kubernetes environment. Most of my hands-on experience so far has been with Docker, AWS EC2, Linux and CI/CD.

It should never generate:

> I managed Kubernetes clusters in production.

unless the candidate has explicitly added evidence supporting that claim.

---

## Rule 3 — Experience connections should feel natural

Do not force a project reference into every answer.

There are three useful experience levels.

### Direct experience

The candidate actually used the concept.

Example:

> I used Nginx as a reverse proxy in my AWS deployment project.

### Indirect / related experience

The candidate has worked with related concepts but not the exact item.

Example:

> I have worked with HTTP, HTTPS and reverse proxy concepts while deploying applications, although I haven't used this specific product directly.

### No direct experience

The candidate understands the concept but has not used it.

Example:

> I understand the basic concept, but I haven't used it directly in a project yet.

This distinction must be preserved throughout the system.

---

## Rule 4 — AI drafts; the candidate owns the final answer

The system should clearly separate:

**AI Draft**
from
**My Answer**

AI generates a useful starting point.

The candidate can:
- edit
- simplify
- correct
- remove claims
- add details
- rewrite it in their own speaking style

Interview Mode should preferably use **My Answer**.

---

## Rule 5 — Every important claim should be traceable

A generated answer should be explainable through its source evidence.

Example:

### Answer
> I used Nginx as a reverse proxy in my AWS deployment project...

### Evidence
- Project: Personal Website Infrastructure & CI/CD
- Technology: Nginx
- Technology: Docker
- Experience: Reverse proxy configuration
- Incident: Nginx/Docker port mismatch causing 502

The user should be able to see **why the system said that**.

---

# 4. Core Product Identity

The key product concept is:

## Experience → Evidence → Claim → Answer

A candidate's information should not be treated as a flat resume.

Instead, the system should transform it into structured evidence.

Example:

### Project
Personal Website Infrastructure & CI/CD

### Technologies
- AWS EC2
- Docker
- Docker Compose
- Nginx
- GitHub Actions

### Responsibilities / actions
- Deployed Dockerized Next.js application
- Configured Nginx reverse proxy
- Created CI/CD workflow
- Added health/readiness checks
- Implemented rollback
- Performed deployment smoke testing

### Problems encountered
- Nginx/Docker port mismatch
- 502 Bad Gateway
- EC2 out-of-memory during build
- Deployment script permission issue
- Application readiness/startup issue

This structured evidence becomes the foundation for personalized preparation.

---

# 5. Main User Workflow

The complete workflow is:

```text
Create Profile
      ↓
Add Resume / Experience
      ↓
Create or Import Job
      ↓
Add Job Description
      ↓
Analyze Resume + JD
      ↓
Build Job-Specific Evidence Map
      ↓
Generate Preparation
      ↓
Create AI Draft Answers
      ↓
Show Experience / Evidence
      ↓
User edits → My Answer
      ↓
Prep Mode
      ↓
Interview Mode
      ↓
Real Interview
      ↓
Record Actual Questions / Weak Answers
      ↓
Weak Areas / Revision Queue
      ↓
Improve My Answers
      ↓
Practice Again
```

The system should become more useful over time because the candidate keeps adding real interview experience.

---

# 6. Main Areas of the Application

## A. Personal Knowledge Base

This is the candidate's long-term information.

Possible sections:
- Profile
- Resume
- Projects
- Skills
- Training
- Certifications
- Experience
- Achievements
- Personal Notes
- Previous Interviews

This information should remain available across different job workspaces.

---

# 7. Job Workspace

Every target job gets its own workspace.

Example:

```text
Linux Administrator — Company A
Cloud Infrastructure Engineer Intern — Company B
DevOps Engineer — Company C
```

Each workspace contains information specific to that job.

### Job Workspace sections

#### Overview
- Job title
- Company
- Location
- Job Description
- Important requirements
- Interview focus
- Overall preparation status

#### Resume Match
Shows:
- strong matches
- partial matches
- missing skills
- experience differences
- potentially risky claims

#### Prep Mode
Study material for the specific role.

#### Interview Mode
Practice questions and answers.

#### Interview History
Questions actually asked in previous rounds.

#### Weak Areas
Topics that need additional preparation.

#### Notes
Free-form personal notes.

---

# 8. Resume + JD Analysis

The system accepts:

### Resume
The candidate's actual resume.

### JD
The job description for the target position.

The system extracts structured information.

### From the Resume

Examples:
- technologies
- projects
- responsibilities
- training
- certifications
- experience level
- concrete incidents/problems solved

### From the JD

Examples:
- required technologies
- responsibilities
- preferred skills
- role expectations
- likely technical areas
- soft-skill expectations

---

# 9. Resume-to-JD Matching

The system should not only produce a percentage score.

A more useful representation is:

### Strong match
The candidate has direct evidence.

### Related match
The candidate has adjacent experience.

### Knowledge only
The candidate has learned the concept but lacks project evidence.

### Gap
The candidate has no meaningful evidence yet.

Example:

| Requirement | Candidate Evidence | Status |
|---|---|---|
| Linux | Linux administration + projects | Direct |
| Docker | Multiple projects | Direct |
| AWS EC2 | Personal deployment project | Direct |
| Kubernetes | Basic knowledge | Knowledge only |
| Terraform | No evidence | Gap |

Avoid pretending that a numerical match score is inherently meaningful. The evidence behind the match is more important.

---

# 10. Question Generation

Questions can come from several sources.

## Resume-based questions

Questions about things the candidate actually listed.

Example:
> Why did you use Nginx in your deployment project?

## JD-based questions

Questions based on job requirements.

Example:
> How would you troubleshoot a Linux server with high CPU usage?

## Resume + JD questions

Questions where the job requirement intersects with the candidate's experience.

Example:
> The role requires AWS EC2 experience. Explain how you deployed an application on EC2.

## Follow-up questions

Questions that an interviewer may naturally ask after the previous answer.

Example:

```text
Why did you choose EC2?
    ↓
How did you secure the EC2 instance?
    ↓
How did you deploy the application?
    ↓
What happened when deployment failed?
    ↓
How did you implement rollback?
```

## User-created questions

The candidate can create any question manually.

---

# 11. Question Sources

Every question should have a source label.

Possible labels:

- Resume
- JD
- Resume + JD
- Previous Interview
- User Created
- AI Suggested

The label helps the candidate understand where the question came from.

---

# 12. Answer Generation

Every answer should follow a simple structure when appropriate.

Possible internal ingredients:

```text
Concept
+
Relevant Experience
+
Project / Situation
+
What I Actually Did
+
Result / Learning
+
Possible Follow-up
```

The system does not have to show all of these in every answer.

It chooses what is relevant.

---

# 13. Experience Anchoring

This is one of the defining features of the product.

For every generated answer, the system should try to find relevant experience.

### Example

Question:
> Why is Nginx used as a reverse proxy?

Answer:

> Nginx can sit in front of an application and forward incoming requests to the backend. I used it that way in my personal website deployment, where Nginx handled incoming HTTP traffic and forwarded it to the Dockerized application.

Evidence:

```text
Project: Personal Website Infrastructure & CI/CD
Technology: Nginx
Technology: Docker
Experience: Reverse proxy
```

This allows the candidate to study concepts through things they have actually worked on.

---

# 14. Evidence Panel

Every personalized answer can have an expandable evidence section.

Example:

```text
Answer
────────────────────────
I used Nginx as a reverse proxy...

Why this answer?
────────────────────────
✓ Project: Personal Website Infrastructure & CI/CD
✓ Nginx
✓ Docker
✓ Reverse proxy
✓ 502 troubleshooting incident

Experience level:
DIRECT
```

The candidate can then correct the evidence if the connection is wrong.

---

# 15. Claim Safety

A major feature is preventing unsupported claims.

Suppose an AI draft says:

> I automated Kubernetes deployments...

But the evidence only contains Docker and GitHub Actions.

The system should identify this.

Example:

```text
⚠ Unsupported claim

"Kubernetes deployments" is not supported
by your current experience data.

Available evidence:
- Docker
- GitHub Actions
- AWS EC2

[Edit Answer]
[Add Experience]
```

This feature should be taken seriously because the product's value depends on trust.

---

# 16. Answer Versions

Each question can contain:

### AI Draft
System-generated.

### My Answer
Candidate-approved version.

The candidate may update My Answer at any time.

Useful metadata:

- Created date
- Last edited
- Last reviewed
- Experience source
- Confidence/familiarity
- Needs revision

---

# 17. Prep Mode

Prep Mode is for studying.

It should be calm and structured rather than gamified.

A question can be opened like:

```text
▶ What is Docker?

▶ Why did you use Docker in your project?

▶ What is a Docker image?

▶ What is a Docker container?

▶ How do you troubleshoot a container that keeps restarting?
```

Clicking a question opens its answer and supporting evidence.

Useful study controls:
- Mark as reviewed
- Mark as weak
- Pin
- Add note
- Edit answer
- View evidence
- View follow-ups

---

# 18. Interview Mode

Interview Mode is for practice rather than studying.

The system asks a question without immediately showing the answer.

Example:

```text
INTERVIEW MODE

Question:
Why did you use Nginx in your project?

[Speak / Type Answer]

──────────────────────

[Reveal My Answer]
[Show Evidence]
[Mark Weak]
[Next]
```

The goal is to make the candidate retrieve the answer rather than simply reread it.

---

# 19. Different Interview Modes

Keep these simple.

### Quick Interview
Random questions from the current job workspace.

### Technical
Only technical questions.

### Resume Drill
Questions specifically about the candidate's resume.

### HR
Introduction, strengths, weaknesses, career questions, etc.

### Weak Areas
Only questions previously marked as weak.

### Previous Interview
Real questions collected from previous interviews.

---

# 20. Follow-Up Question Tree

This is important because interviews rarely stop after one answer.

Example:

```text
What is Docker?
       │
       ├── Why did you use Docker?
       │
       ├── Docker vs VM?
       │
       ├── What happens when a container crashes?
       │
       └── How did you use Docker in your project?
```

The candidate should be able to practice the chain.

---

# 21. Previous Interview Memory

After a real interview, the candidate can add:

```text
Company:
Role:
Date:

Round:
Technical

Question:
How do you troubleshoot high memory usage in Linux?

My answer:
...

How I performed:
Weak

Interviewer follow-up:
What commands would you use?

Notes:
I forgot free, top and ps.

Revision needed:
Linux memory troubleshooting
```

These questions become permanent knowledge in the system.

---

# 22. Weak Area Queue

The system should automatically collect things marked as weak.

Example:

```text
Needs Revision

1. Linux memory troubleshooting
2. EC2 security groups
3. Docker networking
4. DNS troubleshooting
5. Kubernetes basics
```

This creates a practical revision queue.

Instead of asking:

> What should I study today?

the user can open:

> **Things I previously struggled with**

---

# 23. General Handbook

Alongside job-specific workspaces, maintain a general static handbook.

Example:

```text
Linux
  ├── Processes
  ├── Permissions
  ├── Disk
  ├── Memory
  ├── Networking
  └── Troubleshooting

Networking
  ├── TCP/IP
  ├── DNS
  ├── HTTP/HTTPS
  ├── SSH
  └── Troubleshooting

AWS
  ├── EC2
  ├── S3
  ├── IAM
  ├── Security Groups
  └── CloudWatch

Docker
  ├── Images
  ├── Containers
  ├── Networking
  ├── Volumes
  └── Troubleshooting

CI/CD
  ├── Git
  ├── GitHub Actions
  ├── Deployment
  └── Rollback
```

This handbook is reusable across jobs.

---

# 24. General Handbook vs Job Workspace

These should not be separate unrelated systems.

A useful flow is:

```text
General Handbook
      ↓
Select useful questions
      ↓
Add to Job Workspace
      ↓
Personalize using Resume + JD + Experience
```

For example:

General handbook:

> What is a reverse proxy?

Job workspace:

> What is a reverse proxy, and how did you use one in your deployment project?

This gives the product both:
- reusable knowledge
- personalized preparation

---

# 25. User Editing Must Be First-Class

The candidate should never feel trapped by the AI.

Everything important should be editable.

The user should be able to:
- edit a question
- edit an answer
- delete a question
- add a question
- merge questions
- split questions
- edit evidence
- remove a wrong project connection
- add a missing experience item
- change the experience level
- rewrite the answer completely
- add personal notes

AI is an assistant, not the owner of the knowledge base.

---

# 26. Search

Search should be fast.

Example queries:

```text
nginx
502
ec2
docker networking
linux memory
github actions
```

It should search across:
- Questions
- Answers
- Projects
- Skills
- Notes
- Previous interviews
- JD
- Evidence

---

# 27. Organization

Useful lightweight organization:

- Categories
- Tags
- Favorites
- Pinned questions
- Weak questions
- Recently reviewed
- Last reviewed date

Avoid turning this into a complicated project-management application.

---

# 28. Local-First Principle

The product should be usable without depending on a paid online service.

Core personal information should remain under the user's control.

Desired principles:

- local storage first
- offline-friendly where practical
- no mandatory paid API
- no account required for the basic personal workflow
- exportable data
- importable data
- no vendor lock-in

AI can be optional.

The knowledge base should remain useful even when AI is turned off.

---

# 29. AI Is an Optional Layer

The application should still work as a normal knowledge base without AI.

Without AI, the candidate can:
- create questions
- create answers
- edit notes
- track progress
- search
- practice
- record interviews
- maintain evidence

With AI, the candidate can additionally:
- analyze resume
- analyze JD
- suggest questions
- map requirements to experience
- draft answers
- suggest follow-ups
- identify unsupported claims

This separation keeps the product useful and avoids making it just another AI wrapper.

---

# 30. Company-Specific Information

The system may eventually allow:

### Reported interview questions

Questions the candidate personally received or found from trustworthy sources.

### Predicted questions

Questions generated from:
- JD
- role
- resume
- known topics

These must remain clearly separated.

Do not present AI predictions as though they were actual questions asked by the company.

Example:

```text
REPORTED
"How did you troubleshoot the 502 error?"

SOURCE:
My interview — 02 Oct 2026
```

versus:

```text
AI SUGGESTED
"How would you troubleshoot a 502 error?"
```

---

# 31. Answer Style

Answers should generally be:

- simple
- conversational
- technically accurate
- honest
- short enough to speak
- expandable when more detail is needed

Avoid:

> textbook definition → five buzzwords → unnecessary detail

Prefer:

> concept → what I know → what I used → relevant example

The product should help the candidate **talk**, not memorize essays.

---

# 32. Resume Evidence Example

A project record should ideally contain more than the project title.

Example:

```text
Project:
Personal Website Infrastructure & CI/CD

Used:
AWS EC2
Docker
Docker Compose
Nginx
GitHub Actions

What I did:
- Deployed Dockerized Next.js application
- Configured Nginx reverse proxy
- Created CI/CD workflow
- Added health checks
- Added rollback
- Performed smoke testing

Problems I solved:
- Nginx/Docker port mismatch
- 502 error
- EC2 memory failure
- Deployment permission issue
- Application readiness issue
```

This produces much better interview preparation than a simple list of technologies.

---

# 33. Evidence Types

Evidence can eventually be categorized as:

```text
Technology
Project
Action
Responsibility
Problem
Solution
Result
Learning
Training
Certification
Interview Experience
User Note
```

This allows the system to reason about experience rather than simply matching keywords.

---

# 34. Example End-to-End Flow

Imagine the JD says:

> Linux administration, AWS, Docker, troubleshooting and basic Kubernetes knowledge.

The candidate's evidence contains:

```text
Linux
AWS EC2
Docker
Nginx
GitHub Actions
Linux troubleshooting
Kubernetes basics
```

The system produces:

### Strong direct areas
- Linux
- AWS EC2
- Docker
- troubleshooting

### Related areas
- Nginx
- CI/CD

### Knowledge-only
- Kubernetes

### Gap
Any JD technology for which the candidate has no evidence.

Then questions are generated.

### Direct experience

> Tell me about a problem you faced while deploying your application on EC2.

### Experience-linked technical

> How did Docker fit into your EC2 deployment?

### Troubleshooting

> You mentioned a 502 issue. How did you diagnose it?

### Knowledge-only

> What is Kubernetes?

Answer:

> I have basic knowledge of Kubernetes, but I haven't used it in a real project. My stronger hands-on experience is with Docker and AWS EC2.

This is exactly the type of honesty the product should encourage.

---

# 35. What Makes the Project Different

The project should not claim that individual features are completely new.

Resume analysis, AI question generation, mock interviews, local-first tools and experience-aware answers already exist in various forms.

The differentiating idea is the **workflow and trust model**:

> **Treat the candidate's real experience as structured evidence and use that evidence to generate interview preparation that the candidate can trace, edit and defend.**

The product is therefore closer to a:

**Personal Interview Evidence System**

than a generic:

**AI Interview Generator**

---

# 36. Features We Should NOT Prioritize

Do not turn the first version into a huge platform.

Avoid unnecessary scope such as:

- automatic job applications
- job-board aggregation
- resume marketplace
- recruiter marketplace
- salary analytics
- LinkedIn automation
- AI video avatar
- complex emotion detection
- huge company database
- automatic web scraping everywhere
- coding judge
- elaborate gamification
- social network
- subscription/paywall system

These distract from the core problem.

---

# 37. Suggested Initial Scope

The first build should prove one thing:

> Can the system take my real experience + a JD and help me produce honest, personalized, editable interview answers?

A sensible first build can contain:

### Foundation
- local workspace
- profile
- projects
- skills
- notes

### Job preparation
- create job
- paste/upload JD
- analyze JD
- connect JD requirements to evidence

### Questions
- generate questions
- source labels
- categories
- search

### Answers
- AI Draft
- My Answer
- Experience level
- Evidence panel
- unsupported-claim warning

### Practice
- Prep Mode
- Interview Mode
- mark weak

### Learning loop
- Previous Interview
- Weak Area Queue

That is enough to demonstrate the product concept.

---

# 38. Features for Later

After the core workflow works:

- better evidence graphs
- richer follow-up trees
- spaced review
- advanced interview analytics
- optional local LLM integration
- voice practice
- answer timing
- multiple resumes
- multiple versions of the same experience
- import/export formats
- browser extension
- mobile/PWA improvements

These should come after the central evidence workflow is proven.

---

# 39. Proposed Internal Data Model

A simple conceptual model:

```text
User
 ├── Profile
 ├── Experiences
 │    ├── Projects
 │    ├── Training
 │    ├── Skills
 │    └── Certifications
 │
 ├── Jobs
 │    ├── JD
 │    ├── Requirements
 │    ├── Questions
 │    │    ├── AI Draft
 │    │    ├── My Answer
 │    │    ├── Evidence Links
 │    │    ├── Experience Level
 │    │    └── Follow-ups
 │    │
 │    ├── Weak Areas
 │    ├── Interview History
 │    └── Notes
 │
 └── General Handbook
```

Important principle:

**Questions should reference evidence instead of copying evidence into themselves wherever practical.**

That makes updates easier.

---

# 40. The Core Entities

The most important entities are:

### Experience
Something the candidate actually knows or did.

### Evidence
A concrete piece of information supporting an experience claim.

### Job
A target role.

### Requirement
Something the job expects.

### Question
An interview question.

### Answer
The candidate's response to a question.

### Evidence Link
The connection between an answer and the candidate's evidence.

### Interview
A real or practice interview session.

### Feedback
What happened during the interview or practice session.

### Weak Area
A topic that requires additional preparation.

---

# 41. Product Success Criteria

The project is successful when a candidate can do this:

```text
I upload my resume.
        ↓
I add a JD.
        ↓
The system understands my real experience.
        ↓
It identifies what matches the job.
        ↓
It identifies what I don't actually know.
        ↓
It generates relevant questions.
        ↓
It builds answers around my evidence.
        ↓
It shows me why each claim was generated.
        ↓
I edit the answers into my own words.
        ↓
I practice them.
        ↓
I record what the interviewer actually asked.
        ↓
The system helps me prepare better for the next interview.
```

If these steps work smoothly, the project has achieved its core purpose.

---

# 42. The Product in One Sentence

When returning to this project later, remember:

> **This is a personal, local-first interview knowledge system where the candidate's real experience is the source of truth, and AI helps transform that experience into honest, job-specific, editable interview preparation.**

---

# 43. Build Philosophy

Before implementation, keep these principles fixed:

1. **Evidence first.**
2. **No fabricated experience.**
3. **Direct, indirect and no-experience states must be distinct.**
4. **AI Draft and My Answer are separate.**
5. **Every important personalized claim should be traceable.**
6. **The user can correct everything.**
7. **The knowledge base should remain useful without AI.**
8. **Job-specific preparation builds on a reusable personal knowledge base.**
9. **Actual interview experiences should improve future preparation.**
10. **Keep the product small and useful rather than turning it into a giant career platform.**

---

# 44. Before Coding

The product concept is considered sufficiently defined when these questions have clear answers:

### What is the source of truth?
The candidate's own evidence.

### What does AI do?
Analyze, connect, suggest, draft and detect unsupported claims.

### What does the user do?
Verify, edit, correct, personalize and practice.

### What happens when evidence is missing?
The system says so rather than inventing experience.

### What is the central object?
The candidate's experience/evidence, not the generated question.

### What is the long-term loop?
Prepare → interview → record → identify weakness → revise → practice.

---

# Final Product Definition

**Personal Interview Evidence & Preparation System**

A free, local-first, open-source, Notion-style personal interview workspace.

Its purpose is to help a candidate convert **real personal experience into trustworthy interview preparation**.

It combines:

**Personal Knowledge Base**  
+  
**Resume & JD Analysis**  
+  
**Experience Evidence Mapping**  
+  
**Evidence-Grounded Question Generation**  
+  
**Honest Answer Drafting**  
+  
**AI Draft / My Answer**  
+  
**Prep Mode**  
+  
**Interview Mode**  
+  
**Previous Interview Memory**  
+  
**Weak-Area Revision**

The defining workflow is:

> **Experience → Evidence → Job Requirement → Question → Answer → User Correction → Practice → Real Interview → Feedback → Revision**

That is the product to build.
