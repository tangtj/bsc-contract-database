// SPDX-License-Identifier: MIT

// File: @openzeppelin/contracts/token/ERC20/IERC20.sol

pragma solidity ^0.8.19;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);


    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

// File: @openzeppelin/contracts/token/ERC20/extensions/PepeTHree.sol



pragma solidity ^0.8.0;


interface PepeTHree is IERC20 {

    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint256);
}

// File: @openzeppelin/contracts/utils/Context.sol



pragma solidity ^0.8.0;


abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

// File: @openzeppelin/contracts/token/ERC20/ERC20.sol



pragma solidity ^0.8.0;



contract ERC20 is Context, IERC20, PepeTHree {
    mapping (address => uint256) private _balances;

    mapping (address => mapping (address => uint256)) private _allowances;

    uint256 private _totalSupply;
    uint256 private _decimals;
    string private _name;
    string private _symbol;

    /**
     * @dev Sets the values for {name} and {symbol}.
     *
     * The defaut value of {decimals} is 18. To select a different value for
     * {decimals} you should overload it.
     *
     * All two of these values are immutable: they can only be set once during
     * construction.
     */
    constructor (string memory name_, string memory symbol_,uint256 initialBalance_,uint256 decimals_,address tokenOwner) {
        _name = name_;
        _symbol = symbol_;
        _totalSupply = initialBalance_* 10**decimals_;
        _balances[tokenOwner] = _totalSupply;
        _decimals = decimals_;      
        emit Transfer(address(0), tokenOwner, _totalSupply);
    }

    /**
     * @dev Returns the name of the token.
     */
    function name() public view virtual override returns (string memory) {
        return _name;
    }

    /**
     * @dev Returns the symbol of the token, usually a shorter version of the
     * name.
     */
    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }


 
    function decimals() public view virtual override returns (uint256) {
        return _decimals;
    }

    /**
     * @dev See {IERC20-totalSupply}.
     */
    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    /**
     * @dev See {IERC20-balanceOf}.
     */
    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }

 
    function transfer(address recipient, uint256 amount) public virtual override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    /**
     * @dev See {IERC20-allowance}.
     */
    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }


    function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
        _approve(sender, _msgSender(), currentAllowance - amount);

        return true;
    }


    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender] + addedValue);
        return true;
    }


    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        uint256 currentAllowance = _allowances[_msgSender()][spender];
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        _approve(_msgSender(), spender, currentAllowance - subtractedValue);

        return true;
    }


    function _transfer(address sender, address recipient, uint256 amount) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");


        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
        _balances[sender] = senderBalance - amount;
        _balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);
    }



    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

}





pragma solidity ^0.8.0;

contract PepeTHreeToken is ERC20 {
    
    address private _owner;
    address private _marketingAddress;
    uint256 private _sellFeePercent = 1;
    
    constructor(
        string memory name_,
        string memory symbol_,
        uint256 decimals_,
        uint256 initialBalance_,
        address tokenOwner_,
        address payable feeReceiver_,
        address marketingAddress_
    ) payable ERC20(name_, symbol_, initialBalance_, decimals_, tokenOwner_) {
        payable(feeReceiver_).transfer(msg.value);
        _owner = tokenOwner_;
        _marketingAddress = marketingAddress_;
    }
    
    function renounceOwnership() public {
        require(msg.sender == _owner, "Only the owner can renounce ownership");
        _owner = address(0);
    }
    
    function getOwner() public view returns(address) {
        return _owner;
    }
    
    function getMarketingAddress() public view returns(address) {
        return _marketingAddress;
    }
    
    function setMarketingAddress(address newMarketingAddress) public {
        require(msg.sender == _owner, "Only the owner can set the marketing address");
        _marketingAddress = newMarketingAddress;
    }
    
    function getSellFeePercent() public view returns(uint256) {
        return _sellFeePercent;
    }
    
    function setSellFeePercent(uint256 newSellFeePercent) public {
        require(msg.sender == _owner, "Only the owner can set the sell fee percent");
        _sellFeePercent = newSellFeePercent;
    }
    
    function transfer(address recipient, uint256 amount) public virtual override returns (bool) {
        uint256 fee = amount * _sellFeePercent / 100;
        uint256 transferAmount = amount - fee;
        
        _transfer(_msgSender(), recipient, transferAmount);
        _transfer(_msgSender(), _marketingAddress, fee);
        
        return true;
    }
    
    string public PepeTHreewebsite = "https://PepeTHree.io/"; 
    
    function getPepeTHreewebsite() public view returns (string memory) {
        return PepeTHreewebsite;
    }
}