===============================FORTRAN=====================================

                            ARCHIVO 1-loop-interch

===========================================================================
===========================================================================

* Corriendo sin optimizar (-O0) se tarda:

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

-Usando un time ./ : 

real	0m1.300s   tiempo desde el comienzo al final de la llamada 
user	0m1.220s   tiempo de CPU que toma ejecutar el proceso
sys	0m0.080s   tiempo de CPU empleado en el kernel dentro del proceso
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

-USANDO GPROF

granularity: each sample hit covers 2 byte(s) for 0.79% of 1.27 seconds

index % time    self  children    called     name
                1.27    0.00       1/1           main [2]
[1]    100.0    1.27    0.00       1         MAIN__ [1]
-----------------------------------------------
                                                 <spontaneous>
[2]    100.0    0.00    1.27                 main [2]
                1.27    0.00       1/1           MAIN__ [1]
-----------------------------------------------
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

-USANDO PERF: 

 Performance counter stats for './1-loop-interch.e':

       1318,750782      task-clock (msec)         #    1,000 CPUs utilized          
                 7      context-switches          #    0,005 K/sec                  
                 0      cpu-migrations            #    0,000 K/sec                  
            73.325      page-faults               #    0,056 M/sec                  
     4.495.089.245      cycles                    #    3,409 GHz                    
     6.662.062.152      instructions              #    1,48  insn per cycle         
       535.719.859      branches                  #  406,233 M/sec                  
           202.637      branch-misses             #    0,04% of all branches        

       1,319032317 seconds time elapsed


============================================================================
============================================================================

* Corriendo con -O1 :

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

-Usando un time ./ :
real	0m1.234s
user	0m1.169s
sys	0m0.064s


++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
-Usando gprof:


granularity: each sample hit covers 2 byte(s) for 0.79% of 1.27 seconds

index % time    self  children    called     name
                                                 <spontaneous>
[1]    100.0    1.27    0.00                 etext [1]
                0.00    0.00       1/1           MAIN__ [2]
-----------------------------------------------
                0.00    0.00       1/1           etext [1]
[2]      0.0    0.00    0.00       1         MAIN__ [2]
-----------------------------------------------
tiempo acumulado: 1.27
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Usando perf:

 Performance counter stats for './1-loop-interch.e':

       1313,820487      task-clock (msec)         #    1,000 CPUs utilized          
                 2      context-switches          #    0,002 K/sec                  
                 0      cpu-migrations            #    0,000 K/sec                  
            73.326      page-faults               #    0,056 M/sec                  
     4.472.578.129      cycles                    #    3,404 GHz                    
     6.661.027.432      instructions              #    1,49  insn per cycle         
       535.154.103      branches                  #  407,327 M/sec                  
           207.127      branch-misses             #    0,04% of all branches        

       1,314118272 seconds time elapsed


=============================================================================
=============================================================================

* Corriendo con -O3:

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
real	0m0.426s
user	0m0.365s
sys	0m0.060s
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
-Usando gprof: 

Each sample counts as 0.01 seconds.
  %   cumulative   self              self     total           
 time   seconds   seconds    calls  ms/call  ms/call  name    
 46.56      0.59     0.59                             etext
 30.77      0.98     0.39        1   390.84   390.84  MAIN__
 17.36      1.20     0.22                             atexit
  3.95      1.25     0.05                             __libc_csu_init
  1.58      1.27     0.02                             __libc_csu_fini

tiempo acumulado: 1.27 


Según gprof para la compilación con -O0, con -O1 y -O3, la corrida demoró el mismo tiempo ! Esto no hayu forma
de que sea cierto... Como bien nos dijeron en clase, el gprof puedo ser muy mentiroso con esto. 


++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Con PERF 

 Performance counter stats for './1-loop-interch.e':

        422,592846      task-clock (msec)         #    0,975 CPUs utilized          
               246      context-switches          #    0,582 K/sec                  
                 0      cpu-migrations            #    0,000 K/sec                  
            73.327      page-faults               #    0,174 M/sec                  
     1.447.963.397      cycles                    #    3,426 GHz                    
       586.893.157      instructions              #    0,41  insn per cycle         
        85.482.605      branches                  #  202,281 M/sec                  
           191.251      branch-misses             #    0,22% of all branches        

       0,433591205 seconds time elapsed


Vemos que PERF si reconoce la diferencia de tiempo que tardan en correr las tres distintas compilaciones y calcula con mucha mayor precisión ! 
==============================================================================
==============================================================================
