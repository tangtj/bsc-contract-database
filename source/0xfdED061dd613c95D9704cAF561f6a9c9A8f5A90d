// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract DiziGoldCoin {
    address public owner;

    modifier onlyOwner() {
        require(msg.sender == owner, 'Caller is not the owner');
        _;
    }

    uint256 public constant MAX_SUPPLY = 11_000_000 * 10 ** 18;
    uint256 public constant HOLDERS_REWARD_THRESHOLD = 10_000 * 10 ** 18;
    uint256 public constant MAX_HOLDING_LIMIT = 10_000 * 10 ** 18;

    uint256 public buyTaxRate = 5;     // 5% buy tax
    uint256 public sellTaxRate = 5;    // 5% sell tax
    uint256 public burnRate = 1;       // 1% burn
    uint256 public liquidityRate = 1;  // 1% liquidity
    uint256 public holderRewardRate = 3; // 3% reward to holders with < 10,000 tokens

    address public liquidityPool;
    address public rewardsPool;
    bool public isBuyEnabled = true;
    bool public isSellEnabled = true;

    mapping(address => bool) public blacklist;
    mapping(address => bool) public whitelist;
    mapping(address => uint256) public rewardsBalance;
    mapping(address => uint256) public buyLimit;  // Buy limit per address
    mapping(address => uint256) public sellLimit; // Sell limit per address

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    uint256 private _totalSupply;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor() {
        owner = msg.sender;
        _mint(msg.sender, MAX_SUPPLY);
    }

    function _mint(address account, uint256 amount) internal {
        require(account != address(0), 'BEP20: mint to the zero address');
        _beforeTokenTransfer(address(0), account, amount);

        uint256 accountBalance = _balances[account];
        uint256 newBalance = accountBalance + amount;
        _balances[account] = newBalance;
        _totalSupply += amount;

        emit Transfer(address(0), account, amount);
    }

    function transferOwnership(address newOwner) external onlyOwner {
        require(newOwner != address(0), 'Invalid address');
        owner = newOwner;
    }

    function setLiquidityPool(address _liquidityPool) external onlyOwner {
        liquidityPool = _liquidityPool;
    }

    function setRewardsPool(address _rewardsPool) external onlyOwner {
        rewardsPool = _rewardsPool;
    }

    function setTaxRates(uint256 _buyTaxRate, uint256 _sellTaxRate) external onlyOwner {
        require(_buyTaxRate <= 100 && _sellTaxRate <= 100, 'Invalid tax rate');
        buyTaxRate = _buyTaxRate;
        sellTaxRate = _sellTaxRate;
    }

    function setRewardsRates(uint256 _burnRate, uint256 _liquidityRate, uint256 _holderRewardRate) external onlyOwner {
        require(_burnRate + _liquidityRate + _holderRewardRate <= 100, 'Invalid rewards rates');
        burnRate = _burnRate;
        liquidityRate = _liquidityRate;
        holderRewardRate = _holderRewardRate;
    }

    function addToBlacklist(address _address) external onlyOwner {
        blacklist[_address] = true;
    }

    function removeFromBlacklist(address _address) external onlyOwner {
        blacklist[_address] = false;
    }

    function addToWhitelist(address _address) external onlyOwner {
        whitelist[_address] = true;
    }

    function removeFromWhitelist(address _address) external onlyOwner {
        whitelist[_address] = false;
    }

    function calculateRewards(address _address) internal view returns (uint256) {
        if (balanceOf(_address) >= HOLDERS_REWARD_THRESHOLD) {
            return 0;
        }

        uint256 balance = balanceOf(_address);
        return balance * holderRewardRate / 100;
    }

    function updateRewards(address _recipient) internal {
        if (rewardsBalance[_recipient] > 0) {
            uint256 rewards = rewardsBalance[_recipient];
            rewardsBalance[_recipient] = 0;
            _transfer(rewardsPool, _recipient, rewards);
        }
    }

    function withdrawRewards() external {
        address recipient = msg.sender;
        require(rewardsBalance[recipient] > 0, 'No rewards to withdraw');

        uint256 rewards = rewardsBalance[recipient];
        rewardsBalance[recipient] = 0;
        _transfer(rewardsPool, recipient, rewards);
    }

    function setBuyEnabled(bool _isBuyEnabled) external onlyOwner {
        isBuyEnabled = _isBuyEnabled;
    }

    function setSellEnabled(bool _isSellEnabled) external onlyOwner {
        isSellEnabled = _isSellEnabled;
    }

    function setBuyLimit(address _address, uint256 _buyLimit) external onlyOwner {
        buyLimit[_address] = _buyLimit;
    }

    function setSellLimit(address _address, uint256 _sellLimit) external onlyOwner {
        sellLimit[_address] = _sellLimit;
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal {
        require(!blacklist[sender] && !blacklist[recipient], 'Address in blacklist');

        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, 'BEP20: transfer amount exceeds balance');
        _balances[sender] = senderBalance - amount;

        uint256 recipientBalance = _balances[recipient];
        _balances[recipient] = recipientBalance + amount;

        emit Transfer(sender, recipient, amount);
    }

    function _burn(address account, uint256 amount) internal {
        require(account != address(0), 'BEP20: burn from the zero address');
        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, 'BEP20: burn amount exceeds balance');
        _balances[account] = accountBalance - amount;
        _totalSupply -= amount;

        emit Transfer(account, address(0), amount);
    }

    function balanceOf(address account) public view returns (uint256) {
        return _balances[account];
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), 'BEP20: approve from the zero address');
        require(spender != address(0), 'BEP20: approve to the zero address');

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _beforeTokenTransfer(address from, address to, uint256 amount) internal { }

    function allowance(address owner, address spender) public view returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transfer(address recipient, uint256 amount) public returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public returns (bool) {
        uint256 currentAllowance = _allowances[sender][msg.sender];
        require(currentAllowance >= amount, 'BEP20: transfer amount exceeds allowance');
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, currentAllowance - amount);
        return true;
    }
}