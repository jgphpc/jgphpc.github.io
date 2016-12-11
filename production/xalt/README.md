# XALT use cases
## cray-hdf5
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














