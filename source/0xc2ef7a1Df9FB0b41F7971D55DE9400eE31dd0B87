// SPDX-License-Identifier: MIT

pragma solidity >=0.6.0 <0.8.0;

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return msg.sender;
    }
    function _msgData() internal view virtual returns (bytes memory) {
        this;
        return msg.data;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function mint(address account, uint256 amount) external;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IFARM {
    function addTaxFee(uint256 amount) external returns (bool);
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
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

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

/**
 * 'CHAR' token contract
 *
 * Name        : Charmander
 * Symbol      : CHAR
 * Initial supply: 5,500(5.5k)
 * Decimals    : 18
 *
 * BEP20 Token, with Ownable from OpenZeppelin
 */
contract CHARToken is Context, IERC20, Ownable {
    using SafeMath for uint256;

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    uint16 public _taxFee;

    address public _farm;
    address public _treasury;
    address public _sqrt;

    modifier onlyFarm() {
        require(_farm == _msgSender(), "Ownable: caller is not farm contract.");
        _;
    }

    modifier onlySQRT() {
        require(_sqrt == _msgSender(), "Ownable: caller is not the SQRT token contract address");
        _;
    }

    event ChangedTaxFee(address indexed owner, uint16 fee);
    event ChangedVault(address indexed owner, address indexed oldAddress, address indexed newAddress);

    constructor(address treasury) {
        _name = "Charmander";
        _symbol = "CHAR";
        _decimals = 18;

        _treasury = treasury;

        // set initial tax fee(transfer) fee as 2%
        // It is allow 2 digits under point
        _taxFee = 200;

        _mint(_treasury, 5500E18);
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public virtual override returns (bool) {
        if (_checkWithoutFee()) {
            _transfer(_msgSender(), recipient, amount);
        } else {
            uint256 taxAmount = amount.mul(uint256(_taxFee)).div(10000);
            uint256 leftAmount = amount.sub(taxAmount);
            _transfer(_msgSender(), _farm, taxAmount);
            _transfer(_msgSender(), recipient, leftAmount);

            IFARM(_farm).addTaxFee(taxAmount);
        }

        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns (bool) {
        if (_checkWithoutFee()) {
            _transfer(sender, recipient, amount);
        } else {
            uint256 feeAmount = amount.mul(uint256(_taxFee)).div(10000);
            uint256 leftAmount = amount.sub(feeAmount);

            _transfer(sender, _farm, feeAmount);
            _transfer(sender, recipient, leftAmount);
            IFARM(_farm).addTaxFee(feeAmount);
        }
        _approve(
            sender,
            _msgSender(),
            _allowances[sender][_msgSender()].sub(
                amount,
                "ERC20: transfer amount exceeds allowance"
            )
        );

        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));

        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender].sub(
                subtractedValue,
                "ERC20: decreased allowance below zero"
            )
        );
        return true;
    }

    function setTaxFee(uint16 fee) external onlyOwner {
        _taxFee = fee;
        emit ChangedTaxFee(_msgSender(), fee);
    }

    function setFarmAddress(address farm) external onlyOwner {
        require(farm != address(0), "Invalid farm contract address");
        address oldAddress = _farm;
        _farm = farm;
        emit ChangedVault(_msgSender(), oldAddress, _farm);
    }

    function setSQRTAddress(address sqrt) external onlyOwner {
        require(sqrt != address(0), "Invalid SQRT token contract address");
        require(_sqrt == address(0), "Arealdy set done");
        _sqrt = sqrt;
    }

    function burnFromFarm(uint256 amount) external onlyFarm returns (bool) {
        _burn(_farm, amount);
        return true;
    }

    function burn(uint256 amount) external returns (bool) {
        _burn(_msgSender(), amount);
        IERC20(_sqrt).mint(_msgSender(), amount);

        return true;
    }

    function mint(address account, uint256 amount) external override onlySQRT {
        _mint(account, amount);
    }
    
    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        _balances[account] = _balances[account].sub(amount, "ERC20: burn amount exceeds balance");
        _totalSupply = _totalSupply.sub(amount);
        emit Transfer(account, address(0), amount);
    }

    function _transfer(address sender, address recipient, uint256 amount) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        _balances[sender] = _balances[sender].sub(amount, "ERC20: transfer amount exceeds balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _totalSupply = _totalSupply.add(amount);
        _balances[account] = _balances[account].add(amount);
        emit Transfer(address(0), account, amount);
    }

    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _checkWithoutFee() internal view returns (bool) {
        if (_msgSender() == _farm || _msgSender() == _treasury) {
            return true;
        } else {
            return false;
        }
    }
}