# Base Guild Learn

Repositorio educativo del Guild de Base.

Aquí encontrarás material para aprender desde cero cómo desarrollar, desplegar y usar aplicaciones en la red Base.

Base permite a cualquier developer de Ethereum desplegar sus contratos sin cambios, pero con costos mucho más bajos y mayor velocidad.
# Ecosistema Base

Base ha crecido rápidamente gracias a:
- Integración nativa con Coinbase
- Gran cantidad de DEXs, bridges y protocolos de lending
- Soporte oficial de muchas herramientas de desarrollo
- Comunidades activas de builders

Algunos pilares del ecosistema:
- Bridges oficiales
- Exploradores (BaseScan)
- Infraestructura de oráculos y indexadores
- Wallets y onboarding simplificado

# Formatos de eventos

- Workshops de smart contracts en Base
- Spaces de Twitter/X sobre el ecosistema
- Hackathons internos del guild
- Sesiones de code-review
- Meetups presenciales cuando sea posible

Cualquier miembro puede proponer un evento.

# Base Guild Tools

Colección de herramientas, scripts y utilidades creadas por y para el Guild de Base.

Base ofrece un entorno ideal para experimentar y crear herramientas por sus bajos costos y compatibilidad total con el ecosistema Ethereum.

# Filosofía de herramientas en Base

Creemos en construir herramientas simples, open-source y reutilizables.

Gracias a las comisiones tan bajas de Base, es viable crear y probar utilidades on-chain que en mainnet serían demasiado caras de iterar.

Este repositorio existe para centralizar esas herramientas y facilitar su uso dentro del guild.
# Ideas de herramientas

- Scripts de deploy automatizado en Base
- Dashboards de actividad del guild
- Herramientas de airdrop y distribución
- Utilidades de monitoring
- Helpers para interactuar con contratos comunes

Cualquier miembro puede proponer o contribuir una herramienta.
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract OwnableCounter {
    uint256 public count;
    address public owner;

    event CountChanged(uint256 newCount);
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }

    function increment() external onlyOwner {
        count += 1;
        emit CountChanged(count);
    }

    function reset() external onlyOwner {
        count = 0;
        emit CountChanged(count);
    }

    function transferOwnership(address newOwner) external onlyOwner {
        require(newOwner != address(0), "Invalid address");
        emit OwnershipTransferred(owner, newOwner);
        owner = newOwner;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MessageBoard {
    string public lastMessage;
    address public lastAuthor;
    uint256 public messageCount;

    event NewMessage(address indexed author, string message);

    function postMessage(string calldata message) external {
        lastMessage = message;
        lastAuthor = msg.sender;
        messageCount += 1;
        emit NewMessage(msg.sender, message);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Vote {
    uint256 public yesVotes;
    uint256 public noVotes;
    mapping(address => bool) public hasVoted;

    event Voted(address indexed voter, bool support);

    function vote(bool support) external {
        require(!hasVoted[msg.sender], "Already voted");
        hasVoted[msg.sender] = true;

        if (support) {
            yesVotes += 1;
        } else {
            noVotes += 1;
        }

        emit Voted(msg.sender, support);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Ownership {
    address public owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        owner = msg.sender;
        emit OwnershipTransferred(address(0), msg.sender);
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }

    function transferOwnership(address newOwner) external onlyOwner {
        require(newOwner != address(0), "Invalid address");
        emit OwnershipTransferred(owner, newOwner);
        owner = newOwner;
    }

    function renounceOwnership() external onlyOwner {
        emit OwnershipTransferred(owner, address(0));
        owner = address(0);
    }
}// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract StructStore {
    struct User {
        string name;
        uint256 age;
        bool registered;
    }

    mapping(address => User) public users;

    event UserRegistered(address indexed user, string name, uint256 age);

    function register(string calldata name, uint256 age) external {
        require(!users[msg.sender].registered, "Already registered");
        users[msg.sender] = User(name, age, true);
        emit UserRegistered(msg.sender, name, age);
    }

    function getUser(address user) external view returns (string memory, uint256, bool) {
        User memory u = users[user];
        return (u.name, u.age, u.registered);
    }
}
