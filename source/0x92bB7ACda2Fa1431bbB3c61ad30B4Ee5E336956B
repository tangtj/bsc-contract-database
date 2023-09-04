// SPDX-License-Identifier: MIT
// File: contracts/IMangoSports.sol


pragma solidity ^0.8.0;

interface IMangoSportsClone {
    function initialize(
        address _owner,
        address _token, 
        string memory _equipoA, 
        string memory _equipoB, 
        uint256 _fechaHoraLimite
    ) external;
}

// File: @openzeppelin/contracts/security/ReentrancyGuard.sol


// OpenZeppelin Contracts (last updated v4.8.0) (security/ReentrancyGuard.sol)

pragma solidity ^0.8.0;

/**
 * @dev Contract module that helps prevent reentrant calls to a function.
 *
 * Inheriting from `ReentrancyGuard` will make the {nonReentrant} modifier
 * available, which can be applied to functions to make sure there are no nested
 * (reentrant) calls to them.
 *
 * Note that because there is a single `nonReentrant` guard, functions marked as
 * `nonReentrant` may not call one another. This can be worked around by making
 * those functions `private`, and then adding `external` `nonReentrant` entry
 * points to them.
 *
 * TIP: If you would like to learn more about reentrancy and alternative ways
 * to protect against it, check out our blog post
 * https://blog.openzeppelin.com/reentrancy-after-istanbul/[Reentrancy After Istanbul].
 */
abstract contract ReentrancyGuard {
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.

    // The values being non-zero value makes deployment a bit more expensive,
    // but in exchange the refund on every call to nonReentrant will be lower in
    // amount. Since refunds are capped to a percentage of the total
    // transaction's gas, it is best to keep them low in cases like this one, to
    // increase the likelihood of the full refund coming into effect.
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and making it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        // On the first call to nonReentrant, _status will be _NOT_ENTERED
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}

// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.6.0) (token/ERC20/IERC20.sol)

pragma solidity ^0.8.0;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

// File: contracts/mangoSportsClone.sol


pragma solidity ^0.8.0;



