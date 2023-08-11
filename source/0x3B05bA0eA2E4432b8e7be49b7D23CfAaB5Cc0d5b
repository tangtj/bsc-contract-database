// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

contract RulePIPIgame {
    address public owner = 0xAf516750896062Ce2AA596DC99110b051Bcd7067;
    mapping(address => uint256) public bank;
    uint256 public jackpot;
    uint256 public gan;
    bool public isGamePaused;

    event OnPause(address);
    event OnResume(address);
    event OnGameResult(bool);
    event OnViewJackpot(uint256);

    constructor() {
        bank[msg.sender] = 500000;
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

    function play(uint256 number) public returns (bool)
    {
        require(bank[msg.sender] >= 1000, "Tienes que tener mas de 1000");
        
        uint256 rnd = 50;
       
        if (number != rnd){
            jackpot += 800;
            gan += 200;
            bank[msg.sender] -= 1000;
            emit OnGameResult(false);
            return false;
        }else{
            bank[msg.sender]+=800;
            gan += 200;
            jackpot=0;
             emit OnGameResult(true);
            return true;
        }

        
    }

}