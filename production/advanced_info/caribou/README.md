# CRAY SWMG
## CARIBOU/SONEXION @ CSCS demo
### Alpha release (03/2017)

> .

> .

> .

> .

> Matteo Chesi / Jg Piccinali

---
## TDS: Cray XC (dom)
>  Lustre layout 
Lustre implements a separation of data and metadata:
* The metadata is stored on a Metadata Target (`MDT`)
* The data is stored on a number of Object Storage Targets (`OSTs`)
*  A Metadata Server (`MDS`) serves all file system requests for metadata, 
and it looks after the MDT
* A number of Object Storage Servers (`OSS`) 
each look after several OSTs and serve requests for data on those OSTs.

![dom1](img/dom_df.png)

<img src="img/dom_osts.png" alt="ddt_daint" width="40%">

![dom3](img/dom_lfs_df.png)

---
## TDS: Cray XC (dom)
> Lustre striping 
Lustre allows the user to have explicit control over how a file is striped
over the OSTs: chunks are sent to the different OSTs to improve disk bandwidth.

* export `MPICH_MPIIO_STATS`=1
* srun -n192 ./GNU.DOM

    <img src="img/slurm_files_written.png" width="45%">

* lfs setstripe `-c 1` and lfs setstripe `-c 2`

    <img src="img/slurm_stripe1.png" width="45%">
    <img src="img/slurm_stripe2.png" width="45%">

---
## [Cray Caribou](https://badile01.cscs.ch)
![caribou1](img/caribou_welcome_page.png)
<img src="img/caribou_jobs.png" width="70%">

---
## kibana
![kibana1](img/kibana.png)

---
## [grafana:job](http://badile01.cscs.ch:3000)

<img src="img/grafana_osts_1job.png" width="80%">
<img src="img/grafana_ost0_1job.png" width="45%">
<img src="img/grafana_ost1_1job.png" width="45%">

---
## [grafana:ost](http://badile01.cscs.ch:3000)
<img src="img/grafana_osts_all.png" width="90%">
<img src="img/grafana_ost0_all.png" width="45%">
<img src="img/grafana_ost1_all.png" width="45%">

---
## [grafana:striping on 2 osts](http://badile01.cscs.ch:3000)
<img src="img/grafana_osts_all_stripe2.png" width="90%">
<img src="img/grafana_osts_all_stripe2_ost0.png" width="40%">
<img src="img/grafana_osts_all_stripe2_ost1.png" width="40%">

---
## CUG'17
* Tuesday May 9th / 4:40pm

`HPC Storage Operations BOF`
> Matteo Chesi (CSCS), Tina Declerck (NERSC), Oliver Treiber (ECMWF)

* Thursday May 11th / 10:30am

`Caribou : Monitoring and Metrics for Cray Sonexion Storage`
> Patricia Langer, Craig Flaskerud (Cray)

