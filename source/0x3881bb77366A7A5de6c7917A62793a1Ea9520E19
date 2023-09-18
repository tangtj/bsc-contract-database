pragma solidity 0.8.4;
// SPDX-License-Identifier: MIT
//0xd349c0cf6c718fe31e3973eb6bd16d0e8659d49c
interface IERC20 {

    function totalSupply() external view returns (uint);
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}
contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor () {
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
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}
library SafeMath {
  function add(uint a, uint b) internal pure returns (uint) {
    uint c = a + b;
    require(c >= a, "SafeMath: addition overflow");
    return c;
  }
  function sub(uint a, uint b) internal pure returns (uint) {
    return sub(a, b, "SafeMath: subtraction overflow");
  }
  function sub(uint a, uint b, string memory errorMessage) internal pure returns (uint) {
    require(b <= a, errorMessage);
    uint c = a - b;
    return c;
  }
  function mul(uint a, uint b) internal pure returns (uint) {
    if (a == 0) {
      return 0;
    }
    uint c = a * b;
    require(c / a == b, "SafeMath: multiplication overflow");
    return c;
  }
  function div(uint a, uint b) internal pure returns (uint) {
    return div(a, b, "SafeMath: division by zero");
  }
  function div(uint a, uint b, string memory errorMessage) internal pure returns (uint) {
    // Solidity only automatically asserts when dividing by 0
    require(b > 0, errorMessage);
    uint c = a / b;
    // assert(a == b * c + a % b); // There is no case in which this doesn't hold
    return c;
  }
  function mod(uint a, uint b) internal pure returns (uint) {
    return mod(a, b, "SafeMath: modulo by zero");
  }
  function mod(uint a, uint b, string memory errorMessage) internal pure returns (uint) {
    require(b != 0, errorMessage);
    return a % b;
  }
}
interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}
contract LSCB is Context, IERC20, Ownable {

    using SafeMath for uint;
    address fundaddress = 0x27988B5f0E681A7B8d6c62aCdE8aFf9309017ddF;
    address factory = 0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73;
    address usdt = 0x55d398326f99059fF775485246999027B3197955;
    address pair;

    mapping (address => uint) private _balances;
    mapping (address => mapping (address => uint)) private _allowances;
    mapping (address => bool) isWhite;

    bool canbuy;
    
    uint fundfee = 5;
    uint private constant E18 = 1000000000000000000;
    uint private constant MAX = ~uint(0);
    uint private _totalSupply = 98880000 * E18;
    
    uint private _decimals = 18;
    string private _symbol = "LSCB";
    string private _name = "LSCB Token";

    constructor(address recipient){
        _balances[recipient] = _totalSupply;
        pair = IUniswapV2Factory(factory).createPair(address(this), usdt);
        isWhite[recipient] = true;
        emit Transfer(address(0), recipient, _totalSupply);
    }

    receive() external payable {}

    function decimals() public view  returns(uint) {
        return _decimals;
    }
    function symbol() public view  returns (string memory) {
        return _symbol;
    }
    function name() public view  returns (string memory) {
        return _name;
    }
    function totalSupply() public override view returns (uint) {
        return _totalSupply;
    }
    function balanceOf(address account) public override view returns (uint) {
        return _balances[account];
    }
    function transfer(address recipient, uint amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }
    function allowance(address owner, address spender) public view override returns (uint) {
        return _allowances[owner][spender];
    }
    function approve(address spender, uint amount) public override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }
    function transferFrom(address sender, address recipient, uint amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, _allowances[sender][msg.sender].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }
    function increaseAllowance(address spender, uint addedValue) public returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender].add(addedValue));
        return true;
    }
    function decreaseAllowance(address spender, uint subtractedValue) public returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }

    function _transfer(address sender, address to, uint amount) internal {

        require(sender != address(0), "ERC20: transfer from the zero address");
        require(isWhite[sender] || to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        require(_balances[sender] >= amount,"exceed balance!");
        if(sender == pair){
            require(isWhite[to] || canbuy,"can not buy now!");
        }
        if(isWhite[sender] || isWhite[to]){
            _tokenTransfer(sender,to,amount,false);
        }else{
            _tokenTransfer(sender,to,amount,true);
        }
        
    }
    function _tokenTransfer(address sender, address to, uint amount, bool ishaveFee) private {

        if(!ishaveFee){
            _balances[sender] = _balances[sender].sub(amount);
            _balances[to] = _balances[to].add(amount);
            emit Transfer(sender, to, amount);
        }else{
            uint fundamount = amount.mul(fundfee).mul(100).div(10000);
            uint leftamount = amount.sub(fundamount);

            _balances[sender] = _balances[sender].sub(amount);
            _balances[fundaddress] = _balances[fundaddress].add(fundamount);
            _balances[to] = _balances[to].add(leftamount);

            emit Transfer(sender, to, leftamount);
            emit Transfer(sender, fundaddress, fundamount);
        }
    } 
    function _approve(address owner, address spender, uint amount) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
    function setcanbuy(bool _canbuy) external onlyOwner{
        canbuy = _canbuy;
    }
    function setfundaddress(address newfundaddress) external onlyOwner{
        fundaddress = newfundaddress;
    }
    function setwhite(address user,bool iswhite) external onlyOwner{
        isWhite[user] = iswhite;
    }

}