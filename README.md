# Prolog

Sintaxis básica de Prolog.

## Tabla de contenido

- [Conceptos básicos](#conceptos-basicos).
 - [Prolog](#prolog).
 - [Predicados](#predicados).
 - [Hechos](#hechos).
 - [Reglas](#reglas).
 - [Variables](#variables).
 - [Operadores aritméticos](#operadores-aritmeticos).
 - [Operadores relacionales](#operadores-relacionales).
 - [Operadores de igualdad](#operadores-de-igualdad).
 - [Comentarios](#comentarios).
- [Recursividad](#recursividad).
- [Listas](#listas).
- [Ejemplos](#ejemplos).
 - [Problema 1](#problema-1).

## Conceptos basicos

### Prolog

Es un lenguaje de programación declarativo, lo que significa que, se centra en la lógica del algoritmo mas que en el control de la secuencia (imperativo). Ejemplo de lenguajes declarativos: SQL, Prolog, Html, Xml, etc...

Prolog bastante conocido en el área de la Ingeniería Informática para investigación en la IA.

Su paradigma es lógico, lo que conlleva a construir una base de conocimiento (un archivo plano con extensión .pl o .pr) compuesto por *predicados*, *hechos* y *reglas*, la cual será consultada y el sistema sólo responderá con `true`, `false` o un valor.

Para el desarrollo de una base de conocimiento y de consultas, se puede utilizar el IDEs [SWI-Prolog](http://www.swi-prolog.org/),J-prolog (java), Win-prlog, SWI-prolog, Eclipse.

### Predicados

Un predicado es cuando se afirma algo del sujeto o hablan del sujeto, por ejemplo: "Juan es alto". En la base de conocimiento de prolog, un predicado se denota así:

```plain

alto(juan). % José es alto.

```

Para Prolog, esto también sería un hecho, debido que afirma que Juan es alto si o si, lo que significa que Prolog supone que todos los hechos declarados en la base de conocimientos son verdaderos.

Si quiero consultar la base, sería:

```plain

?- alto(juan).
true

```

### Hechos

En Prolog, los hechos son predicados que prolog va asumir que son verdaderos. Ejemplo:

```plain

padreDe(juan, maria). % juan es padre de maria.

```

### Reglas

Una regla se define para confirmar si un hecho es o no es lo que dice afirmar. Las reglas está compuestas de una cabeza (que es un hecho) y un cuerpo. Dicho cuerpo está compuesto por otras series de hechos para poder confirmar si la cabeza declarada es verdadero o falso.

Sintaxis:

```plain

cabeza(argumento1, ..., argumentoN) :- otrosHechos(argumento1, ..., argumentoN).

```

Ejemplo:

```plain

padre(juan, maria) :- hija(maria, juan). % Juan es padre de Maria, si Maria es hija de Juan.

```

El simbolo `:-` se lee como **si**.

El cuerpo de las reglas pueden estar compuestas de operadores lógicos como el *AND*, que se representa con `,` y *OR*, que se representa con el `;`. Ejemplo:


```plain

abuelo(juan, pepe) :- hijo(pepe, pedro), padre(juan, pedro).
% juan es abuelo de pepe, si pepe es hijo de pedro y juan es padre de pedro.

```


### Variables

Se denotan empezando con la letra mayúscula y sirven para tomar un valor y mostrarlo en la consulta.

```plain

?- alto(X). 
X = Juan

```
NOTA: Con `;` pregunto de nuevo a la base de conocimiento, si existe otra afirmación. Ejemplo:

```plain

?- alto(X). 
X = Juan;
X = Maria;
X = Pepe;
false

```

También, se pueden enviar constantes los cuales ya deben estar definidos en los hechos de la base. Ejemplo:

```plain

?- alto(juan). 
true

```

### Operadores aritmeticos

| Operador | Función |
| ----- | ---- |
| + | Suma |
| - | Resta |
| * | Multiplicación |
| / | División |
| mod | Módulo de división |

Estas operaciones sólo se realizan por medio del operador `is`. Ejemplo:

```plain

% Reglas:
% La variable Valor retorna el resultado de la operación.

suma(X, Y, Valor) :- Valor is X + Y.
resta(X, Y, Valor) :- Valor is X - Y.
multi(X, Y, Valor) :- Valor is X * Y.
divi(X, Y, Valor) :- Valor is X / Y.
es_par(X) :- 0 is X mod 2. % retorna true o false si X es par o no.

```

### Operadores relacionales

| Función | Significado |
| ----- | ---- |
| < | Menor que |
| > | Mayor que |
| <= | Menor o igual que |
| >= | Mayor o igual que |


### Operadores de igualdad

| Función | Significado |
| ----- | ---- |
| = | Unificación: Es verdadero si ambos operandos se unifican. |
| /= | No unificación: Es verdadero si ambos operandos no unifican. |
| is | Evaluador: Se utiliza para evaluar las expresiones aritméticas y funciones. Evalúa la parte de la derecha y unifica a la parte izquierda |
| == | Es exactamente igual. No unifican. |
| \== | Es falso, cuando dos términos son exactamente iguales. No unifican. |
| =:= | Mismo valor. Evalúa los dos operandos, a derecha y a izquierda, y es verdadero si los valores obtenidos son iguales. No unifican. |

Ejemplo función `=` unificación.

```plain

?- X + Y = 3 + 5.
X = 3, 
Y = 5.

?- X = 3 + 5.
X = 3 + 5.

```

Ejemplo función `=:=` mismo valor.

```plain

?- 3*3 =:= 9.
True

```

### Comentarios

Podemos realizar comentarios en la base de conocimientos. Para comentarios con múltiples lineas, los encerramos con `/*  */` y para comentarios de una sóla linea, anteponemos el `%`.


## recursividad

En el paradigma de programación declarativa, no se puede realizar iteraciones. Por lo tanto, se utiliza el método de la recursión, el cual, consiste el llamado de la misma regla dentro de su cuerpo.

Ejemplo: Recursividad de un número factorial.

En la base de conocimiento:

```plain

factorial(0, 1).
factorial(X, Y) :- 	X > 0,
					          Xmenos1 is X - 1,
						   		  factorial(Xmenos1, Z),
						   		  Y is X * Z.

```

Una consulta de ejemplo, sería: `factorial(5, N).`

El caso base, sería: `factorial(0, 1).` y en realidad no es una regla sino un *hecho*, de que el factorial de 0 es 1. La siguiente linea si consiste en una regla que define el caso general de la recursividad para un factorial, sólo que tiene más pasos de lo acostumbrado a como se hace en POO:

```java

if(n == 0){
	return 1; // -> caso base
} else {
	return n * factorial(n-1). // -> caso general.
}

```

Como X debe ser más pequeño en el próximo llamado, en prolog no podemos hacer lo siguiente: factorial(X-1, Z), sino que toca hacer los calculos antes: `Xmenos1 is X - 1` y al final hacer la multiplicación por separado: `Y is X * Z`.

## Listas

Las listas son colecciones de elementos en Prolog, cerrados en `[]`. Una lista se divide en dos partes: Cabeza, que es el primer elemento de la lista y cola, que es una lista con el resto de los elementos de la lista. La cabeza y la cola de una lista se separan con el símbolo "|".

Sintaxis:

`lista([cabeza | cola]).`

Ejemplo:

`lista([1,2,3]). % lista creada.`

Consulta:

```plain

?- lista([Cabeza | Cola]).
Cabeza = 1,
Cola = [2, 3, 4, 5].

```

## Ejercicios

### Problema 1

Dado el siguiente ![documento](images/problema-1.pdf), cree las reglas para las consultas solicitadas.

Solución. Desarrollo de la base de conocimiento:

```prolog

vuelo(cali,	medellin,	hk_4817,	lunes,		[2, 3, 5, 4]).
vuelo(medellin,	bogota,		hk_4812,	lunes,		[5]).
vuelo(bogota,	cartagena,	hk_4817,	miercoles,	[5, 6, 3, 7, 4]).
vuelo(cali,	pasto,		hk_4093,	sabado,		[6, 8, 2]).
vuelo(bogota,	santamarta,	hk_3212,	jueves,		[4, 8, 9, 1, 2]).
vuelo(pasto,	leticia,	hk_3212,	viernes,	[2, 3, 6]).
vuelo(cartagena, sanandres,	hk_4812,	domingo,	[1, 4]).
vuelo(medellin,	leticia,	hk_4093,	martes,		[3, 2, 9, 5]).
vuelo(bogota,	sanandres,	hk_3212,	viernes,	[7, 4]).

ciudad(cali,		1536,	calido,		1000).
ciudad(medellin,	1675,	calido,		1500).
ciudad(bogota,		1539,	templado,	2630).
ciudad(pasto,		1539,	templado,	2527).
ciudad(cartagena,	1533,	calido,		10).
ciudad(santamarta,	1525,	tropical,	5775).
ciudad(leticia,		1867,	calido,		100).

avion(hk_4817,	180,	a320).
avion(hk_4812,	160,	a330-200).
avion(hk_3212,	120,	a330-200).
avion(hk_4093,	200,	a320).


/*
*** Consultas sin reglas ***
Punto 1: vuelo(cali, Destino, _, _, _).
Punto 2: ciudad(Ciudad, _, calido, Altitud).
Punto 3: vuelo(Origen, Destino, _, viernes, Pasaje).
Punto 4: vuelo(bogota, Destino, _, _,_), ciudad(Destino, _, Clima, _).
Punto 5: vuelo(bogota, Destino, _, jueves, _), ciudad(Destino, _, Clima, _).
Punto 6: vuelo(Origen, Destino, Matricula, lunes, _), ciudad(Destino, _, _, Altitud), avion(Matricula, NumAsientos, _).
*/

% Reglas

% Obtener los destinos a los cuales se viajó desde cali.
obtenerPuntoUno(Origen, Destino) :- vuelo(Origen, Destino, _, _,_). 


% Obtener las ciudades y altitud de las ciudades que son de clima calido.
obtenerPuntoDos(Clima, Ciudad, Altitud) :- ciudad(Ciudad, _, Clima, Altitud). 


% Obtener la ciudad de origen, destino y pasajes, de los vuelos realizado el viernes
obtenerPuntoTres(Dia, Origen, Destino, Pasaje) :- vuelo(Origen, Destino, _, Dia, Pasaje). 


% Obtener el nombre y clima de las ciudades a las que se viajó desde bogota
obtenerPuntoCuatro(Origen, Destino, Clima) :- obtenerPuntoUno(Origen, Destino), ciudad(Destino, _, Clima, _).

% Obtener el nombre y clima de las ciudades a las que se viajó desde bogota el dia jueves
obtenerPuntoCinco(Dia, Origen, Destino, Clima) :- obtenerPuntoTres(Dia, Origen, Destino, _), ciudad(Destino, _, Clima, _).

% La ciudad de origen, destino, altitud y número de asientos, de los viajes realizados el lunes
obtenerPuntoSeis(Dia, Origen, Destino, Altitud, NumAsientos) :- vuelo(Origen, Destino, Matricula, Dia, _), 
																ciudad(Destino, _, _, Altitud),
																avion(Matricula, NumAsientos, _).

```