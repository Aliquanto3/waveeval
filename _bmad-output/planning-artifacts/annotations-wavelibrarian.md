# Annotations humaines d'Anaël dans WaveLibrarian

Note du 2026-10-26, à verser aux entrées de spécification de WaveEval : module 1 « Mesurer la mesure » et module RAG.

## De quoi il s'agit

Pour calibrer les Juges du banc WaveLibrarian (story 8.10), Anaël a annoté à la main 92 couples Réponse × Critère, en verdict binaire `yes` / `no`, avec 53 notes libres. Les Réponses viennent d'un run RAG réel sur le Mini-corpus : le Generator est minicpm5:1b, les Questions sont celles du split dev et quatre architectures sont comparées. La saisie s'est faite sur une page HTML d'annotation.

| | |
|---|---|
| Réponses | 40, sur 19 Questions |
| Architectures | rag-naif 30 lignes, pageindex 23, raptor 22, bm25 17 |
| Critères | faithfulness 40 (réannotée : 14 oui, 26 non ; première annotation : 36 oui, 4 non) ; completeness 40 (8 oui, 32 non) ; correctness 12 (4 oui, 8 non) |
| Annotateur | un seul (`anael`) |
| Licence | Mini-corpus original en CC-BY-4.0, dépôt public sous Apache 2.0, aucune donnée client |

## Où les trouver

Dépôt `https://github.com/Aliquanto3/wavelibrarian` (local : `C:\Users\Utilisateur\Documents\GitHub\wavelibrarian`) :

