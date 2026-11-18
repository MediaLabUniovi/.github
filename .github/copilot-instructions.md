# MediaLabUniovi — Copilot & Agent Instructions

All AI coding assistants (GitHub Copilot, Antigravity, etc.) operating in MediaLabUniovi repositories must comply with our organizational standards:

1. **Clean Markdown Headers:** Never include emojis in Markdown headings (`#`, `##`, `###`) to preserve slug anchors.
2. **Minimal Emoji Palette:** Restrict emojis strictly to `📁`, `🛠️`, `🔌`, `⚠️`, `📌` in trees/links only. Never in explanatory text or tables.
3. **Typology Enforcement:** Do NOT create empty or parasite directories. Embedded/IoT projects use dual `hardware/` and `software/`. Software-only repos must never have `hardware/`. Hardware-only repos must never have `software/`.
4. **Secret Protection:** Never commit API keys, passwords, or credentials. Always isolate secrets in dedicated files (`credentials.h`, `.env`), untrack them with `git rm --cached`, add them to `.gitignore`, and provide `*.example` templates.
5. **Conventional Commits:** All commit messages must follow Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:`, `refactor:`).
6. **Platform Agnostic:** Do not hardcode machine-specific serial ports (e.g. use `<PUERTO_SERIE>` instead of `COM6`).
7. **Issues Tracking:** Check `gh auth status` before CLI issue operations; fallback to `TODO.md` when offline/unauthenticated. Close issues via commit messages (`Closes #X`).

For detailed documentation, refer to [NORMAS_PROYECTOS_AGENTES.md](../NORMAS_PROYECTOS_AGENTES.md) and [AGENTS.md](../AGENTS.md).
