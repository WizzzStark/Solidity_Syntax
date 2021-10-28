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

## Inmutabilidad de los contratos
Para empezar, despues de implementar un contrato en Ethereum, es inmutable, lo que significa que nunca va a ser modificado o actualizado de nuevo.

El código inicial que implementes en el contrato es el que va a permanecer, permanentemente, en la blockchain. Esto es debido a que una de las mayores preocupaciones de Solidity es la seguridad. Si hay un error en el código del contrato, no hay forma de solucionarlo más adelante. Tendrás que decirles a tus usuarios que empiecen a usar otra dirección de contrato inteligente que incluya ese arreglo.

Pero es también una característica de los contratos inteligentes. El código es la ley. Si lees el código de un contrato inteligente y lo verificas, vas a estar totalmente seguro de que cada vez que lo llames va a hacer exactamente lo que el código dice que va a hacer. Nadie va a poder más adelante cambiar la función y que te devuelva resultados inesperados.

## Dependencias externas 
Si estamos llamando a otro contrato que no es nuestro y por alún motivo cambia su dirección, si hemos definido su dirección hardcodeada dejaría a nuestra DApp completamente inservible - nuestra DApp apuntará a una dirección hardcodeada que no devolverá valores nunca más y no podremos modificar nuestro contrato para solucionarlo.

Por este motivo, a veces tiene sentido programar funciones que te permitan actualizar partes de nuestra DApp.

## Contrato Ownable de OppenZeppelin
Abajo está el contrato Ownable definido en la librería Solidity de OpenZeppelin. OpenZeppelin es una librería segura donde hay contratos inteligentes para utilizar en tus propias DApps revisados por la comunidad. En este caso es para asignarnos permisos especiales.
```solidity
/**
 * @title Ownable
 * @dev The Ownable contract has an owner address, and provides basic authorization control
 * functions, this simplifies the implementation of "user permissions".
 */
contract Ownable {
  address public owner;
  event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

  /**
   * @dev The Ownable constructor sets the original `owner` of the contract to the sender
   * account.
   */
  function Ownable() public {
    owner = msg.sender;
  }

  /**
   * @dev Throws if called by any account other than the owner.
   */
  modifier onlyOwner() {
    require(msg.sender == owner);
    _;
  }

  /**
   * @dev Allows the current owner to transfer control of the contract to a newOwner.
   * @param newOwner The address to transfer ownership to.
   */
  function transferOwnership(address newOwner) public onlyOwner {
    require(newOwner != address(0));
    OwnershipTransferred(owner, newOwner);
    owner = newOwner;
  }
}
```
Alguna de las cosas que no hemos visto todavía:
 -  Constructores: `function Ownable()` es un `constructor`, que es una función especial opcional que tiene el mismo nombre que el contrato. Será ejecutada una única vez, cuando el  contrato sea creado por primera vez.
 -  Modificadores de Funciones: `modifier onlyOwner()`. Los `modificadores` son como semifunciones que son usadas para modificar otras funciones, normalmente para comprobar algunos requisitos antes de la ejecución. En este caso, onlyOwner puede ser usada para limitar el acceso y que solo el dueño del contrato pueda ejecutar función. 
 -  Palabra clave `indexed`, se explicará mas adelante.

Basicamente `Ownable` hace lo siguiente:

Cuando el contrato ha sido creado, su constructor inicializa `owner` con `msg.sender` (la persona que lo ha implementado)

Añade el modificador `onlyOwner`, que puede restringir el acceso a solo el owner en una función.

Permite transferir el contrato a un nuevo `owner`.

`onlyOwner` es un requisito tan común que la mayoría de las DApps en Solidity suelen empezar con un copia/pega de este contrato Ownable, y después su primer contrato heredaría de él.

## modificadores de función

Un modificador de función es igual que una función, pero usa la palabra clave modifier en vez de function. Pero no puede ser llamado directamente como una función - en vez de eso, podemos añadirle el nombre del modificador al final de la definición de la función para cambiar el comportamiento de ella.

Vamos a verlo con más detalle examinando onlyOwner:
```solidity
/**
 * @dev Throws if called by any account other than the owner.
 */
modifier onlyOwner() {
  require(msg.sender == owner);
  _;
}
```
Tendremos que usar el modificador de esta manera:
```solidity
contract MyContract is Ownable {
  event LaughManiacally(string laughter);

  // Mira como se usa `onlyOwner` debajo:
  function likeABoss() external onlyOwner {
    LaughManiacally("Muahahahaha");
  }
}
```
Observa el modificador `onlyOwner` en la función `likeABoss`. Cuando llamas a likeABoss, el código dentro de onlyOwner se ejecuta primero. Entonces cuando se encuentra con la sentencia _; en onlyOwner, vuelve y ejecuta el código dentro de likeABoss.

