# Parte B
## Paradigma Lógico

_Tenemos un predicado_ `toma`/2 _que relaciona a una persona con aquella bebida que le gusta tomar. La bebida puede ser cerveza, que tiene variedad, un amargor y un porcentaje de alcohol (0 si es cerveza sin alcohol), o vino, que tiene un tipo y una cantidad de años de añejamiento. El vino siempre tiene alcohol. Y también existen gaseosas varias._

```
toma(juan, coca).
toma(juan, vino(malbec, 3)).
toma(daian, cerveza(golden, 18, 0)).
toma(gisela, cerveza(ipa, 52, 7)).
toma(edu, cerveza(stout, 28, 6)).
```

```
tieneProblemas (Persona) :-
  findall(C, (toma(Persona, cerveza(C,_,A)), A > 0), Cs),
  findall(V, toma(Persona, vino(V,_)), Vs),
  findall(T, toma(Persona, T), Ts),
  length(Cs, CCs),
  length(Vs, CVs),
  length(Ts, CTs),
  CTs is CCs + CVs.
```

1. _Justificar V o F:_

  a. _No se repite código, dado que la lista de bebidas alcohólicas son distintas._

  b. _La solución planteada para_ `tieneProblemas`/1 _es declarativa._

  c. _La solución planteada no utiliza polimorfismo._

2. _Explique y justfique cuál es el significado de lo que se estaría consultando en el siguiente código:_

  `?- tieneProblemas (P).`

3. _Implemente una solución superadora de_ `tieneProblemas`/1

---

#### Respuestas:

1. Verdadero o Falso:

  a. _Falso, se repite código en los findall, ya que al ser todas bebidas podríamos tratarlas polimorficamente (como se muestra en el punto 3)._
  
  b. _Falso, es poco declarativa ya que conocemos demasiado la lógica de la consulta._

  c. _Verdadero, no aplica polimorfismo para ningún caso ya que podría tratar a las bebidas de la misma forma y no lo hace (como se muestra en el punto 3)_

2. _La consulta_ `tieneProblemas (P)` _ responde a la consulta de si la persona  tomo bebidas alcohólicas (vino o cerveza con graduación alcohólica >0) dando True en el caso de que sí y Falso en el caso de caso de que no. No permite consultas existenciales ya que utiliza funciones de orden superior sin usar generados, por ende solo permite consultas individuales._

3. Código:
  ```
  persona(Persona):- toma(Persona,_).

tieneProblemas(Persona):-
persona(Persona),
forall(toma(Persona,Bebida),esAlcoholica(Bebida)).

esAlcoholica(vino(,)).
esAlcoholica(cerveza(,,A)):- A > 0.
  ```
