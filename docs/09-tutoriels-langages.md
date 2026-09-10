# 9. Exemples de code par langage et outil

Cette page rassemble, pour chaque langage/outil disponible sur Taouey, un exemple minimal *Hello World* et le script SLURM correspondant pour le soumettre. La structure suit celle confirmée officiellement par la CINERI pour HDF5 ([Taouey/Docs](https://github.com/Taouey/Docs)) : code source → script SLURM → `sbatch`.

> ⚠️ **Important** : seul l'exemple HDF5 ci-dessous a été vérifié mot pour mot contre le dépôt officiel [Taouey/Docs](https://github.com/Taouey/Docs) (GitHub bloque le listing automatique des autres dossiers). Les autres exemples sont des versions génériques, écrites dans la même logique et la même convention de nommage de modules (`nom/version/compilateur`), à **confirmer/ajuster** auprès de votre référent CINERI ou en consultant directement le dossier correspondant sur [Taouey/Docs](https://github.com/Taouey/Docs) avant un usage en production.

## Sommaire

- [9.1 C](#91-c)
- [9.2 Fortran](#92-fortran)
- [9.3 MPI](#93-mpi)
- [9.4 OpenMP](#94-openmp)
- [9.5 CUDA](#95-cuda)
- [9.6 OpenCL](#96-opencl)
- [9.7 Python](#97-python)
- [9.8 R](#98-r)
- [9.9 MATLAB](#99-matlab)
- [9.10 Mathematica](#910-mathematica)
- [9.11 Scilab](#911-scilab)
- [9.12 Scala](#912-scala)
- [9.13 Chapel](#913-chapel)
- [9.14 Erlang](#914-erlang)
- [9.15 HDF5 (vérifié officiellement)](#915-hdf5-vérifié-officiellement)
- [9.16 NetCDF](#916-netcdf)
- [9.17 OpenBLAS](#917-openblas)
- [9.18 Utilitaires SLURM](#918-utilitaires-slurm)

---

## 9.1 C

**`hello.c`**
```c
#include <stdio.h>

int main(int argc, char *argv[]) {
    printf("Hello World depuis Taouey !\n");
    return 0;
}
```

**Compilation et script `hello_c.sh`**
```bash
#!/bin/bash
#SBATCH --job-name=hello_c
#SBATCH --output=hello_c.out
#SBATCH --error=hello_c.err
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=00:10:00

module purge
module load gcc/11.2.0

gcc -O2 -o hello hello.c
./hello
```
```bash
sbatch ./hello_c.sh
```

---

## 9.2 Fortran

**`hello.f90`**
```fortran
program hello
    implicit none
    print *, "Hello World depuis Taouey !"
end program hello
```

**Script `hello_fortran.sh`**
```bash
#!/bin/bash
#SBATCH --job-name=hello_fortran
#SBATCH --output=hello_fortran.out
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=00:10:00

module purge
module load gcc/11.2.0

gfortran -O2 -o hello hello.f90
./hello
```

---

## 9.3 MPI

**`hello_mpi.c`**
```c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char *argv[]) {
    int rank, size;
    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);
    printf("Hello depuis le processus %d sur %d (Taouey)\n", rank, size);
    MPI_Finalize();
    return 0;
}
```

**Script `hello_mpi.sh`** (multi-nœuds)
```bash
#!/bin/bash
#SBATCH --job-name=hello_mpi
#SBATCH --output=hello_mpi.out
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=4
#SBATCH --time=00:15:00

module purge
module load openmpi/4.1.4/gcc-11.2.0

mpicc -O2 -o hello_mpi hello_mpi.c
srun ./hello_mpi
```

---

## 9.4 OpenMP

**`hello_omp.c`**
```c
#include <stdio.h>
#include <omp.h>

int main() {
    #pragma omp parallel
    {
        int id = omp_get_thread_num();
        printf("Hello depuis le thread %d (Taouey)\n", id);
    }
    return 0;
}
```

**Script `hello_omp.sh`**
```bash
#!/bin/bash
#SBATCH --job-name=hello_omp
#SBATCH --output=hello_omp.out
#SBATCH --nodes=1
#SBATCH --cpus-per-task=8
#SBATCH --time=00:10:00

module purge
module load gcc/11.2.0

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
gcc -O2 -fopenmp -o hello_omp hello_omp.c
./hello_omp
```

---

## 9.5 CUDA

**`hello_cuda.cu`**
```cuda
#include <stdio.h>

__global__ void helloGPU() {
    printf("Hello depuis le GPU, thread %d (Taouey)\n", threadIdx.x);
}

int main() {
    helloGPU<<<1, 8>>>();
    cudaDeviceSynchronize();
    return 0;
}
```

**Script `hello_cuda.sh`** (partition GPU — V100)
```bash
#!/bin/bash
#SBATCH --job-name=hello_cuda
#SBATCH --output=hello_cuda.out
#SBATCH --partition=gpu
#SBATCH --gres=gpu:1
#SBATCH --nodes=1
#SBATCH --time=00:10:00

module purge
module load cuda/12.2

nvcc -O2 -o hello_cuda hello_cuda.cu
./hello_cuda
```

---

## 9.6 OpenCL

**`hello_ocl.c`** *(extrait — détection de plateforme)*
```c
#include <stdio.h>
#include <CL/cl.h>

int main() {
    cl_uint num_platforms;
    clGetPlatformIDs(0, NULL, &num_platforms);
    printf("Nombre de plateformes OpenCL détectées sur Taouey : %d\n", num_platforms);
    return 0;
}
```

**Script `hello_opencl.sh`**
```bash
#!/bin/bash
#SBATCH --job-name=hello_opencl
#SBATCH --output=hello_opencl.out
#SBATCH --partition=gpu
#SBATCH --gres=gpu:1
#SBATCH --time=00:10:00

module purge
module load opencl/2.2

gcc -O2 -o hello_ocl hello_ocl.c -lOpenCL
./hello_ocl
```

---

## 9.7 Python

**`hello.py`**
```python
import platform

print(f"Hello World depuis Taouey ! (nœud : {platform.node()})")
```

**Script `hello_python.sh`**
```bash
#!/bin/bash
#SBATCH --job-name=hello_python
#SBATCH --output=hello_python.out
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=00:10:00

module purge
module load python/3.11/gcc-11.2.0

python3 hello.py
```

---

## 9.8 R

**`hello.R`**
```r
cat("Hello World depuis Taouey ! (nœud :", Sys.info()["nodename"], ")\n")
```

**Script `hello_r.sh`**
```bash
#!/bin/bash
#SBATCH --job-name=hello_r
#SBATCH --output=hello_r.out
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=00:10:00

module purge
module load r/4.3.1/gcc-11.2.0

Rscript hello.R
```

---

## 9.9 MATLAB

**`hello.m`**
```matlab
fprintf('Hello World depuis Taouey ! (nœud : %s)\n', getenv('HOSTNAME'));
```

**Script `hello_matlab.sh`**
```bash
#!/bin/bash
#SBATCH --job-name=hello_matlab
#SBATCH --output=hello_matlab.out
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=00:10:00

module purge
module load matlab/R2023b

matlab -nodisplay -nosplash -r "run('hello.m'); exit;"
```

---

## 9.10 Mathematica

**`hello.wls`** *(Wolfram Script)*
```mathematica
Print["Hello World depuis Taouey ! (nœud : " <> $MachineName <> ")"]
```

**Script `hello_mathematica.sh`**
```bash
#!/bin/bash
#SBATCH --job-name=hello_mathematica
#SBATCH --output=hello_mathematica.out
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=00:10:00

module purge
module load mathematica/13.3

wolframscript -file hello.wls
```

---

## 9.11 Scilab

**`hello.sce`**
```scilab
printf("Hello World depuis Taouey !\n");
```

**Script `hello_scilab.sh`**
```bash
#!/bin/bash
#SBATCH --job-name=hello_scilab
#SBATCH --output=hello_scilab.out
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=00:10:00

module purge
module load scilab/2023.1.0

scilab-cli -f hello.sce -quit
```

---

## 9.12 Scala

**`Hello.scala`**
```scala
object Hello extends App {
  println(s"Hello World depuis Taouey ! (nœud : ${java.net.InetAddress.getLocalHost.getHostName})")
}
```

**Script `hello_scala.sh`**
```bash
#!/bin/bash
#SBATCH --job-name=hello_scala
#SBATCH --output=hello_scala.out
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=00:10:00

module purge
module load scala/2.13.12
module load java/17

scalac Hello.scala
scala Hello
```

---

## 9.13 Chapel

**`hello.chpl`**
```chapel
writeln("Hello World depuis Taouey !");
```

**Script `hello_chapel.sh`**
```bash
#!/bin/bash
#SBATCH --job-name=hello_chapel
#SBATCH --output=hello_chapel.out
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=00:10:00

module purge
module load chapel/1.32.0

chpl -o hello hello.chpl
./hello
```

---

## 9.14 Erlang

**`hello.erl`**
```erlang
-module(hello).
-export([start/0]).

start() ->
    io:format("Hello World depuis Taouey !~n").
```

**Script `hello_erlang.sh`**
```bash
#!/bin/bash
#SBATCH --job-name=hello_erlang
#SBATCH --output=hello_erlang.out
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=00:10:00

module purge
module load erlang/26.1

erlc hello.erl
erl -noshell -s hello start -s init stop
```

---

## 9.15 HDF5 (vérifié officiellement)

> ✅ Exemple confirmé contre le dépôt officiel [Taouey/Docs/HDF5](https://github.com/Taouey/Docs/blob/main/HDF5/readme.md).

**`hello_hdf5.c`** *(structure confirmée : création, écriture, lecture d'un fichier HDF5)*
```c
#include "hdf5.h"

int main() {
    hid_t file_id, dataset_id, dataspace_id;
    hsize_t dims[1] = {6};
    int data[6] = {1, 2, 3, 4, 5, 6};

    file_id = H5Fcreate("hello.h5", H5F_ACC_TRUNC, H5P_DEFAULT, H5P_DEFAULT);
    dataspace_id = H5Screate_simple(1, dims, NULL);
    dataset_id = H5Dcreate2(file_id, "/dataset", H5T_STD_I32LE, dataspace_id,
                             H5P_DEFAULT, H5P_DEFAULT, H5P_DEFAULT);
    H5Dwrite(dataset_id, H5T_NATIVE_INT, H5S_ALL, H5S_ALL, H5P_DEFAULT, data);

    H5Dclose(dataset_id);
    H5Sclose(dataspace_id);
    H5Fclose(file_id);
    return 0;
}
```

**Script `hello_hdf5.sh`** *(confirmé — convention de module réelle)*
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

h5cc -o hello_hdf5 hello_hdf5.c
./hello_hdf5
```
```bash
sbatch ./hello_hdf5.sh
```

---

## 9.16 NetCDF

**`hello_netcdf.c`** *(création d'un fichier NetCDF minimal)*
```c
#include <netcdf.h>
#include <stdio.h>

int main() {
    int ncid, retval;
    retval = nc_create("hello.nc", NC_CLOBBER, &ncid);
    if (retval) { printf("Erreur NetCDF\n"); return 1; }
    nc_close(ncid);
    printf("Fichier hello.nc créé sur Taouey !\n");
    return 0;
}
```

**Script `hello_netcdf.sh`**
```bash
#!/bin/bash
#SBATCH --job-name=hello_netcdf
#SBATCH --output=hello_netcdf.out
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --time=00:10:00

module purge
module load netcdf/4.9.2/gcc-11.2.0

gcc -o hello_netcdf hello_netcdf.c -lnetcdf
./hello_netcdf
```

---

## 9.17 OpenBLAS

**`hello_blas.c`** *(produit matriciel simple via BLAS)*
```c
#include <stdio.h>
#include <cblas.h>

int main() {
    double A[4] = {1, 2, 3, 4};
    double B[4] = {5, 6, 7, 8};
    double C[4] = {0, 0, 0, 0};

    cblas_dgemm(CblasRowMajor, CblasNoTrans, CblasNoTrans,
                2, 2, 2, 1.0, A, 2, B, 2, 0.0, C, 2);

    printf("Resultat : [%.1f %.1f ; %.1f %.1f]\n", C[0], C[1], C[2], C[3]);
    return 0;
}
```

**Script `hello_openblas.sh`**
```bash
#!/bin/bash
#SBATCH --job-name=hello_openblas
#SBATCH --output=hello_openblas.out
#SBATCH --nodes=1
#SBATCH --cpus-per-task=4
#SBATCH --time=00:10:00

module purge
module load openblas/0.3.24/gcc-11.2.0

gcc -O2 -o hello_blas hello_blas.c -lopenblas
./hello_blas
```

---

## 9.18 Utilitaires SLURM

Commandes de suivi et de gestion des jobs, utiles quel que soit le langage :

```bash
# Soumettre un job
sbatch mon_script.sh

# Voir mes jobs en cours/en attente
squeue -u $USER

# Voir l'historique de mes jobs (terminés, échoués...)
sacct -u $USER --format=JobID,JobName,Partition,State,Elapsed,ExitCode

# Annuler un job
scancel <job_id>

# Détails complets d'un job (raison d'attente, ressources allouées...)
scontrol show job <job_id>

# Lister les partitions et leur état
sinfo
```

---

➡️ Retour au [sommaire](../README.md)

