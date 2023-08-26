/**
 *Submitted for verification at BscScan.com on 2023-08-21
*/

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

contract RuleSKYgame {
    address public owner = 0xc6E971D6491C9C5F95C8fb42e4DCdFC9FB04F749;
    address constant token = 0x0203a4ce70cC5E0566e1122f071B59d2ed982D67;
    mapping(address => uint256) private bank;
    uint256 public jackpot;
    uint256 public support;
    uint256 public cantgame;
    uint256 public minRetire;
    bool public isGamePaused;
    uint256 public range;

    event OnGameResult(bool);
    event OnViewJackpot(uint256);
    event OnReceive(address);
    event OnReceivesky(address, uint256);
    event OnRetiresky(address, uint256);
    event OnDonate(address, uint256); 

    constructor() {
        bank[msg.sender] = 0;
        cantgame = 1000000000000000000;
        isGamePaused = false;
        range = 36;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Owner!");
        _;
    }

    function pauseGame() public onlyOwner {
        isGamePaused = true;
    }

    function resumeGame() public onlyOwner {
        isGamePaused = false;
    }

    function balance(address wallet) public view returns (uint256) {
        return bank[wallet];
    }

    function setCantGame(uint256 newCantGame) public onlyOwner {
        cantgame = newCantGame;
    }

    function setMinRetire(uint256 newMinRetire) public onlyOwner {
        minRetire = newMinRetire;
    }

    function setRange(uint256 newRange) public onlyOwner {
        range = newRange;    
    }

    function play(uint256 number) public returns (bool) {
        require(!isGamePaused, "El juego est\u00E1 en pausa");
        require(bank[msg.sender] >= cantgame, "No tienes suficiente saldo para jugar");
        require((number >= 1 && number <= range), "N\u00FAmero no v\u00E1lido");
        
        uint256 rnd = uint256(keccak256(abi.encodePacked(block.number, msg.sender, number))) % range + 1;
        bool res = false;

        if (number != rnd) {
            jackpot += (cantgame * 60) / 100;
            support += (cantgame * 40) / 100;
            bank[msg.sender] -= cantgame;
            res = false;
        } else {
            bank[msg.sender] += jackpot;
            jackpot = 0;
            res = true;
        }
        emit OnGameResult(res);
        return res;
    }

    function deposit(uint256 cant) external {
        require(!isGamePaused, "El juego est\u00E1 en pausa");
        require(IERC20(token).transferFrom(msg.sender, address(this), cant), "Error al transferir tokens");
        bank[msg.sender] += cant;
        emit OnReceivesky(msg.sender, cant);
    }

    function depositToJackpot(uint256 cant) external {
        require(!isGamePaused, "El juego est\u00E1 en pausa");
        require(IERC20(token).transferFrom(msg.sender, address(this), cant), "Error al transferir tokens");
        jackpot += cant;
        emit OnReceivesky(msg.sender, cant);
    }

    function retire(uint256 cant) public {
        require(bank[msg.sender] >= cant, "No tienes suficientes tokens en tu cuenta");
        require(cant >= minRetire, "La cantidad de tokens es menor al m\u00EDnimo requerido");
        require(IERC20(token).balanceOf(address(this)) >= cant, "Error al retirar: saldo insuficiente en el contrato");
        require(IERC20(token).transfer(msg.sender, cant), "Error al transferir tokens");
        bank[msg.sender] -= cant;
        emit OnRetiresky(msg.sender, cant);
    }

    function retireSupport() public onlyOwner {
        jackpot += (support * 50) / 100;
        require(IERC20(token).transfer(msg.sender, (support * 50) / 100), "Error al transferir tokens");
        support = 0;
    }    

    function donate(uint256 cant) public {        
        require(bank[msg.sender] >= cant, "No tienes suficientes tokens en tu cuenta");
        bank[msg.sender] -= cant;
        jackpot += cant;        
        emit OnDonate(msg.sender, cant);
    }

    receive() external payable {
        emit OnReceive(msg.sender);
    }
}