Hay otras maneras de usar los `modificadores`, uno de los casos de uso más comunes es añadir una rápida comprobación require antes de que se ejecute la función.

En el caso de onlyOwner, añadiendole este modificador a la función hace que solo el dueño del contrato (tú, si eres el que lo ha implementado) puede llamar a la función.
> Nota: Darle poderes especiales de esta manera al dueño a lo largo del contrato es usualmente necesario, pero puede también ser usado malintencionadamente.Por ejemplo, el dueño puede añadir una función oculta que le permita hacer lo que quiera en cualquier momento. Así que es importante recordar que solo porque una DApp esté en Ethereum no significa automáticamente que sea descentralizada — tienes que leerte el código fuente completo para asegurarte que esté libre de poderes especiales controlados por su dueño que puedan ser potencialmente preocupantes. Hay un cuidadoso balance entre mantener el control sobre la DAPP para poder arreglar los bugs potenciales, y construir una plataforma sin dueño donde tus usuarios puedan confiar la seguridad de sus datos.

## Gas — el combustible que mueven las DApps de Ethereum
En Solidity, tus usuarios tienen que pagar cada vez que ejecuten una función en tu DApp usando una divisa llamada gas. Los usuarios compran gas con Ether (la divisa de Ethereum), así que deben gastar ETH para poder ejecutar funciones en tu DApp.

La cantidad de gas necesaria para ejecutar una función depende en cuán compleja sea la lógica de la misma. Cada operación individual tiene un coste de gas basado aproximadamente en cuantos recursos computacionales se necesitarán para llevarla a cabo (p. ej. escribir en memoria es más caro que añadir dos integers). El total coste de gas de tu función es la suma del coste de cada una de sus operaciones.

Como ejecutar funciones cuestan dinero real a los usuarios, la optimización de código es mucho más importante en Ethereum que en cualquier otro lenguaje. Si tu código es descuidado, tus usuarios van a tener que pagar un premium para ejecutar tus funciones - esto puede suponer millones de dolares gastados innecesariamente por miles de usuarios en tasas.

### Porqué es necesario el gas?
Ethereum es como un ordenador grande, lento, pero extremandamente seguro. Cuando ejecutas una función, cada uno de los nodos de la red necesita ejecutar esa misma función para comprobar su respuesta - miles de nodos verificando cada ejecución de funciones es lo que hace a Ethereum descentralizado, y que sus datos sean inmutables y resistentes a la censura.

Los creadores de Ethereum querian estar seguros de que nadie pudiese obstruir la red con un loop infinito, o acaparar todos los recursos de la red con cálculos intensos. Por eso no hicieron las transacciones gratuitas, y los usuarios tienen que pagar por su poder de computo así como por su espacio en memoria.
> Nota: Esto no es necesariamente verdadero en las sidechains, así como los autores de CryptoZombies están construyendo en Loom Network. Es probable que nunca se ejecute un juego como World of Warcraft directamente en la mainnet de Ethereum - el coste de gas sería excesivamente caro. Pero puede ejecutarse en una sidechain con un algoritmo de consenso diferente.

## Empaquetado struct para ahorrar gas
Normalmente no hay ningún beneficio en usar cualquiera de estos subtipos porque Solidity reserva 256 bits de almacenamiento independientemente del tamaño del uint. Por ejemplo, usando uint8 en vez de uint (uint256) no te ahorrará nada de gas.

Pero hay una excepción para esto: dentro de los struct.

Si tienes varios uints dentro de una estructura, usar un uint de tamaño reducido cuando sea posible permitirá a Solidity empaquetar estas variables para que ocupen menos espacio en memoria. Por ejemplo:
```solidity
struct NormalStruct {
  uint a;
  uint b;
  uint c;
}

struct MiniMe {
  uint32 a;
  uint32 b;
  uint c;
}

// `mini` costará menos gas que `normal` debido al empaquetado de la estructura
NormalStruct normal = NormalStruct(10, 20, 30);
MiniMe mini = MiniMe(10, 20, 30); 
```
Por esta razón, dentro de una estructura querrás usar los subtipos más pequeños que vayas a necesitar.

Querrás también agrupar los tipos de datos que sean iguales (es decir, ponerlos al lado en la estructura) así Solidity podrá minimizar el espacio requerido. Por ejemplo, una estructura con campos uint c; uint32 a; uint32 b; costará menos gas que una estructura con campos uint32 a; uint c; uint32 b; porque los campos uint32 están agrupados al lado.

