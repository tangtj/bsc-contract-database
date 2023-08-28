// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface Token {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 value) external returns (bool);
    function allowance(address owner, address spender) external returns (uint256);
}

contract DomesticPVP {
    address public owner;
    Token public token;

    // Percentage of total amount to be allocated to the reward pool
    uint256 private REWARD_POOL_FEE_PERCENTAGE = 5;
    // Percentage of total amount to be allocated to the liquidity
    uint256 private LIQUIDITY_FEE_PERCENTAGE = 0;
    // Percentage of total amount to be allocated to the development team
    uint256 private DEV_FEE_PERCENTAGE = 5;

    // The fee for creating a battle
    uint256 private BATTLE_FEE = 0.001 ether;
    uint256 private MAX_SEATS = 100;
    uint256 private MIN_SEATS = 2;
    uint256 private MIN_BET = 100;

    address public rewardPoolWallet = address(0xBFf6241a60e611f2bB7659c029d64a6a70764386);
    address public liquidityWallet = address(0x9F109A85eC8f181F7a0471833050D63489c93A47);
    address public devWallet = address(0x65995F63f41B21D6410E1b165CB0AEb591096fb1);

    event UpdateTokenContract(address indexed newTokenContract);
    event UpdateWallets(address indexed newRewardPoolWallet, address indexed newLiquidityWallet, address indexed newDevWallet);
    event CreateBattle(uint256 indexed battleId, address indexed creator, uint256 ticketPrice, uint256 totalTickets, uint256 _playerTickets);
    event JoinBattle(uint256 indexed battleId, address indexed player, uint256 ticketCount);
    event PayWinner(uint256 indexed battleId, address indexed player, uint256 amount);
    event UpdateBattleFee(uint256 battleFee);
    event UpdateFees(uint256 rewardPoolFee, uint256 liquidityFee, uint256 devFee);
    event UpdateMinMax(uint256 minSeats, uint256 maxSeats, uint256 minBet);

    struct Battle {
        uint256 id;
        address creator;
        uint256 totalSeats;
        uint256 seatPrice;
        uint256 occupiedSeats;
        mapping(address => uint256) seatBalances;
    }

    mapping(uint256 => Battle) public battles;
    mapping(uint256 => address) public winners;

    uint256 public battleCount;

    constructor() {
        owner = msg.sender;
        token = Token(address(0x9F109A85eC8f181F7a0471833050D63489c93A47));
        battleCount = 1;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the contract owner can call this function");
        _;
    }

    function createBattle(uint256 _totalSeats, uint256 _seatPrice, uint256 _seatsToBuy) external payable returns(uint256) {
        require(msg.value >= BATTLE_FEE, "Insufficient BNB sent for gas costs");
        require(_seatPrice >= MIN_BET, "The seat price is to low");
        require(_totalSeats > 0 && _seatPrice > 0 && _seatsToBuy > 0, "Invalid inputs");

        _seatPrice = _seatPrice * 10 ** 18;

        require(_totalSeats <= MAX_SEATS, "To many total seats chosen");
        require(_totalSeats >= MIN_SEATS, "To low total seats chosen");

        Battle storage newBattle = battles[battleCount];
        newBattle.id = battleCount;
        newBattle.creator = msg.sender;
        newBattle.totalSeats = _totalSeats;
        newBattle.seatPrice = _seatPrice;
        newBattle.occupiedSeats = _seatsToBuy;
        newBattle.seatBalances[msg.sender] = _seatsToBuy;

        uint256 totalPrice = _seatPrice * _seatsToBuy;

        require(token.allowance(msg.sender, address(this)) >= totalPrice, "The contract needs to be approved");
        require(token.transferFrom(msg.sender, address(this), totalPrice), "Token transfer failed");

        battleCount++;

        // Refund excess Ether if more than the battle fee was sent
        if (msg.value > BATTLE_FEE) {
            payable(msg.sender).transfer(msg.value - BATTLE_FEE);
        }

        emit CreateBattle(newBattle.id, msg.sender, _seatPrice, _totalSeats, _seatsToBuy);

        return newBattle.id;
    }

    function joinBattle(uint256 _battleId, uint256 _seatsToBuy) external {
        require(_battleId < battleCount, "Invalid battle ID");
        Battle storage battle = battles[_battleId];
        require(_seatsToBuy > 0 && battle.occupiedSeats + _seatsToBuy <= battle.totalSeats, "There is not enought seats left");

        uint256 cost = battle.seatPrice * _seatsToBuy;

        require(token.allowance(msg.sender, address(this)) >= cost, "The contract needs to be approved");
        require(token.transferFrom(msg.sender, address(this), cost), "Token transfer failed");
        
        battle.occupiedSeats += _seatsToBuy;
        battle.seatBalances[msg.sender] += _seatsToBuy;

        emit JoinBattle(_battleId, msg.sender, _seatsToBuy);
    }

    ////////////////////////////////////////////////////////////////////////////
    // Admin functions
    function updateTokenContract(address _newTokenContract) external onlyOwner {
        token = Token(_newTokenContract);
        emit UpdateTokenContract(_newTokenContract);
    }

    function chooseWinner(uint256 _battleId, address _winner) external onlyOwner {
        require(winners[_battleId] == address(0), "Winner already paid");
        require(_battleId < battleCount, "Invalid battle ID");
        Battle storage battle = battles[_battleId];
        require(battle.seatBalances[_winner] > 0, "Invalid winner");

        uint256 totalAmount = battle.occupiedSeats * battle.seatPrice;
        uint256 winnerAmount = totalAmount;

        // Distribute fees
        if (LIQUIDITY_FEE_PERCENTAGE > 0) {
            uint256 liquidityAmount = (totalAmount * LIQUIDITY_FEE_PERCENTAGE) / 100;
            winnerAmount -= liquidityAmount;
            token.transfer(liquidityWallet, liquidityAmount);
        }
        if (REWARD_POOL_FEE_PERCENTAGE > 0) {
            uint256 rewardPoolAmount = (totalAmount * REWARD_POOL_FEE_PERCENTAGE) / 100;
            winnerAmount -= rewardPoolAmount;
            token.transfer(rewardPoolWallet, rewardPoolAmount);
        }
        if (DEV_FEE_PERCENTAGE > 0) {
            uint256 devAmount = (totalAmount * DEV_FEE_PERCENTAGE) / 100;
            winnerAmount -= devAmount;
            token.transfer(devWallet, devAmount);
        }

        // Pay the winner
        token.transfer(_winner, winnerAmount);

        winners[battle.id] = _winner;

        emit PayWinner(_battleId, _winner, winnerAmount);
    }

    function recoverTokens(address tokenAddress, uint256 amount) external onlyOwner {
        require(tokenAddress != address(0), "Token address not set");
        require(amount > 0, "Amount must be greater than 0");

        Token t = Token(tokenAddress);
        uint256 contractBalance = t.balanceOf(address(this));

        require(contractBalance >= amount, "Insufficient contract balance");

        bool success = t.transfer(owner, amount);
        require(success, "Token transfer failed");
    }

    function recoverEther(uint256 amount) external onlyOwner {
        require(amount > 0, "Amount must be greater than 0");
        require(address(this).balance >= amount, "Insufficient contract balance");

        (bool success, ) = payable(owner).call{value: amount}("");
        require(success, "Ether transfer failed");
    }

    function updateBattleFee(uint256 _newBattleFee) external onlyOwner {
        BATTLE_FEE = _newBattleFee;

        emit UpdateBattleFee(BATTLE_FEE);
    }

    function updateWallets(
        address _newRewardPoolWallet,
        address _newLiquidityWallet,
        address _newDevWallet
    ) external onlyOwner {
        rewardPoolWallet = _newRewardPoolWallet;
        liquidityWallet = _newLiquidityWallet;
        devWallet = _newDevWallet;

        emit UpdateWallets(_newRewardPoolWallet, _newLiquidityWallet, _newDevWallet);
    }

    function updateFees(
        uint256 _newRewardPoolFee,
        uint256 _newLiquidityFee,
        uint256 _newDevFee
    ) external onlyOwner {
        REWARD_POOL_FEE_PERCENTAGE = _newRewardPoolFee;
        LIQUIDITY_FEE_PERCENTAGE = _newLiquidityFee;
        DEV_FEE_PERCENTAGE = _newDevFee;

        emit UpdateFees(REWARD_POOL_FEE_PERCENTAGE, LIQUIDITY_FEE_PERCENTAGE, DEV_FEE_PERCENTAGE);
    }

    function updateMinMax(
        uint256 _newMinSeats,
        uint256 _newMaxSeats,
        uint256 _newMinBet
    ) external onlyOwner {
        MIN_SEATS = _newMinSeats;
        MAX_SEATS = _newMaxSeats;
        MIN_BET = _newMinBet;

        emit UpdateMinMax(MIN_SEATS, MAX_SEATS, MIN_BET);
    }

    function getBattleCount() external view returns (uint256) {
        return battleCount;
    }
}