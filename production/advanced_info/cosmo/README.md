## MeteoSwiss-APN/comm_overlap_bench

* [github.com/barebones](https://github.com/MeteoSwiss-APN/comm_overlap_bench/issues)
   * ba8596fd1e9e5fac9f387c128496f8efb8f48404 (Feb16 2017)

---
### Kesch:
mvapich2_cce/2.0.1.3 
mvapich2_cce/2.0.1.4 
mvapich2_cce/2.0.1_cray83 
mvapich2_cce/2.2b.0.1 
mvapich2gdr_pgi/2.2_cuda_7.5 
mvapich2gdr_pgi/2.2a_cuda_7.0 

mvapich2_gnu/2.0.1.3 
mvapich2_gnu/2.0.1.4 
mvapich2_gnu/2.0.1_gnu48 
mvapich2_gnu/2.2b.0.1 

gmvapich2/15.11 
gmvapich2/15.11_cuda_6.5_gdr 
gmvapich2/15.11_cuda_7.0_gdr 

mvapich2gdr_gnu/2.2a_cuda_7.0 
mvapich2gdr_gnu/2.2_cuda_7.5 

---
### Kesch: test only gnu
~~mvapich2_cce/2.0.1.3~~
~~mvapich2_cce/2.0.1.4~~
~~mvapich2_cce/2.0.1_cray83~~
~~mvapich2_cce/2.2b.0.1~~
~~mvapich2gdr_pgi/2.2_cuda_7.5~~
~~mvapich2gdr_pgi/2.2a_cuda_7.0~~

mvapich2_gnu/2.0.1.3
mvapich2_gnu/2.0.1.4
mvapich2_gnu/2.0.1_gnu48
mvapich2_gnu/2.2b.0.1

gmvapich2/15.11
gmvapich2/15.11_cuda_6.5_gdr
gmvapich2/15.11_cuda_7.0_gdr

mvapich2gdr_gnu/2.2a_cuda_7.0
mvapich2gdr_gnu/2.2_cuda_7.5

---
### Kesch: can't use /opt/cray/mvapich2_gnu
~~mvapich2_cce/2.0.1.3~~
~~mvapich2_cce/2.0.1.4~~
~~mvapich2_cce/2.0.1_cray83~~
~~mvapich2_cce/2.2b.0.1~~
~~mvapich2gdr_pgi/2.2_cuda_7.5~~
~~mvapich2gdr_pgi/2.2a_cuda_7.0~~

~~mvapich2_gnu/2.0.1.3~~
~~mvapich2_gnu/2.0.1.4~~
~~mvapich2_gnu/2.0.1_gnu48~~
~~mvapich2_gnu/2.2b.0.1~~

gmvapich2/15.11
gmvapich2/15.11_cuda_6.5_gdr
gmvapich2/15.11_cuda_7.0_gdr

mvapich2gdr_gnu/2.2a_cuda_7.0

mvapich2gdr_gnu/2.2_cuda_7.5

---
### Kesch: can't use gmvapich2/15.11
~~mvapich2_cce/2.0.1.3~~
~~mvapich2_cce/2.0.1.4~~
~~mvapich2_cce/2.0.1_cray83~~
~~mvapich2_cce/2.2b.0.1~~
~~mvapich2gdr_pgi/2.2_cuda_7.5~~
~~mvapich2gdr_pgi/2.2a_cuda_7.0~~

~~mvapich2_gnu/2.0.1.3~~
~~mvapich2_gnu/2.0.1.4~~
~~mvapich2_gnu/2.0.1_gnu48~~
~~mvapich2_gnu/2.2b.0.1~~

~~gmvapich2/15.11~~
~~gmvapich2/15.11_cuda_6.5_gdr~~
~~gmvapich2/15.11_cuda_7.0_gdr~~

mvapich2gdr_gnu/2.2a_cuda_7.0 (/opt/mvapich2 = OSU)
mvapich2gdr_gnu/2.2_cuda_7.5  (/opt/mvapich2 = OSU)

---
### Kesch: Setup

* /opt/mvapich2 (OSU/CSCS)

```
module purge 
module load craype-haswell craype-network-infiniband
mvapich2gdr_gnu/2.2_cuda_7.5 or mvapich2gdr_gnu/2.2a_cuda_7.0
    cudatoolkit/7.5.18       or cudatoolkit/7.0.28
    GCC/4.9.3-binutils-2.25
+ Boost/1.49.0-gmvolf-15.11-Python-2.7.10
```

---
### Kesch: Compile
* `.cpp`: -DENABLE_CUDA_STREAMS -DMVAPICH2 -g -w -fexceptions -fstack-protector -std=gnu++11
* ` .cu`: -arch=sm_37 -std=c++11 -DMVAPICH2 -DENABLE_CUDA_STREAMS -DNVCC

===> [-lmpi linking issue](https://github.com/MeteoSwiss-APN/comm_overlap_bench/issues/3)

---
### Kesch: Run

> export G2G=2 

> export CUDA_AUTOBOOST=1 

> export GCLOCK=875

> srun -p debug --gres=gpu:2 -n2 --ntasks-per-node=2 ./exe

```
Domain : [128,128,60]
Sync? : 0
NoComm? : 0
NoComp? : 0
Number Halo Exchanges : 2
Number benchmark repetitions : 1000
In Order halo exchanges? : 0
Device ID :0
SLURM_PROCID :0
Compiled for mvapich2

ELAPSED TIME : 7.55524 +- + 0.000499567 = cuda/7.5 (slower)
ELAPSED TIME : 3.13486 +- + 0.00271736  = cuda/7.0
```
===> `mvapich2gdr_gnu/2.2_cuda_7.5` slower than `mvapich2gdr_gnu/2.2a_cuda_7.0`

---
### Kesch: Score-P
* module load `Score-P/3.0-gmvapich2-17.02_cuda_7.5_gdr`
* make PREP='scorep --cuda --mpp=mpi --keep-files'

===> srun segfaults in `scorep_finalize / SCOREP_SynchronizeClocks`
```
../src/measurement/paradigm/mpi/scorep_ipc_mpi.c:179: 
SCOREP_IpcGroup_GetSize:
Assertion SCOREP_Status_IsMppInitialized() failed.
[keschcn-0011:mpi_rank_1][error_sighandler] 
Caught error: Aborted (signal 6)
```

---
### Kesch: Summary
* Compile: ok with a hack
* Run: ok but slow
* Score-P: no report generated
* Git

---
### Daint:

> export MPICH_RDMA_ENABLED_CUDA=1

> srun -C gpu -p debug -N2 -n2 --ntasks-per-node=1 -t5 ./GNU5.3.0-CUDAV8.0.53-Stencils

```
ELAPSED TIME : 6.54026 +- + 0.000297186
*** Error in GNU5.3.0-CUDAV8.0.53-Stencils: free(): 
    invalid next size (fast): 0x0000000003061130 ***
```


 
---

<img src="img/0.png" alt="The end." >
