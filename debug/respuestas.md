# Debug

## Floating point exception

En la carpeta `fpe/` hay tres códigos de C, independientes, para compilar. 
Cada uno de estos códigos genera un ejecutable. Hay además una carpeta que
define la función `set_fpe_x87_sse_`. Una vez compilados los tres ejecutables
sin la opción `-DTRAPFPE`, responder las siguientes preguntas:

- ¿Qué función requiere agregar `-DTRAPFPE`? ¿Cómo pueden hacer que el 
programa *linkee* adecuadamente?

Para compilar los archivos **.c** agregamos el flag `-lm` para linkear a la libreria *m*. Ademas, para utilizar la funcion *set_fpe_x87_sse_* necesitamos agregar el flag `-DTRAPFPE`.
Para compilar los archivos **.c** con el flag `-DTRAPFPE` generamos los objetos respectivos y luego compilamos linkeando el objeto de la funcion *set_fpe_x87_sse_* con el objeto *test_fpe_dtrap*.

- Para cada uno de los ejecutables, ¿qué hace agregar la opción `-DTRAPFPE` al 
compilar? ¿En qué se diferencian los mensajes de salida con y sin esa opción?

Agregar el flag hace que se ejecute la funcion *set_fpe_x87_sse_* entonces se tienen en cuenta las excepciones de punto flotante y por lo tanto compilar utilizando o no este flag genera diferentes resultados. Por ejemplo:

```
$ ./test_fpe1.e
Insert a, b 
2, 8
c = inf 
$ ./test_fpe1_dtrap.e 
Insert a, b 
2, 8
Floating point exception (core dumped)
```

## Segmentation Fault

En la carpeta `sigsegv/` hay códigos de C y de FORTRAN. Elijan alguno
y compilen y corren el programa de acuerdo a los comentarios en el código,
para obtener los ejecutables `small.x` y `big.x`.
Identifiquen los errores que devuelven (¡si devuelven alguno!) los ejecutables.

- Al correr el ejecutable `small.x` no devuelve ningun error mientras que `big.x` genera un error de tipo *Segmentation fault(core dump)*. Para generar el archivo **core** y poder analizarlo con gdb necesitamos utilizar el comando `ulimit -c unlimited` y volver a correr el ejecutable.

- Al analizarlo con gdb (`$gdb ./big.x core`) obtenemos:
```
Program terminated with signal SIGSEGV, Segmentation fault.
#0  0x0000000000400641 in mat_Tmat_mul (
    A=<error reading variable: Cannot access memory at address 0x7ffccf7dcf88>, C=<error reading variable: Cannot access memory at address 0x7ffccf7dcf80>)
    at source.c:36
36	void mat_Tmat_mul( float * A, float * C ){
```

La explicacion al posible error esta en la pagina <https://stackoverflow.com/questions/14471564/what-does-ulimit-s-unlimited-do> y tiene que ver con overflowing the stack.

Ahora ejecuten `ulimit -s unlimited` en la terminal y vuelvan a correrlo. Luego
responder las siguientes preguntas:

- ¿Devuelven el mismo error que antes?

Despues de utilizar dicho comando el ejecutable `big.x` no genera error pero si tarda mucho tiempo en correr.

- Averigüen qué hicieron al ejecutar la sentencia `ulimit -s unlimited`. Algunas pistas
son: abran otra terminal distinta y fíjense si vuelve al mismo error, fíjense la diferencia
entre `ulimit -a` antes y después de ejecutar `ulimit -s unlimited`, googleen, etcétera.

Al utilizar el comando `ulimit -s unlimited` estamos modificando el limite de espacio en el *stack* que puede usar el programa. Esto lo podemos ver con el comando `ulimit -a` antes y despues de la modificacion o directamente con el comando `ulimit -s`:

```
$ ulimit -s
8192

$ ulimit -s
unlimited
```

- La "solución" anterior, ¿es una solución en el sentido de debugging?

Esta "solucion" no resuelve el problema porque solo eliminamos el sintoma temporalmente, ya que si cambiamos de terminal y corrremos nuevamente el ejecutable entonces el error reaparece. Por lo tanto, la solucion hallada no es tal en el sentido de debugging.

## Valgrind

En la carpeta `valgrind/` hay ejemplos en C y FORTRAN que se pueden ejecutar
con valgrind. Describan el error y por qué sucede

## Funny

En la carpeta `funny/` hay un código de C. Describan las diferencias de los ejecutables
al compilar con y sin el flag `-DDEBUG`. ¿De dónde vienen esas diferencias?

