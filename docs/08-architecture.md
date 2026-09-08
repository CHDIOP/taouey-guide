# 8. Architecture matérielle de Taouey

Cette page décrit l'infrastructure physique de Taouey, telle que présentée officiellement par l'équipe HPC de la CINERI. Ces informations aident à comprendre **quelle partition choisir** selon votre type de calcul (voir [chapitre 4](04-soumission-jobs-slurm.md)).

## 8.1 Vue d'ensemble

Taouey est bâti sur un système **Atos/Bull Sequana XH2000**, à architecture conteneurisée modulaire — 3ᵉ supercalculateur d'Afrique lors de son acquisition (décembre 2019), avec **537,60 TFlops** de puissance crête.

## 8.2 Nœuds de calcul (246 nœuds hétérogènes)

| Type de nœud | Quantité | Détails |
|---|---|---|
| **CPU — Xeon Gold 6138** | 198 nœuds | 80 cœurs logiques/nœud (2 × 20 cœurs @ 2,0 GHz), architecture Skylake-SP, RAM DDR4 haute densité |
| **Many-core — Intel Xeon Phi (KNL)** | 36 nœuds | 256 cœurs/nœud, Knights Landing 2ᵉ génération, cache MCDRAM 16 Go HBW, vectorisation AVX-512 native |
| **GPU — NVIDIA V100 Tensor Core** | 12 nœuds | 4 × GPU V100/nœud, 5 120 cœurs CUDA + 640 Tensor Cores, 32 Go HBM2/GPU, NVLink inter-GPU |
| **Nœuds de service** | — | Intel Haswell E7-8860v3, 16 cœurs @ 2,2 GHz, 140W, 6 To DDR4 (2 133 MT/s) — 256 cœurs au total |

### Comment choisir votre type de nœud

- **Calcul classique parallèle (MPI, simulations physiques génériques)** → nœuds **Xeon Gold 6138**
- **Calcul vectorisé intensif, codes optimisés AVX-512** → nœuds **Xeon Phi (KNL)**
- **Deep learning, entraînement de modèles, calcul Tensor** → nœuds **GPU V100**

> Les noms exacts des partitions SLURM correspondant à chaque type de nœud (ex: `cpu`, `knl`, `gpu`) doivent être confirmés auprès de votre référent CINERI — utilisez `sinfo` une fois connecté pour lister les partitions réellement configurées.

## 8.3 Stockage

| Caractéristique | Valeur |
|---|---|
| **Capacité totale** | 1,1 Pétaoctet (Po) |
| **Protocole** | NFS — accès concurrent depuis tous les nœuds |
| **Espace NFS dédié** | 10 To |

Voir le [chapitre 5](05-stockage-donnees.md) pour les bonnes pratiques d'utilisation de cet espace.

## 8.4 Réseau d'interconnexion

```
Switch Core (InfiniBand EDR — 100 Gb/s)
        │
        ▼
Switch Leaf, un par rack (InfiniBand EDR — 100 Gb/s)
        │
        ▼
246 nœuds de calcul (HCA InfiniBand intégré par nœud)
```

- **Réseau de calcul** : InfiniBand EDR à 100 Gb/s, faible latence — essentiel pour les jobs multi-nœuds (MPI)
- **Réseau d'administration (OOB)** : Ethernet 1 GbE, séparé du réseau de calcul

## 8.5 Infrastructure physique

- **Système** : Atos/Bull Sequana XH2000, architecture conteneurisée modulaire
- **Refroidissement** : tour adiabatique haute efficacité + refroidissement liquide (technologie Mobull)
- **Alimentation** : groupe électrogène de secours en mode automatique, arrivée eau de ville pour le refroidissement

## 8.6 Ce que ça implique pour vos jobs

- Avec 80 cœurs/nœud sur la partition Xeon Gold, un job `--ntasks-per-node=80` sature un nœud CPU classique.
- Le réseau InfiniBand EDR justifie l'usage de MPI pour les calculs multi-nœuds : la latence reste faible même en répartissant votre job sur plusieurs nœuds.
- Avec 32 Go de HBM2 par GPU V100, vérifiez que votre modèle (deep learning notamment) tient en mémoire GPU avant de lancer un entraînement long — sinon réduisez la taille de batch.

➡️ Retour au [sommaire](../README.md) ou consultez la [FAQ](07-faq.md)
