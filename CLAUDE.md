# WaveEval

Application pédagogique pour se former à l'évaluation d'IA générative : pipelines d'évaluation, LLM juges et leurs consignes, jeux de données, évaluation du RAG et de la recherche documentaire, données et évaluation pour le fine-tuning de LLM. Usage en autoformation d'abord, puis pour des collègues ou des clients, en autonomie ou en atelier.

## État
En spécification : le concept, les spécifications et la maquette sont à produire avec BMAD (`_bmad/`, sorties dans `_bmad-output/`). La stack n'est pas encore choisie.

## Conventions
- Le processus de développement suit la méthode BMAD. Documents en français.
- Code et identifiants en anglais, textes d'interface et contenus pédagogiques en français par défaut.
- Si la stack est Python : `uv` pour tout (`uv add`, `uv run`), jamais pip ; `ruff` et `pytest`.

## Commandes
- Contrôle rapide de fin de story : `uv run python .claude/cost-kit/controle_rapide.py` (à adapter quand la stack sera choisie).
