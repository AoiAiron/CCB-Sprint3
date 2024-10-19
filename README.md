# PROYECTO BATTLE CREATURES (nombre provisional)

Proyecto Blockchain GameFi NFT´s donde se simula un entorno de combate entre criaturas equipadas con items.
- El Proyecto consta de 3 smart Contracts:

**Contrato Creatures.sol** 

Permite mintear un NFT ERC721 al que asocia como creature.

Caracteristicas de las criaturas:
- name: El nombre de la criatura, que se saca aleatorio de un array.
- elementType: El tipo de elemento (fuego, agua, tierra o aire).
- level: El nivel de la criatura, que empieza en 1.
- attackPower: La potencia de ataque de la criatura (valor entre 1 y 100, generado aleatoriamente).
- defensePower: La potencia de defensa de la criatura (valor entre 1 y 100, también generado aleatoriamente). 


**Variables Principales**

- **_nextTokenId:** número del token.
- **enum ElementType:** nombre de elemento

**Estructura**

- string name;             // Nombre de la criatura (asignado al azar)
- ElementType elementType; // Enumera el tipo de elemento
- uint256 level;           // Level de la criatura (empieza en 1)
- uint256 attackPower;     // Poder de ataque (randomly assigned)
- uint256 defensePower;    // Poder de defensa (randomly assigned)

- mapping(uint256 => Creature) public creatures;

**Parámetros del constructor**
 constructor() ERC721("CryptoCreaturesBattle", "CCB")
  
- **obtainCreature** // Function to obtain a new creature and assign it to a player
- **ElementType elementType** // Generate random name from the list of names string memory randomName 
- **Creature memory newCreature** // Create the creature with random name, level 1, and random stats for attack an defense

**Funciones Principales**
- **_mint** // Mintea una criatura
- **function random** // genera números aleatorios 


**Contrato Item.sol**

Permite mintear diferentes tipos de objetos: armas (NFT), armaduras (NFT) y pociones (Tokens Fungibles).
SmartContract ERC 1155

 **Caracteristicas de los items**
- **Armas: NFT** Tienen nombre (igual de un array), y poder de ataque (valor entre 1 y 100). El ID de las armas siempre va a ser numeros pares.
- **Armaduras NFT** Tienen nombre (igual de un array), y poder de defensa (valor entre 1 y 100). El ID de las armas siempre va a ser numeros inpares.
- **Pociones** Son tokens fungibles, solo le agregue una function adicional de token supply. El ID de las pociones es 0. 





Try running some of the following tasks:

```
*Agregar OpenZeppelin Contracts*
npm install @openzeppelin/contracts

*Compilar*
npx hardhat compile

*Verificar test*
npx hardhat test

*Inicial nodo local*
npx hardhat node

*Desplegar contrato en local*
npx hardhat ignition deploy ./ignition/modules/creatures.js --network localhost
npx hardhat ignition deploy ./ignition/modules/items.js --network localhost
npx hardhat ignition deploy ./ignition/modules/battle.js --network localhost
```
