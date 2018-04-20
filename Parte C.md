# Parte C
## Paradigma Objetos

_Tenemos un pequeño programa que modela superhéroes y villanos de la empresa "Maravilla" de cómics (y películas, series, videojuegos, etc.). Nos piden modelar los ataques de los personajes a otros personajes, y para esto debemos representarlos. Los personajes pueden ser héroes o villanos. Un héroe sólo puede atacar a sus enemigos, mientras que un villano puede atacar a cualquier otro personaje. En el caso del villano, si ataca a un enemigo, ese enemigo recibe el doble de daño. Un héroe ataca con tanto poder como la suma de sus poderes potenciados por sus aliados. Cada uno tiene un determinado poder, el cual es multiplicado por la cantidad de aliados del héroe. Los villanos, en cambio, tienen armas, que producen un determinado daño total sobre un área de efecto dada. El poder se calcula como el daño total dividido por el área de efecto._

_Se tiene en cuenta el siguiente modelo base planteado:_

### Wollok
```
class Personaje {
  var enemigos = []
  method recibirDaño(cantidad) {...}
}

class Heroe inherits Personaje {
  var habilidades = []
  var aliados = []
  method atacar(personaje) {
    if (enemigos.contains(personaje))
      personaje.recibirDaño(self.poder())
  }
  method poder() = habilidades.sum(
    {h => h.poder() * aliados.size()})
}

class Villano inherits Personaje {
  method atacar(personaje) {
    personaje.recibirDaño(self.poder())
    if (enemigos.contains(personaje))
      personaje.recibirDaño(self.poder())
  }
  method poder() = armas.sum(
    {a => a.daño() / a.areaDeEfecto()})
}

class Habilidad {
  var poder
}

class Arma {
  var daño
  var areaDeEfecto
}
```

### Smalltalk
```
#Personaje (v.i. enemigos)
>>recibirDaño: cantidad
  "Something..."

#Heroe (subclase de Personaje - v.i. habilidades, aliados)
>>atacar: personaje
  (enemigos includes: personaje) ifTrue: [
    personaje recibirDaño: self poder
  ]
>>poder
  ^habilidades sum: [ :h | h poder * aliados size ]

#Villano (subclase de Personaje - v.i. armas)
>>atacar: personaje
  personaje recibirDaño: self poder.
  (enemigos includes: personaje) ifTrue: [
    personaje recibirDaño: self poder
  ]
  (enemigos includes: personaje) ifTrue: [
    personaje recibirDaño: self poder
  ]

>>poder
  ^armas sum: [ :a  | a daño / a areaDeEfecto ]

#Habilidad (v.i. poder)

#Arma (v.i. daño, areaDeEfecto)
```

1. Responder Verdadero o Falso y justificar:

  a. El ataque de un héroe no maneja correctamente el caso de atacar a un personaje que no es enemigo.

  b. La lógica de condición de saber si el personaje a atacar es enemigo y la consecuencia de atacarlo (es decir, que el otro reciba daño) se repite en ambas clases, por lo que dicha lógica debería estar en la superclase Personaje.

  c. Puede verse el uso polimórfico de villanos y héroes en la solución.

2. Proponer una solución alternativa que corrija las observaciones realizadas en el punto anterior y cualquier otra mejora que sea apropiada.


1. Respuestas:

  a. _Falso, en caso de no cumplir la condición finaliza el método sin hacer nada._

  b. _Verdadero, pero va a ser necesario llamar el método súper en la clase Villano para poder asignar la condición de que pueda dañar a cualquier personaje._

  c. _Verdadero, el método_ `recibirDaño` _es común a todos los personajes pero su aplicación varía según el personaje (varía el daño que aplica)._

2. Solución alternativa:

### Wollok
```
class Personaje {
  var enemigos = []
  method recibirDaño(cantidad) {...}
  method atacar(personaje) {
    if (enemigos.contains(personaje))
    personaje.recibirDaño(self.poder())
  }
}

class Heroe inherits Personaje {
  var habilidades = []
  var aliados = []
  method poder() = habilidades.sum(
    {h => h.poder() * aliados.size()})
}

class Villano inherits Personaje {
  method atacar(personaje) = super() {
    personaje.recibirDaño(self.poder())
  }
  method poder() = armas.sum(
    {a => a.daño() / a.areaDeEfecto()})
}

class Habilidad {
  var poder
}

class Arma {
  var daño
  var areaDeEfecto
}
```
