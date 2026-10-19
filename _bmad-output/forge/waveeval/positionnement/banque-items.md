# Banque d'items du positionnement — avec corrigé

> **Anaël : ne pas ouvrir avant la passation.** Claude présente les items un par un dans la conversation.

Chaque item : niveau (F, M, D), énoncé, options A à C, D = « Je ne sais pas ». Puis le corrigé : bonne réponse, idée fausse visée par chaque distracteur, grille de justification (2 = raison clé, 1 = partielle, 0 = absente ou fausse).

---

## A. Pipeline d'évaluation

### A1 (M, aiguillage)
Ton assistant interne passe d'un prompt v1 à un prompt v2. Sur les mêmes 50 questions de ton jeu d'éval, v1 obtient 78 % de réponses jugées correctes et v2 84 %. Quelle conclusion est la plus défendable ?
- A. v2 est meilleure : 6 points d'écart sur un même jeu de questions suffisent à trancher.
- B. Il faut relancer l'éval avec un second juge, et garder v2 si les deux juges concordent.
- C. L'écart peut être du bruit à cette taille ; il faut un test apparié sur les mêmes questions avant de conclure.

**Corrigé : C.** 39 contre 42 bonnes réponses ; l'intervalle de Wilson à 95 % de v2 va d'environ 71 % à 92 %. Ce qui compte en apparié, ce sont les questions discordantes (juste en v1 et fausse en v2, et l'inverse) : un écart net de 3 questions est loin d'être significatif (McNemar, bootstrap apparié). A : confond écart observé et écart réel. B : un second juge traite la fiabilité de la mesure, pas l'incertitude d'échantillonnage.
**Grille** : 2 = incertitude liée à la taille **et** appariement ou test/intervalle ; 1 = seulement « trop peu de questions ».

### A2 (D)
Depuis trois mois, ton équipe améliore un chatbot en consultant à chaque itération le même jeu d'éval de 200 questions. Le score est passé de 70 % à 91 %, mais la satisfaction des utilisateurs stagne. Explication la plus probable et remède ?
- A. Le système s'est ajusté au jeu d'éval lui-même ; il faut un jeu de test réservé, jamais consulté, enrichi de cas réels.
- B. Le juge est devenu trop indulgent au fil des versions ; il faut le remplacer par un modèle plus strict et plus récent.
- C. 200 questions ne suffisent plus ; il faut porter le jeu à 2 000 questions générées par un LLM pour couvrir plus de cas.

**Corrigé : A.** Loi de Goodhart, surajustement au jeu de développement. B : rien n'indique une dérive du juge, qui reste figé. C : du volume synthétique ne corrige ni la non-représentativité ni le fait que le jeu reste consulté.
**Grille** : 2 = surajustement/Goodhart **et** test réservé ou données de production ; 1 = un seul des deux.

### A3 (D)
Tu dois évaluer un assistant qui rédige des courriers administratifs. Rien n'est encore en place. Quelle première étape apporte le plus de valeur ?
- A. Constituer un jeu de courriers de référence et calculer ROUGE et BERTScore entre les sorties et ces références.
- B. Lire un échantillon de sorties réelles, classer les erreurs observées, puis définir les critères à mesurer à partir de ces échecs.
- C. Mettre en place un LLM juge qui note de 1 à 10 la qualité globale de chaque courrier, pour obtenir vite une base de comparaison.

**Corrigé : B.** L'analyse d'erreurs précède le choix des métriques (pratique défendue par Husain et Shankar ; Shankar et al. 2024, « Who Validates the Validators? » : les critères se précisent en lisant les sorties). A : les métriques de recouvrement corrèlent mal avec la qualité d'un texte libre. C : une note globale non définie et non validée ne mesure rien de précis.
**Grille** : 2 = partir des échecs réels pour définir les critères ; 1 = « regarder les sorties » sans le lien avec les critères.

### A4 (F)
Quelle est la différence entre évaluation hors ligne (offline) et évaluation en ligne (online) ?
- A. Hors ligne, l'évaluation est faite par des humains ; en ligne, elle est faite automatiquement par des métriques ou des juges.
- B. Hors ligne, on évalue sur un jeu de données figé avant déploiement ; en ligne, sur le trafic réel en production.
- C. Hors ligne, on évalue un modèle exécuté en local ; en ligne, un modèle appelé par API chez un fournisseur.

**Corrigé : B.** A et C confondent le moment et la source des données avec le mode de notation ou d'hébergement.
**Grille** : 2 = jeu figé avant déploiement contre trafic réel ; 1 = l'un des deux.

### A5 (F)
À quoi sert un jeu de régression dans un pipeline d'évaluation ?
- A. À suivre l'évolution du score moyen dans le temps, pour en estimer la tendance et anticiper les baisses.
- B. À rassembler les erreurs du modèle pour les réutiliser comme données d'entraînement au prochain cycle.
- C. À vérifier qu'une modification (prompt, modèle, index) ne casse pas des cas qui fonctionnaient déjà.

**Corrigé : C.** B mélange évaluation et entraînement : c'est une source de contamination.
**Grille** : 2 = détecter qu'un changement casse l'existant ; 1 = « vérifier la qualité » sans l'idée de non-régression.

---

## B. LLM juges

### B1 (M, aiguillage)
Tu compares deux réponses avec un LLM juge (« laquelle est la meilleure ? »). En inversant l'ordre de présentation des deux réponses, le verdict change dans 30 % des cas. Que fais-tu ?
- A. Juger chaque paire dans les deux ordres, compter une égalité quand les verdicts divergent, et suivre ce taux d'incohérence.
- B. Fixer l'ordre une fois pour toutes, en plaçant toujours la réponse de référence en premier, pour que la comparaison soit stable.
- C. Garder le protocole : 70 % de verdicts stables, c'est un niveau courant pour un juge, et le bruit se compense sur un grand jeu.

**Corrigé : A.** Biais de position (Zheng et al. 2023, MT-Bench). B : fixer l'ordre rend le biais systématique, il favorise toujours le même côté. C : un biais n'est pas un bruit aléatoire, il ne se compense pas.
**Grille** : 2 = biais de position **et** double ordre ; 1 = l'un des deux.

### B2 (D)
Pour noter la fidélité au contexte de réponses RAG, tu as 150 réponses annotées par deux experts. Leur accord mutuel : kappa de Cohen 0,62. Ton juge candidat obtient un kappa de 0,58 avec l'expert 1 et de 0,60 avec l'expert 2. Conclusion ?
- A. Le juge est insuffisant : un juge n'est utilisable qu'au-delà d'un kappa de 0,8 avec chaque expert.
- B. Le juge est bon : un kappa de 0,6 signifie qu'il est d'accord avec les experts sur 60 % des réponses.
- C. Le juge approche le plafond fixé par l'accord entre humains ; il faut maintenant analyser ses désaccords.

**Corrigé : C.** L'accord humain borne ce qu'on peut attendre du juge. A : seuil absolu qui ignore ce plafond. B : le kappa n'est pas un pourcentage d'accord, il corrige l'accord dû au hasard. Bonus : un kappa humain de 0,62 suggère une consigne d'annotation à préciser.
**Grille** : 2 = comparaison au plafond humain ; 1 = « le kappa est moyen » sans référence à l'accord humain.

### B3 (D)
Ton juge note la « qualité globale » des réponses de 1 à 10. Tu constates que les réponses longues reçoivent en moyenne 1,5 point de plus, à contenu équivalent. Que fais-tu ?
- A. Ajouter au prompt du juge la consigne « ne tiens pas compte de la longueur », puis reprendre les notations.
- B. Remplacer la note globale par des critères binaires précis, puis vérifier sur un échantillon annoté que l'écart disparaît.
- C. Diviser chaque note par la longueur de la réponse, pour neutraliser l'effet de verbosité dans tous les scores.

**Corrigé : B.** Biais de verbosité. A peut aider mais ne vaut rien sans vérification. C pénalise arbitrairement les réponses longues légitimes.
**Grille** : 2 = critères précis **et** vérification contre des humains ; 1 = l'un des deux.

### B4 (F)
Qu'appelle-t-on « LLM-as-a-judge » ?
- A. Le routage par lequel un LLM choisit quel modèle, parmi plusieurs, doit répondre à une requête donnée.
- B. L'usage d'un LLM pour noter ou comparer les sorties d'un système d'IA selon des critères définis.
- C. Le classement public de modèles établi à partir des votes d'utilisateurs humains sur des duels.

**Corrigé : B.** C décrit les arènes à votes humains (type Chatbot Arena).
**Grille** : 2 = noter ou comparer des sorties selon des critères ; 1 = « un LLM qui évalue » sans plus.

### B5 (F)
Pourquoi préfère-t-on souvent, pour un juge, des critères binaires (oui / non) à une note de 1 à 10 ?
- A. Ils se définissent plus précisément, se reproduisent mieux et se vérifient plus facilement contre des annotations humaines.
- B. Ils consomment beaucoup moins de tokens, ce qui réduit fortement le coût d'une campagne d'évaluation à grande échelle.
- C. Les LLM manipulent mal les nombres, si bien qu'une échelle de 1 à 10 produit des notes presque aléatoires.

**Corrigé : A.** B : écart de coût négligeable. C : exagéré ; le problème est l'ambiguïté des échelons, pas le calcul.
**Grille** : 2 = précision ou reproductibilité **et** vérifiabilité ; 1 = l'une des deux.

---

## C. Données d'évaluation

### C1 (M, aiguillage)
Tu constitues un jeu d'éval pour un assistant RH interne. Quelle approche est la plus solide ?
- A. Faire générer 1 000 questions par un LLM à partir de la base documentaire RH, pour couvrir tous les documents.
- B. Partir de vraies questions d'utilisateurs par catégorie, compléter les cas rares par du synthétique relu, faire valider les références.
- C. Reprendre un benchmark public de questions-réponses en français, déjà validé, et y ajouter quelques questions RH.

**Corrigé : B.** A : des questions générées depuis les documents en reprennent la formulation, couvrent ce qui est dans les documents et non les vrais besoins, et oublient les questions sans réponse. C : hors domaine.
**Grille** : 2 = représentativité des vraies questions **et** validation des références ; 1 = l'un des deux.

### C2 (D)
Deux annotateurs étiquettent 200 réponses « correcte / incorrecte ». Environ 90 % des réponses sont correctes, et chacun en étiquette environ 90 % comme correctes. Ils sont d'accord sur 92 % des cas. Que penser de cet accord ?
- A. Il est plus modeste qu'il n'y paraît : le hasard seul en explique une grande part, une mesure corrigée est nécessaire.
- B. Il est excellent : 92 % d'accord brut place ces annotations parmi les plus fiables qu'on puisse espérer.
- C. Il faut plutôt une corrélation de Pearson entre les deux annotateurs pour juger de leur accord réel.

**Corrigé : A.** Accord attendu par hasard : 0,9 × 0,9 + 0,1 × 0,1 = 0,82, d'où un kappa ≈ (0,92 − 0,82) / (1 − 0,82) ≈ 0,56. C : une corrélation mesure l'association, pas l'accord.
**Grille** : 2 = accord dû au hasard élevé par déséquilibre des classes **et** kappa ou alpha ; 1 = « il faut un kappa » sans raison.

### C3 (D)
Un modèle open-weights obtient sur un benchmark public de 2023 un score bien supérieur à ce que suggère son usage réel. Hypothèse prioritaire ?
- A. Le benchmark est trop facile pour les modèles récents ; le saut de score reflète simplement leurs progrès généraux.
- B. Les conditions de génération (température, graine) étaient plus favorables lors du benchmark que lors de l'usage réel.
- C. Le benchmark a pu fuiter dans les données d'entraînement ; il faut tester des variantes reformulées ou inédites.

**Corrigé : C.** Contamination. Vérifications : variantes reformulées ou nouvelles de même difficulté (écart de score), complétion mot pour mot de l'énoncé, jeux privés ou postérieurs à la date de coupure.
**Grille** : 2 = contamination **et** une méthode de vérification ; 1 = contamination seule.

### C4 (F)
Qu'est-ce qu'un « golden set » ?
- A. Un jeu de questions avec des réponses de référence validées, qui sert d'étalon stable pour comparer les versions.
- B. Un recueil des meilleures réponses produites par le modèle, sélectionnées pour servir d'exemples dans le prompt.
- C. La part des données d'entraînement jugée la plus fiable, que l'on surpondère lors du fine-tuning.

**Corrigé : A.**
**Grille** : 2 = références validées **et** usage d'étalon ; 1 = l'un des deux.

### C5 (F)
Que mesure l'accord inter-annotateurs ?
- A. La part des réponses du modèle que les annotateurs jugent correctes, toutes annotations confondues.
- B. Le degré d'accord entre les étiquettes des annotateurs humains et celles produites par le LLM juge.
- C. À quel point des annotateurs indépendants donnent les mêmes étiquettes aux mêmes items, signe d'une consigne claire.

**Corrigé : C.** B décrit l'accord juge-humain, qui se mesure de la même façon mais n'est pas l'accord inter-annotateurs.
**Grille** : 2 = annotateurs indépendants **et** clarté de la tâche ou de la consigne ; 1 = l'un des deux.

---

## D. RAG et recherche documentaire

### D1 (M, aiguillage)
Ton RAG répond faux sur 25 % des questions de test. Quelle première étape de diagnostic est la plus efficace ?
- A. Évaluer séparément la recherche (les bons passages sont-ils parmi les k récupérés ?) et la génération (fidélité à ces passages).
- B. Remplacer le LLM générateur par un modèle plus puissant, puis mesurer le gain obtenu sur les mêmes questions de test.
- C. Ajouter un reranker après la recherche, puisque c'est l'amélioration qui apporte en général le plus de gain sur un RAG.

**Corrigé : A.** Localiser l'étage défaillant avant de corriger. B et C sont des remèdes choisis sans diagnostic.
**Grille** : 2 = séparer recherche et génération ; 1 = « analyser les erreurs » sans la séparation.

### D2 (D)
Une métrique de fidélité (faithfulness) donne 0,95 sur ton RAG, mais des experts jugent 30 % des réponses incorrectes. Explication la plus cohérente ?
- A. Le générateur hallucine fréquemment, et la métrique de fidélité ne parvient pas à détecter ces hallucinations.
- B. La réponse peut être fidèle aux passages récupérés et fausse quand même, si ces passages sont hors sujet ou obsolètes.
- C. Les experts appliquent une grille plus sévère que la métrique ; il faut aligner leur grille sur celle de la métrique.

**Corrigé : B.** La fidélité mesure le soutien par le contexte, pas l'exactitude. B explique les deux chiffres sans supposer un outil défaillant ; A reste à vérifier (valider la métrique sur un échantillon), mais elle n'est pas l'explication la plus économique. C inverse la référence : ce sont les experts qui servent d'étalon.
**Grille** : 2 = fidélité ≠ exactitude, la recherche est en cause ; 1 = « la métrique ne mesure pas tout ». Bonus : proposer de valider aussi la métrique.

### D3 (D)
Tu évalues le classement d'un moteur de recherche documentaire. Pour chaque requête, plusieurs documents sont pertinents, à des degrés différents (très ou partiellement pertinent). Quelle métrique est la plus adaptée ?
- A. Le MRR (rang réciproque moyen), qui récompense le système quand le premier document pertinent apparaît tôt.
- B. Le rappel@k, qui mesure la part des documents pertinents retrouvés parmi les k premiers résultats renvoyés.
- C. Le nDCG@k, qui tient compte à la fois du degré de pertinence de chaque document et de sa position.

**Corrigé : C.** A ne regarde que le premier pertinent ; B est binaire et ignore l'ordre dans le top k.
**Grille** : 2 = pertinence graduée **et** position ; 1 = l'une des deux.

### D4 (F)
Que mesure le rappel@5 (recall@5) d'un système de recherche ?
- A. La part des 5 premiers résultats qui sont pertinents, c'est-à-dire la proportion de bons documents dans ce top 5.
- B. La part de l'ensemble des documents pertinents qui figure parmi les 5 premiers résultats renvoyés.
- C. Le rang moyen auquel apparaît le premier document pertinent, calculé sur les 5 premiers résultats.

**Corrigé : B.** A est la précision@5, C ressemble au MRR.
**Grille** : 2 = rapporté à l'ensemble des pertinents ; 1 = définition floue.

### D5 (F)
Dans l'évaluation de la fidélité d'un RAG, qu'appelle-t-on une « hallucination » ?
- A. Une affirmation de la réponse qui n'est pas soutenue par les passages fournis au modèle.
- B. Une réponse fausse au regard de la réalité, quelles que soient les sources fournies au modèle.
- C. Une réponse qui cite une source existante mais sans en reprendre les termes exacts.

**Corrigé : A.** B est le sens large (erreur factuelle), pas celui de la fidélité au contexte.
**Grille** : 2 = non soutenu par le contexte fourni ; 1 = « invente des choses ».

---

## E. Données et évaluation du fine-tuning

### E1 (M, aiguillage)
Tu fine-tunes un petit modèle (SFT avec LoRA) pour répondre à des questions administratives. Sur ton jeu de test interne, le score passe de 55 % à 88 %. Quelle vérification est prioritaire avant de conclure ?
- A. Vérifier que la perte d'entraînement a bien convergé et qu'elle reste stable sur les dernières étapes.
- B. Relancer le test à plusieurs températures, pour s'assurer que le gain ne dépend pas d'un réglage de génération.
- C. Vérifier l'absence de questions du test (ou de reformulations) dans l'entraînement, et contrôler l'oubli des capacités générales.

**Corrigé : C.** Contamination entre entraînement et test, oubli catastrophique. A renseigne sur l'entraînement, pas sur la généralisation.
**Grille** : 2 = fuite entre entraînement et test **et** oubli ; 1 = l'un des deux.

### E2 (D)
Pour un alignement par DPO (Direct Preference Optimization), de quoi tes données doivent-elles être constituées ?
- A. De triplets (prompt, réponse préférée, réponse rejetée), proches de ce que produit le modèle et contrastés sur le critère visé.
- B. De paires (prompt, réponse idéale) rédigées par des experts, les plus nombreuses et variées possible.
- C. De réponses notées de 1 à 10 par des annotateurs, la note servant directement de récompense au modèle.

**Corrigé : A** (Rafailov et al. 2023). B décrit des données de SFT ; C, une récompense de type apprentissage par renforcement.
**Grille** : 2 = paires préférée/rejetée **et** proximité avec la distribution du modèle ou contraste ciblé ; 1 = triplet seul.

### E3 (D)
Tu as 50 000 exemples SFT générés par un grand modèle à partir de tes documents. Un fine-tuning sur un sous-ensemble filtré de 5 000 exemples donne de meilleurs résultats que sur les 50 000. Explication la plus plausible ?
- A. Un LoRA a une capacité limitée : au-delà de quelques milliers d'exemples, il sature et commence à se dégrader.
- B. Le filtrage a retiré doublons, erreurs et exemples pauvres : qualité et diversité comptent plus que le volume.
- C. Avec moins d'exemples, l'entraînement est plus court, ce qui laisse moins de temps au modèle pour surapprendre.

**Corrigé : B** (cf. LIMA, Zhou et al. 2023, et les travaux sur le filtrage des données d'instruction). Bonus : vérifier que le filtre n'a pas rapproché les données du jeu de test.
**Grille** : 2 = qualité, doublons ou diversité ; 1 = « moins de bruit » sans précision.

### E4 (F)
Pour apporter à un LLM les connaissances d'un domaine, quelle différence entre RAG et fine-tuning ?
- A. Le RAG modifie les poids du modèle à partir des documents ; le fine-tuning se contente de les placer dans le prompt.
- B. Le fine-tuning est plus précis que le RAG pour les faits, mais il coûte plus cher à mettre en place et à maintenir.
- C. Le RAG fournit les documents à la requête (à jour, citables) ; le fine-tuning modifie les poids, plutôt pour un style ou un format.

**Corrigé : C.**
**Grille** : 2 = documents à la requête contre poids modifiés, avec l'usage de chacun ; 1 = mécanisme sans les usages.

### E5 (F)
Pourquoi garder un jeu de test séparé des données de fine-tuning ?
- A. Pour mesurer la généralisation à des cas non vus : évaluer sur des données d'entraînement surestime la performance.
- B. Pour pouvoir l'ajouter à l'entraînement en fin de projet, une fois les hyperparamètres choisis, et gagner des points.
- C. Pour raccourcir l'entraînement, puisque chaque exemple retiré des données réduit la durée de chaque époque.

**Corrigé : A.**
**Grille** : 2 = généralisation **et** surestimation ; 1 = l'un des deux.

---

## Scénarios ouverts (niveau « juger »)

### J1 — Le RAG juridique d'un collègue
« Notre RAG juridique obtient 4,6 / 5 de qualité moyenne. Un LLM juge, le même modèle que notre générateur, a noté 40 questions que j'ai rédigées moi-même. Pour l'aider, il voyait des réponses de référence, que nous avions produites avec le RAG puis relues rapidement. » Liste les défauts de ce protocole et ce que tu ferais à la place.

**Défauts attendus** (maîtrise à partir de 5, dont au moins 2 marqués ★) :
- ★ Références produites par le système évalué : circularité ;
- ★ Juge non validé contre des annotations humaines ;
- Juge identique au générateur : biais d'auto-préférence ;
- 40 questions : intervalle large, aucune incertitude rapportée ;
- Questions rédigées par le développeur : non représentatives, biais de confirmation ;
- « Qualité » 1 à 5 non définie ; moyenne d'une échelle ordinale ;
- Recherche et génération non évaluées séparément ;
- Aucune référence de comparaison (baseline) ;
- Domaine juridique : exactitude et citations à vérifier par un expert, cas sans réponse absents.

### J2 — Le modèle fine-tuné prêt à déployer ?
« Nous avons fine-tuné un modèle de 3 milliards de paramètres pour rédiger les réponses aux usagers. La perplexité sur notre jeu de validation a baissé de 30 %, et trois collègues trouvent les réponses meilleures sur 10 exemples. On déploie ? » Que manque-t-il pour décider ?

**Manques attendus** (maîtrise à partir de 5, dont au moins 2 marqués ★) :
- ★ La perplexité ne mesure pas la qualité de la tâche ;
- ★ Aucune comparaison à des références de base (modèle de base avec un bon prompt, plus grand modèle, RAG) ;
- 10 exemples, sans test apparié ni incertitude ;
- Évaluateurs non aveugles, ordre non aléatoire, critères non définis ;
- Régressions non vérifiées : capacités générales, refus, sécurité, hallucinations ;
- Contamination possible entre entraînement et validation ;
- Coût, latence, conditions réelles d'usage, suivi après déploiement.
