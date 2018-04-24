# Parte A
## Paradigma Funcional

_Se cuenta con la siguiente información:_

```
-- Para cada medicamento
amoxicilina = cura "infección"
bicarbonato = cura "picazón"
ibuprofeno = cura "dolor" . cura "hinchazón"
todosLosMedicamentos = [amoxicilina, bicarbonato, ibuprofeno]

cura síntoma = filter (/= síntoma)

-- Para cada enfermedad / conjunto de síntomas
malMovimiento = ["dolor"]
varicela = repeat "picazón" ¹

-- Asumimos que existe al menos un medicamento capaz de curar cada enfermedad
mejorMedicamentoPara síntomas = find (curaTodosLos síntomas) todosLosMedicamentos
curaTodosLos síntomas medicamento = medicamento síntomas == []
```

_Se pide:_

1. _Definir el tipo de medicamento genérico y de_ `curaTodosLos`.
2. _¿Cuáles son los dos conceptos funcionales más importantes aplicados en_ `mejorMedicamentoPara` _y de qué sirvieron?_
3. _¿Qué responde esta consulta? Justificar conceptualmente. encaso de errores o comportamientos inesperados, indicar cuáles son y dónde ocurren:_

  `mejorMedicamentoPara varicela`

  _Si se reemplaza_ `todosLosMedicamentos` _con lo de abajo, ¿qué respondería la consulta anterior? Justificar conceptualmente:_
  ```
  sugestión síntomas = []
  todosLosMedicamentos = [sugestión, amoxicilina, bicarbonato, ibuprofeno]
  ```

¹ `repeat x = x : repeat x`

---

#### Respuestas:

1. _Tipos:_
  ```
  medicamento :: p -> p
  curaTodosLos :: [a] -> b -> [a]
  ```

2. _Se aplican orden superior y aplicacion parcial, el orden superior se aplica en el find, ya que recibe una funcion y una lista para filtrar en base a esa funcion, entonces,
al ser "find" una funcion que ejecuta otra funcion para su funcionamiento, es un claro ejemplo de orden superior. Aplicacion parcial se usa en (curaTodosLos síntomas) , ahí se ejecuta parcialmente ya que no se le pasa el medicamento,
y eso genera una nueva funcion que está esperando recibir un medicamento para poder funcionar._
3. _No devuelve nada, como_ `varicela` _genera una lista infinita del síntoma_ `"picazon"` _y no se puede terminar de iterar sobre esa lista, ya que para ser el mejor medicamento contra la varicela hay que probar que podes curar
todos los síntomas provocados por esa enfermedad y no se puede saber ya que la lista nunca termina.
Si se agregara_ `sugestión` _como medicamento entonces la consulta anterior devolvería_ `"sugestión"` _como el mejor medicamento, a pesar de que necesita tratar con un numero infinito de síntomas, no le importa ya que directamente vacía la
lista de síntomas y "cura" al paciente._