contract mangoSports is ReentrancyGuard {
    address public owner;
    IERC20 public token; // Representa el token, como BUSD.
    uint256 public totalApostado;
    uint256 public totalApostadores;
    mapping(address => uint256) public balances;
    bool public apuestasPausadas;
    uint256 public comisionPorcentaje = 5;
    uint256 public totalApostadoA;
    uint256 public totalApostadoB;
    string public equipoA;
    string public equipoB;
    uint256 public fechaHoraLimite;
    string public equipoGanador;

    enum Opcion {
        A,
        B,
        C
    }

    struct Apuesta {
        Opcion opcion;
        uint256 monto;
    }

    mapping(address => Apuesta) public apuestas;
    address[] public apostadores;

    event Ganador(address ganador, uint256 monto);

    function initialize(
        address _owner,
        address _token,
        string memory _equipoA,
        string memory _equipoB,
        uint256 _fechaHoraLimite
    ) public {
        require(owner == address(0), "Ya ha sido inicializado");

        owner = _owner; // Establece al propietario directamente durante la inicialización.
        token = IERC20(_token);
        equipoA = _equipoA;
        equipoB = _equipoB;
        fechaHoraLimite = _fechaHoraLimite;
        comisionPorcentaje = 5;
    }

    modifier onlyOwner() {
        require(
            msg.sender == owner,
            "Solo el owner puede realizar esta accion"
        );
        _;
    }

    modifier beforeDeadline() {
        require(
            block.timestamp < fechaHoraLimite,
            "La hora limite para apostar ha pasado"
        );
        _;
    }

    function setComisionPorcentaje(uint256 _comisionPorcentaje)
        public
        onlyOwner
    {
        require(
            _comisionPorcentaje <= 5,
            "La comision no puede ser mayor al 5%"
        );
        comisionPorcentaje = _comisionPorcentaje;
    }

    function apostar(Opcion opcionSeleccionada, uint256 monto)
        public
        beforeDeadline
    {
        require(monto > 0, "Monto de apuesta debe ser mayor que 0");
        require(
            apuestas[msg.sender].monto == 0,
            "Ya has realizado una apuesta"
        );

        require(
            opcionSeleccionada == Opcion.A || opcionSeleccionada == Opcion.B,
            "Opcion no valida"
        );

        token.transferFrom(msg.sender, address(this), monto); // Transferencia del token.
        if (opcionSeleccionada == Opcion.A) {
            totalApostadoA += monto;
        } else if (opcionSeleccionada == Opcion.B) {
            totalApostadoB += monto;
        }

        if (apuestas[msg.sender].monto == 0) {
            apostadores.push(msg.sender);
            totalApostadores++;
        }

        apuestas[msg.sender] = Apuesta({
            opcion: opcionSeleccionada,
            monto: apuestas[msg.sender].monto + monto
        });
        totalApostado += monto;
    }

    function elegirGanador(Opcion opcionGanadora) public onlyOwner {
        require(totalApostado > 0, "No hay apuestas realizadas");

        uint256 montoPerdedor;

        if (opcionGanadora == Opcion.A) {
            montoPerdedor = totalApostado - totalApostadoA;
            equipoGanador = equipoA;
        } else if (opcionGanadora == Opcion.B) {
            montoPerdedor = totalApostado - totalApostadoB;
            equipoGanador = equipoB;
        } else if (opcionGanadora == Opcion.C) {
            equipoGanador = "Empate"; 
            // Cuando es un empate
            for (uint256 i = 0; i < apostadores.length; i++) {
                balances[apostadores[i]] += apuestas[apostadores[i]].monto;
            }
            resetState();
            return; // <-- Aquí terminamos la función si es un empate.
        }

        uint256 totalGanadores = 0;
        for (uint256 i = 0; i < apostadores.length; i++) {
            if (apuestas[apostadores[i]].opcion == opcionGanadora) {
                totalGanadores++;
            }
        }

        require(totalGanadores > 0, "No hay ganadores");

        uint256 montoGanador;
        if (opcionGanadora == Opcion.A) {
            montoGanador = totalApostadoA;
        } else {
            montoGanador = totalApostadoB;
        }

        for (uint256 i = 0; i < apostadores.length; i++) {
            if (apuestas[apostadores[i]].opcion == opcionGanadora) {
                uint256 montoApostado = apuestas[apostadores[i]].monto;
                uint256 recompensaGanancia = (montoApostado * montoPerdedor) /
                    montoGanador;
                balances[apostadores[i]] += (montoApostado +
                    recompensaGanancia);
                emit Ganador(apostadores[i], recompensaGanancia);
            }
        }

        resetState();
    }

    function resetState() private {
        totalApostadores = 0;
        totalApostado = 0;
        totalApostadoA = 0;
        totalApostadoB = 0;
    }

    function retirarGanancias() public nonReentrant {
        uint256 cantidad = balances[msg.sender];
        require(cantidad > 0, "No tienes ganancias disponibles para retirar");

        uint256 comision = (cantidad * comisionPorcentaje) / 100;
        uint256 cantidadNeta = cantidad - comision;

        balances[msg.sender] = 0;
        token.transfer(owner, comision);
        token.transfer(msg.sender, cantidadNeta);
    }

    function emergencyButton() public onlyOwner {
        uint256 balance = token.balanceOf(address(this));
        token.transfer(owner, balance);

        for (uint256 i = 0; i < apostadores.length; i++) {
            delete apuestas[apostadores[i]];
        }
        delete apostadores;
        totalApostadores = 0;
        totalApostado = 0;
    }
}

// File: @openzeppelin/contracts/proxy/Clones.sol


// OpenZeppelin Contracts (last updated v4.9.0) (proxy/Clones.sol)

pragma solidity ^0.8.0;

/**
 * @dev https://eips.ethereum.org/EIPS/eip-1167[EIP 1167] is a standard for
 * deploying minimal proxy contracts, also known as "clones".
 *
 * > To simply and cheaply clone contract functionality in an immutable way, this standard specifies
 * > a minimal bytecode implementation that delegates all calls to a known, fixed address.
 *
 * The library includes functions to deploy a proxy using either `create` (traditional deployment) or `create2`
 * (salted deterministic deployment). It also includes functions to predict the addresses of clones deployed using the
 * deterministic method.
 *
 * _Available since v3.4._
 */
