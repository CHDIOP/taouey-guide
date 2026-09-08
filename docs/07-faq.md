# 7. Foire aux questions (FAQ)

### Qui peut demander un compte sur Taouey ?

Principalement les chercheurs, enseignants-chercheurs et étudiants encadrés d'institutions publiques, ainsi que des partenaires publics/privés dans le cadre de projets validés par la CINERI. Contactez la CINERI pour connaître les conditions exactes selon votre profil.

### Comment savoir si mon job a échoué et pourquoi ?

```bash
sacct -j <job_id> --format=JobID,State,ExitCode
cat logs/<nom_job>_<job_id>.err
```

Les codes de sortie non nuls (`ExitCode` différent de `0:0`) et le fichier `.err` indiquent généralement la cause de l'échec.

### Mon job reste en attente (`PENDING`) très longtemps, pourquoi ?

Cela peut venir de :
- Ressources demandées trop importantes ou non disponibles actuellement
- Priorité plus basse selon votre quota d'usage ou celui de votre projet
- Partition surchargée

```bash
squeue -u $USER --start   # Estimation du temps de démarrage
scontrol show job <job_id>  # Voir la raison précise (champ "Reason")
```

### Comment installer un logiciel qui n'est pas disponible en module ?

Utilisez généralement :
- Un environnement virtuel (`venv`, `conda`) pour Python
- Une compilation locale dans votre `$HOME` si vous avez les droits
- Un conteneur (Singularity/Apptainer, souvent disponible sur les clusters HPC) si l'outil le permet

Demandez conseil à la CINERI si le logiciel nécessite des droits administrateur.

### Puis-je utiliser Taouey pour un projet privé/commercial ?

Cela dépend des accords en vigueur entre votre organisation et la CINERI. Renseignez-vous directement auprès d'eux — les conditions d'usage diffèrent probablement entre recherche académique et usage commercial.

### Où signaler un bug ou demander de l'aide technique ?

- Pour un problème lié au **cluster lui-même** (compte, accès, panne) : contactez directement la CINERI ([https://cineri.sn](https://cineri.sn))
- Pour une amélioration de **ce guide** : ouvrez une [issue sur ce dépôt](../../issues)

### Ce guide est-il officiel ?

**Non.** Ce dépôt est une initiative communautaire indépendante destinée à faciliter la prise en main de Taouey. Pour toute information officielle et engageante, référez-vous toujours à la CINERI.

---

Une question n'est pas couverte ici ? [Ouvrez une issue](../../issues) pour l'ajouter !
