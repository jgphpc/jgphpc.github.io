# XALT use cases
## cray-hdf5

| hdf5        | /opt/cray/pe/hdf5/1.10.0/ |  /opt/cray/pe/hdf5/1.8.16/ |
|-------------|---------------------------| ---------------------------|
| cce/8.5.x   | CRAY/8.3                  |  CRAY/8.3                  |
|-------------|---------------------------| ---------------------------|
| gcc/4.9.x   | GNU/4.9                   |  GNU/4.9                   |
| gcc/5.3.x   | GNU/5.1                   |  GNU/5.1                   |
| gcc/6.x     | GNU/5.1                   |  GNU/5.1                   |
|-------------|---------------------------| ---------------------------|
| intel/15.x  | INTEL/15.0                |  INTEL/15.0                |
| intel/16.x  | INTEL/15.0                |  INTEL/15.0                |
| intel/17.x  | INTEL/15.0                |  INTEL/15.0                |
|-------------|---------------------------| ---------------------------|
| pgi/16.4    | PGI/15.3                  |  PGI/15.3                  |
| pgi/16.5    | PGI/15.3                  |  PGI/15.3                  |
| pgi/16.7    | PGI/15.3                  |  PGI/15.3                  |
| pgi/16.9    | PGI/15.3                  |  PGI/15.3                  |
|-------------|---------------------------| ---------------------------|
### just compiling 
* module load daint-gpu cray-hdf5
* SELECT module_name, object_path, date FROM xalt_link, join_link_object,xalt_object WHERE xalt_link.link_id = join_link_object.link_id AND    join_link_object.obj_id = xalt_object.obj_id AND module_name IS NOT NULL AND module_name="cray-hdf5/1.10.0" AND date>="2016-12-11";

```
+------------------+-------------------------------------------------------------+---------------------+
| module_name      | object_path                                                 | date                |
+------------------+-------------------------------------------------------------+---------------------+
| cray-hdf5/1.10.0 | /opt/cray/pe/hdf5/1.10.0/CRAY/8.3/lib/libhdf5_cray.a        | 2016-12-11 16:53:00 |
| cray-hdf5/1.10.0 | /opt/cray/pe/hdf5/1.10.0/PGI/15.3/lib/libhdf5_pgi.a         | 2016-12-11 16:53:43 |
| cray-hdf5/1.10.0 | /opt/cray/pe/hdf5/1.10.0/INTEL/15.0/lib/libhdf5_intel_150.a | 2016-12-11 16:53:55 |
| cray-hdf5/1.10.0 | /opt/cray/pe/hdf5/1.10.0/GNU/5.1/lib/libhdf5_gnu_51.a       | 2016-12-11 16:54:06 |

| cray-hdf5/1.10.0 | /opt/cray/pe/hdf5/1.10.0/GNU/4.9/lib/libhdf5_gnu_49.a       | 2016-12-11 17:39:54 |
```

* SELECT module_name, object_path, date FROM xalt_link, join_link_object,xalt_object WHERE xalt_link.link_id = join_link_object.link_id AND    join_link_object.obj_id = xalt_object.obj_id AND module_name IS NOT NULL AND module_name="cray-hdf5/1.8.16" AND date>="2016-12-11";

```
+------------------+-------------------------------------------------------------+---------------------+
| module_name      | object_path                                                 | date                |
+------------------+-------------------------------------------------------------+---------------------+
| cray-hdf5/1.8.16 | /opt/cray/pe/hdf5/1.8.16/GNU/5.1/lib/libhdf5_gnu_51.a       | 2016-12-11 17:12:31 |
| cray-hdf5/1.8.16 | /opt/cray/pe/hdf5/1.8.16/INTEL/15.0/lib/libhdf5_intel_150.a | 2016-12-11 17:12:38 |
| cray-hdf5/1.8.16 | /opt/cray/pe/hdf5/1.8.16/PGI/15.3/lib/libhdf5_pgi.a         | 2016-12-11 17:12:38 |
| cray-hdf5/1.8.16 | /opt/cray/pe/hdf5/1.8.16/CRAY/8.3/lib/libhdf5_cray.a        | 2016-12-11 17:12:39 |
```



