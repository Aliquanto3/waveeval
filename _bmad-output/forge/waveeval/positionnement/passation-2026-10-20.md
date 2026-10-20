# Passation du 2026-10-20 — Anaël

## Auto-évaluation
A3 B3 C2 D3 E1

## Réponses

### A1 (M, aiguillage)
- Libre : « v2 probablement meilleure ; vérifier par méthode statistique que ce n'est pas un effet d'échantillon trop faible ; IC de Clopper-Pearson pour chaque version, conclure si les IC ne se recoupent pas, sinon élargir l'échantillon. » Contexte : a beaucoup utilisé win rates et IC pour des tournois de Magic.
- Note libre provisoire : 1,5 — incertitude d'échantillonnage bien vue, mais méthode non appariée (IC séparés, test de chevauchement trop conservateur, ignore que ce sont les mêmes questions). À restituer : chevauchement d'IC ≠ test ; appariement (McNemar, bootstrap apparié).
- Choix : C, confiance 3. Justification = réponse libre (2/2 à la lettre de la grille : incertitude + intervalle). Classe : maîtrisé → branche D (A2, A3). Nuance : l'appariement n'a pas été produit spontanément, il a été reconnu dans l'option.

### B1 (M, aiguillage)
- Libre : « biais du juge ; forcer une explication / chain-of-thought sur les critères ; ancrage sémantique (vocabulaire psychométrique) pour lui signaler le biais d'ordre ; éventuellement présenter deux fois en inversant et moyenner ; ou changer de juge (hypothèse : post-training récompensant la première option). »
- Note libre provisoire : 1,5 — biais d'ordre identifié (non nommé « biais de position »), double ordre cité mais parmi d'autres pistes, non priorisé ; remèdes par le prompt (CoT, consigne) proposés en premier alors qu'ils ne suffisent pas sans mesure ; pas d'idée de suivre le taux d'incohérence. À restituer : prioriser la mesure et le contrôle par construction (double ordre, égalité si divergence) sur les consignes ; hypothèse post-training intéressante.
- Choix : A, confiance 3. Justification 2/2 à la lettre (biais d'ordre + double ordre). Classe : maîtrisé → branche D (B2, B3). Nuance : le traitement des divergences en égalité et le suivi du taux ont été reconnus, pas produits.

### C1 (M, aiguillage)
- Libre : « d'abord des données réelles (historique d'un ancien chatbot RH, messages Outlook/Teams reçus par les RH avec leurs réponses) ; réponses de l'ancien chatbot à vérifier ; compléter par génération depuis le corpus (main et/ou LLM) et questions RH courantes trouvées en ligne, réponses vérifiées dans la doc de l'entreprise ; relecture par des RH, voire rédaction par eux. »
- Note libre provisoire : 2 — réel d'abord, synthétique en complément, validation experte. Manques à restituer : RGPD et anonymisation (messages RH = données sensibles), stratification par catégorie et cas rares, questions sans réponse ou hors périmètre (abstention), questions adverses, taille du jeu.
- Choix : B, confiance 3. Justification 2/2. Classe : maîtrisé → branche D (C2, C3). Auto-évaluation C2 : sous-estimation probable.

