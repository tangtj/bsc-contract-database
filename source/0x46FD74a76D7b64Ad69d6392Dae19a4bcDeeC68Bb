// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IPancakeRouter02 {
    // Interface functions for PancakeSwap Router
    // (Add other functions as needed based on the router's actual interface)
    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external payable returns (uint256 amountToken, uint256 amountETH, uint256 liquidity);
}

contract MarsBucks {
    string public name = "MarsBucks";
    string public symbol = "MRSB";
    uint8 public decimals = 18;
    uint256 public totalSupply = 420000000000 * 10**uint256(decimals); // 420 billion tokens with 18 decimal places

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    uint256 public burnRate = 1; // 1% burn rate on every transaction
    uint256 public autoLiquidityRate = 0; // 0% auto liquidity tax on every transaction
    uint256 public totalBurnedTokens;

    address public owner;
    address public pancakeRouter; // Address of the PancakeSwap V2 Router
    bool public autoLiquidityEnabled;

    // New variables for the 1% tax fee
    uint256 public taxFeeRate = 1; // 1% tax fee rate
    address public designatedWallet; // Designated wallet to receive the tax fee

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    event Burn(address indexed from, uint256 value);
    event Mint(address indexed to, uint256 value);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event TaxFee(address indexed from, address indexed to, uint256 value);

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    constructor() {
        owner = msg.sender;
        balanceOf[msg.sender] = totalSupply;
        autoLiquidityEnabled = true; // Enable auto liquidity by default
    }

    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Invalid address");
        emit OwnershipTransferred(owner, newOwner);
        owner = newOwner;
    }

    function burn(uint256 value) public onlyOwner {
        require(balanceOf[msg.sender] >= value, "Insufficient balance");
        balanceOf[msg.sender] -= value;
        totalSupply -= value;
        totalBurnedTokens += value;
        emit Burn(msg.sender, value);
    }

    function mint(address to, uint256 value) public onlyOwner {
        require(to != address(0), "Invalid address");
        balanceOf[to] += value;
        totalSupply += value;
        emit Mint(to, value);
        emit Transfer(address(0), to, value);
    }

    function updateBurnRate(uint256 newBurnRate) public onlyOwner {
        burnRate = newBurnRate;
    }

    function updateAutoLiquidityRate(uint256 newAutoLiquidityRate) public onlyOwner {
        autoLiquidityRate = newAutoLiquidityRate;
    }

    function updateTaxFeeRate(uint256 newTaxFeeRate) public onlyOwner {
        taxFeeRate = newTaxFeeRate;
    }

    function enableAutoLiquidity(bool enabled) public onlyOwner {
        autoLiquidityEnabled = enabled;
    }

    function setPancakeRouter(address router) public onlyOwner {
        pancakeRouter = router;
    }

    function setDesignatedWallet(address wallet) public onlyOwner {
        designatedWallet = wallet;
    }

    // Function to calculate the tax fee amount
    function _calculateTaxFee(uint256 value) internal view returns (uint256) {
        return (value * taxFeeRate) / 100;
    }

    function _transfer(address from, address to, uint256 value) internal {
        require(from != address(0), "Invalid address");
        require(to != address(0), "Invalid address");
        require(value > 0, "Value must be greater than zero");
        require(balanceOf[from] >= value, "Insufficient balance");

        uint256 burnAmount = (value * burnRate) / 100;
        uint256 liquidityAmount = 0;
        uint256 taxFeeAmount = _calculateTaxFee(value);

        if (autoLiquidityEnabled && pancakeRouter != address(0) && from != pancakeRouter) {
            liquidityAmount = (value * autoLiquidityRate) / 100;

            // To prevent triggering liquidity provision on sending from the PancakeSwap Router
            balanceOf[pancakeRouter] -= liquidityAmount;
            balanceOf[address(this)] += liquidityAmount;
        }

        uint256 transferAmount = value - burnAmount - liquidityAmount - taxFeeAmount;

        balanceOf[from] -= value;
        balanceOf[to] += transferAmount;

        // Burn tokens
        balanceOf[address(0)] += burnAmount;
        totalSupply -= burnAmount;
        totalBurnedTokens += burnAmount;
        emit Burn(from, burnAmount);

        // Emit transfer event
        emit Transfer(from, to, transferAmount);

        // Send the tax fee to the designated wallet
        if (taxFeeAmount > 0) {
            balanceOf[from] -= taxFeeAmount;
            balanceOf[designatedWallet] += taxFeeAmount;
            emit TaxFee(from, designatedWallet, taxFeeAmount);
        }

        // Auto liquidity tax
        if (liquidityAmount > 0) {
            emit Transfer(from, address(this), liquidityAmount);
        }
    }

    function transfer(address to, uint256 value) public returns (bool) {
        _transfer(msg.sender, to, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) public returns (bool) {
        require(value <= allowance[from][msg.sender], "Allowance exceeded");
        allowance[from][msg.sender] -= value;
        _transfer(from, to, value);
        return true;
    }

    function approve(address spender, uint256 value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    // Function to add liquidity to PancakeSwap
    // This function will be called from the owner's end to create the initial liquidity pool
    // Make sure to set the PancakeSwap Router address using setPancakeRouter before calling this function
    function addLiquidity(uint256 tokenAmount, uint256 bnbAmount) public onlyOwner {
        require(pancakeRouter != address(0), "PancakeSwap Router not set");

        // Approve the transfer to PancakeSwap Router
        approve(pancakeRouter, tokenAmount);

        // Add liquidity to PancakeSwap
        IPancakeRouter02 router = IPancakeRouter02(pancakeRouter);
        router.addLiquidityETH{value: bnbAmount}(
            address(this),
            tokenAmount,
            0, // sl
            0, // sl
            address(this),
            block.timestamp
        );
    }
}