# 2. Obtenir un compte et se connecter

> ✅ Le processus décrit ci-dessous est le processus **officiel**, tel que présenté par l'équipe HPC de la CINERI.

## 2.1 Processus officiel de demande d'accès

L'accès à Taouey suit un circuit en 5 étapes, encadré par le Conseil Scientifique de la CINERI :

| # | Étape | Détail |
|---|---|---|
| 1 | **Dépôt du dossier** | Soumission en ligne sur le portail CINERI : description du projet, besoins en ressources (CPU/GPU), calendrier |
| 2 | **Évaluation** | Analyse par le Conseil Scientifique : faisabilité technique, pertinence scientifique, adéquation aux ressources disponibles |
| 3 | **Notification** | Réponse sous **3 semaines**. Attribution d'un **quota d'heures CPU/GPU** et d'un espace de stockage |
| 4 | **Accès & exécution** | Connexion SSH sécurisée, soumission de jobs via **SLURM/PBS**, monitoring temps réel des ressources utilisées |
| 5 | **Rapport & renouvellement** | Rapport de résultats à fournir. Le renouvellement du quota est soumis à l'évaluation des livrables produits |

> 💡 Préparez à l'avance une estimation réaliste de vos besoins (heures CPU/GPU, volume de stockage) : c'est un critère d'évaluation de votre dossier à l'étape 2.

Portail de dépôt : [https://cineri.sn](https://cineri.sn)

## 2.2 Prérequis techniques

- Un client SSH (intégré sous Linux/macOS ; sous Windows, utilisez [PuTTY](https://www.putty.org/), Windows Terminal, ou WSL)
- Une paire de clés SSH (recommandé plutôt qu'un mot de passe)

### Générer une paire de clés SSH

```bash
ssh-keygen -t ed25519 -C "votre_email@exemple.sn"
```

Communiquez ensuite votre **clé publique** (`~/.ssh/id_ed25519.pub`) à la CINERI lors de la demande de compte. **Ne partagez jamais votre clé privée.**

## 2.3 Se connecter au cluster

L'adresse du nœud de connexion est `taouey.cineri.sn`. Remplacez `<votre_identifiant>` par le login qui vous a été communiqué par la CINERI :

```bash
ssh <votre_identifiant>@taouey.cineri.sn
```

Exemple :

```bash
ssh jdupont@taouey.cineri.sn
```

## 2.4 Bonnes pratiques de sécurité

- Utilisez toujours une **authentification par clé SSH**, pas de mot de passe seul si possible
- Ne réutilisez jamais le mot de passe de votre compte Taouey ailleurs
- Déconnectez-vous (`exit`) après chaque session, surtout sur un poste partagé
- Ne partagez jamais vos identifiants, même avec des collègues du même projet — demandez un compte séparé pour chaque personne

## 2.5 Premiers réflexes après connexion

```bash
# Vérifier votre quota de stockage
quota -s          # ou la commande spécifique fournie par la CINERI

# Voir l'état général du cluster
sinfo

# Voir vos jobs en cours
squeue -u $USER
```

> ℹ️ **SLURM ou PBS ?** L'équipe HPC mentionne les deux ordonnanceurs (SLURM/PBS) comme systèmes de soumission de jobs sur Taouey. Ce guide documente principalement **SLURM** (le plus répandu), mais vérifiez auprès de votre référent CINERI lequel est actif sur votre allocation — les commandes PBS (`qsub`, `qstat`, `qdel`) diffèrent de celles de SLURM.

➡️ Passez au chapitre suivant : [Gérer son environnement avec les modules](03-environnement-modules.md)
