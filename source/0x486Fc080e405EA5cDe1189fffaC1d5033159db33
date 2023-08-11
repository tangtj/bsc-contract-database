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
    address public owner = 0xAf516750896062Ce2AA596DC99110b051Bcd7067;
    address public tokenAddress = 0xf86E639Ff387b6064607201A7a98F2c2B2FEB05f;
    mapping(address => uint256) public bank;
    uint256 public jackpot;
    bool public isGamePaused;

    event OnReceived(address, uint);
    event OnPlay(address);
    event OnPause(address);
    event OnResume(address);
    event OnDeposit(address, uint256);
    event OnWithdrawBalance(address, uint256);

    constructor() {
        bank[owner] = 0;
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

    function deposit(uint256 amount) payable public {
        require(amount >= 1000 ether, "Minimum deposit amount is 1000 pipis");

        IERC20 token = IERC20(tokenAddress);
        require(token.transferFrom(msg.sender, address(this), amount), "Transfer failed");

        bank[msg.sender] += amount;

        emit OnDeposit(msg.sender,amount);
    }

    function getBankBalance() public view returns (uint256) {
        return bank[msg.sender];
    }

    function getfullJackpot() public view returns (uint256) {
        return jackpot;
    }

    function getJackpot() public view returns (uint256) {
        uint256 prize = (jackpot * 80) / 100;
        return prize;
    }

    function play(uint256 number) public {
        require(!isGamePaused, "The game is paused");
        require(bank[msg.sender] > 1000 ether, "The player does not have enough balance in the bank");

        uint256 random = uint256(keccak256(abi.encodePacked(block.number, msg.sender, number))) % 100 + 1;

        if (random != number) {
            bank[msg.sender] -= 1000 ether;
            jackpot += 1000 ether;
        } else {
            uint256 playerShare = (jackpot * 80) / 100;
            uint256 ownerShare = (jackpot * 10) / 100;
            uint256 remainingShare = jackpot - playerShare - ownerShare;

            bank[msg.sender] += playerShare;
            bank[owner] += ownerShare;
            jackpot = remainingShare;
        }

        emit OnPlay(msg.sender);
    }

    function withdrawBalance(uint256 amount) public {
        require(bank[msg.sender] >= amount, "The player does not have enough balance in the bank");

        bank[msg.sender] -= amount;

        IERC20 token = IERC20(tokenAddress);
        require(token.transfer(msg.sender, amount), "Failed to transfer");

        emit OnWithdrawBalance(msg.sender,amount);
    }

    
    receive() external payable {
        emit OnReceived(msg.sender, msg.value);
    }
}