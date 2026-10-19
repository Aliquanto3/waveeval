# Positionnement initial — protocole

Lisible avant la passation. La banque d'items et son corrigé sont dans `banque-items.md` : **ne pas l'ouvrir avant la passation**.

## But et limites

- **But** : cartographier, thème par thème, ce qu'Anaël sait reconnaître, ce qu'il sait justifier et où il a des idées fausses, pour ordonner les modules de WaveEval. C'est un diagnostic formatif, pas une certification.
- **Limites** : un seul répondant ; difficultés fixées a priori, non calibrées (pas d'IRT possible avec n = 1) ; items rédigés par Claude, donc à relire et à défendre avant d'entrer dans l'app (règle du projet). La banque sera calibrée plus tard sur les passations des collègues (taux de réussite, discrimination point-bisériale, analyse des distracteurs).

## Construit et plan de test

Cinq thèmes, croisés avec trois niveaux cognitifs (taxonomie de Bloom révisée) :

| Thème | Facile (F) : se souvenir, comprendre | Moyen (M) : appliquer | Difficile (D) : analyser, évaluer |
|---|---|---|---|
| A. Pipeline d'évaluation | 2 | 1 | 2 |
| B. LLM juges | 2 | 1 | 2 |
| C. Données d'évaluation | 2 | 1 | 2 |
| D. RAG et recherche documentaire | 2 | 1 | 2 |
| E. Données et évaluation du fine-tuning | 2 | 1 | 2 |

Plus deux scénarios ouverts de diagnostic (niveau « juger »), notés par grille.

## Format d'un item

QCM à **3 options plus « Je ne sais pas »** (trois options bien construites valent quatre, Rodriguez 2005), rédigé selon les règles de Haladyna, Downing et Rodriguez (2002) : énoncé autonome, une seule meilleure réponse, distracteurs tirés d'idées fausses réelles, options de longueur comparable, pas de « toutes les réponses ».

À chaque item, Anaël donne :
1. **Son choix** (ou « Je ne sais pas », sans pénalité : mieux vaut un aveu qu'un coup de chance) ;
2. **Une justification en une phrase** (item à deux niveaux, Treagust 1988) ;
3. **Sa confiance** : 1 = je devine, 2 = plutôt sûr, 3 = certain (notation par la certitude, Gardner-Medwin).

**Items M et D : question ouverte d'abord.** Claude montre l'énoncé seul ; Anaël répond librement en deux ou trois phrases (réponse produite, notée 0 à 2 selon la grille). Ensuite seulement, les options apparaissent et Anaël choisit, justifie et donne sa confiance. L'écart entre la réponse produite et le choix parmi les options mesure directement l'écart entre produire et reconnaître. Les items F restent en QCM simple (étage « Comprendre »).

## Classement de chaque réponse

| Classe | Règle |
|---|---|
| Maîtrisé | bonne réponse, justification juste (2/2), confiance ≥ 2 |
| Fragile | bonne réponse, mais justification partielle ou absente, ou confiance 1 |
| Lacune | mauvaise réponse avec confiance ≤ 2, ou « Je ne sais pas » |
| Idée fausse | mauvaise réponse avec confiance 3 : priorité de remédiation |

La justification est notée 0, 1 ou 2 par Claude selon la grille du corrigé. Anaël peut contester une note ; la contestation est consignée.

## Déroulé adaptatif (test en deux étapes par thème)

1. **Auto-évaluation** (2 min) : pour chaque thème, un niveau de 1 à 4 — 1 : je connais à peine les termes ; 2 : je comprends si on m'explique ; 3 : je sais appliquer sur un cas simple ; 4 : je sais justifier mes choix et critiquer un protocole.
2. **Par thème**, un item d'aiguillage (M) :
   - maîtrisé → deux items difficiles (D) ;
   - sinon → deux items faciles (F) ; si les deux sont maîtrisés, un item D de rattrapage (protège contre une erreur d'aiguillage).
3. **Ordre** : les thèmes sont alternés (A, B, C, D, E pour les aiguillages, puis les branches) pour limiter la fatigue sur un même sujet.
4. **Scénarios ouverts** J1 et J2 à la fin, sans options.
5. **Pas de correction pendant la passation** : le corrigé est donné à la fin, pour ne pas influencer les items suivants.

Durée prévue : 45 à 50 minutes (items F à 1 min 30, items M et D à 3 min avec la question ouverte, deux scénarios à 6 min).

## Restitution

- Profil par thème (nombre d'items maîtrisés, fragiles, en lacune, en idée fausse), pas de score total ;
- Calibration : auto-évaluation comparée au niveau mesuré, thème par thème ;
- Liste des idées fausses, avec leur correction ;
- Conséquence pour WaveEval : ordre des modules et niveau d'entrée de chacun.

Après la passation, les réponses vont dans `passation-2026-10-20.md`, et la banque peut être versionnée (avant, elle resterait lisible sur le dépôt public).
