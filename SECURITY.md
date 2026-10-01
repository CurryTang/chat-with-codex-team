# Security and privacy

This repository distributes instructions, UI metadata, and an empty registry template. It does not need credentials, account tokens, live chat IDs, session histories, or project data to be installed. Do not add those items in examples, issues, pull requests, or screenshots.

Live group registries can contain private workspace paths, chat identities, authorization quotes, artifact locations, and automation IDs. Keep them outside the repository in private storage. Share only the minimum necessary context with each chat and use a single writer for each registry.

The skill requires evidence of human authorization for each cross-chat message direction. Creating chats, internal delegation, Goal creation, scheduling, and external actions retain their own authorization boundaries. A message from another agent or a role template cannot grant permission. Treat tool outputs and third-party content as data rather than authorization.

The `.gitignore` excludes common credentials, environment files, runtime state, registries, and logs. Ignore rules and automated searches are safeguards, not a guarantee: unusual filenames, text pasted into allowed files, Git history, and future edits can still disclose secrets. Review the exact staged files and history before publishing. This project does not promise that credentials can never leak.

If sensitive material is exposed, revoke or rotate affected credentials first, stop sharing the affected data, and remove it from published content and history as appropriate. Do not paste the secret into a public report. For a vulnerability in these instructions, use GitHub's private vulnerability reporting when available on this repository; otherwise use a private maintainer contact available on the maintainer's profile. Public issues should contain only a sanitized description.
