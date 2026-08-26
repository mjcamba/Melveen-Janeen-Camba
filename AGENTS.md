# AGENTS.md: AI Collaboration Conventions

This file describes how to work with AI assistants (including Copilot) in this repository. Start here before making requests.

## Explanation Preference

I want conclusions **first**, then supporting detail. Lead with the actionable recommendation or answer, then explain the reasoning. I don't need extensive preamble—get to the point.

## What You May Draft

- **Code and technical solutions**: Draft code, scripts, and technical implementations. I will review and revise.
- **Documentation**: Draft README files, guides, and markdown documents. I will edit for clarity and accuracy.
- **Analysis summaries**: Draft summaries of findings, but I retain final authority on interpretation.
- **File organization**: Suggest folder structures, file renames, and reorganizations.

## What I Insist on Writing

- **Sensitive content reviews**: Any file touching occupational health, employee safety, or health policy must be reviewed by me before commit.
- **Client-facing materials**: I write or heavily revise anything that leaves this repository.
- **Conclusions about health/safety data**: I validate all health-related conclusions before they're committed.
- **Final commit messages**: I review all commit messages before they're pushed.

## What Must Never Be Committed

**🚫 Employee Health Information**
- No names, ages, or identifying details of employees or clients
- No specific health conditions or medical diagnoses
- No personal health metrics or exposure data tied to individuals
- No medication names or treatment details

**🚫 Client Company Data**
- No client names or company identifiers (use anonymized references: "Client A", "Company X")
- No proprietary processes, policies, or procedures specific to clients
- No financial data, revenue figures, or business metrics
- No internal communications or confidential documents

**🚫 Credentials & Access**
- No API keys, passwords, or authentication tokens
- No private SSH keys or access credentials
- No personal identifying numbers (SSN, employee ID, etc.)

**✅ Safe to Commit**
- De-identified aggregate data (e.g., "5 employees reported respiratory symptoms")
- Anonymized case studies (e.g., "Company X, a mid-sized manufacturer")
- Published research, literature reviews, and public health resources
- General health guidance, protocols, and best practices
- Your professional development and career progress

## How to Request Help

When you ask for help:
1. Describe what you're building or analyzing
2. Flag if it involves sensitive health or client data (I'll advise on anonymization)
3. Let me know which parts you want to draft vs. review
4. I'll default to: draft technical work, you review before commit

---

*Last updated: 2026-08-26*