## Unidades de Tiempo
Solidity proporciona algunas unidades nativas para trabajar con el tiempo.

La variable now devolverá el actual tiempo unix (la cantidad de segundos que han pasado desde el 1 de Enero de 1970). El tiempo unix cuando escribía esto es 1635406629.
> Nota: El tiempo unix es tradicionalmente guardado en un número de 32 bits. Esto nos llevará a el problema del "Año 2038", donde las variables timestamp de tipo unix desbordarán y dejará inservibles muchos sistemas antiguos. Así que si queremos que nuestra DApp siga funcionando después de 20 años, podemos usar un número de 64 bits - pero de mientras nuestros usuarios tendrán que gastar más gas para usar nuestra DApp. ¡Decisiones de diseño!
Solidity también contiene segundos, minutos, horas, días, semanas y años como unidades de tiempo. Estos convertirán a un uint la cantidad de segundos que contengan esos números. Es decir 1 minuto son 60, 1 hora son 3600 (60 segundos x 60 minutos), 1 día son 86400 (24 horas x 60 minutos x 60 segundos), etc.

Aquí un ejemplo de como estas unidades pueden ser útiles:
```solidity
uint lastUpdated;

// Ajustamos `lastUpdated` a `now`
function updateTimestamp() public {
  lastUpdated = now;
}

// Devolverá `true` si han pasado 5 minutos desde que `updateTimestamp`
// fue llamado, `false` si no han pasdo 5 minutos todavía
function fiveMinutesHavePassed() public view returns (bool) {
  return (now >= (lastUpdated + 5 minutes));
}
```

## Pasando estructuras como argumentos
Puedes pasar un puntero storage a una estructura como argumento a una función private o internal. 
La sintaxis sería asi:
```solidity
function _doStuff(Zombie storage _zombie) internal {
  // hacer cosas con _zombie
}
```
De esta manera podemos pasar una referencia a nuestro zombi en una función en vez de pasar la ID del zombi y comprobar cual es.

## Modificadores de función con argumentos
También podemos pasarle argumentos a los modificadores:
```solidity
// Un mapeo para guardar la edad de un usuario:
mapping (uint => uint) public age;

// Modificador que requiere que ese usuario sea mayor a cierta edad:
modifier olderThan(uint _age, uint _userId) {
  require (age[_userId] >= _age);
  _;
}

// Tiene que ser mayor a 16 años para conducir un coche (en EEUU, al menos).
// Podemos llamar al modificador de función `olderThan` pasandole argumentos de esta manera:
function driveCar(uint _userId) public olderThan(16, _userId) {
  // La lógica de la función
}
```
Puedes ver aquí como el modificador olderThan recibe los argumentos de la misma manera que las funciones. Y cómo la función driveCar le pasa sus argumentos al modificador.

## Funciones View y el gas
Las funciones view no cuestan gas cuando son llamadas externamente por un usuario.

Esto es debido a que las funciones view no cambia nada en la blockchain - solo leen datos. Así que marcar una función con view le dice a web3.js que solo necesita consultar a tu nodo local de Ethereum para ejecutar la función, y que no necesita crear ninguna transacción en la blockchain (la cual debería ejecutarse por todos y cada uno de los nodos, y costaría gas).

Hablaremos sobre configurar web3.js en tu propio nodo mas tarde. Por ahora la mayor ventaja es que puedes optimizar el uso de gas de tu DApp haciendo que tus usuarios utilicen funciones external view siempre que sea posible.
> Nota: Si una función view es llamada internamente por otra función que no sea view en el mismo contrato, esta seguira costando gas. Esto es porque la otra función crea una transacción en Ethereum, y necesita ser verificada por el resto de nodos. Por lo que las funciones view son gratuitas siempre que sean llamadas externamente.

### Storage es caro
Una de las operaciones más caras en Solidity es usar storage — especialmente la escritura.

Esto es debido a que cada vez que escribes o cambias algún dato, este se guarda permanentemente en la blockchain. ¡Para siempre! Miles de nodos alrededor del mundo necesitan guardar esos datos en sus discos duros, y esa cantidad de datos sigue creciendo a lo largo del tiempo a medida que crece la blockchain. Así que tiene que haber un coste para hacer eso.

Para seguir manteniendo los costes bajos, querrás evitar escribir datos en "storage" a no ser que sea absolutemente necesario. A veces esto implica usar en programación una lógica ineficiente - como volver a construir un array en memoria cada vez que una función es llamada en vez de simplemente guardar ese array en una variable para acceder a sus datos más rápido.

