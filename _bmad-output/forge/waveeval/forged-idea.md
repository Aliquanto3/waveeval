# WaveEval — idée forgée (2026-10-20)

## Problème réel
Anaël ne sait ni défendre ni relire les choix d'évaluation de ses projets (décisions et code rédigés par Claude, option recommandée prise dans ~95 % des cas). Positionnement mesuré le 2026-10-20 : niveau ~2/4 sur les cinq thèmes ; bonne option reconnue 79 %, réponse produite seul 29 % ; point aveugle transversal : ne jamais valider l'instrument de mesure ; remède proposé avant le diagnostic. Détail : `positionnement/passation-2026-10-20.md`.

## Décisions
- WaveEval forme au **jugement** (décider, justifier, diagnostiquer), pas seulement aux bonnes pratiques.
- **Code délégué aux agents ; contenu et décisions d'évaluation par Anaël.** Rien n'entre qu'il ne sache défendre.
- **Deux étages** par module : Comprendre (exposition + QCM, entrée des collègues), puis Juger (réponse libre d'abord, diagnostic de cas, vraie évaluation).
- **Positionnement adaptatif** réutilisable : 3 options + « Je ne sais pas », justification, confiance 1-3, réponse libre avant les options pour les items moyens et difficiles, test en deux étapes par thème. Banque : `positionnement/`.
- **Module 1 : « Mesurer la mesure »** (accord corrigé du hasard, plafond humain, validation d'un juge et d'une métrique), fil rouge de l'app. Données : jeu public annoté par plusieurs humains, puis réponses du pilote `slm-quality-bench` ; décision réelle à produire : seuils d'accord et de biais de ce banc.
- **Fin d'un module** : nouvelles questions, réponse libre ≥ 1,5/2 sur chaque item moyen et difficile, aucune erreur à confiance 3 ; simulation d'entretien de 10 min sur un cas inédit ; note de décision écrite par Anaël sans brouillon de Claude. Ces preuves alimentent le portfolio.
- **Publics** : Anaël, puis collègues Wavestone (atelier, puis autonomie). Vraies évaluations ; précalculées pour les collègues sans clé.
- **Octobre en local**, clés personnelles sur données publiques. **Novembre** : hébergement Wavestone + SSO + Azure Foundry ; l'industrialisation devient un second objectif d'apprentissage, retardable.
- **Architecture** : fournisseur d'IA interchangeable (clés perso, Foundry, Ollama), identité branchable, passage local → hébergé sans réécriture.
- **Au 31/10 (point d'étape, pas une deadline)** : module 1 terminé, V1 locale qui le sert, atelier test facultatif. Ordre provisoire ensuite : juges, RAG (`wavelibrarian`), données, fine-tuning (`service-public-slm-fine-tuning`) ; pipeline transversal.

## Rejeté
- Un QCM seul pour se positionner : il mesure la reconnaissance, le geste même du problème.
- Contenu généré par Claude puis validé : reproduit le problème.
- Authentification maison en octobre : jetable dès le SSO Wavestone.
- Industrialisation tout de suite : disperse l'effort avant le premier module.
- Données du module 1 dépendantes du pilote : bloquerait le démarrage.

## À vérifier (recherche d'état de l'art)
- Jeux publics annotés par plusieurs humains, utilisables pour le module 1 (licence, langue, accord publié).
- Pratiques actuelles de validation des juges et des métriques ; catalogue des modèles Azure Foundry utilisables comme juges.
