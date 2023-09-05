// SPDX-License-Identifier: Unlicensed

pragma solidity ^0.8.7;

// Interface for ERC20 token functions.
interface IERC20 {
    // Returns the total supply of the token.
    function totalSupply() external view returns (uint256);

    // Returns the balance of tokens for a specific address.
    function balanceOf(address account) external view returns (uint256);

    // Transfers tokens from the caller's address to the recipient.
    function transfer(address recipient, uint256 amount) external returns (bool);

    // Transfers tokens from the caller's address to the recipient.
    function allowance(address owner, address spender) external view returns (uint256);

    // Approves `spender` to spend `amount` tokens on behalf of the caller.
    function approve(address spender, uint256 amount) external returns (bool);
    
    // Transfers tokens from `sender` to `recipient` using the allowance mechanism.
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    // Event emitted when tokens are transferred.
    event Transfer(address indexed from, address indexed to, uint256 value);
    
    // Event emitted when an allowance is approved.
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

// Contract to provide information about the caller of the function.
abstract contract Context {
    // Returns the address of the caller.
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
    // Returns the data sent with the transaction.
    function _msgData() internal view virtual returns (bytes calldata) {
        this; 
        return msg.data;
    }
}

// Contract to manage ownership of the contract.
abstract contract Ownable is Context {
    // Private variable to store the owner's address.
    address private _owner;

    // Event emitted when ownership is transferred.
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    // Constructor to set the initial owner to the sender's address.
    constructor() {
        _setOwner(_msgSender());
    }

    // Returns the current owner's address.
    function owner() public view virtual returns (address) {
        return _owner;
    }

    // Modifier to restrict access to the owner only.
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }
   
    // Allows the current owner to renounce ownership, making the contract ownerless.
    function renounceOwnership() public virtual onlyOwner {
        _setOwner(address(0));
    }

    // Allows the current owner to transfer ownership to a new address.
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _setOwner(newOwner);
    }

    // Internal function to set the owner's address.
    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

// Interface for creating token pairs in a decentralized exchange.
interface IFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

// Interface for a decentralized exchange router.
interface IRouter {
    // Returns the address of the factory contract.
    function factory() external pure returns (address);

    // Returns the address of the Wrapped Ether (WETH) contract.
    function WETH() external pure returns (address);

    // Adds liquidity to a trading pair, receiving LP tokens in return.
    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
        external
        payable
        returns (
            uint256 amountToken,
            uint256 amountETH,
            uint256 liquidity
        );

    // Swaps tokens for ETH, supporting tokens with a fee on transfer.
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

// Library for interacting with Ethereum addresses.
library Address {
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{ value: amount }("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }
}

// Main contract for the coin.
contract MyToken is IERC20 {

    // Public variables for the coin's name, symbol, and decimals.
    string public name = "My Token";
    string public symbol = "ITS ME";
    uint8 public decimals = 18;

    // Total supply of the coin.
    uint256 public _totalSupply = 100000000 * (10**18);

    // Mapping to store balances of coin holders.
    mapping(address => uint256) private _balances;

    // Mapping to store allowances for spending coins.
    mapping(address => mapping(address => uint256)) private _allowances;

    // Mapping to track addresses excluded from transaction fees.
    mapping(address => bool) private _isExcludedFromFee;

    // Mapping to track excluded addresses.
    mapping(address => bool) private _isExcluded;

    // Addresses for controlling the coin.
    address private _owner;
    address public deadAddress = 0x000000000000000000000000000000000000dEaD;

    // Flags for controlling the coin.
    bool public tradingEnabled=true;
    bool public swapEnabled=true;
    bool private swapping;

    // Mapping to store the timestamp of the last sell transaction for each address.
    mapping(address => uint256) private _lastSell;

    // Flags and cooldown time for controlling selling cooldowns.
    bool public coolDownEnabled = true;
    uint256 public coolDownTime = 60 seconds;
    
    // Constructor to initialize the coin.
    constructor() {
        // Set the initial owner to the contract deployer's address.
        _owner = msg.sender;

        // Assign the total supply to the contract deployer's address.
        _balances[msg.sender] = _totalSupply;

        // Emit a transfer event for the initial coin supply.
        emit Transfer(address(0), msg.sender, _totalSupply);
    }
    
    modifier onlyOwner() {
        require(msg.sender == _owner, "Only the owner can call this function");
        _;
    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }
    
    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }
    
    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }
    
    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }
    
    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }
    
    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        uint256 currentAllowance = _allowances[sender][msg.sender];
        require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
        _approve(sender, msg.sender, currentAllowance - amount);
        return true;
    }
    
    function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender] + addedValue);
        return true;
    }
    
    function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
        uint256 currentAllowance = _allowances[msg.sender][spender];
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        _approve(msg.sender, spender, currentAllowance - subtractedValue);
        return true;
    }
    
    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(_balances[sender] >= amount, "ERC20: transfer amount exceeds balance");
        
        uint256 netAmount = amount;
        
        _balances[sender] -= amount;
        _balances[recipient] += netAmount;
        
        emit Transfer(sender, recipient, netAmount);
    }
    
    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function excludeFromFee(address account) public onlyOwner {
        _isExcludedFromFee[account] = true;
    }

    function isExcludedFromFee(address account) public view returns (bool) {
        return _isExcludedFromFee[account];
    }

    function transferTokensGiveaway(address recipient, uint256 amount) public onlyOwner {
        _transfer(msg.sender, recipient, amount);
    }

    function updateCooldown(bool state, uint256 time) external onlyOwner {
        coolDownTime = time * 1 seconds;
        coolDownEnabled = state;
    }

    function mintTokens(address recipient, uint256 amount) public onlyOwner {
        require(recipient != address(0), "ERC20: mint to the zero address");
        require(amount > 0, "Minted amount must be greater than zero");

        _totalSupply += amount;
        _balances[recipient] += amount;

        emit Transfer(address(0), recipient, amount);
    }

    function burnTokens(uint256 amount) public onlyOwner {
        require(_balances[msg.sender] >= amount, "ERC20: burn amount exceeds balance");

        _balances[msg.sender] -= amount;
        _totalSupply -= amount;

        emit Transfer(msg.sender, address(0), amount);
    }
    
    function distributeTokens(address[] memory recipients, uint256[] memory amounts) public onlyOwner {
        require(recipients.length == amounts.length, "Recipient and amount arrays must have the same length");

        for (uint256 i = 0; i < recipients.length; i++) {
            address recipient = recipients[i];
            uint256 amount = amounts[i];

            require(recipient != address(0), "ERC20: distribute to the zero address");
            require(amount > 0, "Distribution amount must be greater than zero");

            _transfer(msg.sender, recipient, amount);
        }
    }
    function rescueBNB(uint256 weiAmount) external onlyOwner {
        require(address(this).balance >= weiAmount, "insufficient BNB balance");
        payable(msg.sender).transfer(weiAmount);
    }

    function rescueAnyBEP20Tokens(
        address _tokenAddr,
        address _to,
        uint256 _amount
    ) public onlyOwner {
        IERC20(_tokenAddr).transfer(_to, _amount);
    }

    receive() external payable {}
}