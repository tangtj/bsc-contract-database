/**
 *Submitted for verification at Etherscan.io on 2023-07-07
*/

// SPDX-License-Identifier: MIT
pragma solidity >0.4.0 <= 0.9.0;

interface IPancakeRouter02 {
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
}

interface IBEP20 {
    function totalSupply() external view returns (uint256);

    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function getOwner() external view returns (address);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address _owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract Context {
    constructor () {}
    function _msgSender() internal view returns (address) {
        return msg.sender;
    }

    function _msgData() internal view returns (bytes memory) {
        this;
        // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
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

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
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
        // Solidity only automatically asserts when dividing by 0
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

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

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
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
    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public onlyOwner {
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract Token is Context, IBEP20, Ownable {
    using SafeMath for uint256;
    struct Card {
        uint256 price;
        uint256 ep;
        uint256 epRatio;
    }

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _ban; // 黑名单
    mapping(address => bool) private _pass; // 白名单
    address private _LP = 0x0000000000000000000000000000000000000000;
    address private _black_hole = 0x0000000000000000000000000000000000000111;
    address private _market_value_address = 0x7D2149B8cE21D038ea0C604337ab86065aA9b93D;
    address private _address_a = 0x4450720E00663579FFF8020C9d9796570f5C6729;
    address private _address_b = 0xeD3811E695F065699BBDeDE58d2E252662846D2a;
    //test net: 0x3966aeb2c687ea5f64440ea494e6aa7690fac518
    address private _usdt_address = 0x55d398326f99059fF775485246999027B3197955;
    //test net: 0xD99D1c33F9fC3444f8101754aBC46c52416550D1
    IPancakeRouter02 private _router = IPancakeRouter02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
    IBEP20 private _ep = IBEP20(0xc937270816789662f61044ab5eF25e78D5655601);
    uint256 private _totalSupply;
    uint8 private _decimals;
    string private _symbol;
    string private _name;
    bool private _banTransfer;
    mapping(uint8 => Card) public cards;

    constructor() {
        _name = "ART";
        _symbol = "ART";
        _decimals = 18;
        _totalSupply = 10 ** uint256(_decimals) * 210000000;
        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
        initCard();
    }

    function getOwner() external view returns (address) {
        return owner();
    }

    function decimals() external view returns (uint8) {
        return _decimals;
    }

    function symbol() external view returns (string memory) {
        return _symbol;
    }

    function name() external view returns (string memory) {
        return _name;
    }

    function totalSupply() external view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external view returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) external returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function batchTransfer(address[] memory recipients, uint256[] memory amounts) public {
        require(recipients.length == amounts.length, "Invalid input parameters");
        for (uint256 i = 0; i < recipients.length; i++) {
            _transfer(_msgSender(), recipients[i], amounts[i]);
        }
    }

    function allowance(address owner, address spender) external view returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "BEP20: transfer amount exceeds allowance"));
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "BEP20: decreased allowance below zero"));
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(recipient != address(0), "BEP20: transfer to the zero address");
        require(!_ban[sender] && !_ban[recipient], "baned user");
        require(!_banTransfer || _pass[sender] || _pass[recipient], "baned for transfer");
        _balances[sender] = _balances[sender].sub(amount, "BEP20: transfer amount exceeds balance");
        if (sender == _LP && !_pass[recipient]) {//买
            uint256 fee = amount.mul(uint256(1)).div(100);
            _balances[_address_a] = _balances[_address_a].add(fee);
            uint256 value = amount.sub(fee, "BEP20: transfer fee insufficient");
            _balances[recipient] = _balances[recipient].add(value);
            emit Transfer(sender, recipient, value);
            emit Transfer(sender, _address_a, fee);
        } else if (recipient == _LP && !_pass[sender]) {//卖
            uint256 fee = amount.mul(uint256(6)).div(100);
            uint256 value = amount.sub(fee, "BEP20: transfer fee insufficient");
            _balances[recipient] = _balances[recipient].add(value);
            emit Transfer(sender, recipient, value);
            _balances[_black_hole] = _balances[_black_hole].add(fee.mul(uint256(50)).div(100));
            emit Transfer(sender, _black_hole, fee.mul(uint256(50)).div(100));
            _balances[_address_b] = _balances[_address_b].add(fee.mul(uint256(50)).div(100));
            emit Transfer(sender, _address_b, fee.mul(uint256(50)).div(100));
        } else {
            _balances[recipient] = _balances[recipient].add(amount);
            emit Transfer(sender, recipient, amount);
        }
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function setup(address lp) public onlyOwner {
        _LP = lp;
    }

    function initCard() private {
        cards[1] = Card(100, 33, 33);
        cards[2] = Card(500, 165, 33);
        cards[3] = Card(2000, 660, 33);
        cards[4] = Card(5000, 1650, 33);
        cards[5] = Card(10000, 3300, 33);
    }

    function setCard(uint8 id, uint256 price, uint256 ep, uint256 epRatio) public onlyOwner {
        cards[id].price = price;
        cards[id].ep = ep;
        cards[id].epRatio = epRatio;
    }

    function mold(uint8 id, uint256 no, bool useEP) public {
        address[] memory path = new address[](2);
        path[0] = _usdt_address;
        path[1] = address(this);
        uint256 d = 10 ** uint256(_decimals);
        uint256 amount = _router.getAmountsOut(cards[id].price * d, path)[1];
        if (useEP) {
            amount = amount.div(100).mul(cards[id].epRatio);
            _ep.transferFrom(_msgSender(), address(this), cards[id].ep * d);
            emit Mold(_msgSender(), id, cards[id].price, cards[id].ep, cards[id].epRatio, amount, no);
        } else {
            emit Mold(_msgSender(), id, cards[id].price, 0, 0, amount, no);
        }
        _transfer(_msgSender(), _market_value_address, amount.div(10).mul(uint256(3)));
        _transfer(_msgSender(), _black_hole, amount.div(10).mul(uint256(7)));
    }

    function addPass(address[] calldata addr) external onlyOwner {
        for (uint i = 0; i < addr.length; i++) {
            _pass[addr[i]] = true;
        }
    }

    function removePass(address[] calldata addr) external onlyOwner {
        for (uint i = 0; i < addr.length; i++) {
            delete _pass[addr[i]];
        }
    }

    function isPass(address addr) public view returns (bool) {
        return _pass[addr];
    }

    function addBan(address[] calldata addr) external onlyOwner {
        for (uint i = 0; i < addr.length; i++) {
            _ban[addr[i]] = true;
        }
    }

    function removeBan(address[] calldata addr) external onlyOwner {
        for (uint i = 0; i < addr.length; i++) {
            delete _ban[addr[i]];
        }
    }

    function isBan(address addr) public view returns (bool) {
        return _ban[addr];
    }

    function setBanTransfer(bool banTransfer) external onlyOwner {
        _banTransfer = banTransfer;
    }

    event Mold(address indexed account, uint8 id, uint256 price, uint256 ep, uint256 epRatio, uint256 tokenA, uint256 no);
}