En la mayoría de lenguajes de programación, usar bucles sobre largos conjuntos de datos es costoso. Pero en Solidity, esta es una manera más barata que usar storage si está en una función external view, debido a que las funciones view no les cuesta a los usuarios nada de gas. (¡Y el gas le cuesta a tus usuarios dinero real!).

Veremos los bucles for en el siguiente capítulo, pero primero, vamos a ver como declarar los arrays en memoria.

### Declarado arays en memoria
Puedes usar la palabra clave memory con arrays para crear un nuevo arrays dentro de una función sin necesidad de escribir nada en storage. El array solo existirá hasta el final de la llamada de la función, y esto es más barato en cuanto a gas que actualizar un array en storage - gratis si está dentro de una función view llamada externamente.

Así es como se declara un array en memoria:
```solidity
function getArray() external pure returns(uint[]) {
  // Instanciamos un nuevo array en memoria con una longitud de 3
  uint[] memory values = new uint[](3);
  // Le añadimos algunos valores
  values.push(1);
  values.push(2);
  values.push(3);
  // Devolvemos el arrays
  return values;
}
```
Esto es un ejemplo trivial para enseñarte a cómo usar la sintaxis, pero en el próximo apartado veremos como combinarlo con bucles `for` para usarlo en casos de uso reales.
> Nota: los arrays de tipo `memory` deben ser creados con una longitud como argumento (en este ejemplo, 3). Actualmente no pueden ser redimensionados como los arrays `storage` pueden serlo usando `array.push()`, de todas maneras esto podría cambiar en futuras versiones de Solidity.

## Bucles for
La sintaxis de los bucles for en Solidity es similar a JavaScript.

Vamos a ver un ejemplo donde queremos hacer un array de números pares:
```solidity
function getEvens() pure external returns(uint[]) {
  uint[] memory evens = new uint[](5);
  // Guardamos el índice del nuevo array:
  uint counter = 0;
  // Iteramos del 1 al 10 con un bucle for:
  for (uint i = 1; i <= 10; i++) {
    // Si `i` es par...
    if (i % 2 == 0) {
      // Añadelo a nuestro array
      evens[counter] = i;
      // Incrementamos el contador al nuevo índice vacío de `evens`:
      counter++;
    }
  }
  return evens;
}
```
La función devolverá un array con este contenido [2, 4, 6, 8, 10].

## Payable
Las funciones payable son parte de lo que hace de Solidity y Ethereum algo tan genial — son un tipo de función especial que pueden recibir Ether.

Pienselo por un momento. Cuando llama una función API en un servidor web normal, no puede enviar dólares (USD$) junto con su llamada de función — ni enviar Bitcoin.

Pero en Ethereum, ya que tanto el dinero (Ether), los datos (payload de transacción) y el mismo código de contrato viven en Ethereum, es posible para usted llamar a una función y pagar dinero por el contrato al mismo tiempo.

Esto abarca una lógica realmente interesante, como requerir cierto pago por el contrato para, de esta manera, ejecutar una función.
Un ejempllo:
```solidity
contract OnlineStore {
  function buySomething() external payable {
    // Check to make sure 0.001 ether was sent to the function call:
    require(msg.value == 0.001 ether);
    // If so, some logic to transfer the digital item to the caller of the function:
    transferThing(msg.sender);
  }
}
```
Aquí, msg.value es una manera de ver cuanto Ether fue enviado al contrato, y ether es una unidad incorporada.

Lo que sucede aquí es que alguien llamaría a la función desde web3.js (desde la interfaz JavaScript del DApp) de esta manera:
```
// Assuming `OnlineStore` points to your contract on Ethereum:
OnlineStore.buySomething({from: web3.eth.defaultAccount, value: web3.utils.toWei(0.001)})
```
Nótese el campo value, donde la llamada de función javascript especifíca cuánto de ether enviar (0.001). Si piensas en la transacción como un sobre, y los parámetros que usted envía a la llamada de función son los contenidos de la carta que coloca adentro, entonces añadir un value es como poner dinero en efectivo dentro del sobre — la carta y el dinero son entregados juntos al receptor.
> Nota: Si una función no es marcada como payable y usted intenta enviar Ether a esta, como se hizo anteriormente, la función rechazará su transacción.

## Retiros
En el apartado anterior, aprendimos cómo enviar Ether a un contrato. Entonces ¿Qué ocurre cuando lo envía?

Luego de enviar Ether a un contrato, este se almacena en la cuenta de Ethereum del contrato y estará atrapado ahí — a menos que añada una función para retirar el Ether del contrato