### Unused versions ?
* SELECT DISTINCT SUBSTRING_INDEX(xo.module_name,'/',1) AS Modules, SUBSTRING_INDEX(xo.module_name,'/',-1) as Versions FROM xalt_object xo where SUBSTRING_INDEX(xo.module_name,'/',1)="`cray-hdf5`";

```
+-----------+----------+
| Modules   | Versions |
+-----------+----------+
| cray-hdf5 | 1.10.0   |
| cray-hdf5 | 1.8.16   |
+-----------+----------+
```

* SELECT DISTINCT SUBSTRING_INDEX(xo.module_name,'/',1) as Modules, COUNT(DISTINCT xl.link_id) as Count, COUNT(DISTINCT xl.build_user) as UniqueUser FROM xalt_object xo, join_link_object jlo, xalt_link xl WHERE jlo.link\_id = xl.link_id AND jlo.obj_id = xo.obj_id AND xo.module_name IS NOT NULL AND SUBSTRING_INDEX(xo.module_name,'/',1)="`cray-hdf5`";

```
+-----------+-------+------------+
| Modules   | Count | UniqueUser |
+-----------+-------+------------+
| cray-hdf5 |   259 |          2 |
+-----------+-------+------------+
```

---
# xalt_run

```
+---------------+----------------------
| Field         | Type
+---------------+----------------------
| run_id        | int(11) unsigned
| job_id        | char(64)
| run_uuid      | char(36)
| date          | datetime
| syshost       | varchar(64)
| uuid          | char(36)
| hash_id       | char(40)
| account       | varchar(20)
| exec_type     | char(7)
| start_time    | double
| end_time      | double
| run_time      | double
| num_cores     | int(11) unsigned
| job_num_cores | int(11) unsigned
| num_nodes     | int(11) unsigned
| num_threads   | smallint(6) unsigned
| queue         | varchar(32)
| exit_code     | smallint(6)
| user          | varchar(32)
| exec_path     | varchar(1024)
| module_name   | varchar(64)
| cwd           | varchar(1024)
| cmdline       | blob
```

---
## /opt/cray/pe/hdf5/

### setup
```
ssh dom101
module load daint-gpu
#
module rm xalt/daint-2016.11 
module load xaltjg/dom-2016.11
```

#### cray-hdf5/1.10.0

```
module load cray-hdf5/1.10.0
cc -g h5ex_d_chunk.c -o $PE_ENV.$CRAY_HDF5_VERSION.$HOSTNAME
```

---
# x
xxx

~~~c
void main(){
int i;
i++;
}
~~~

~~~python
foo = (1,2)
print('Hello world!')
~~~

**c**

---
# Most used library ?
* python library_usage.py daint

```
==============================================================
   # Inst.  # Unq.Usr                           Libraries
==============================================================
      5438         13                          cray-mpich
      5268         10                                 gcc
      5186         13                                 rca
      4970         11                         cray-libsci
      2217         10                         cudatoolkit
      ...
         9          1                                 GSL <----

====================================================================
   # Inst.  # Unq.Usr                Libraries (with ver)
====================================================================
      5438         13                    cray-mpich/7.5.0
      5265         10                           gcc/5.3.0
      5186         13 rca/2.0.10_g66b76b7-2.51(craype/2.5.7:craype-network-aries)
      4970         11                 cray-libsci/16.11.1
      2217         10 cudatoolkit/8.0.44_GA_2.2.7_g4a6c213-2.1
      ...
         2          1  GSL/2.1-CrayGNU-2016.11(daint-gpu)
```

---
# Most used modulefile ?
* python software_lastusage.py daint (BUG)

