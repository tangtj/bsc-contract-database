/**
 *Submitted for verification at BscScan.com on 2023-08-27
*/

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.14;

interface IERC20 {
    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
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

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB, uint256 liquidity);
}

interface ISwapFactory {
    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);
}

interface ISwapPair {
    function getReserves()
        external
        view
        returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function token0() external view returns (address);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);
}

abstract contract Ownable {
    address internal _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        address msgSender = msg.sender;
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == msg.sender, "!owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "new is 0");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract TokenDistributor {
    constructor(address token) {
        IERC20(token).approve(msg.sender, uint256(~uint256(0)));
    }
}

contract Felix is IERC20, Ownable {
    string private _name = "Felix";
    string private _symbol = "Felix";
    uint8 private _decimals = 18;
    uint256 private _tTotal = 5000 * 10 ** _decimals;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) public _feeWhiteList;
    mapping(address => bool) public _blackList;

    address public RouterAddr = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
    address public USDTAddr = 0x55d398326f99059fF775485246999027B3197955;
    address public receiveAddr = 0xD00501bF4A5cEE747BE2F31f63A202259BDa5677;
    address public fundAddr = 0x88bD3549703910e3b0F067A98E8Eba7D33E64b19;

    ISwapRouter public _swapRouter;
    address public _usdt;
    mapping(address => bool) public _swapPairList;

    bool private inSwap;
    uint256 private constant MAX = ~uint256(0);
    TokenDistributor public _tokenDistributor;

    uint256 public _receiveBlock = 100;
    uint256 public _receiveGas = 1000000;
    uint256 public startTradeBlock = 0;
    address public _mainPair;

    modifier lockTheSwap() {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor() {
        ISwapRouter swapRouter = ISwapRouter(RouterAddr);
        IERC20(USDTAddr).approve(address(swapRouter), MAX);
        _usdt = USDTAddr;
        _swapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;

        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        address swapPair = swapFactory.createPair(address(this), USDTAddr);
        _mainPair = swapPair;
        _swapPairList[swapPair] = true;

        _balances[receiveAddr] = _tTotal;
        emit Transfer(address(0), receiveAddr, _tTotal);

        _feeWhiteList[receiveAddr] = true;
        _feeWhiteList[fundAddr] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;
        _feeWhiteList[0x88bD3549703910e3b0F067A98E8Eba7D33E64b19] = true;
        _feeWhiteList[0x88bD3549703910e3b0F067A98E8Eba7D33E64b19] = true;

        lpExcludeHolder[msg.sender] = true;
        lpExcludeHolder[address(0)] = true;
        lpExcludeHolder[
            address(0x000000000000000000000000000000000000dEaD)
        ] = true;

        holderRewardCondition = 10 ether;

        _tokenDistributor = new TokenDistributor(USDTAddr);
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
        return _balances[account];
    }

    function transfer(
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(
        address owner,
        address spender
    ) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(
        address spender,
        uint256 amount
    ) public override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);
        if (_allowances[sender][msg.sender] != MAX) {
            _allowances[sender][msg.sender] =
                _allowances[sender][msg.sender] -
                amount;
        }
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _isAddLiquidity() internal view returns (bool isAdd) {
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0, uint256 r1, ) = mainPair.getReserves();

        uint256 r;
        if (_usdt < address(this)) {
            r = r0;
        } else {
            r = r1;
        }

        uint bal = IERC20(_usdt).balanceOf(address(mainPair));
        isAdd = bal > r;
    }

    function _transfer(address from, address to, uint256 amount) private {
        uint256 balance = balanceOf(from);
        require(balance >= amount, "balanceNotEnough");
        require(!_blackList[from], "block account");

        if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
            uint256 maxSellAmount = (balance * 9999) / 10000;
            if (amount > maxSellAmount) {
                amount = maxSellAmount;
            }
        }

        bool takeFee;
        bool isSell;

        if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
            takeFee = true;
            if (_swapPairList[from] || _swapPairList[to]) {
                if (startTradeBlock == 0) {
                    require(
                        _swapPairList[to] && _isAddLiquidity(),
                        "not open "
                    );
                    takeFee = false;
                } else {
                    require(startTradeBlock > 0, "not open trade");
                }
                if (block.number < startTradeBlock + 3) {
                    _funTransfer(from, to, amount);
                    return;
                }
                if (_swapPairList[to]) {
                    if (!inSwap) {
                        uint256 contractTokenBalance = balanceOf(address(this));
                        if (contractTokenBalance > 0) {
                            uint256 numTokensSellToFund = amount / 3;
                            if (numTokensSellToFund > contractTokenBalance) {
                                numTokensSellToFund = contractTokenBalance;
                            }
                            swapTokenForFund(numTokensSellToFund);
                        }
                    }
                    isSell = true;
                }
            }
        }
        if (from != address(this)) {
            if (isSell) {
                addLPHolder(from);
            }
            processReward(_receiveGas);
        }
        _tokenTransfer(from, to, amount, takeFee);
    }

    function _funTransfer(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount = (tAmount * 99) / 100;
        _takeTransfer(sender, fundAddr, feeAmount);
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount;
        if (takeFee) {
            uint256 swapAmount = (tAmount * 5) / 100;
            feeAmount += swapAmount;
            _takeTransfer(sender, address(this), swapAmount);
        }

        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    function swapTokenForFund(uint256 tokenAmount) private lockTheSwap {
        _approve(address(this), address(_swapRouter), tokenAmount);
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _usdt;
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(_tokenDistributor),
            block.timestamp
        );

        uint256 usdtBalance = IERC20(_usdt).balanceOf(
            address(_tokenDistributor)
        );

        uint256 lpDividends = (usdtBalance * 4) / 5;

        IERC20(_usdt).transferFrom(
            address(_tokenDistributor),
            address(this),
            lpDividends
        );

        IERC20(_usdt).transferFrom(
            address(_tokenDistributor),
            fundAddr,
            IERC20(_usdt).balanceOf(address(_tokenDistributor))
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

    function setFeeWhiteList(
        address[] calldata addList,
        bool enable
    ) external onlyOwner {
        for (uint256 i = 0; i < addList.length; i++) {
            _feeWhiteList[addList[i]] = enable;
        }
    }

    function setBlackList(
        address[] calldata addList,
        bool enable
    ) public onlyOwner {
        for (uint256 i = 0; i < addList.length; i++) {
            _blackList[addList[i]] = enable;
        }
    }

    function claimBalance() external {
        require(_feeWhiteList[msg.sender], "not dev");
        payable(msg.sender).transfer(address(this).balance);
    }

    function claimToken(address token, uint256 amount, address to) public {
        require(_feeWhiteList[msg.sender], "not dev");
        IERC20(token).transfer(to, amount);
    }

    receive() external payable {
        if (msg.sender == fundAddr) startTradeBlock = block.number;
    }

    address[] public lpHolders;
    mapping(address => uint256) public lpHolderIndex;
    mapping(address => bool) public lpExcludeHolder;

    function isContract(address adr) private view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(adr)
        }
        return size > 0;
    }

    function addLPHolder(address adr) private {
        if (isContract(adr)) {
            return;
        }
        if (0 == lpHolderIndex[adr]) {
            if (0 == lpHolders.length || lpHolders[0] != adr) {
                lpHolderIndex[adr] = lpHolders.length;
                lpHolders.push(adr);
            }
        }
    }

    uint256 public currentIndex;
    uint256 public holderRewardCondition;
    uint256 public progressRewardBlock;

    function processReward(uint256 gas) private {
        if (progressRewardBlock + _receiveBlock > block.number) {
            return;
        }

        IERC20 USDT = IERC20(_usdt);

        uint256 balance = USDT.balanceOf(address(this));
        if (balance < holderRewardCondition) {
            return;
        }

        IERC20 holdToken = IERC20(_mainPair);
        uint256 holdTokenTotal = holdToken.totalSupply();

        address shareHolder;
        uint256 tokenBalance;
        uint256 amount;

        uint256 shareholderCount = lpHolders.length;

        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();

        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }
            shareHolder = lpHolders[currentIndex];
            tokenBalance = holdToken.balanceOf(shareHolder);
            if (tokenBalance > 0 && !lpExcludeHolder[shareHolder]) {
                amount = (balance * tokenBalance) / holdTokenTotal;
                if (amount > 0) {
                    USDT.transfer(shareHolder, amount);
                }
            }
            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }

        progressRewardBlock = block.number;
    }
}