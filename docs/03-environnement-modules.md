# 3. Gérer son environnement avec les modules

Les clusters HPC utilisent un système de **modules** pour éviter d'installer manuellement chaque logiciel : vous chargez à la demande la version dont vous avez besoin, sans conflit avec les autres utilisateurs.

> ✅ **Convention de nommage confirmée sur Taouey** : les modules suivent le format `nom/version/compilateur`, par exemple `hdf5/1.14.3/gcc-4.8.5` (voir le [didacticiel HDF5 officiel](https://github.com/Taouey/Docs/blob/main/HDF5/readme.md)). Gardez ce format en tête lors de vos recherches avec `module avail`.

## 3.1 Commandes essentielles (Environment Modules / Lmod)

```bash
# Lister tous les modules disponibles
module avail

# Rechercher un logiciel précis
module avail python

# Charger un module
module load python/3.11

# Voir les modules actuellement chargés
module list

# Décharger un module
module unload python/3.11

# Décharger tous les modules
module purge

# Afficher des informations sur un module
module show python/3.11
```

## 3.2 Exemple : préparer un environnement Python pour du Machine Learning

```bash
module purge
module load python/3.11/gcc-11.2.0   # format nom/version/compilateur, confirmé sur Taouey
module load cuda/12.2   # si vous utilisez des GPU

# Créer un environnement virtuel isolé
python -m venv ~/envs/mon_projet_ia
source ~/envs/mon_projet_ia/bin/activate

pip install --upgrade pip
pip install torch numpy pandas scikit-learn
```

## 3.3 Automatiser le chargement de vos modules

Ajoutez vos modules fréquemment utilisés dans un script que vous sourcez au début de chaque session :

```bash
# fichier : ~/mon_env.sh
module purge
module load python/3.11
module load cuda/12.2
source ~/envs/mon_projet_ia/bin/activate
```

```bash
source ~/mon_env.sh
```

> 💡 **Astuce** : évitez de charger des modules automatiquement dans votre `.bashrc` sur les nœuds de connexion — cela peut ralentir la connexion ou entrer en conflit avec les scripts de jobs. Préférez charger les modules explicitement dans vos scripts SLURM.

## 3.4 Logiciels couramment disponibles sur un cluster HPC

À titre indicatif (à vérifier avec `module avail` sur Taouey) :
- Langages : Python, R, Julia, C/C++/Fortran (GCC, Intel compilers)
- Calcul parallèle : MPI (OpenMPI, MPICH), OpenMP
- IA/ML : PyTorch, TensorFlow, CUDA/cuDNN
- Calcul scientifique : MATLAB, NumPy, SciPy
- Bioinformatique : BLAST, Bowtie, selon les domaines soutenus

➡️ Passez au chapitre suivant : [Soumettre des jobs avec SLURM](04-soumission-jobs-slurm.md)
