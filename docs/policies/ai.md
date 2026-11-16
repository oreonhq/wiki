# AI Policy
This is our formal AI policy. Anyone who uses AI tools while developing within this project (Oreon) must read it. The policy applies to all AI tools, including but not limited to Claude, ChatGPT, OpenAI Codex, Claude Code, DeepSeek, Kimi, Gemini, Google AI Studio, Microsoft Copilot, GitHub Copilot, GitHub’s auto-generated commit messages, Cursor, and OpenCode.

## Development & Contribution Use
We allow that AI can be used for development and contribution purposes, but you are required to check and review the code it outputs. Do not let the AI vibecode. You must use it as a tool, not a full replacement for **you**. We do not have an opinion on AI use, we just require it to be used as a tool and only a tool, not to step in the place of humans. If it works better for you **as a tool**, then use it, if it doesn't, then don't use it. Be responsible with it. Any defective code made by AI should be fixed as soon as possible, to maintain end-user quality and experience.

You are still responsible for every contribution you submit. That includes understanding what the change does, making sure it builds and behaves correctly, and being able to explain it in review. "The AI wrote it" is not an excuse for broken packages, broken docs, or broken features.

AI output that copies code, text, or other material you do not have rights to contribute still breaks the [Contribution Policy](contributors.md). You must only submit work you have the legal right to contribute, same as if you typed it yourself.

### AI tool Terms of Service
By using an AI tool on Oreon contributions, you agree that **you** will follow that tool's Terms of Service, Acceptable Use Policy, and any other rules that come with the account or product you are using. Oreon allowing AI does not override OpenAI, Anthropic, Google, Microsoft, Cursor, GitHub, or anyone else's terms.

That means you must:

- only use accounts and plans you are allowed to use (no stolen, shared-in-violation, or cracked access)
- follow each provider's rules about what you may upload, generate, and redistribute
- follow each provider's rules about commercial/open-source use when those apply to your Oreon contribution
- not use Oreon as cover for breaking a provider's ban, rate limit, or usage restriction

If a provider's terms conflict with Oreon policy on a specific point, the stricter rule for that point is what you follow. Breaking a provider's TOS while "working on Oreon" is still your problem, and maintainers may reject related contributions.

### Secrets, private data, and abuse
Do not paste Oreon secrets, build credentials, private keys, access tokens, passwords, private infrastructure details, or personal data of you or others into AI tools.

Do not use AI tools to generate malware, phishing, credential stuffing material, or other abuse aimed at Oreon users, infrastructure, or third parties. Security research is covered below and still has to stay within the law and Oreon policies.

### AI Use Disclosure
We do not require you to disclose your use of AI, but we strongly recommend it for transparency as this is an Open-Source project. Either way, you must follow all of the **required** policies below.

### Commit Messages
We do not allow using fully AI-generated commit messages as they look sloppy and show that you did not guard your agent properly. **Read and review the code it outputs as stated in the previous policy item, and then WRITE THE COMMIT MESSAGE YOURSELF.** If we find any AI-generated commit messages in your PR or branch, we will require you to review the code and type your own commit message before it can be merged.

### Pull requests and reviews
PRs still need a clear description of what changed and why. AI can help draft a description, but you have to check it and make sure it matches the actual diff. Do not paste a generated summary that talks about files or behavior that are not in the PR.

Maintainers may reject or send back contributions that are clearly unreviewed AI dumps, that do not match the description, or that ignore project rules (including packaging commit rules).

### Packaging and docs
Packaging work must still follow the packaging docs and the one-package commit rule. AI does not get a free pass on `orbscli` requirements/packaging standards, commit subjects, or missing dependency work. Docs are the same. If AI drafts a page, you still have to read it, fix wrong statements, and make sure commands and examples are correct.

## Security Research Use
We allow AI to be used for security research, but we discourage use of AI for security patches themselves.

AI may help with reading advisories, searching for known issues, or sorting CVEs. The actual patch should be written and checked by a human who understands the vulnerability and the fix. Do not ask an agent to make a security fix and then submit it without understanding what it changes.

## Announcements & Social Media
We do not allow use of AI for writing full on announcements and posts. **Just write it yourself, don't be lazy.**

Short grammar cleanup on something you already wrote is fine.

## Support and community
Do not use AI as a replacement for reading Oreon docs or asking the community. If you bring a support question based on wrong AI output about Oreon, we may ignore it until you can show what you actually ran and what failed.

## Future Changes
This policy is subject to change at any time. We will notify everyone publicly if anything is changed here regarding the use of AI.
<small>Policy Last Updated: November 16, 2026 at 12:56am PT by Brandon Lester</small>
