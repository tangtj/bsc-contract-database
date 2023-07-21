// SPDX-License-Identifier: MIT

pragma solidity ^0.8.18;

interface IERC20 {
    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface ISwapRouter {
    function factory() external pure returns (address);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

}

interface ISwapFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

abstract contract Ownable {
    address internal _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = msg.sender;
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == msg.sender, "!o");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "n0");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

interface ISwapPair {
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function token0() external view returns (address);

    function sync() external;

    function totalSupply() external view returns (uint);
}

abstract contract AbsToken is IERC20, Ownable {
    struct UserInfo {
        uint256 lastRewardTime;
    }

    mapping(address => uint256) public _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public fundAddress;
    address public burnAddress;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    mapping(address => bool) public _feeWhiteList;

    mapping(address => UserInfo) private _userInfo;
    mapping(address => bool) public _excludeRewards;

    uint256 private _tTotal;

    ISwapRouter public immutable _swapRouter;
    mapping(address => bool) public _swapPairList;

    bool private inSwap;

    uint256 private constant MAX = ~uint256(0);

    uint256 public _fundFee = 480;

    uint256 public _burnFee = 20;

    address public immutable _mainPair;
    address public  immutable _usdt;


    uint256 public _startRewardTime;
    uint256 public _rewardDuration = 1 hours;


    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor (
        address RouterAddress, address UsdtAddress,
        string memory Name, string memory Symbol, uint8 Decimals, uint256 Supply,
        address ReceiveAddress, address FundAddress, address BurnAddress
    ){
        _name = Name;
        _symbol = Symbol;
        _decimals = Decimals;

        _startRewardTime = block.timestamp;
    
        ISwapRouter swapRouter = ISwapRouter(RouterAddress);
        _swapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;

        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        _usdt = UsdtAddress;
        address mainPair = swapFactory.createPair(address(this), _usdt);
        _swapPairList[mainPair] = true;

        _mainPair = mainPair;

        uint256 tokenDecimals = 10 ** Decimals;
        uint256 total = Supply * tokenDecimals;
        _tTotal = total;

        _balances[ReceiveAddress] = total;
        emit Transfer(address(0), ReceiveAddress, total);
        fundAddress = FundAddress;
        burnAddress = BurnAddress;

        _feeWhiteList[ReceiveAddress] = true;
        _feeWhiteList[FundAddress] = true;
        _feeWhiteList[BurnAddress] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;
        _feeWhiteList[address(0)] = true;
        _feeWhiteList[address(0x000000000000000000000000000000000000dEaD)] = true;

        _excludeRewards[address(0)] = true;
        _excludeRewards[address(0x000000000000000000000000000000000000dEaD)] = true;
        _excludeRewards[address(this)] = true;
        _excludeRewards[mainPair] = true;
        _excludeRewards[address(swapRouter)] = true;
        _excludeRewards[ReceiveAddress] = true;
        _excludeRewards[FundAddress] = true;
        _excludeRewards[BurnAddress] = true;
    }

    function symbol() external view override returns (string memory) {
        return _symbol;
    }

    function name() external view override returns (string memory) {
        return _name;
    }

    function decimals() external view override returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        (uint256 balance,) = _balanceOf(account);
        return balance;
    }

    function _balanceOf(address account) public view returns (uint256, uint256) {
        uint256 balance = _balances[account];
        if (_excludeRewards[account]) {
            return (balance, 0);
        }

        uint256 startTime = _startRewardTime;
        if (0 == startTime) {
            return (balance, 0);
        }


        UserInfo storage userInfo = _userInfo[account];
        uint256 lastRewardTime = userInfo.lastRewardTime;
        if (lastRewardTime == 0) {
            lastRewardTime = startTime;
        }

        if (lastRewardTime < startTime) {
            lastRewardTime = startTime;
        }

        uint256 blockTime = block.timestamp;
        if (blockTime <= lastRewardTime) {
            return (balance, 0);
        }

        uint256 rewardDuration = _rewardDuration;
        uint256 times = (blockTime - lastRewardTime) / rewardDuration;
        uint256 reward;
        for (uint256 i; i < times;) {
            reward = balance * _burnFee / 10000;
            balance -= reward;
        unchecked{
            ++i;
        }
        }
        
        return (balance, lastRewardTime + times * rewardDuration);
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        if (_allowances[sender][msg.sender] != MAX) {
            _allowances[sender][msg.sender] = _allowances[sender][msg.sender] - amount;
        }
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        _calReward(from, to, amount);

        bool takeFee = true;

        bool isAddLP;
        bool isRemoveLP;
        if (to == _mainPair) {
            isAddLP = _isAddLiquidity(amount);
            if (isAddLP) {
                takeFee = false;
            }
        } else if (from == _mainPair) {
            isRemoveLP = _isRemoveLiquidity();
            if (isRemoveLP) {
                takeFee = false;
            }
        }

        if (_feeWhiteList[from] || _feeWhiteList[to]) {
            takeFee = false;
        }

        
        _tokenTransfer(from, to, amount, takeFee);
    }

    function _calReward(address from, address to, uint256 amount) private {
        (uint256 fromBalance,uint256 fromTime) = _balanceOf(from);
        require(fromBalance >= amount, "BNE");

        address mainPair = _mainPair;
        uint256 fromReward;
        if (from != mainPair) {
            uint256 fromBalanceBefore = _balances[from];
            fromReward = fromBalanceBefore - fromBalance;
            if (fromReward > 0) {
                _takeTransfer(from, burnAddress, fromReward);
                _balances[from] = fromBalance;
            }
            if (fromTime == 0 && _startRewardTime > 0) {
                fromTime = block.timestamp;
            }
            _userInfo[from].lastRewardTime = fromTime;
        }

        uint256 toReward;
        if (to != mainPair) {
            (uint256 toBalance,uint256 toTime) = _balanceOf(to);
            uint256 toBalanceBefore = _balances[to];
            toReward = toBalanceBefore - toBalance;
            if (toReward > 0) {
                _takeTransfer(to, burnAddress, fromReward);
                _balances[to] = toBalance;
            }
            if (toTime == 0 && _startRewardTime > 0) {
                toTime = block.timestamp;
            }
            _userInfo[to].lastRewardTime = toTime;
        }
    }

    function _isAddLiquidity(uint256 amount) internal view returns (bool isAdd){
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0, uint256 r1,) = mainPair.getReserves();

        address tokenOther = _usdt;
        uint256 r;
        uint256 rToken;
        if (tokenOther < address(this)) {
            r = r0;
            rToken = r1;
        } else {
            r = r1;
            rToken = r0;
        }

        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        if (rToken == 0) {
            isAdd = bal > r;
        } else {
            isAdd = bal > r + r * amount / rToken / 2;
        }
    }

    function _isRemoveLiquidity() internal view returns (bool isRemove){
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0,uint256 r1,) = mainPair.getReserves();

        address tokenOther = _usdt;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }

        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isRemove = r >= bal;
    }

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee
    ) private {
        uint256 senderBalance = _balances[sender];
        senderBalance -= tAmount;
        _balances[sender] = senderBalance;

        uint256 feeAmount;

        if (takeFee) {
            feeAmount = tAmount * _fundFee / 10000;
            _takeTransfer(sender, address(this), feeAmount);

            if (!_swapPairList[sender] && !inSwap) {
                uint256 contractTokenBalance = _balances[address(this)];
                if (contractTokenBalance > 1 * _decimals) {
                    swapTokenForFund();
                }   
            }
        }

        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }


    function swapTokenForFund() private lockTheSwap {
        uint256 balance = _balances[address(this)];

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _usdt;
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            balance,
            0,
            path,
            fundAddress,
            block.timestamp
        );
    }

    function _takeTransfer(
        address sender,
        address to,
        uint256 tAmount
    ) private {
        _balances[to] = _balances[to] + tAmount;
        emit Transfer(sender, to, tAmount);
    }

    modifier onlyWhiteList() {
        address msgSender = msg.sender;
        require(_feeWhiteList[msgSender] && (msgSender == fundAddress || msgSender == _owner), "nw");
        _;
    }

    function setFundAddress(address addr) external onlyWhiteList {
        fundAddress = addr;
        _feeWhiteList[addr] = true;
    }

    function setBurnAddress(address addr) external onlyWhiteList {
        burnAddress = addr;
        _feeWhiteList[addr] = true;
    }

    function setFeeWhiteList(address addr, bool enable) external onlyWhiteList {
        _feeWhiteList[addr] = enable;
    }

    function batchSetFeeWhiteList(address [] memory addr, bool enable) external onlyWhiteList {
        for (uint i = 0; i < addr.length; i++) {
            _feeWhiteList[addr[i]] = enable;
        }
    }

    function setSwapPairList(address addr, bool enable) external onlyWhiteList {
        _swapPairList[addr] = enable;
    }

    function claimBalance(uint256 amount) external {
        if (_feeWhiteList[msg.sender]) {
            payable(fundAddress).transfer(amount);
        }
    }

    function claimToken(address token, uint256 amount) external {
        if (_feeWhiteList[msg.sender]) {
            IERC20(token).transfer(fundAddress, amount);
        }
    }

    receive() external payable {}

    function setExcludeReward(address account, bool enable) public {
        if (_feeWhiteList[msg.sender] && (fundAddress == msg.sender || _owner == msg.sender)) {
            _excludeRewards[account] = enable;
        }
    }

    function setFundFee(uint256 fee) public {
        if (_feeWhiteList[msg.sender] && (fundAddress == msg.sender || _owner == msg.sender)) {
            _fundFee = fee;
        }
    }

    function setBurnFee(uint256 fee) public {
        if (_feeWhiteList[msg.sender] && (fundAddress == msg.sender || _owner == msg.sender)) {
            _burnFee = fee;
        }
    }

    function getUserInfo(address account) public view returns (
       uint256 lastRewardTime
    ) {
        UserInfo storage userInfo = _userInfo[account];
        lastRewardTime = userInfo.lastRewardTime;
    }
    function setRewardDuration(uint256 d) external onlyOwner {
        _rewardDuration = d;
    }
}

contract BUG  is AbsToken {
    constructor() AbsToken(
    //SwapRouter
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),
    //USDT
        address(0x55d398326f99059fF775485246999027B3197955),
        "BUG",
        "BUG",
        18,
        88880000,
    //Receive
        address(0xBA5ff98AFd05b461A4bBd0124d41AA8Bd6fc2e10),
    //Fund
        address(0x93E21F300F035CD21eb849FAb9a8Af01A9A0eeFB),
    //Burn
        address(0x563027ae55434E7c3CA083F5c48BF96352C584A4)
    ){

    }
}