```
==============================================================
                        Module Name            Last Linked    By User
==============================================================
 Amber/.14-CrayGNU-2016.11-parallel                    N/A        N/A
   Amber/.14-CrayGNU-2016.11-serial                    N/A        N/A
...
```

---
# Most used executable ?

* SELECT SUBSTRING_INDEX(xalt_run.exec_path,'/',-1) AS execName, ROUND(SUM(run_time*num_cores/3600)) as totalcput, COUNT(distinct job_id) as n_jobs, num_nodes, COUNT(DISTINCT(user)) as n_users FROM xalt_run GROUP BY execName ORDER BY totalcput DESC, n_jobs DESC, n_users Desc;
    
```    
+------------------------------------------+-----------+--------+-----------+---------+
| execName                                 | totalcput | n_jobs | num_nodes | n_users |
+------------------------------------------+-----------+--------+-----------+---------+
| pyfr                                     |    446795 |     81 |        64 |       1 |
| main_dca                                 |    165871 |      1 |      1800 |       1 |
| python3.5                                |    118266 |     29 |         4 |       3 |
| cp2k.psmp                                |     90136 |    178 |       512 |       3 |
| ls                                       |     10372 |      6 |      4000 |       3 |
| tar                                      |      8807 |      5 |      4000 |       2 |
| xhpcg_xc                                 |      3495 |      1 |      1024 |       1 |
| ...
```

* executable_usage.py (equiv\_pattern)

```
==============================================================
 CPU Time.     # Jobs    # Users Exec
==============================================================
       0.0          1          1 a.out
       0.0          8          1 analysis_DCA+_mpi_test
       0.0          4          1 bilayer_lattice_Nc1_interband
       ...
```


---
# Most used compiler ?

* python compiler_usage.py dom    (BUG)

```
=============================================================
               Link Program            # Inst.
=============================================================
                                gcc       3743
                                g++       1221
                           gfortran        244
                              ifort        123
                          driver.cc         65
                                icc         53
                               icpc         44
                     ftn_driver.exe         30
                               pgcc         20
                              pgf90         18
                              pgc++         16
                          configure          2
                           pgacclnk          1

```



---
# hellolib
* module load hellolib
* ftn -g  -I$EBROOTHELLOLIB/include _main.F90  
-o $PE_ENV.exe -L$EBROOTHELLOLIB/lib -lhello_$PE_ENV
* SELECT module_name, object_path, date FROM xalt_link, join_link_object,    xalt_object WHERE xalt_link.link_id = join_link_object.link_id AND    join_link_object.obj_id = xalt_object.obj_id AND module_name IS NOT NULL AND date>="2016-12-11" AND SUBSTRING_INDEX(module_name,'/',1)="hellolib";

```
+--------------------------------+----------------------------------------------------------------------------------------------------+---------------------+
| module_name                    | object_path                                                                                        | date                |
+--------------------------------+----------------------------------------------------------------------------------------------------+---------------------+
| hellolib/1.0-CrayGNU-2016.11   | /users/piccinal/easybuild/dom/haswell/software/hellolib/1.0-CrayGNU-2016.11/lib/libhello_GNU.a     | 2016-12-11 19:16:24 |
| hellolib/1.0-CrayIntel-2016.11 | /users/piccinal/easybuild/dom/haswell/software/hellolib/1.0-CrayIntel-2016.11/lib/libhello_INTEL.a | 2016-12-11 19:16:42 |
| hellolib/1.0-CrayPGI-2016.11   | /users/piccinal/easybuild/dom/haswell/software/hellolib/1.0-CrayPGI-2016.11/lib/libhello_PGI.a     | 2016-12-11 19:16:54 |
| hellolib/1.0-CrayCCE-2016.11   | /users/piccinal/easybuild/dom/haswell/software/hellolib/1.0-CrayCCE-2016.11/lib/libhello_CRAY.a    | 2016-12-11 19:17:04 |
+--------------------------------+----------------------------------------------------------------------------------------------------+---------------------+
```