library Clones {
    /**
     * @dev Deploys and returns the address of a clone that mimics the behaviour of `implementation`.
     *
     * This function uses the create opcode, which should never revert.
     */
    function clone(address implementation) internal returns (address instance) {
        /// @solidity memory-safe-assembly
        assembly {
            // Cleans the upper 96 bits of the `implementation` word, then packs the first 3 bytes
            // of the `implementation` address with the bytecode before the address.
            mstore(0x00, or(shr(0xe8, shl(0x60, implementation)), 0x3d602d80600a3d3981f3363d3d373d3d3d363d73000000))
            // Packs the remaining 17 bytes of `implementation` with the bytecode after the address.
            mstore(0x20, or(shl(0x78, implementation), 0x5af43d82803e903d91602b57fd5bf3))
            instance := create(0, 0x09, 0x37)
        }
        require(instance != address(0), "ERC1167: create failed");
    }

    /**
     * @dev Deploys and returns the address of a clone that mimics the behaviour of `implementation`.
     *
     * This function uses the create2 opcode and a `salt` to deterministically deploy
     * the clone. Using the same `implementation` and `salt` multiple time will revert, since
     * the clones cannot be deployed twice at the same address.
     */
    function cloneDeterministic(address implementation, bytes32 salt) internal returns (address instance) {
        /// @solidity memory-safe-assembly
        assembly {
            // Cleans the upper 96 bits of the `implementation` word, then packs the first 3 bytes
            // of the `implementation` address with the bytecode before the address.
            mstore(0x00, or(shr(0xe8, shl(0x60, implementation)), 0x3d602d80600a3d3981f3363d3d373d3d3d363d73000000))
            // Packs the remaining 17 bytes of `implementation` with the bytecode after the address.
            mstore(0x20, or(shl(0x78, implementation), 0x5af43d82803e903d91602b57fd5bf3))
            instance := create2(0, 0x09, 0x37, salt)
        }
        require(instance != address(0), "ERC1167: create2 failed");
    }

    /**
     * @dev Computes the address of a clone deployed using {Clones-cloneDeterministic}.
     */
    function predictDeterministicAddress(
        address implementation,
        bytes32 salt,
        address deployer
    ) internal pure returns (address predicted) {
        /// @solidity memory-safe-assembly
        assembly {
            let ptr := mload(0x40)
            mstore(add(ptr, 0x38), deployer)
            mstore(add(ptr, 0x24), 0x5af43d82803e903d91602b57fd5bf3ff)
            mstore(add(ptr, 0x14), implementation)
            mstore(ptr, 0x3d602d80600a3d3981f3363d3d373d3d3d363d73)
            mstore(add(ptr, 0x58), salt)
            mstore(add(ptr, 0x78), keccak256(add(ptr, 0x0c), 0x37))
            predicted := keccak256(add(ptr, 0x43), 0x55)
        }
    }

    /**
     * @dev Computes the address of a clone deployed using {Clones-cloneDeterministic}.
     */
    function predictDeterministicAddress(
        address implementation,
        bytes32 salt
    ) internal view returns (address predicted) {
        return predictDeterministicAddress(implementation, salt, address(this));
    }
}

// File: contracts/sportFactory.sol


pragma solidity ^0.8.0;




contract MangoSportsFactory {
    address[] public partidos;
    address public owner;
    address public mangoSportsTemplate;

    event PartidoCreado(address indexed partidoAddress);

    constructor(address _mangoSportsTemplate) {
        owner = msg.sender;
        mangoSportsTemplate = _mangoSportsTemplate;
    }

    modifier onlyOwner() {
        require(
            msg.sender == owner,
            "Solo el owner puede realizar esta accion"
        );
        _;
    }

    function crearPartido(
        address _token,
        string memory _equipoA,
        string memory _equipoB,
        uint256 _fechaHoraLimite
    ) public onlyOwner returns (address) {
        address clone = Clones.clone(mangoSportsTemplate);
        IMangoSportsClone(clone).initialize(
            msg.sender,
            _token,
            _equipoA,
            _equipoB,
            _fechaHoraLimite
        );
        partidos.push(clone);
        emit PartidoCreado(clone);
        return clone;
    }

    function totalPartidos() public view returns (uint256) {
        return partidos.length;
    }

    function setNewTemplate(address _newTemplate) public onlyOwner {
    require(_newTemplate != address(0), "Direccion del nuevo template no puede ser la direccion cero");
    mangoSportsTemplate = _newTemplate;
}

}