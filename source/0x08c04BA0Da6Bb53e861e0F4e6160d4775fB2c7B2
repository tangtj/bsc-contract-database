// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract DiziGoldCoin {
    string public name = "Dizi Gold Coin";
    string public symbol = "DGC";
    uint8 public decimals = 18;
    uint256 public totalSupply;
    uint256 private _maxTransactionAmount = 1000000 * 10**18;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    mapping(address => bool) public isBlacklisted;
    mapping(address => bool) public isExcludedFromFee;
    mapping(address => bool) public isExcludedFromReward;
    mapping(address => uint256) private _rewardTokenHolders;
    mapping(address => uint256) private _lastRewardClaimed;
    address[] private _rewardTokenHolderAddresses;  // New array to store reward token holders

    uint256 private _taxPercentage = 5;
    uint256 private _burnPercentage = 20;
    uint256 private _liquidityPercentage = 30;
    uint256 private _rewardPercentage = 50;
    uint256 private _totalTokensToDistribute;

    bool private _tradingEnabled = true;
    bool private _contractLocked = false;
    address private _owner;

    modifier onlyOwner() {
        require(msg.sender == _owner, "Not the contract owner");
        _;
    }

    modifier canTrade() {
        require(_tradingEnabled && !_contractLocked, "Trading is disabled or contract is locked");
        _;
    }

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor(uint256 initialSupply) {
        totalSupply = initialSupply * 10**uint256(decimals);
        balanceOf[msg.sender] = totalSupply;
        _owner = msg.sender;
        emit Transfer(address(0), msg.sender, totalSupply);
    }

    function approve(address spender, uint256 amount) external canTrade returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transfer(address recipient, uint256 amount) external canTrade returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external canTrade returns (bool) {
        require(amount <= allowance[sender][msg.sender], "Allowance exceeded");
        allowance[sender][msg.sender] -= amount;
        _transfer(sender, recipient, amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) external canTrade returns (bool) {
        allowance[msg.sender][spender] += addedValue;
        emit Approval(msg.sender, spender, allowance[msg.sender][spender]);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) external canTrade returns (bool) {
        uint256 currentAllowance = allowance[msg.sender][spender];
        require(subtractedValue <= currentAllowance, "Allowance cannot be negative");
        allowance[msg.sender][spender] = currentAllowance - subtractedValue;
        emit Approval(msg.sender, spender, allowance[msg.sender][spender]);
        return true;
    }

    function blacklistAddress(address account) external onlyOwner {
        isBlacklisted[account] = true;
    }

    function unblacklistAddress(address account) external onlyOwner {
        isBlacklisted[account] = false;
    }

    function enableTrading() external onlyOwner {
        _tradingEnabled = true;
    }

    function disableTrading() external onlyOwner {
        _tradingEnabled = false;
    }

    function lockContract() external onlyOwner {
        _contractLocked = true;
    }

    function unlockContract() external onlyOwner {
        _contractLocked = false;
    }

    function setTaxPercentage(uint256 taxPercentage) external onlyOwner {
        _taxPercentage = taxPercentage;
    }

    function setMaxTransactionAmount(uint256 maxTransactionAmount) external onlyOwner {
        require(maxTransactionAmount > 0, "Invalid amount");
        _maxTransactionAmount = maxTransactionAmount;
    }

    function setExcludedFromFee(address account, bool isExcluded) external onlyOwner {
        isExcludedFromFee[account] = isExcluded;
    }

    function setExcludedFromReward(address account, bool isExcluded) external onlyOwner {
        isExcludedFromReward[account] = isExcluded;
    }

    function setRewardForHolders(address[] memory holders, uint256[] memory amounts) external onlyOwner {
        require(holders.length == amounts.length, "Arrays length mismatch");

        for (uint256 i = 0; i < holders.length; i++) {
            require(holders[i] != address(0), "Invalid address");
            _rewardTokenHolders[holders[i]] = amounts[i];
        }
    }

    function claimRewards() external {
    require(!isExcludedFromReward[msg.sender], "Excluded from reward");
    require(_rewardTokenHolders[msg.sender] > 0, "No rewards to claim");
    require(_lastRewardClaimed[msg.sender] + 1 days <= block.timestamp, "Wait for the next claim time");

    uint256 rewardAmount = (balanceOf[msg.sender] * _rewardTokenHolders[msg.sender]) / _totalTokensToDistribute;
    balanceOf[msg.sender] += rewardAmount;
    _lastRewardClaimed[msg.sender] = block.timestamp;

    emit Transfer(address(this), msg.sender, rewardAmount);
}

    function burnFromWallet(uint256 amount) external {
        require(amount > 0, "Invalid amount");
        require(balanceOf[msg.sender] >= amount, "Insufficient balance");
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }

    function _transfer(address sender, address recipient, uint256 amount) private {
        require(sender != address(0), "Transfer from the zero address");
        require(recipient != address(0), "Transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        require(amount <= _maxTransactionAmount, "Exceeds max transaction amount");
        require(!isBlacklisted[sender] && !isBlacklisted[recipient], "Blacklisted address");

        uint256 taxAmount = (amount * _taxPercentage) / 100;
        uint256 burnAmount = (taxAmount * _burnPercentage) / 100;
        uint256 liquidityAmount = (taxAmount * _liquidityPercentage) / 100;
        uint256 rewardAmount = (taxAmount * _rewardPercentage) / 100;
        uint256 transferAmount = amount - taxAmount;

        balanceOf[sender] -= amount;
        balanceOf[recipient] += transferAmount;

        // Burn tokens
        totalSupply -= burnAmount;
        emit Transfer(sender, address(0), burnAmount);

        // TODO: Add liquidity to liquidity pool (not implemented in this example)

        // Distribute rewards to holders
        _totalTokensToDistribute = totalSupply - burnAmount;
        distributeRewards(rewardAmount);

        emit Transfer(sender, recipient, transferAmount);
    }

 function distributeRewards(uint256 rewardAmount) private {
    uint256 totalRewardDistributed = 0;

    for (uint256 i = 0; i < _rewardTokenHolderAddresses.length; i++) {
        address holder = _rewardTokenHolderAddresses[i];
        uint256 holderRewardShare = (rewardAmount * _rewardTokenHolders[holder]) / _totalTokensToDistribute;

        if (holderRewardShare > 0 && balanceOf[holder] >= holderRewardShare) {
            balanceOf[holder] += holderRewardShare;
            totalRewardDistributed += holderRewardShare;
            emit Transfer(address(this), holder, holderRewardShare);
        }
    }

    // Distribute remaining rewards to contract owner
    balanceOf[_owner] += (rewardAmount - totalRewardDistributed);
    emit Transfer(address(this), _owner, (rewardAmount - totalRewardDistributed));
}



    // TODO: Add functions for max buy and sell limit, multisend tokens, and transfer ownership
}