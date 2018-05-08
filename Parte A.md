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
  medicamento :: [sintomas] -> (sintoma -> [sintomas])
  curaTodosLos :: [sintomas] -> medicamento -> [sintomas]
  ```

2. _Se aplican orden superior y aplicacion parcial, el orden superior se hace más visible en la función "mejorMedicamentoPara" ya que dentro del "find" se utiliza otra función como parametro, además el "find" tiene control sobre la función "curaTodosLos" ejecutandola primero para poder completar su propia ejecución (un ciudadano de primera clase que controla a otro ciudadano de primera clase) . Aplicacion parcial se usa en (curaTodosLos síntomas) , ahí se ejecuta parcialmente ya que no se le pasa el medicamento,
y eso genera una nueva función que está esperando recibir un medicamento para poder funcionar._

3. _No devuelve nada, como "varicela" genera una lista infinita y "mejorMedicamentoPara" debe ejecutar un filter para curar los sintomas, haskell en este caso no puede ejecutar perezosamente, debe iterar toda la lista filtrando todos los elementos, por lo tanto nunca termina y la consola no devuelve ningun resultado.
Si se agregara_ `sugestión` _como medicamento entonces la consulta anterior devolvería_ `"sugestión"` _como el mejor medicamento, a pesar de que necesita tratar con un numero infinito de síntomas, no le importa ya que directamente vacía la
lista de síntomas y "cura" al paciente, puede ejecutar perezosamente, por lo tanto con solo ejecutar una vez el "sugestion" ya no necesita seguir ejecutando más porque la lista está vacia._
