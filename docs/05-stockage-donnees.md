# 5. Gestion des données et du stockage

## 5.1 Stockage de Taouey en chiffres

| Caractéristique | Valeur |
|---|---|
| Capacité totale du cluster | **1,1 Pétaoctet (Po)** |
| Protocole | **NFS** — accès concurrent depuis tous les nœuds |
| Espace NFS dédié | **10 To** |
| Quota individuel par utilisateur/projet | Attribué à l'étape "Notification" de votre demande d'accès (voir [chapitre 2](02-connexion.md)) — variable selon votre projet |

> ⚠️ Le détail des chemins exacts (`$HOME`, espace projet, éventuel scratch temporaire) et des politiques de rétention/purge n'est pas encore documenté publiquement — demandez ces précisions à votre référent CINERI lors de l'attribution de votre quota.

## 5.2 Types d'espaces de stockage habituels sur un cluster HPC (à titre de repère général)

| Espace | Usage | Caractéristiques typiques |
|---|---|---|
| `$HOME` | Scripts, code, petits fichiers de configuration | Quota limité, sauvegardé |
| `$SCRATCH` | Données temporaires de calcul, gros volumes | Grand quota, **non sauvegardé**, souvent purgé après X jours |
| `$PROJECT` (ou espace projet partagé) | Données de recherche partagées avec votre équipe | Quota par projet, accès partagé |

## 5.2 Bonnes pratiques

- **Ne stockez jamais de données définitives uniquement sur `$SCRATCH`** : cet espace est généralement purgé automatiquement.
- Compressez vos gros fichiers/résultats avant de les transférer ou archiver (`tar`, `gzip`).
- Nettoyez régulièrement vos fichiers temporaires (`squeue` logs, checkpoints obsolètes).
- Vérifiez votre quota régulièrement.

```bash
# Vérifier l'espace utilisé dans votre home
du -sh ~

# Vérifier le quota (commande à confirmer avec la CINERI)
quota -s
```

## 5.3 Transférer des données

### Depuis votre machine locale vers Taouey

```bash
scp mon_fichier.csv <identifiant>@<adresse_taouey>:~/mon_projet/
```

### Transférer un dossier complet

```bash
scp -r ./mon_dossier <identifiant>@<adresse_taouey>:~/mon_projet/
```

### Pour de gros volumes de données, préférez `rsync`

```bash
rsync -avzP ./mes_donnees/ <identifiant>@<adresse_taouey>:~/mon_projet/donnees/
```

L'option `-P` permet de reprendre un transfert interrompu, ce qui est précieux avec de gros fichiers scientifiques.

## 5.4 Confidentialité et souveraineté des données

L'un des objectifs affichés de Taouey est de renforcer la **souveraineté numérique** du Sénégal. En tant qu'utilisateur :
- Respectez les règles de confidentialité de votre institution et de vos partenaires de recherche
- Ne transférez pas de données sensibles ou personnelles vers des services cloud externes sans autorisation
- Signalez toute fuite ou incident de sécurité à la CINERI

➡️ Passez au chapitre suivant : [Bonnes pratiques et étiquette du cluster](06-bonnes-pratiques.md)
