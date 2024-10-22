# CRYPTO BATTLE CREATURES 

Proyecto Blockchain GameFi NFT´s donde se simula un entorno de combate entre criaturas equipadas con items.
El Proyecto consta de 3 smart Contracts: Creatures.sol, Items.sol y Battle.sol

## Contrato Creatures.sol 
Permite mintear un NFT ERC721 al que asocia como creature.

Caracteristicas de las criaturas:
- name: El nombre de la criatura, que se saca aleatorio de un array definido.
- elementType: El tipo de elemento (fuego, agua, tierra o aire), como enum.
- level: El nivel de la criatura, que siempre empieza en 1.
- attackPower: La potencia de ataque de la criatura (valor entre 1 y 100, generado aleatoriamente).
- defensePower: La potencia de defensa de la criatura (valor entre 1 y 100, también generado aleatoriamente). 

### Variables Principales

- **_nextTokenId:**  Lleva registro del próximo ID de token que se asignará. Inicializa en 1 y se incrementa cada vez que se mintea una nueva criatura.
- **creatures:** Un mapping que almacena la información de cada criatura asociada a su respectivo ID.

**Estructura (caracteristicas de la creatura)**
- string name;             // Nombre de la criatura 
- ElementType elementType; // El tipo de elemento
- uint256 level;           // Level de la criatura 
- uint256 attackPower;     // Poder de ataque 
- uint256 defensePower;    // Poder de defensa 

### Funciones Principales
- **obtainCreature(address player):** Permite mintear una creatura, asignandola a un jugador (address) 
- **_random:** Busca generar un número aleatorio, usando informacion disponible en el bloque como el momento en que fue validado, el último randao del bloque del epoch anterior y la dirección que hace la llamada. En este momento es previsible y debido a tiempos en la cadena el poder de ataque suele ser igual al poder de defensa (que usan la fucion), se podria mejorar buscando el número aleatorio externo o usando otros recursos como Chainlink.

### Eventos
- **CreatureObtained:** Se emite cada vez que una criatura es obtenida por un jugador.

## Contrato Items.sol
Permite mintear diferentes tipos de objetos: armas (NFT), armaduras (NFT) y pociones (Tokens Fungibles) usando el estandar ERC-1155.

### Caracteristicas de los items
**Armas y Armaduras:** Ideadas como Tokens no fungibles se rigen con la estructura de Datos *Weapon* y *Armor*:
- name: Nombre del arma, que se elige al azar de una lista predefinida.
- attackPower: Poder de ataque (Solo para armas), asignado aleatoriamente entre 1 y 100.
- defensePower: Poder de defensa (Solo para armaduras), también asignado aleatoriamente entre 1 y 100.

Los IDs de las armas se generan de manera que siempre resulten en números pares, y los de armaduras en números inpares. Lo hicimos de esta manera para evitar colisiones y para diferenciar entre los tipos de ítems sin tener que asignarle un cap a la cantidad de items que puedan crearse.

**Pociones** Son tokens fungibles, se identifican por el ID de token 0. Para llevar un control se agrego una funcion *TokenSupply*.

### Funciones Principales

- **mintWeapon(address player):** Crea un arma con poder de ataque aleatorio asignado a un jugador (address).
- **mintArmor(address player):** Crea una armadura con poder de defensa aleatorio asignado a un jugador (address).
- **mintPotions(address player, uint256 amount):**  Crea tokens fungibles que representan pociones, asignando la cantidad especificada al jugador (address). El suministro total se incrementa con cada llamada para tener mayor control.
- **getTotalPotionSupply():** Retorna el suministro total de pociones.
- **_random:** Busca generar un número aleatorio, funcionando igual que en *Creatures.sol*.

### Eventos

- **WeaponMinted:** Emitido cuando una nueva arma es creada.
- **ArmorMinted:** Emitido cuando una nueva armadura es creada.
- **PotionsMinted:** Emitido cuando nuevas pociones son creadas.

## Contrato Battle.sol

Contrato que interactua con Creatures.sol y Item.sol. Contiene las interfaces de los dos contratos anteriores. El objetivo es que se pueda asociar las armas y armaduras a la creatura y hacer "una batalla":

### Variables y estructuras principales
- **creatures** e **items:**  Variables que almacenan las direcciones de los contratos correspondientes.
- **CreatureEquipment:** Estructura que almacena los IDs de las armas y armaduras equipadas por una criatura.
- **equippedItems:** Mapping que asocia cada criatura (identificada por su ID) con los objetos que tiene equipados. 

### Funciones principales

**equipItems(uint256 creatureId, uint256 weaponId, uint256 armorId):** Permite a un jugador equipar una criatura con un arma y una armadura. Solo el propietario de la criatura puede equipar los objetos, y se verifica que:
- El arma tenga un ID válido (un número par).
- La armadura tenga un ID válido (un número impar).
- El jugador realmente posea el arma y la armadura que está intentando equipar.

**battle(uint256 attackerId, uint256 defenderId):** Permite que dos criaturas equipadas se enfrenten en "una batalla". La lógica de la batalla es la siguiente:
1. Se obtiene el poder de ataque de la criatura atacante y el poder de defensa de la criatura defensora desde *Creatures.sol*.
2. Se obtienen las estadísticas de armas y armaduras desde *Items.sol*.
3. Se suman el poder de ataque de la criatura con el del arma y el poder de defensa de la criatura con el de la armadura.
4. Si el poder de ataque total del atacante es mayor que el poder de defensa total del defensor, el atacante gana. De lo contrario, el defensor gana.

### Eventos
- **ItemsEquipped:** Se emite cada vez que un jugador equipa una criatura con un arma y una armadura.


