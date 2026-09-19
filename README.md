# Come installare questa Skill su Claude.ai

1. Vai su claude.ai -> Impostazioni (Settings) -> Funzionalità (Features) -> Skills
2. Clicca "Create Custom Skill" o "Upload Skill"
3. Carica il file `claude-zip-visualizer-perfect.zip` (o incolla il contenuto di SKILL.md)
4. Attiva la skill
5. Ora carica una ZIP e scrivi "visualizza perfettamente"

Claude genererà automaticamente:
- Report testuale come quello che ti ha già dato
- + Artifact HTML interattivo con anteprime vere

Differenza con Claude Code:
- Claude Code = skill per terminale, usa python + ffprobe in locale
- Claude (chat) = questa skill, usa Artifact HTML + JSZip lato client
