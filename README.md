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

## Mapeos y direcciones
La blockchain de Ethereum está creada por `cuentas`, que podrían ser como cuentas bancarias. Una cuenta tiene un balance de `Ether` y puedes recibir pagos en Ether de otras cuentas. Cada cuenta tiene una dirección que un identificador único que apuntado a una cuenta, y se asemejaría a algo así:
```solidity
0x0cE446255506E92DF41614C46F1d6df9Cc969183
```

### Mapeando
Definir un mapping se asemejaría a esto:
```solidity
// Para una aplicación financial, guardamos un uint con el balance de su cuenta:
mapping (address => uint) public accountBalance;
// O podría usarse para guardar / ver los usuarios basados en ese userId
mapping (uint => string) userIdToName;
```
Un mapeo es esencialmente una asociación valor-clave para guardar y ver datos. En el primer ejemplo, la llave es un address (dirección) y el valor correspondería a un uint, y en el segundo ejemplo la llave es un uint y el valor un string.

## msg.sender
En Solidity, hay una serie de variables globales que están disponibles para todas las funciones. Una de estas es msg.sender, que hace referencia a la dirección de la persona (o el contrato inteligente) que ha llamado a esa función.
> En Solidity, la ejecución de una función necesita empezar con una llamada exterior. Un contrato se sentará en la blockchain sin hacer nada esperando a que alguien llame a una de sus funciones. Así que siempre habrá un msg.sender.
Aquí tenemos un ejemplo de como usar msg.sender y actualizar un mapping:
```solidity
mapping (address => uint) favoriteNumber;

function setMyNumber(uint _myNumber) public {
  // Actualiza tu mapeo `favoriteNumber` para guardar `_myNumber` dentro de `msg.sender`
  favoriteNumber[msg.sender] = _myNumber;
  // ^ La sintaxis para guardar datos en un mapeo es como en los arrays
}

function whatIsMyNumber() public view returns (uint) {
  // Conseguimos el valor guardado en la dirección del emisor
  // Será `0` si el emisor no ha llamado a `setMyNumber` todavía
  return favoriteNumber[msg.sender];
}
``` 
En este trivial ejemplo, cualquiera puede llamar a `setMyNumber` y guardar un `uint` en nuestro contrato, que estará atado a su dirección. Entonces, cuando llamen a `whatIsMyNumber`, debería devolverles el `uint` que han guardado.

Usando `msg.sender` te da la seguridad de la blockchain de Ethereum — la única forma de que otra persona edite la información de esta sería robandole la clave privada asociada a la dirección Ethereum.

## Requiere
require hace que la función lanze un error y pare de ejecutarse si la condición no es verdadera:
```solidity
function sayHiToVitalik(string _name) public returns (string) {
  // Compara si _name es igual a "Vitalik". Lanza un error si no lo son.
  // (Nota: Solidity no tiene su propio comparador de strings, por lo que
  // compararemos sus hashes keccak256 para ver si sus strings son iguales)
  require(keccak256(_name) == keccak256("Vitalik"));
  // Si es verdad, continuamos con la función:
  return "Hi!";
}
```
Si llamas a la función con `sayHiToVitalik("Vitalik")`, esta devolverá "Hi!". Si la llamas con cualquier otra entrada, lanzará un error y no se ejecutará.
De este modo require es muy útil a la hora de verificar que ciertas condiciones sean verdaderas antes de ejecutar una función.

## Herencia

Nuestro código está haciendose un poco largo. Mejor que hacer un contrato extremandamente largo, a veces tiene sentido separar la lógica de nuestro código en multiples contratos para organizar el código.

Una característica de Solidity que hace más manejable esto es la herencia de los contratos:
```solidity
contract Doge {
  function catchphrase() public returns (string) {
    return "So Wow CryptoDoge";
  }
}

contract BabyDoge is Doge {
  function anotherCatchphrase() public returns (string) {
    return "Such Moon BabyDoge";
  }
}
```
BabyDoge hereda de Doge. Eso significa que si compilas y ejecutas BabyDoge, este tendrá acceso tanto a `catchphrase()` como a `anotherCatchphrase()` (y a cualquier otra función publica que definamos en Doge).

## Importar
Cuando tienes multiples archivos y quieres importar uno dentro de otro, Solidity usa la palabra clave import:
```solidity
import "./someothercontract.sol";

contract newContract is SomeOtherContract {

}
```
Entonces si tenemos un fichero llamado `someothercontract.sol` en el mismo directorio que este contrato (eso es lo que significa ./), será importado por el compilador.

## Storage vs Memory

En Solidity, hay dos sitios donde puedes guardar variables - en `storage` y en `memory`.

`Storage` se refiere a las variables guardadas permanentemente en la blockchain. Las variables de tipo `memory` son temporales, y son borradas en lo que una función externa llama a tu contrato. Piensa que es como el disco duro vs la RAM de tu ordenador.
La mayoría del tiempo no necesitas usar estas palabras clave ya que Solidity las controla por defecto. Las variables de estado (variables declaradas fuera de las funciones) son por defecto del tipo storage y son guardadas permanentemente en la blockchain, mientras que las variables declaradas dentro de las funciones son por defecto del tipo memory y desaparecerán una vez la llamada a la función haya terminado.

