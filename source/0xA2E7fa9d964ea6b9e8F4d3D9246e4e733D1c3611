// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

contract RulePIPIgame {
    address public owner = 0xAf516750896062Ce2AA596DC99110b051Bcd7067;
    mapping(address => uint256) public bank;
    uint256 public jackpot;
    uint256 public support;
    uint256 public cantgame;
    bool public isGamePaused;

    event OnPause(address);
    event OnResume(address);
    event OnGameResult(bool);
    event OnViewJackpot(uint256);

    constructor() {
        bank[msg.sender] = 500000;
        cantgame = 1000;
        isGamePaused = false;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function pauseGame() public onlyOwner {
        emit OnPause(msg.sender);
        isGamePaused = true;
    }

    function resumeGame() public onlyOwner {
        emit OnResume(msg.sender);
        isGamePaused = false;
    }

    function balance() public view returns (uint256) {
        return bank[msg.sender];
    }

    function getfullJackpot() public returns (uint256) {
        emit OnViewJackpot(jackpot);
        return jackpot;
    }

    function setCantGame(uint256 newCantGame) public onlyOwner {
        cantgame = newCantGame;
    }

    function play(uint256 number) public returns (bool)
    {
        require(!isGamePaused, "El juego esta en pause");
        require(bank[msg.sender] >= cantgame, "No tienes saldo duficiente");
        
        uint256 rnd = uint256(keccak256(abi.encodePacked(block.number, msg.sender, number))) % 100 + 1;
       
        if (number != rnd){
            jackpot += (cantgame * 80) / 100;
            support += (cantgame * 20) / 100;
            bank[msg.sender] -= cantgame;
            emit OnGameResult(false);
            return false;
        }else{
            bank[msg.sender]+=jackpot;
            jackpot=0;
             emit OnGameResult(true);
            return true;
        }        
    }

    function deposit(address addr, uint256 cant) public onlyOwner {
        bank[addr] += cant;
    }
}