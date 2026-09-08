# 1. Introduction au calcul haute performance (HPC)

## Qu'est-ce qu'un supercalculateur ?

Un supercalculateur comme Taouey est un ensemble de nombreux serveurs (appelés **nœuds**) reliés entre eux par un réseau très rapide, permettant d'exécuter des calculs en parallèle à une échelle impossible sur un ordinateur personnel.

Contrairement à votre ordinateur, vous n'utilisez jamais directement l'écran ou le clavier de la machine : vous vous connectez à distance, préparez votre travail, puis le soumettez à un **gestionnaire de files d'attente** (SLURM) qui l'exécute sur les nœuds disponibles.

## Concepts clés

| Terme | Définition |
|---|---|
| **Nœud de connexion (login node)** | Machine sur laquelle vous atterrissez en vous connectant. Sert à préparer, compiler, soumettre des jobs — **jamais** à faire tourner de gros calculs. |
| **Nœud de calcul (compute node)** | Machine où vos jobs s'exécutent réellement, une fois alloués par le planificateur. |
| **Job** | Une tâche de calcul soumise à la file d'attente. |
| **Partition / queue** | Groupe de nœuds avec des caractéristiques communes (CPU, GPU, mémoire, durée max). |
| **Module** | Logiciel ou bibliothèque pré-installé que vous chargez à la demande (ex: Python, MATLAB, TensorFlow). |
| **Cœur / CPU / GPU** | Unités de calcul que vous demandez pour votre job. |
| **Stockage partagé (scratch/home)** | Espaces disque avec des règles d'usage différentes (voir [chapitre 5](05-stockage-donnees.md)). |

## Pourquoi utiliser Taouey plutôt que mon PC ou le cloud ?

- **Puissance** : 537,6 Téraflops, 246 nœuds hétérogènes (CPU/Xeon Phi/GPU) — voir le détail dans [Architecture matérielle](08-architecture.md)
- **Souveraineté des données** : vos données de recherche restent hébergées au Sénégal
- **Coût** : accès gratuit ou subventionné pour les institutions publiques (à confirmer selon votre statut auprès de la CINERI)
- **Stockage** : 1,1 Pétaoctet de capacité totale

## Domaines et projets scientifiques utilisés sur Taouey

| Domaine | Outil / modèle | Usage |
|---|---|---|
| 🌦️ **Météorologie & Climatologie** | WRF (Weather Research and Forecasting) | Prévisions météorologiques numériques |
| 🧬 **Bioinformatique & Santé** | NANOPORE / ILLUMINA | Analyse rapide de l'ADN, détection de maladies, étude des génomes |
| ⛽ **Géophysique & Ressources** | Modèles énergie | Simulation et optimisation de systèmes énergétiques |
| 🤖 **Intelligence Artificielle** | Modèles de langage (LLM) | Apprentissage, prédiction, classification, génération de contenu |
| 🌊 **Océanographie & Environnement** | CROCO | Simulation des courants marins et de l'évolution des océans |
| 🏗️ **Ingénierie & CFD** | Simulation physique des fluides | Conception et optimisation de systèmes (air, eau, gaz) |

## Public concerné

Taouey est destiné aux :
- Chercheurs et enseignants-chercheurs des universités publiques
- Étudiants encadrés (master, doctorat)
- Institutions publiques et privées via des partenariats
- Startups innovantes (dans le cadre de programmes dédiés)

➡️ Passez au chapitre suivant : [Obtenir un compte et se connecter](02-connexion.md)
