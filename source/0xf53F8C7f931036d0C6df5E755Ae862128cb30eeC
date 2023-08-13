// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);

    function transfer(address to, uint256 value) external returns (bool);
    function approve(address spender, uint256 value) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
}

contract RulePIPIgame {

    address public owner = 0xAf516750896062Ce2AA596DC99110b051Bcd7067; //dirección de Ethereum que representa al propietario del contrato.
    address constant token = 0xf86E639Ff387b6064607201A7a98F2c2B2FEB05f; //dirección del token ERC-20 utilizado en el juego.
    mapping(address => uint256) private bank; //un mapeo que asocia direcciones de Ethereum con cantidades de tokens en el banco de cada jugador.
    uint256 public jackpot; //cantidad de tokens acumulados en el bote del juego.
    uint256 public support; //cantidad de tokens acumulados en el fondo de soporte del juego.
    uint256 public cantgame; //cantidad de tokens que se requieren para jugar una ronda del juego.
    uint256 public minRetire; //cantidad mínima de tokens que un jugador puede retirar de su cuenta bancaria.
    bool public isGamePaused; //indica si el juego está en pausa o no.

    // Esto son los eventos que se disparan para que quede registrado en la cadena de bloques.
    event OnGameResult(bool);
    event OnViewJackpot(uint256);
    event OnReceive(address);
    event OnRecivePipi(address,uint256);
    event OnRetirePipi(address,uint256);

    constructor() {
        //owner = msg.sender;
        bank[msg.sender] = 0;
        cantgame = 1000000000000000000;
        isGamePaused = false;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Owner!");
        _;
    }

    // función que pausa el juego. Solo puede ser llamada por el propietario del contrato.
    function pauseGame() public onlyOwner {
        isGamePaused = true;
    }

    // función que reanuda el juego. Solo puede ser llamada por el propietario del contrato.
    function resumeGame() public onlyOwner {
        isGamePaused = false;
    }

    // función que devuelve la cantidad de tokens en el banco de un jugador.
    function balance(address wallet) public view returns (uint256) {
        return bank[wallet];
    }

    // función que devuelve la cantidad total de tokens acumulados en el bote del juego.
    function getfullJackpot() public returns (uint256) {
        emit OnViewJackpot(jackpot);
        return jackpot;
    }

    // función que establece la cantidad de tokens necesarios para jugar una ronda del juego. 
    // Solo puede ser llamada por el propietario del contrato.
    function setCantGame(uint256 newCantGame) public onlyOwner {
        cantgame = newCantGame;
    }

    // función que establece la cantidad mínima de tokens que un jugador puede retirar de su cuenta bancaria. 
    // Solo puede ser llamada por el propietario del contrato.
    function setMinRetire(uint256 newMinRetire) public onlyOwner {
        minRetire = newMinRetire;
    }

    // función que permite a un jugador jugar una ronda del juego. El jugador debe tener suficientes tokens en su cuenta bancaria para jugar. 
    // Si el número elegido por el jugador no coincide con un número aleatorio generado por el contrato, el 80% de la cantidad de tokens necesarios para jugar se agregan al bote del juego y el 20% se agrega al fondo de soporte. 
    // Si los números coinciden, el jugador gana el bote del juego. 
    // La función devuelve un valor booleano que indica si el jugador ganó o no.
    function play(uint256 number) public returns (bool)
    {
        require(!isGamePaused, "El juego esta en pause");
        require(bank[msg.sender] >= cantgame, "No tienes saldo duficiente");
        require((number >= 1 && number <= 100),"numero entre 1-100");
                
        //uint256 rnd = uint256(keccak256(abi.encodePacked(block.number, msg.sender, number))) % 100 + 1;
        uint256 rnd = 10;

        bool res = false;
        if (number != rnd){
            jackpot += (cantgame * 80) / 100;
            support += (cantgame * 20) / 100;
            bank[msg.sender] -= cantgame;
            res = false;
        }else{
            bank[msg.sender] += jackpot;
            jackpot = 0;            
            res = true;
        }   
        emit OnGameResult(res);    
        return res; 
    }

    // función que permite a un jugador depositar tokens en su cuenta bancaria. 
    // Los tokens se transfieren desde la dirección del jugador al contrato. 
    // La función emite un evento OnRecivePipi.
    function deposit(uint256 cant) external  {
        require(!isGamePaused, "El juego esta en pause");
        require(IERC20(token).transferFrom(msg.sender, address(this), cant), "f");
        bank[msg.sender] += cant;
        emit OnRecivePipi(msg.sender,cant);
    }


    // función que permite a un jugador retirar tokens de su cuenta bancaria. 
    // Los tokens se transfieren desde el contrato a la dirección del jugador. 
    // La función emite un evento OnRetirePipi.
    function retire(uint256 cant) public {
        require(bank[msg.sender] >= cant);
        require(cant >= minRetire);
        require(IERC20(token).balanceOf(address(this)) > cant,"f");
        require(IERC20(token).transfer(msg.sender, cant), "f");
        bank[msg.sender] -= cant;
        emit OnRetirePipi(msg.sender, cant);        
    }

    // función que permite al propietario del contrato retirar la mitad de los tokens acumulados en el fondo de soporte. 
    // La otra mitad se agrega al bote del juego. La función emite un evento OnRetirePipi.
    function retireSuport() public onlyOwner {
        jackpot += (support * 50) / 100;
        require(IERC20(token).transfer(msg.sender, ((support * 50) / 100)), "f");
        support = 0;
    }

    // función que se llama cuando se envían tokens directamente al contrato. 
    // La función emite un evento OnReceive.
    receive() external payable {
        emit OnReceive(msg.sender);
    }
}