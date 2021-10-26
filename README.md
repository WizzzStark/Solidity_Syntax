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
