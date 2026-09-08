# 4. Soumettre des jobs avec SLURM

La plupart des supercalculateurs modernes, dont vraisemblablement Taouey, utilisent **SLURM** (Simple Linux Utility for Resource Management) comme ordonnanceur de jobs.

> ⚠️ Confirmez auprès de la CINERI les noms exacts des **partitions** disponibles (ex: `cpu`, `gpu`, `bigmem`) et les limites (durée max, nombre de cœurs par utilisateur), qui varient selon la configuration réelle de Taouey.

## 4.1 Anatomie d'un script SLURM

Créez un fichier, par exemple `mon_job.slurm` :

```bash
#!/bin/bash
#SBATCH --job-name=mon_calcul          # Nom du job
#SBATCH --output=logs/%x_%j.out        # Fichier de sortie (%x=nom, %j=id job)
#SBATCH --error=logs/%x_%j.err         # Fichier d'erreurs
#SBATCH --partition=cpu                # Partition à utiliser (à adapter)
#SBATCH --nodes=1                      # Nombre de nœuds
#SBATCH --ntasks=1                     # Nombre de tâches
#SBATCH --cpus-per-task=8              # Nombre de cœurs par tâche
#SBATCH --mem=32G                      # Mémoire demandée
#SBATCH --time=02:00:00                # Durée max (HH:MM:SS)

# Préparer l'environnement
module purge
module load python/3.11
source ~/envs/mon_projet/bin/activate

# Lancer le calcul
python mon_script.py
```

## 4.2 Soumettre et suivre son job

```bash
# Créer le dossier de logs
mkdir -p logs

# Soumettre le job
sbatch mon_job.slurm

# Voir l'état de vos jobs
squeue -u $USER

# Voir les détails d'un job précis
scontrol show job <job_id>

# Annuler un job
scancel <job_id>

# Voir l'historique de vos jobs terminés
sacct -u $USER --format=JobID,JobName,Elapsed,State,ExitCode
```

## 4.3 Exemple : job avec GPU

```bash
#!/bin/bash
#SBATCH --job-name=entrainement_ia
#SBATCH --output=logs/%x_%j.out
#SBATCH --partition=gpu
#SBATCH --gres=gpu:1                   # 1 GPU demandé
#SBATCH --cpus-per-task=4
#SBATCH --mem=64G
#SBATCH --time=12:00:00

module purge
module load python/3.11 cuda/12.2
source ~/envs/mon_projet_ia/bin/activate

python entrainement.py --epochs 100
```

## 4.4 Exemple : job parallèle multi-nœuds avec MPI

```bash
#!/bin/bash
#SBATCH --job-name=simulation_mpi
#SBATCH --output=logs/%x_%j.out
#SBATCH --partition=cpu
#SBATCH --nodes=4
#SBATCH --ntasks-per-node=32
#SBATCH --time=06:00:00

module purge
module load openmpi/4.1

srun ./mon_programme_mpi
```

## 4.5 Job interactif (pour tester rapidement)

```bash
srun --partition=cpu --cpus-per-task=4 --mem=8G --time=00:30:00 --pty bash
```

Cela ouvre un shell directement sur un nœud de calcul, pratique pour déboguer avant de lancer un vrai job.

➡️ Passez au chapitre suivant : [Gestion des données et du stockage](05-stockage-donnees.md)