Aun así, hay momentos en los que necesitas usar estas palabras clave, concretamente cuando estes trabajando con structs y arrays dentro de las funciones:
```solidity
contract SandwichFactory {
  struct Sandwich {
    string name;
    string status;
  }

  Sandwich[] sandwiches;

  function eatSandwich(uint _index) public {
    // Sandwich mySandwich = sandwiches[_index];

    // ^ Parece algo sencillo, pero solidity te dará un warning
    // diciendo que deberías declararlo `storage` o `memory`.

    // De modo que deberias declararlo como `storage`, así:
    Sandwich storage mySandwich = sandwiches[_index];
    // ...donde `mySandwich` es un apuntador a `sandwiches[_index]`
    // de tipo storage, y...
    mySandwich.status = "Eaten!";
    // ...esto cambiará permanentemente `sandwiches[_index]` en la blockchain.

    // Si únicamente quieres una copia, puedes usar `memory`:
    Sandwich memory anotherSandwich = sandwiches[_index + 1];
    // ...donde `anotherSandwich` seria una simple copia de 
    // los datos en memoria, y...
    anotherSandwich.status = "Eaten!";
    // ...modificará la variable temporal y no tendrá efecto
    // en `sandwiches[_index + 1]`. Pero puedes hacer lo siguiente:
    sandwiches[_index + 1] = anotherSandwich;
    // ...si quieres que la copia con los cambios se guarde en la blockchain.
  }
}
```

## Más en la Visibilidad de Funciones
### Internal y External
Además de public y private, Solidity tiene dos tipos de visibilidad más para las funciones: internal y external.

internal es lo mismo que private, a excepción de que es también accesible desde otros contratos que hereden de este. (¡Ey, suena como lo que necesitamos aquí!).

external es parecido a public, a excepción que estas funciones SOLO puedes ser llamadas desde fuera del contrato — no pueden ser llamadas por otras funciones dentro de ese contrato. Hablaremos más adelante sobre cuando querrás usar external vs public.

Para declarar las funciones internal o external, la sintaxis es igual que private y public:
```solidity
contract Sandwich {
  uint private sandwichesEaten = 0;

  function eat() internal {
    sandwichesEaten++;
  }
}

contract BLT is Sandwich {
  uint private baconSandwichesEaten = 0;

  function eatWithBacon() public returns (string) {
    baconSandwichesEaten++;
    // Podemos llamar a esta función aquí porque es internal
    eat();
  }
}
```

## Interactuando con otros contratos
Para que nuestro contrato pueda hablar a otro contrato de la blockchain que no poseemos, necesitamos definir una `interfaz`.

Vamos a ver un simple ejemplo. Digamos que hay un contrato en la blockchain tal que así:
```solidity
contract LuckyNumber {
  mapping(address => uint) numbers;

  function setNum(uint _num) public {
    numbers[msg.sender] = _num;
  }

  function getNum(address _myAddress) public view returns (uint) {
    return numbers[_myAddress];
  }
}
```
Este seria un simple contrato donde cualquiera puede guardar su número de la suerte, y este estará asociado a su dirección de Ethereum. De esta forma cualquiera podría ver el número de la suerte de una persona usando su dirección.

Ahora digamos que tenemos un contrato externo que quiere leer la información de este contrato usando la función `getNum`.

Primero debemos usar una interfaz del contrato `LuckyNumber`:
```solidity
contract NumberInterface {
  function getNum(address _myAddress) public view returns (uint);
}
```
Ten en cuenta que esto se asemeja a definir un contrato, con alguna diferencia. Primero, solo declaramos las funciones con las que queremos interactuar - en este caso getNum — y no mencionamos ninguna otra función o variables de estado.

Segundo, no definimos el cuerpo de la función. En vez de usar las llaves ({ y }), solamente terminaremos la función añadiendo un punto y coma al final de la declaración (;).

Sería como definir el esqueleto del contrato. Así es como conoce el compilador a las interfaces.

Incluyendo esta interfaz en el código de tu dapp nuestro contrato sabe como son las funciones de otro contrato, como llamarlas, y que tipo de respuesta recibiremos.

Continuando con nuestro ejemplo anterior de NumberInterface, una vez hemos definido la interfaz como:
```solidity
contract NumberInterface {
  function getNum(address _myAddress) public view returns (uint);
}
```
Podemos usarla en el contrato de esta manera:
```solidity
contract MyContract {
  address NumberInterfaceAddress = 0xab38... 
  // ^ La dirección del contrato FavoriteNumber en Ethereum
  NumberInterface numberContract = NumberInterface(NumberInterfaceAddress)
  // Ahora `numberContract` está apuntando al otro contrato

  function someFunction() public {
    // Ahora podemos llamar a `getNum` de ese contrato:
    uint num = numberContract.getNum(msg.sender);
    // ...y haz algo con `num` aquí
  }
}
```
De esta manera, tu contrato puede interactuar con otro contrato de la blockchain de Ethereum, siempre y cuando la función esté definida como public o external.

## Manejando Múltiples Valores Devueltos
```solidity
function multipleReturns() internal returns(uint a, uint b, uint c) {
  return (1, 2, 3);
}

function processMultipleReturns() external {
  uint a;
  uint b;
  uint c;
  // Así es como hacemos múltiples asignaciones:
  (a, b, c) = multipleReturns();
}

// O si solo nos importa el último de estos valores:
function getLastReturnValue() external {
  uint c;
  // Podemos dejar el resto de campos en blanco:
  (,,c) = multipleReturns();
}
```
## Sentencias if
Una sentencia if en Solidity es igual que en javascript:
```solidity
function eatBLT(string sandwich) public {
  // Recuerda que con strings, debemos comparar sus hashes keccak256 
  // para comprobar su equidad
  if (keccak256(sandwich) == keccak256("BLT")) {
    eat();
  }
}
```
