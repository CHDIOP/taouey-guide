# 9. Didacticiels officiels par langage et outil

L'équipe HPC de la CINERI maintient un dépôt séparé, **[Taouey/Docs](https://github.com/Taouey/Docs)**, avec des didacticiels détaillés pour soumettre des jobs SLURM selon votre langage ou outil scientifique. Chaque didacticiel suit le même schéma : un exemple *Hello World* minimal, puis le script SLURM correspondant.

> 💡 Ce guide-ci (`taouey-guide`) couvre les concepts généraux d'utilisation du cluster ; pour une recette prête à l'emploi dans un langage précis, allez directement sur **[Taouey/Docs](https://github.com/Taouey/Docs)**.

## 9.1 Langages et outils de calcul

| Outil | Lien |
|---|---|
| C | [github.com/Taouey/Docs/tree/main/C](https://github.com/Taouey/Docs/tree/main/C) |
| Chapel | [github.com/Taouey/Docs/tree/main/Chapel](https://github.com/Taouey/Docs/tree/main/Chapel) |
| CUDA | [github.com/Taouey/Docs/tree/main/Cuda](https://github.com/Taouey/Docs/tree/main/Cuda) |
| Erlang | [github.com/Taouey/Docs/tree/main/Erlang](https://github.com/Taouey/Docs/tree/main/Erlang) |
| Fortran | [github.com/Taouey/Docs/tree/main/Fortran](https://github.com/Taouey/Docs/tree/main/Fortran) |
| HDF5 | [github.com/Taouey/Docs/blob/main/HDF5/readme.md](https://github.com/Taouey/Docs/blob/main/HDF5/readme.md) |
| MPI | [github.com/Taouey/Docs/tree/main/MPI](https://github.com/Taouey/Docs/tree/main/MPI) |
| Mathematica | [github.com/Taouey/Docs/tree/main/Mathematica](https://github.com/Taouey/Docs/tree/main/Mathematica) |
| MATLAB | [github.com/Taouey/Docs/tree/main/Matlab](https://github.com/Taouey/Docs/tree/main/Matlab) |
| NetCDF | [github.com/Taouey/Docs/tree/main/NetCDF](https://github.com/Taouey/Docs/tree/main/NetCDF) |
| OpenBLAS | [github.com/Taouey/Docs/tree/main/OpenBLAS](https://github.com/Taouey/Docs/tree/main/OpenBLAS) |
| OpenCL | [github.com/Taouey/Docs/tree/main/OpenCL](https://github.com/Taouey/Docs/tree/main/OpenCL) |
| OpenMP | [github.com/Taouey/Docs/tree/main/OpenMP](https://github.com/Taouey/Docs/tree/main/OpenMP) |
| Python | [github.com/Taouey/Docs/tree/main/Python](https://github.com/Taouey/Docs/tree/main/Python) |
| R | [github.com/Taouey/Docs/tree/main/R](https://github.com/Taouey/Docs/tree/main/R) |
| Scala | [github.com/Taouey/Docs/tree/main/Scala](https://github.com/Taouey/Docs/tree/main/Scala) |
| Scilab | [github.com/Taouey/Docs/tree/main/Scilab](https://github.com/Taouey/Docs/tree/main/Scilab) |

## 9.2 Utilitaires et gouvernance des jobs

- **Utilitaires SLURM** (suivi, annulation, historique des jobs) : [github.com/Taouey/Docs/tree/main/SLURM](https://github.com/Taouey/Docs/tree/main/SLURM)

## 9.3 Documentation théorique complémentaire

Pour approfondir les concepts sous-jacents au HPC :

- **Concepts théoriques des systèmes distribués** : [github.com/Taouey/Docs/tree/main/Systèmes%20distribués](https://github.com/Taouey/Docs/tree/main/Syst%C3%A8mes%20distribu%C3%A9s)
- **Programmation parallèle** : [github.com/DiopBabacarEdu/HPC](https://github.com/DiopBabacarEdu/HPC)

## 9.4 Exemple de structure d'un didacticiel officiel (HDF5)

À titre d'illustration, voici comment est structuré le didacticiel HDF5 officiel — le même schéma s'applique aux autres langages :

1. Code source minimal (`hello_hdf5.c`) démontrant la création, l'écriture et la lecture d'un fichier
2. Script SLURM (`hello_hdf5.sh`) chargeant le module correspondant et lançant le calcul :
   ```bash
   #!/bin/bash
   #SBATCH --job-name=hello_world
   #SBATCH --output=hello_world.out
   #SBATCH --error=hello_world.err
   #SBATCH --nodes=1
   #SBATCH --ntasks-per-node=1
   #SBATCH --time=1:00:00

   module purge
   module load hdf5/1.14.3/gcc-4.8.5
   ```
3. Soumission avec `sbatch ./hello_hdf5.sh`

## 9.5 Contact et support

Pour toute question technique non couverte par ces didacticiels : [support@cineri.sn](mailto:support@cineri.sn) ou [www.cineri.sn](https://www.cineri.sn).

➡️ Retour au [sommaire](../README.md)