## Configuracion y Despliegue con Hardhat

### 1. Agregar OpenZeppelin Contracts 
```
npm install @openzeppelin/contracts
```
### 2. Compilar
```
npx hardhat compile
```
### 3. Verificar test
```
npx hardhat test
```
### 4. Iniciar nodo
```
npx hardhat node
```
### 5. Abrir otra consola para desplegar los contratos en local
```
npx hardhat ignition deploy ./ignition/modules/creatures.js --network localhost
npx hardhat ignition deploy ./ignition/modules/items.js --network localhost
npx hardhat ignition deploy ./ignition/modules/battle.js --network localhost
```
**NOTA IMPORTANTE:** Es necesario desplegar primero los contratos *creatures.sol* e *items.sol*, ya que el contrato *battle.sol* depende de ambos. Al configurar el script *battle.js* para el despliegue, asegúrate de actualizar los parámetros para incluir las direcciones de los contratos de **creatures** e **items** previamente desplegados.


## TEST
Solo pudimos realizar el test de *creatures.sol* e *items.sol* en Hardhat, para *battle.sol* realizamos tests manuales en Remix.
<a href="https://ibb.co/ygBVbgz"><img src="https://i.ibb.co/WBpsYBr/test-hardhat.jpg" alt="test-hardhat" border="0"></a>

## DESPLIEGUE
<a href="https://ibb.co/LQ7W6BH"><img src="https://i.ibb.co/yhj15K7/despliegue-red-local.jpg" alt="despliegue-red-local" border="0"></a>
<a href="https://ibb.co/Fn8VNjV"><img src="https://i.ibb.co/vDLYgKY/battle.jpg" alt="battle" border="0"></a>
<a href="https://ibb.co/Wk85BK2"><img src="https://i.ibb.co/F5Z387D/items.jpg" alt="items" border="0"></a>
<a href="https://ibb.co/vBtPHRv"><img src="https://i.ibb.co/26btyHY/creatures.jpg" alt="creatures" border="0"></a>

## CONTRATOS EN ETHERSCAN VERIFICADOS

### Contrato e interacciones Creatures.sol
- **Contrato Creatures.sol:** (https://sepolia.etherscan.io/address/0xe9943f2690ec030939919679039f7a189d79ba1b)
- **Interacción con contrato(obtener creatura 1)** (https://sepolia.etherscan.io/tx/0x1a6e73bf6c4bf0309df62f180bc516b310511052c2d9875d7e728e43086d47b2)
- **Interacción con contrato(obtener creatura 2)** (https://sepolia.etherscan.io/tx/0x0d22b1b3bf73bee0f3e37ef191c1db013ad87489dbb3aead270f49e1afa94c7c)
  
<a href="https://ibb.co/2PWX2xM"><img src="https://i.ibb.co/y5dLTtk/Screenshot-2024-10-22-105438.png" alt="Creature-Event" border="0"></a>

### Contrato e interacciones Items.sol
- **Creación contrato Items.sol:** (https://sepolia.etherscan.io/address/0xcd5f263d5491055d63a40b65bb1debcd46f6e610)
- **Interacción con contrato:(mint armor)** (https://sepolia.etherscan.io/tx/0xdeb3bdcd15cb5261ad3f979e1d8400d42fdf1262325bebd6517567b7bb6b195b)
- **Interacción con contrato:(mint weapon)** (https://sepolia.etherscan.io/tx/0x31e816e9bc4ed7b7ba1e0f6bcf0e4e8b17fa52960531f4830fc0aef2377b567c)
- **Interacción con contrato:(mint potion)** (https://sepolia.etherscan.io/tx/0x49cb8b99773d3f00be4ac0de1b965cd454f265b8dee0c3cfa9c63d805aecddd2)

<a href="https://ibb.co/Hp1BtJw"><img src="https://i.ibb.co/n8Z67Wd/Screenshot-2024-10-22-110341.png" alt="Items-Event" border="0"></a>

- **Creación contrato Battle.sol:** (https://sepolia.etherscan.io/address/0x90a84f03e16e8022622deeb7f54e5c43d15f165a)
- **Interacción con contrato: (Equipar a creatura 1):** (https://sepolia.etherscan.io/tx/0x57e8efc682e8dd37a3ab54c7245acf25ddd98c062a034152349aad32208e0101)
- **Interacción con contrato: (Equipar a creatura 2):** (https://sepolia.etherscan.io/tx/0xda5c00106d487e51900e3808bf68d79aba884a83b70596d2c1168bd6e4ceeaca)

<a href="https://ibb.co/3k2xL0q"><img src="https://i.ibb.co/6NzKf4x/Screenshot-2024-10-22-113456.png" alt="Equipar-Event" border="0"></a>
<a href="https://ibb.co/g7Jg0Vy"><img src="https://i.ibb.co/RScHXy2/Screenshot-2024-10-22-113520.png" alt="Batalla" border="0"></a><br />

## Posibles Mejoras
- Reducir los costes de gas ya sea disminuyendo el almacenamiento o empleando JSONs usando parte de la logica de manera externa.
- Mejorar la funcion de randomizacion, puede ser usando Chainlink o mejorando la funcion en si.
- Mejorar la funcion de batalla, por el momento es basica para verificar que todo esta funcionando correctamente, pero podemos implementar el tipo de elemento en juego, por ejemplo que tipo agua tenga ventajas sobre tipo fuego, podrian haber armaduras que tambien tengan poder de ataque, el nivel de la creatura podria depender de las pociones... El proyecto en si es muy escalable.
- A nivel de seguridad estamos usando Ownable, pero en un proyecto real lo mas probable habria sido implementar Acess Control.



