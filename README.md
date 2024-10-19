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

**Estructura Items**
- **struct Weapon** // Nombre del arma, poder de ataque
- **struct Armor** // Nombre de armadura, poder de defensa
( aqui no sé si ponerle lo de las pociones)

**Constructor**
- constructor() ERC1155("") 


**Funciones Principales**

- **mintWeapon** // Crea un arma con poder de ataque aleatorio asignado a una address.
- **_mint** // Crea el arma como un NFT//Mint the weapon as a unique token (NFT)
- **mintArmor** // Crea una armadura con poder de ataque aleatorio asignado a una address.
- **_mint** // Crea la armadura como un NFT//Mint the armor as a unique token (NFT)
( aqui no sé si ponerle lo de las pociones y su tottal supply)
- **function random** // genera números aleatorios



**Contrato Battle.sol**

Contrato que interactua con Creatures.sol y Item.sol. Contiene las interfaces de los dos contratos anteriores. El objetivo es que se pueda asociar las armas y armaduras a la creatura y hacer "una batalla":

**Caracteristicas**
- **CreatureEquipment:** Esta estructura se utiliza para almacenar el ID del arma y la armadura que una criatura tiene equipada. Cada criatura puede tener un arma (con un ID par) y una armadura (con un ID impar).
- **equippedItems:** Este mapeo asocia cada criatura (identificada por su ID) con los objetos que tiene equipados. 


**Estructura**
- **struct CreatureEquipment** // Estructura para guardar los items equipados por una criatura
- mapping(uint256 => CreatureEquipment) public equippedItems; // Mapeo para asociar una criatura con su equipo



**Constructor**
- creatures = ICreatures
- items = IItems 


**FUNCIONES**

**equipItems:** Esta función permite a un jugador equipar una criatura con un arma y una armadura. Solo el propietario de la criatura puede equipar los objetos, y se verifica que:
- El arma tenga un ID válido (un número par).
- La armadura tenga un ID válido (un número impar).
- El jugador realmente posea el arma y la armadura que está intentando equipar.

**battle:** Esta función permite que dos criaturas equipadas se enfrenten en una batalla. La lógica de la batalla es la siguiente:
1. Se obtiene el poder de ataque de la criatura atacante y el poder de defensa de la criatura defensora desde el primer contrato.
2. Se obtienen las estadísticas de armas y armaduras desde el segundo contrato.
3. Se suman el poder de ataque de la criatura con el del arma y el poder de defensa de la criatura con el de la armadura.
4. Si el poder de ataque total del atacante es mayor que el poder de defensa total del defensor, el atacante gana. De lo contrario, el defensor gana.

**function equipItems:** Función para que un jugador equipe un arma y una armadura a su criatura

**function battle** // Función para realizar una batalla entre dos criaturas

(no sé si agregar el código de la batalla)

# Agregar OpenZeppelin Contracts 
```
npm install @openzeppelin/contracts
```
# Compilación
```
npx hardhat compile
```
# Despliegue de los contratos en local
```
npx hardhat ignition deploy ./ignition/modules/creatures.js --network localhost
npx hardhat ignition deploy ./ignition/modules/items.js --network localhost
npx hardhat ignition deploy ./ignition/modules/battle.js --network localhost
```

# Iniciar nodo
```
npx hardhat node
```

# Verificar test
```
npx hardhat test
```

<a href="https://ibb.co/ygBVbgz"><img src="https://i.ibb.co/WBpsYBr/test-hardhat.jpg" alt="test-hardhat" border="0"></a>
<a href="https://ibb.co/LQ7W6BH"><img src="https://i.ibb.co/yhj15K7/despliegue-red-local.jpg" alt="despliegue-red-local" border="0"></a>
<a href="https://ibb.co/Fn8VNjV"><img src="https://i.ibb.co/vDLYgKY/battle.jpg" alt="battle" border="0"></a>
<a href="https://ibb.co/Wk85BK2"><img src="https://i.ibb.co/F5Z387D/items.jpg" alt="items" border="0"></a>
<a href="https://ibb.co/vBtPHRv"><img src="https://i.ibb.co/26btyHY/creatures.jpg" alt="creatures" border="0"></a>



# VERIFICACIONES CONTRATOS ETHERSCAN
(aqui me falta desplegar los contratos en metamask y ver sus transacciones en etherscan)







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