### D1 (M, aiguillage)
- Libre : « d'abord tester le retrieval, composant central ; selon le résultat, descendre vers le reranker puis le LLM et son prompt, ou remonter vers l'embedding et la distance cosinus, voire le contenu de la base (faux ou incomplet). »
- Note libre provisoire : 2 — localisation étage par étage, amont (corpus, embeddings) et aval (reranker, génération) bien vus. Manques à restituer : comment tester la recherche (passages de référence annotés, rappel@k), et lire d'abord les 25 % d'échecs pour les classer (analyse d'erreurs), ce qui oriente vers le bon étage plus vite.
- Choix : A, confiance 3. Justification 2/2. Classe : maîtrisé → branche D (D2, D3).

### E1 (M, aiguillage)
- Libre : « vérifier l'absence de régression sur les capacités généralistes, pour s'assurer que le modèle reste pertinent en généralisation au-delà du jeu de test. »
- Note libre provisoire : 1 — oubli catastrophique vu ; contamination entre entraînement et test non vue (cause la plus fréquente d'un saut de 33 points). Confusion légère à restituer : la régression des capacités générales n'est pas la généralisation dans le domaine (cas non vus du même domaine).
- Choix : C, confiance 2. Ajout : « oublié la contamination ; normalement à vérifier avant même de lancer l'entraînement » (remarque juste : déduplication en amont). Justification 1/2 (contamination reconnue après les options). Classe : fragile → branche F (E4, E5), rattrapage D si les deux sont maîtrisés.

## Branches

### A2 (D)
- Libre : « jeu d'éval non adapté à l'usage réel ; repartir des interactions enregistrées pour un jeu plus proche des attentes, vérifier si le chatbot stagne dessus ; hypothèse de style : questions courantes déjà bien traitées, gains sur des questions complexes peu posées ; forme, ton, références culturelles non mesurés. »
- Note libre provisoire : 1 — représentativité et données de production vues ; bonne intuition de validité (les critères mesurés ne couvrent pas ce qui fait la satisfaction). Non vu : le surajustement au jeu consulté à chaque itération (Goodhart) et le jeu de test réservé jamais consulté.
- Choix : A, confiance 3. Justification « il y a eu surentraînement » : 1/2 (reconnu après les options ; terme inexact : pas d'entraînement ici, mais un surajustement par sélection itérative de prompts et réglages sur le même jeu). Classe : fragile.

### B2 (D)
- Libre : « je ne sais pas ce qu'est un kappa ». Note libre : 0 (lacune déclarée, notion de kappa de Cohen inconnue).
- Choix : D (je ne sais pas). Classe : lacune.

### C2 (D)
- Libre : « accord proche du taux de réponses correctes ; ordre de grandeur cohérent pour des humains ; ne sait pas trop quoi conclure, sinon que la précision reste insuffisante pour des systèmes critiques. »
- Note libre provisoire : 0 — l'accord dû au hasard (déséquilibre 90/10) n'est pas vu ; intuition juste que 92 % ressemble au taux de base, sans en tirer la conséquence. « Précision » employé au sens courant.
- Choix : C (Pearson), confiance 2. Faux. Classe : lacune (confiance 2). Idée à corriger : une corrélation mesure l'association, pas l'accord ; ici l'accord attendu par hasard est 0,82, d'où un kappa ≈ 0,56.

### D2 (D)
- Libre : « problème à la création du jeu de données (IA ou non-experts) ; le recréer avec les experts dans la boucle. »
- Note libre provisoire : 0 — la distinction fidélité (soutien par les passages récupérés, métrique sans référence) / exactitude n'est pas faite ; l'hypothèse suppose que la fidélité dépend de réponses de référence, ce qui n'est pas le cas.
- Choix : B, confiance 2. Bonne réponse, reconnue après les options, alors que la réponse libre allait ailleurs. Justification 0/2. Classe : fragile (reconnaissance sans production : écart typique).

### A3 (D)
- Libre : « récupérer des courriers administratifs réels (avec leur contexte), les faire noter par des humains (bon / pas bon, avec justification), pour créer un jeu séparé en train/test afin d'entraîner puis valider l'assistant. »
- Note libre provisoire : 0,5 — bonne intuition de partir du réel et des jugements humains justifiés (proche de l'émergence des critères), mais regarde des courriers humains, pas les sorties de l'assistant ; glisse de l'évaluation vers l'entraînement (split train/test) alors que la question porte sur l'évaluation.
- Choix : A (ROUGE / BERTScore), confiance 2. Faux. Classe : lacune. Idée à corriger : les métriques de recouvrement corrèlent mal avec la qualité d'un texte libre ; l'analyse d'erreurs sur les sorties précède le choix des métriques.

### B3 (D)
- Libre : « biais structurel probable (les LLM favorisent les réponses longues) ; option lourde : fine-tuner le juge ou en choisir un moins biaisé ; d'abord modifier le prompt pour valoriser la concision ; ou comparer deux réponses sur le fond puis préférer la plus synthétique à fond égal. »
- Note libre provisoire : 1 — biais de verbosité identifié ; idée intéressante de séparer le fond puis départager. Non vu : remplacer la note globale par des critères précis, et vérifier sur un échantillon annoté humain que le biais disparaît. Risque à restituer : récompenser la concision dans le prompt peut créer le biais inverse, sans mesure on ne le voit pas.
- Choix : B, confiance 2. Bonne réponse ; justification 1/2 (biais vu, remède et vérification reconnus après les options). Classe : fragile.

### C3 (D)
- Libre : « très clairement de la contamination : les données publiées depuis 2023 ont dû entrer dans le corpus d'entraînement. »
- Note libre provisoire : 1 — contamination vue, aucune méthode de vérification proposée (variantes reformulées ou inédites, complétion mot pour mot, jeux privés ou postérieurs à la coupure). « Très clairement » : certitude élevée sur une hypothèse non vérifiée.
- Choix : C, confiance 3. Pas de méthode de vérification ajoutée. Justification 1/2. Classe : fragile.

### D3 (D)
- Libre : « la distance cosinus (et/ou BM25) a dû servir à récupérer les résultats, il faut donc un autre critère de pertinence ; intuition : appliquer un reranker, sans savoir sur quel critère il juge la pertinence. »
- Note libre provisoire : 0 — confusion importante : la fonction de score qui produit le classement (cosinus, BM25, reranker) n'est pas la métrique qui évalue ce classement contre des jugements de pertinence humains (qrels). Aucune métrique de classement citée.
- Choix : C, confiance 2. Bonne réponse reconnue après les options (le libre confondait classement et évaluation). Justification 0/2. Classe : fragile.

### E4 (F)
- Choix : C, confiance 3, sans justification. Classe : fragile à la lettre de la grille (justification absente) ; probablement maîtrisé vu ses projets (RAG et fine-tuning pratiqués).
- Justification ajoutée après coup : « le fine-tuning réentraîne un modèle existant, modifie plutôt le comportement qu'il n'apporte des connaissances ; le RAG est du context engineering qui retrouve les extraits pertinents ; bien plus facile à mettre à jour. » 2/2. Classe révisée : maîtrisé.

### E5 (F)
- Choix : A, confiance 3. Justification : split train/test vu en cours de data science, s'applique aux LLM ; données non vues pour s'assurer qu'il n'y a pas d'overfitting. 2/2 (nuance : le test détecte le surapprentissage, il ne l'empêche pas). Classe : maîtrisé → rattrapage D : E3.

### E3 (D, rattrapage)
- Libre : « ne sait pas ce que signifie SFT ; suppose de l'overfitting quand on met trop de documents ; le sous-ensemble filtré suggère des exemples de meilleure qualité, d'où l'importance de données d'entraînement propres. »
- Note libre provisoire : 1 — qualité des données vue. Idée fausse : plus de données augmente le surapprentissage (c'est l'inverse, à qualité égale). Terme SFT (supervised fine-tuning) inconnu, alors qu'il est au cœur de son projet service-public-slm-fine-tuning. Rien proposé à vérifier.
- Choix : B, confiance 3. Justification : propreté du jeu d'entraînement, intuition initiale confirmée par l'option. 2/2. Classe : maîtrisé (idée fausse annexe à restituer : volume → surapprentissage).

## Scénarios ouverts

### J1 — RAG juridique
- Défauts trouvés : juge = générateur (auto-préférence, surestimation ; changer de famille) ; références à rédiger à la main ou relire avec soin (★ circularité traitée par le remède, sans être nommée) ; 40 questions insuffisantes, passer à quelques centaines.
- Bilan : 3 défauts sur 9, dont 1 ★. Seuil de maîtrise (5 dont 2 ★) non atteint. Manqués : ★ juge non validé contre des annotations humaines ; questions rédigées par le développeur (représentativité) ; « qualité » 1 à 5 non définie et moyenne d'échelle ordinale ; recherche et génération non séparées ; aucune baseline ; expertise juridique, citations, cas sans réponse.

### J2 — fine-tuning prêt à déployer ?
- Manques trouvés : échantillon trop faible (10 exemples, 3 évaluateurs ; idée avancée d'intervalles par grappes d'évaluateurs) ; plus de questions pour des tests appariés.
- Bilan : 2 manques sur 7, aucun ★. Perplexité inconnue (★ manqué : elle ne mesure pas la qualité de la tâche). Manqués : ★ aucune baseline ; évaluateurs non aveugles, critères non définis ; régressions (capacités générales, refus, hallucinations) ; contamination ; coût, latence, suivi.

## Restitution

| Thème | Auto-éval | Maîtrisé | Fragile | Lacune | Niveau mesuré |
|---|---|---|---|---|---|
| A. Pipeline | 3 | A1 | A2 | A3 | ~2 |
| B. Juges | 3 | B1 | B3 | B2 | ~2 |
| C. Données | 2 | C1 | C3 | C2 | ~2 |
| D. RAG | 3 | D1 | D2, D3 | — | ~2 |
| E. Fine-tuning | 1 | E4, E5, E3 | E1 | — | ~2,5 |

- Aucune idée fausse au sens strict (aucune erreur à confiance 3). Idées fausses repérées dans les réponses libres : chevauchement d'IC comme test ; plus de données → plus de surapprentissage ; fidélité calculée contre des références ; fonction de classement confondue avec métrique d'évaluation.
- Produire contre reconnaître (14 items M et D) : bonne option choisie 11 fois (79 %), réponse libre ≥ 1,5 seulement 4 fois (29 %).
- Les 5 aiguillages réussis : bons premiers réflexes (données réelles, séparation des étages, incertitude statistique, double ordre).
- Motif transversal : ne jamais proposer de valider l'instrument de mesure (juge contre humains, métrique de fidélité, vérification de contamination, accord corrigé du hasard). Remède proposé avant le diagnostic (CoT, prompt, reranker, entraînement).
- Vocabulaire manquant : kappa, accord dû au hasard, nDCG, perplexité, SFT.
- Calibration : surestimation sur A, B, D ; sous-estimation sur E.
- Banque : grille de A1 trop indulgente (le chevauchement d'IC obtient 2/2), à durcir : exiger l'appariement. Rappeler la justification dans les items F.
