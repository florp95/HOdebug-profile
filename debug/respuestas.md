=========================================================================================================================
                                                   BUGS 
=========================================================================================================================

----------------------------------------segfault-------------------------------------------------------------------------

*Corriendo el add_array_segfault.e veo que me salta un warning:

warning: implicit declaration of function ‘abs’ [-Wimplicit-function-declaration]
     sum += abs(a[i]);

Pero cuando corro el programa con gdb ./add_array_segfault.e , no me da información sobre lo que está pasando.

* Agregando el flag -g y -O0 (para que no optimice) al archivo Makefile y corriendo ahora nos sale mas información:

Program received signal SIGSEGV, Segmentation fault.
0x00000000004005eb in main (argc=1, argv=0x7fffffffdb88)
    at add_array_segfault.c:19
19	    b[i] = i;


Significa que el programa esta intentando acceder a un lugar de memoria para el cual no tiene permitido el acceso. Revisamos el código y vemos que el problema estaba en que estabamos definiendo en la función un for que se mueve en en una cantidad de lugares mayor a la cantidad de componentes del vector que le estabamos pasando desde el main.

Además otro gran problema que estabamos teniendo es que en la línes 19 :  b[i] = i , le estabamos asignando a b un lugar en el cual ya estaba a ! Estabamos pisando el espacio de a (Esto lo corroboramos viendo los espacios de memoria de a y b). Para solucionar este problema lo solucionamos colocando: 

 a = malloc(sizeof(int) * 3);
 b = malloc(sizeof(int) * 3);    

con lo cual asignamos primero los lugares para los tres enteros que espera recibir el array, sin que se pisen. 

----------------------------------------Dynamic-------------------------------------------------------------------------

*Corriendo el add_array_dynami.e obtenemos un resultado, sin warnings ni errores, pero es cero !!! Esta calculando mal claramente ... 

Cuando debuggeamos volvemos a obtener 0 !!!!!!

Bueno, miramos el código y nos damos cuenta de que de nuevo estabamos pasandole a la funcion un array de tres lugares siendo que el for se mueve por 5 lugares, pero ahora el compilador encontró espacios de memoria que tenian un cero y por lo tanto a esas dos componentes que faltaban las asignó allí.  



=========================================================================================================================
NOTAS PARA LA YO DEL FUTURO: usar make clean para borrar los ejecutables si es que hice una modificación y quiero ejecutar de nuevo. 

Para corroborar los lugares de memoria de a y b lo que hice fue usar dentro del gdb: 

(gdb) p &a
$1 = (int **) 0x7fffffffda90
(gdb) p &b
$2 = (int **) 0x7fffffffda98
(gdb) a[0]
orden  ambigua «a[0]»: .
(gdb) p a[0]
$3 = 0
(gdb) p b[0]
No se puede acceder a la memoria en la dirección 0x0
=========================================================================================================================

=========================================================================================================================


=========================================================================================================================