- `corpora/minicorpus/annotations/anael.yaml` : le paquet annoté. Chaque ligne porte `answer_id`, `criterion`, `verdict` et `note`, plus ce que voient les Juges : question, réponse de référence, réponse, ancres citées, passages de référence et de contexte.
- `corpora/minicorpus/annotations/anael.html` : la page d'annotation de la première passe, lisible dans un navigateur.
- Historique git de `anael.yaml` : la première annotation (commit `56b7855`) et la réannotation de la fidélité (commit `d02ef03`). `git show 56b7855:corpora/minicorpus/annotations/anael.yaml` rend la première version.
- La page de réannotation de la fidélité est hors dépôt : `C:\Users\Utilisateur\Documents\annotation-fidelite-wavelibrarian.html`.
- `corpora/minicorpus/questions.yaml` et `corpora/minicorpus/docs/` : le jeu de Questions gelé et les documents du corpus.
- `_bmad-output/implementation-artifacts/verifications-reelles-2026-10-18-v0.2.0.md` : le run source (`20261018-115941-a2a512`) et la constitution du paquet (story 8.9).
- À venir dans le même dossier : la série de vérifications réelles des stories 8.10 et 8.11, avec les kappas Juge contre humain. Les fichiers bruts de calibration (`calibration*.json`, verdicts des Juges par passe) restent hors dépôt, dans `C:\Users\Utilisateur\Documents\GitHub\wl-runs-v0.2.0\runs\20261018-115941-a2a512\`.

## Avantages pour WaveEval

- **Un cas réel et complet de validation de juge.** Sur les mêmes 92 lignes, on aura les verdicts humains et ceux de quatre Juges : granite4.2:3b et qwen3.5:4b en local (5 passes chacun), Claude Haiku 5.5 et Claude Opus 5.5 (2 passes chacun). Cela donne de quoi travailler le kappa Juge contre humain, l'instabilité entre passes et l'écart entre un petit modèle local et un modèle frontière.
- **Des critères déséquilibrés, idéaux pour le paradoxe du kappa.** Completeness compte 80 % de non, et la première annotation de faithfulness comptait 90 % de oui : un Juge peut atteindre un fort accord brut et un kappa faible. C'est exactement la leçon « accord corrigé du hasard » du module 1.
- **Un cas complet d'erreur de consigne, avec avant et après** : voir la section suivante.
- **Des notes qui montrent les ambiguïtés de consigne.** Exemple : sur q-044, Anaël écrit « il n'y avait pas d'extrait attendu, c'est pour dire que l'info n'existe pas dans le corpus ? ». En réalité, la Question attendait un document ajouté par une mise à jour que le run n'avait pas jouée. C'est un vrai cas de diagnostic, non fabriqué par Claude, conforme à la règle de WaveEval qui rejette le « contenu généré par Claude puis validé ».
- **Un lien direct avec le module RAG.** Les mêmes Réponses servent à comparer des architectures de recherche (BM25, RAG naïf, RAPTOR, PageIndex) critère par critère.

## Cas d'étude : la fidélité mal comprise (2026-10-26)

**Constat.** À la première calibration des Juges Claude, Opus 5.5 a eu un kappa de 1,00 en correctness et de 0,73 en completeness, mais de 0,15 seulement en faithfulness (accord brut 53 %). Les 19 désaccords allaient tous dans le même sens : l'humain disait oui, Opus disait non.

**Diagnostic.** Il y avait deux lectures de la même consigne, « tout ce qu'elle affirme se trouve dans le contexte fourni ».
- La lecture de l'annotateur était lexicale : les mots de la réponse se retrouvent-ils dans un extrait ? Exemple, q-020 : « Qui commande le caboteur lancé en 1412 ? » → « Maëlis Torvane ». Note de l'annotateur : « le système cite bien une information d'un extrait ».
- La lecture d'Opus est celle d'étayage : l'affirmation, prise comme réponse à la question, est-elle soutenue par le contexte ? Pour q-020, le registre désigne Ilsa Marenne ; Maëlis Torvane n'apparaît jamais comme capitaine. C'est la lecture standard, celle de RAGAS : la part des affirmations de la réponse que l'on peut déduire du contexte.

**Causes du côté de l'outil**, pas seulement de l'annotateur :
- la consigne de la page était courte et ambiguë (« Tout ce qu'elle affirme se lit-il dans les extraits ? ») ;
- la page **surlignait dans les extraits les mots de la réponse**, ce qui pousse à chercher des mots plutôt qu'un sens ;
- la réponse attendue était affichée, ce qui mêle justesse et fidélité.

**Correction.** Les 40 lignes de fidélité ont été réannotées **à l'aveugle**, sans voir les verdicts des Juges, sur une nouvelle page :
- une consigne explicite : lire la réponse comme une réponse à la question ; retrouver les mots ne suffit pas ; une réponse qui n'affirme rien est fidèle ; une réponse fidèle à un extrait faux reste fidèle ;
- plus de surlignage ;
- plus de réponse attendue.

Reprendre les verdicts d'Opus, ou ne revoir que les 19 désaccords en voyant ses réponses, aurait rendu le kappa circulaire.

**Résultat.** Les kappas ont été recalculés sur les verdicts déjà rendus par les Juges, sans nouvel appel.

| Juge | faithfulness avant | après | après, sans les 5 réponses vides |
|---|---|---|---|
| Claude Opus 5.5 | 0,15 | **0,84** | 0,80 |
| Claude Haiku 5.5 | 0,26 | 0,64 | 0,59 |

Opus passe le seuil de 0,6 sur les trois Critères. Haiku reste exclu, à cause de sa completeness (0,14).

Les Juges locaux (Ollama, CPU, 5 passes) ont aussi été calibrés sur ces 92 lignes. Ils restent loin du seuil, ce qui illustre bien l'écart entre un petit modèle local et un modèle frontière comme juge.

| Juge | correctness | completeness | faithfulness (1re annotation → réannotation) |
|---|---|---|---|
| qwen3.5:4b | 0,53 | 0,26 | 0,19 → 0,31 |
| granite4.2:3b | 0,23 | 0,21 | 0,22 → 0,40 |

Détail complet dans `wavelibrarian/_bmad-output/implementation-artifacts/verifications-reelles-2026-10-26-v0.2.0.md`.

**Ce qui reste.** Restent 3 désaccords (q-004 deux fois, q-019). L'humain a dit non parce que la réponse était incomplète alors que les extraits contenaient la réponse complète (« un denier » au lieu de « un denier par mesure »). C'est un défaut de completeness, pas de faithfulness : une réponse partielle mais exacte au regard des extraits reste fidèle. Ces labels ont été laissés tels quels.

**Cas limite : aucun extrait.** 5 Réponses de PageIndex n'ont reçu aucun extrait et sont vides. Sans extrait, toute affirmation factuelle est infidèle, même juste, car elle vient de la mémoire du modèle. Une réponse vide n'affirme rien et est donc fidèle par convention. Opus et la nouvelle consigne retiennent cette convention ; RAGAS, lui, ne donne pas de score quand il n'y a aucune affirmation. Ces « oui » n'apprennent rien sur le système et gonflent la part des oui. Une case « sans objet » serait plus juste qu'un choix binaire.

**Leçons pour WaveEval :**
- un kappa bas peut venir de la consigne d'annotation, pas du juge : on diagnostique le sens des désaccords avant d'exclure un juge ;
- l'outil d'annotation influence le label (surlignage, informations affichées) ;
- on réannote à l'aveugle, jamais en voyant le verdict du juge ;
- la frontière entre fidélité, complétude et justesse se travaille sur des exemples ;
- prévoir « sans objet » quand un critère ne s'applique pas.

## Limites

- **Un seul annotateur.** On ne peut calculer ni accord inter-annotateurs ni plafond humain, qui sont au cœur du module 1. Ces annotations complètent le jeu public annoté par plusieurs humains prévu pour le module 1, sans le remplacer.
- **Un petit échantillon.** 92 lignes, dont 12 seulement en correctness : les intervalles de confiance des kappas seront larges. Ça suffit pour un exercice, pas pour fixer un seuil.
- **Un annotateur qui est aussi l'apprenant.** Pour un exercice de WaveEval destiné à Anaël lui-même, ses propres labels ne sont pas un jeu neuf. Pour des collègues, ils le sont.
- **Un domaine étroit.** C'est un corpus fictif et court en français (chroniques, conventions), avec des réponses d'un modèle de 1B souvent incomplètes. Le taux de completeness (20 % de oui) reflète ce modèle, pas un RAG de production.
- **Une Question à part.** q-044 n'était pas annotable fiablement en phase `base` : l'Ancrage de référence est absent. Ses 4 lignes sont à signaler ou à écarter dans un exercice.
- **Une licence à vérifier.** La licence des sorties de minicpm5 n'a pas été vérifiée avant toute republication des Réponses dans WaveEval.

## Piste

Faire annoter 20 à 30 lignes du même paquet par un second annotateur, avec la même page HTML. On obtiendrait un plafond humain sur des données RAG pour WaveEval, et la double annotation partielle prévue par la story 8.10 de WaveLibrarian.
