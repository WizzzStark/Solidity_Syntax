# Solidity_Syntax
Apuntes de Solidity, lenguaje de programación orientado a contratos y enfocado específicamente a la Máquina Virtual de Etehreum.

## Contatos
El código en solidity se estructura en contratos, que son los bloques más básicos de las aplicaciones en Ethereum, tanto las variables como las funciones pertenecen a contratos, un contrato vacío se vería así:
```solidity
contract HolaMundo {

}
```

## Versión Pragma
En el código fuente de solidity debemos especificar al comienzo la versión del compilador que debe usarse para ese código, la declaración del mismo sería algo como: `pragma solidity ^0.4.25;` por lo que al comenzar un nuevo proyecto siempre deberemos empezar de la siguiente forma:
```solidity
pragma solidity ^0.4.25;
contract HolaMundo {

}
```

## Variables de estado y números enteros
Las variables de estado se guardan de forma permanente en el almacenamiento del contrato, esto significa que se amacenan en la cadena de bloques de ethereum.
```solidity
contract Ejemplo {
  // Esto se guardará permanentemente en la cadena de bloques
  uint myUnsignedInteger = 100;
}
```

## Enteros sin signo: uint
El tipo de dato `uint` es un entero sin signo el cual su valor debe ser siempre no-negativo, también existe un tipo `int` para enteros con signo. 
> En solidity el uint es un alias a `uint256` refiriendose a sus bits, tambien se puede especificar con declaraciones como `uint8`, `uint16`, `uint32` etc... 

## Operaciones Matemáticas
Las operaciones en solidity son prácticamente las mismas que en cualquier otro lenguaje de programación:
- Suma: x + y
- Resta: x - y
- Multiplicación: x * y
- División: x / y
- Módulo: x % y (Te devuelve el resto de la división)
- Exponencial: x ** y
Ejemplo exponencial:
```solidity
uint x = 5 ** 2; // es igual a 5^2 = 25
```

## Estructuras
Cuando necesitemos tipos de datos mas complejos podremos usar `structs` (estructuras de datos):
```solidity
struct Person {
  uint age;
  string name;
}
```
Las estructuras nos permiten crear datos mas complejos con varias propiedades.
> El tipo de dato string se usa para cadenas de texto UTF-8 de longitud arbitraria, ejemplo: string saludo = "Hola mundo!"

## Arrays
Si queremos tener una colección de datos podemos usar arrays, en solidity hay dos tipos, los fijos y los dinámicos.
```
// Un Array con una longitud fija de 2 elementos:
uint[2] fixedArray;
// otro Array fijo, con longitud de 5 elementos:
string[5] stringArray;
// un Array dinámico, sin longitud fija que puede seguir creciendo:
uint[] dynamicArray;
```
También podemos usar arrays de estructuras, por ejemplo podríamos hacer uno con la estructura Person del apartado anterior.
```solidity
Person[] people; //Array dinámico, podemos seguir añadiéndole elementos
```
Crear un array de estructuras puede ser muy útil para guardar datos estructurados en tu contrato, es decir, que se queden guardados en la blockchain.

### Arrays Públicos
Puedes declarar un array como público y solidity creará una función `getter` para acceder a él, esta es la sintaxis:
```solidity
Person[] public people;
```
Haciendo esto otros contratos podrán leer (sin escribir) de ese array, sirva para guardar datos públicos en tu contrato.

## Declaración de Funciones
Una declaración de función en Solidity sería algo como esto:
```solidity
function eatHamburgers(string _name, uint _amount) {

}
```
Esta función llamada eatHamburguers tomará dos parámetros: una cadena de texto (string) y un número entero sin signo (uint).
> La convención es llamar a los parámetros que toma la función con un subrayado (_) para diferenciarlos de las variables globales.

Y para llamar a la función haremos lo siguiente:
```solidity
eatHamburgers("vitalik", 100);
```

## Trabajando con estrucutas y arrays

### Creando nuevas estructuras (structs)
Recordemos el ejemplo anterior:
```solidity
struct Person {
  uint age;
  string name;
}

Person[] public people;
```
Ahora crearemos un nuevo Person y lo añadiremos a nuestro array people.
```solidity
// crear un nuevo `Person`
Person satoshi = Person(172, "Satoshi");

// añadir esta persona a nuestro array
people.push(satoshi);
```
También se puede combinar esto en una misma linea:
```solidity
people.push(Person(16, "Vitalik"));
```
Debemos tener en cuenta que array.push() añade el elemento al final del array por lo que seguirán el orden.
```solidity
uint[] numbers;
numbers.push(5);
numbers.push(10);
numbers.push(15);
// el array numbers ahora es [5, 10, 15]
```