Puede escribir una función para retirar Ether del contrato de la siguiente forma:
```solidity
contract GetPaid is Ownable {
  function withdraw() external onlyOwner {
    owner.transfer(this.balance);
  }
}
```
Nótese que estamos utilizando owner y onlyOwner del contrato Ownable, asumiendo que este fue importado.

Puede transferir Ether a una dirección utilizando la función transfer y this.balance devolverá el balance total almacenado en el contrato. Así que si 100 usuarios han pagado 1 Ether a nuestro contrato, this.balance sería igual a 100 Ether.

Puede utilizar transfer para enviar fondos a cualquier dirección de Ethereum. Por ejemplo, podría tener una función que transfiera Ether de vuelta al msg.sender si rebasan el precio al pagar un item.
```solidity
uint itemFee = 0.001 ether;
msg.sender.transfer(msg.value - itemFee);
```
O en un contrato con un comprador y un vendedor, usted podría guardar la dirección del vendedor en la memoria, luego, cuando alguien adquiera su item, transferirle la tasa pagada por el comprador: 
`seller.transfer(msg.value)`.

Estos son algunos ejemplos de lo que hace de la programación de Ethereum algo realmente genial — puede tener mercados descentralizados como este que no son controlados por nadie.

## Números aleatorios
La mejor fuente de aleatoriedad que tenemos en solidity es la función hash `keccak256`.

Podríamos hacer algo como lo siguiente para generar un número aleatorio:
```solidity
// Generate a random number between 1 and 100:
uint randNonce = 0;
uint random = uint(keccak256(now, msg.sender, randNonce)) % 100;
randNonce++;
uint random2 = uint(keccak256(now, msg.sender, randNonce)) % 100;
```
Lo que esto haría es tomar la marca de tiempo de now, el msg.sender, y un nonce (un número que sólo se utiliza una vez, para que no ejecutemos dos veces la misma función hash con los mismos parámetros de entrada) en incremento.

Luego entonces utilizaría keccak para convertir estas entradas a un hash aleatorio, convertir ese hash a un uint y luego utilizar % 100 para tomar los últimos 2 dígitos solamente, dándonos un número totalmente aleatorio entre 0 y 99.

### Este método es vulnerable a ataques de nodos deshonestos
En Ethereum, cuando llama a una función en un contrato, lo transmite a un nodo o nodos en la red como una transacción. Los nodos en la red luego recolectan un montón de transacciones, intentan ser el primero en resolver el problema de matemática intensamente informático como una "Prueba de Trabajo", para luego publicar ese grupo de transacciones junto con sus Pruebas de Trabajo (PoW) como un bloque para el resto de la red.

Una vez que un nodo ha resuelto la PoW, los otros nodos dejan de intentar resolver la PoW, verifican que las transacciones en la lista de transacciones del otro nodo son válidas, luego aceptan el bloque y pasan a tratar de resolver el próximo bloque.

Esto hace que nuestra función de números aleatorios sea explotable .

Digamos que teníamos un contrato coin flip — cara y duplica su dinero, sello y pierde todo. Digamos que utilizó la función aleatoria anterior para determinar cara o sello. (random >= 50 es cara, random < 50 es sello).

Si yo estuviera ejecutando un nodo, podría publicar una transacción a mi propio nodo solamente y no compartirla. Luego podría ejecutar la función coin flip para ver si gané — y si perdí, escojo no incluir esa transacción en el próximo bloque que estoy resolviendo. Podría seguir haciendo esto indefinidamente hasta que finalmente gané el lanzamiento de la moneda y resolví el siguiente bloque, beneficiandome de ello.

### Como mejorar la seguridad
Ya que todos los contenidos de la blockchain son visibles para todos los participantes, este es un problema dificil, y su solución está más allá del rango de este tutorial. Puede leer [este hilo de StackOverflow](https://ethereum.stackexchange.com/questions/191/how-can-i-securely-generate-a-random-number-in-my-smart-contract) para que se haga un idea. Una idea sería utilizar un oráculo para ingresar una función de número aleatorio desde fuera de la blockchain de Ethereum.

Por supuesto, debido a que cientos de miles de nodos de Ethereum en la red están compitiendo por resolver el próximo bloque, mis probabilidades de resolver el siguiente bloque son extremadamente escasas. Me tomaría mucho tiempo o recursos informáticos para explotar esto y que sea beneficioso — pero si la recompensa fuera lo suficientemente alta (como si pudiera apostar $100,000,000 en la función coin flip), para mi valdría la pena atacar.

Así que mientras esta generación de número aleatorio NO sea segura en Ethereum, en la práctica a menos que nuestra función aleatoria tenga mucho dinero en riesgo, es probable que los usuarios de su juego no tengan suficientes recursos para atacarla.
