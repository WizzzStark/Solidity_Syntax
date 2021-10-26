# Apuntes Solidity
Apuntes de Solidity, lenguaje de programación orientado a contratos y enfocado específicamente a la Máquina Virtual de Ethereum. Estos apuntes están hechos a partir de la [página web](https://cryptozombies.io/) de cryptozombies, donde se puede poner en práctica todo esto.

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
```solidity
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

## Estrucutas y Arrays

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
## Funciones Públicas y Privadas
En solidity las funciones son públicas (`public`) por defecto, esto significa que cualquiera o cualquier contrato puede llamarlas y ejecutar su código. Pero esto no es algo que queremos que pase siempre y puede ser peligroso, por lo que necesitaremos establecer algunas funciones como privadas (`private`) y solo hacer públicas las que queramos exponer.
Una función privada se declararía de la siguiente forma:
```solidity
uint[] numbers;

function _addToArray(uint _number) private {
  numbers.push(_number);
}
```
Esto significa que solo otras funciones dentro de tu contrato podrán llamar a esta función, como puedes ver, se usa la clave `private` después del nombre de la función, y de la misma forma que los parámetros, la convención es nombrar las funciones privadas empezando con una barra baja (_).

### Valores de Retorno
Para devolver un valor desde la función, se hace lo siguiente:
```solidity
string greeting = "Qué tal viejo!";

function sayHello() public returns (string) {
  return greeting;
}
```
En solidity, la declaración de funciones contiene al final el tipo de dato de retorno.

### Modificadores de Función
La función de arriba no cambia el estado en Solidity, esto es que no cambia ningún valor o escribe nada. En este caso podríamos declararla como función `view` que significa que solo puede ver datos sin modificarlos:
```solidity
function sayHello() public view returns (string) {
```
Solidity también contiene funciones `pure` que significa qie ni siquiera accede a los datos de la aplicación:
```solidity
function _multiply(uint a, uint b) private pure returns (uint) {
  return a * b;
}
```
Esta función no lee desde el estado de la aplicación - el valor devuelto depende por completo de los parámetros que le pasemos.

## Keccak256 y Encasillado de tipo
Ethereum incluye una función hash llamada `keccak256`, que es una versión de SHA3, lo que hace es mapear una cadena de caracteres a un número aleatorio hexadecimal de 256-bits, un ejemplo:
```solidity
//6e91ec6b618bb462a4a6ee5aa2cb0e9cf30f7a052bb467b0ba58b8748c00d2e5
keccak256("aaaab");
//b1f078126895a1424524de5321b339ab00408010b7cf0e6ed451514981e58aa9
keccak256("aaaac");
```
Como puedes ver, el valor devuelto para cada caso es completamente distinto, a pesar de que sólo hemos cambiado un carácter del argumento.

## Casteo de Variables
A veces necesitaremos convertir entre tipos de datos, por ejemplo en el siguiente caso:
```solidity
uint8 a = 5;
uint b = 6;
// dará un error porque a * b devuelve un `uint` y no un `uint8`:
uint8 c = a * b;
// debemos de forzar la variable b para que sea convertida a `uint8`
uint8 c = a * uint8(b);
```

## Eventos
Los eventos son la forma en la que nuestro contrato comunica que algo sucedió en la cadena de bloques a la interfaz de usuario, la cual puede estar escuchando ciertos eventos y hacer algo cuando sucedan. Ejemplo:
```solidity
// declara el evento
event IntegersAdded(uint x, uint y, uint result);

function add(uint _x, uint _y) public {
  uint result = _x + _y;
  // lanza el evento para hacer saber a tu aplicación que la función ha sido llamada:
  emit IntegersAdded(_x, _y, result);
  return result;
}
```
La aplicación con la interfaz de usuario podría entonces estar escuchando el evento. Una implementación en JavaScript sería así:
```solidity
YourContract.IntegersAdded(function(error, result) {
  // hacer algo con `result`
}
```
