# Transfert depuis slm-quality-bench : un cas réel pour le module « Mesurer la mesure »

Note du 2026-10-26. Source : [slm-quality-bench](https://github.com/Aliquanto3/slm-quality-bench), calibration du juge sur la campagne `pilot-20261017`. Le module 1 prévoit déjà d'utiliser les réponses de ce pilote (`_bmad-output/forge/waveeval/forged-idea.md`). Publication des données : sans objection d'Anaël.

## Le jeu disponible

Sur les mêmes 100 clés de jugement :

- **une vérité humaine** : 100 étiquettes d'Anaël (oui/non) et 55 notes libres, sur des critères binaires de réponses de rédaction en français (tâches de forme et de contenu : temps verbal, faits à mentionner, sigles, nombre de répliques…) ;
- **quatre verdicts de juges** : Opus 5.5, MiMo, Haiku 5.5 et un rejeu de Haiku ;
- **le contexte** : la consigne, la réponse du modèle (5 petits modèles locaux, dont des Qwen pour mesurer un biais de famille) et la grille complète de chaque question.

Fichiers (dans le dépôt slm-quality-bench, sous `data/quality_bench/`, hors git) :

| Contenu | Fichier |
|---|---|
| Étiquettes humaines (`qb.label/1`) | `calibration/labels.jsonl` |
| Verdicts des juges (`qb.judgment/1`, champ `verdict`) | `campaigns/pilot-20261017/judgment-{opus,mimo,haiku,haiku-replay}/judgment.jsonl` |
| Réponses des modèles | `campaigns/pilot-20261017/generation.jsonl` |
| Consignes et critères | `sets/set-v2/questions.jsonl` (`writing/writing-fr/NNNN`) |
| Rapport de calibration | `campaigns/pilot-20261017/calibration-report.{md,json}` |

Jointure : clé `pilot-20261017|<modèle>|<piste>|<question>|<seed>|<critère>` ; la clé de génération est la même sans le critère. Un export autonome pour WaveEval (un JSONL par item : consigne, réponse, critère, étiquette, verdicts) reste à faire.

## Les résultats

| Juge | Accord | IC 95 % (Wilson) | Défauts humains repérés | Biais Qwen |
|---|---|---|---|---|
| Opus 5.5 | 100 % | [96,3 ; 100] | 12 / 12 | 0 pt |
| MiMo | 97 % | [91,5 ; 99,0] | 9 / 12 | +2 pts |
| Haiku 5.5 | 90 % | [82,6 ; 94,5] | 5 / 12 | +4 pts |

Seuils fixés par Anaël : accord ≥ 0,95, biais Qwen ≤ 0,05. Opus et MiMo admissibles, Haiku écarté.

## Leçons pour le module

1. **Le piège du taux de base.** 88 critères sur 100 sont satisfaits : « toujours oui » ferait 88 %. Les 90 % de Haiku ne valent presque rien ; il faut regarder les défauts repérés, l'indulgence et la sévérité.
2. **La stabilité n'est pas la justesse.** Haiku rejoué : 99,7 % de verdicts identiques, et pourtant 7 défauts sur 12 manqués.
3. **L'intervalle de confiance d'abord.** Avec 50 étiquettes, la marge était d'environ ±10 points, trop pour départager les juges ; avec 100, environ ±6.
4. **Le critère flou.** Le seul désaccord initial humain/Opus venait d'un critère à deux exigences ; après relecture, l'humain a donné raison au juge. Exercice : réécrire le critère.
5. **Ce que la grille ne voit pas.** Les notes libres relèvent des défauts qu'aucun critère ne couvre : infidélité au document (7 notes), fautes de langue (5), réponse en anglais (3), dialogues invraisemblables (4).
6. **Pourquoi pas un LLM comme étiqueteur ?** Étiqueter avec Claude reviendrait à mesurer Claude contre lui-même ; la référence doit être humaine.
7. **Les comptages se vérifient par le code**, pas par un juge (slm-quality-bench, `grading/counting.py`).

## Limite

Un seul annotateur : pas d'accord inter-humains. Le module prévoit un jeu public multi-annotateurs pour cette partie ; à défaut, un réétiquetage à l'aveugle d'une vingtaine d'items par Anaël donnerait un accord intra-annotateur.
