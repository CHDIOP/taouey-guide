# Guide d'utilisation — Supercalculateur Taouey 🇸🇳

> Dépôt communautaire de documentation pour aider les chercheurs, ingénieurs et étudiants à utiliser **Taouey**, le supercalculateur national du Sénégal, géré par la [CINERI](https://cineri.sn) (Cyber-infrastructure nationale pour l'Enseignement supérieur, la Recherche et l'Innovation).

![Statut](https://img.shields.io/badge/statut-en%20construction-yellow)
![Licence](https://img.shields.io/badge/licence-CC--BY--4.0-blue)

## 🖥️ À propos de Taouey

| Caractéristique | Détail |
|---|---|
| Nom | Taouey (nom d'un cours d'eau reliant le lac de Guiers au fleuve Sénégal) |
| Puissance crête | 537,60 Téraflops |
| Rang | 3ᵉ supercalculateur d'Afrique lors de son acquisition |
| Acquisition | Décembre 2019, partenariat État du Sénégal / groupe Atos |
| Mise en service | Mars 2024 |
| Système | Atos/Bull Sequana XH2000 |
| Nœuds de calcul | 246 nœuds hétérogènes : 198 CPU (Xeon Gold 6138), 36 Xeon Phi (KNL), 12 GPU (NVIDIA V100) |
| Stockage | 1,1 Pétaoctet, protocole NFS |
| Réseau | InfiniBand EDR 100 Gb/s |
| Gestionnaire | CINERI (cineri.sn) |
| Domaines cibles | Climatologie (WRF), IA/LLM, génomique (Nanopore/Illumina), géophysique, océanographie (CROCO), ingénierie/CFD |

> ✅ Ces chiffres sont issus de la présentation officielle de l'équipe HPC de la CINERI. Pour le détail complet de l'architecture, voir [docs/08-architecture.md](docs/08-architecture.md).

## 🎯 Objectif de ce dépôt

Ce dépôt n'est pas la documentation officielle de la CINERI, mais un **guide pratique communautaire** destiné à :

- Aider les nouveaux utilisateurs à faire leurs premiers pas sur Taouey
- Centraliser les bonnes pratiques d'utilisation d'un cluster HPC
- Fournir des exemples de scripts (soumission de jobs, chargement de modules, etc.)
- Répondre aux questions fréquentes

## 📚 Sommaire de la documentation

1. [Introduction au calcul haute performance (HPC)](docs/01-introduction.md)
2. [Obtenir un compte et se connecter](docs/02-connexion.md)
3. [Gérer son environnement avec les modules](docs/03-environnement-modules.md)
4. [Soumettre des jobs avec SLURM](docs/04-soumission-jobs-slurm.md)
5. [Gestion des données et du stockage](docs/05-stockage-donnees.md)
6. [Bonnes pratiques et étiquette du cluster](docs/06-bonnes-pratiques.md)
7. [FAQ](docs/07-faq.md)
8. [Architecture matérielle de Taouey](docs/08-architecture.md)
9. [Didacticiels officiels par langage (C, Python, MATLAB, MPI, CUDA...)](docs/09-tutoriels-langages.md)

## 🔗 Ressources officielles complémentaires

- **[Taouey/Docs](https://github.com/Taouey/Docs)** — dépôt officiel CINERI avec des didacticiels détaillés par langage/outil (C, Python, R, MATLAB, MPI, CUDA, OpenMP, SLURM...)
- Site officiel : [www.cineri.sn](https://www.cineri.sn)
- Support technique : support@cineri.sn

## 🚀 Démarrage rapide

```bash
# Se connecter au cluster (remplacez par votre identifiant fourni par la CINERI)
ssh <votre_identifiant>@taouey.cineri.sn

# Lister les modules disponibles
module avail

# Charger un module (exemple)
module load python/3.11

# Soumettre un job SLURM
sbatch mon_job.slurm
```

## 🤝 Contribuer

Ce guide est collaboratif et s'améliore avec les retours de la communauté d'utilisateurs (chercheurs, universités, startups comme Baamtu, Suqali Mbaymi, Akademia, etc.). Voir [CONTRIBUTING.md](CONTRIBUTING.md).

## 🙏 Remerciements

Les informations techniques de ce guide s'appuient sur la documentation et la présentation officielles de l'**équipe HPC de la CINERI** :

- Adama COLY — Directeur de la direction, Ingénieur système sénior
- Cheikh DIOP — Ingénieur Calcul Scientifique sénior
- Oumar FALL — Ingénieur Calcul Scientifique junior
- Mamadou KEITA — Ingénieur Calcul Scientifique junior
- Issa GUEYE — Ingénieur Calcul Scientifique junior
- Bara BEYE — Ingénieur Système junior

## 📬 Contact officiel

Pour toute demande de compte, incident technique ou question officielle, contactez directement la **CINERI** : [https://cineri.sn](https://cineri.sn)

## 📄 Licence

Ce contenu est distribué sous licence [CC-BY-4.0](LICENSE) — libre de réutilisation avec attribution. Le nom "Taouey" et le supercalculateur appartiennent à l'État du Sénégal / CINERI.
