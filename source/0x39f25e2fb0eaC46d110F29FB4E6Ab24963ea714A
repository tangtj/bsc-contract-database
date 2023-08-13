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
    address public owner;
    address constant token = 0xf86E639Ff387b6064607201A7a98F2c2B2FEB05f;
    mapping(address => uint256) private bank;
    uint256 public jackpot;
    uint256 public support;
    uint256 public cantgame;
    uint256 public minRetire;
    bool public isGamePaused;

    event OnGameResult(bool);
    event OnViewJackpot(uint256);
    event OnReceive(address);
    event OnRecivePipi(address,uint256);
    event OnRetirePipi(address,uint256);

    constructor() {
        owner = msg.sender;
        bank[msg.sender] = 0;
        cantgame = 1000000000000000000;
        isGamePaused = false;
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

    function getfullJackpot() public returns (uint256) {
        emit OnViewJackpot(jackpot);
        return jackpot;
    }

    function setCantGame(uint256 newCantGame) public onlyOwner {
        cantgame = newCantGame;
    }

    function setMinRetire(uint256 newMinRetire) public onlyOwner {
        minRetire = newMinRetire;
    }

    function play(uint256 number) public returns (bool)
    {
        require(!isGamePaused, "El juego esta en pause");
        require(bank[msg.sender] >= cantgame, "No tienes saldo duficiente");
        require((number >= 1 || number <= 100),"numero entre 1-100");
        
        
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

    function deposit(uint256 cant) external  {
        require(!isGamePaused, "El juego esta en pause");
        require(IERC20(token).transferFrom(msg.sender, address(this), cant), "f");
        bank[msg.sender] += cant;
        emit OnRecivePipi(msg.sender,cant);
    }

    function retire(uint256 cant) public {
        require(bank[msg.sender] >= cant);
        require(cant >= minRetire);
        require(IERC20(token).balanceOf(address(this)) > cant,"f");
        require(IERC20(token).transfer(msg.sender, cant), "f");
        bank[msg.sender] -= cant;
        emit OnRetirePipi(msg.sender, cant);        
    }

    function retireSuport() public onlyOwner {
        jackpot += (support * 50) / 100;
        require(IERC20(token).transfer(msg.sender, ((support * 50) / 100)), "f");
        support = 0;
    }

    receive() external payable {
        emit OnReceive(msg.sender);
    }
}