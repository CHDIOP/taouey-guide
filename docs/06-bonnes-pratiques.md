# 6. Bonnes pratiques et étiquette du cluster

Un supercalculateur est une **ressource partagée** entre de nombreux chercheurs, institutions et startups. Voici les règles de bon usage universellement admises sur les clusters HPC.

## 6.1 À faire

✅ Toujours estimer la durée et les ressources nécessaires **avant** de soumettre un job, pour éviter de bloquer inutilement des ressources.

✅ Tester votre code sur un petit jeu de données ou en mode interactif avant de lancer un job de plusieurs heures.

✅ Demander uniquement les ressources dont vous avez réellement besoin (CPU, mémoire, GPU) — sur-demander pénalise les autres utilisateurs et peut aussi retarder le démarrage de votre propre job.

✅ Nommer vos jobs et organiser vos logs pour vous y retrouver facilement.

✅ Utiliser des **checkpoints** pour les longs calculs (sauvegarder l'état régulièrement, pour reprendre en cas d'interruption).

## 6.2 À éviter

❌ **Ne jamais lancer de calculs lourds directement sur le nœud de connexion** (login node). Ce nœud est partagé par tous pour naviguer, éditer, compiler — pas pour calculer. Utilisez toujours `sbatch` ou `srun`.

❌ Ne pas soumettre des dizaines de jobs quasi-identiques par erreur (vérifiez vos scripts avant de les boucler).

❌ Ne pas stocker indéfiniment des données volumineuses inutilisées sur les espaces partagés.

❌ Ne pas partager ses identifiants de connexion.

## 6.3 Optimiser ses calculs

- Profilez votre code avant d'optimiser (identifiez les vrais goulots d'étranglement)
- Privilégiez les bibliothèques optimisées et parallélisées disponibles via les modules plutôt que réinventer des implémentations moins efficaces
- Pour l'IA/ML, vérifiez l'utilisation réelle du GPU pendant l'entraînement (`nvidia-smi`) pour ne pas gaspiller de ressources

```bash
# Surveiller l'utilisation GPU en temps réel (dans un job interactif)
watch -n 1 nvidia-smi
```

## 6.4 Citer Taouey dans vos publications

Si votre recherche a bénéficié des ressources de calcul de Taouey, pensez à le mentionner dans vos remerciements/acknowledgments — cela aide à démontrer l'impact de l'infrastructure et à justifier son développement futur. Consultez la CINERI pour la formule de citation recommandée.

➡️ Passez au chapitre suivant : [FAQ](07-faq.md)
