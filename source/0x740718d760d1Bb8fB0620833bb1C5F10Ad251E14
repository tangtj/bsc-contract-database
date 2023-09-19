// SPDX-License-Identifier: MIT

pragma solidity ^0.8.6;

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
    function createPair(address tokenA, address tokenB) external returns (address pair);

    function feeTo() external view returns (address);
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

contract TokenDistributor {
    address public _owner;
    constructor(
        address token,
        address receiver
    ) {
        _owner = receiver;
        IERC20(token).approve(msg.sender, uint256(~uint256(0)));
    }

    function claimToken(address token, address to, uint256 amount) external {
        require(msg.sender == _owner, "!owner");
        IERC20(token).transfer(to, amount);
    }
}

interface ISwapPair {
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function totalSupply() external view returns (uint);

    function balanceOf(address account) external view returns (uint256);

    function kLast() external view returns (uint);

    function sync() external;
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

abstract contract AbsToken is IERC20, Ownable {

    mapping(address => uint256) public _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public fundAddress;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    mapping(address => bool) public _isExcludedFromFees;

    uint256 private _tTotal;

    ISwapRouter public immutable _swapRouter;
    mapping(address => bool) public _swapPairList;
    mapping(address => bool) public _swapRouters;

    bool private inSwap;

    uint256 private constant MAX = ~uint256(0);

    TokenDistributor public _tokenDistributor; //

    NFTReward public xleashNftReward;
    NFTReward public shibNftRewrad;
    LPRewardProcessor public lpRewardProcessor;

    uint256 public _buyDestroyFee = 100;
    uint256 public _buyLPDividendFee = 150;
    uint256 public _buyNFTFee = 200;
    uint256 public _buyXshibNFTFee = 50;

    uint256 public _sellDestroyFee = 200;
    uint256 public _sellLPDividendFee = 100;
    uint256 public _sellNFTFee = 150;
    uint256 public _sellXshibNFTFee = 50;   

    uint256 public _walletLimitAMount = 100*1e18;

    address public immutable _mainPair;
    address public  immutable _usdt;

    uint256 public _startTradeTime;

    address public Xshib;

    uint256 public _numToSell;

    uint256 public _xleashNftRewardCount;

    uint256 public _shibNftRewradCount;

    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor (
        address RouterAddress, 
        address UsdtAddress,
        address ReceiveAddress
    ){
        require(address(UsdtAddress) < address(this),"usdt must be token0");

        _name = "Xleash";
        _symbol = "Xleash";
        _decimals = 18;

        ISwapRouter swapRouter = ISwapRouter(RouterAddress);
        _swapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;
        _swapRouters[address(swapRouter)] = true;

        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        _usdt = UsdtAddress;
        IERC20(_usdt).approve(address(swapRouter), MAX);
        address pair = swapFactory.createPair(address(this), _usdt);
        _swapPairList[pair] = true;
        _mainPair = pair;

        uint256 tokenUnit = 10 ** _decimals;
        uint256 total = 100000000 * tokenUnit;
        _tTotal = total;

        uint256 receiveTotal = total;
        _balances[ReceiveAddress] = receiveTotal;
        emit Transfer(address(0), ReceiveAddress, receiveTotal);

        xleashNftReward = new NFTReward(0x2cc5451EfcBFA9e5AcFFC7298034e67AA7C4aD8f, msg.sender);
        shibNftRewrad =  new NFTReward(0xA04Be0083E16c1baAB65532cAEb18F2b1D5bC880, msg.sender);
        lpRewardProcessor =  new LPRewardProcessor(_usdt,_mainPair);

        Xshib = 0x64771885Fa0f6A49ae0e1B925242c3935Dbf6F34;

        _tokenDistributor = new TokenDistributor(_usdt,ReceiveAddress);

        fundAddress = 0x356D4083f05C44967dA51E16a7e1D7408E935b63;

        _isExcludedFromFees[ReceiveAddress] = true;
        _isExcludedFromFees[fundAddress] = true;
        _isExcludedFromFees[address(this)] = true;
        _isExcludedFromFees[address(swapRouter)] = true;
        _isExcludedFromFees[msg.sender] = true;
        _isExcludedFromFees[address(0)] = true;
        _isExcludedFromFees[address(0x000000000000000000000000000000000000dEaD)] = true;
        _isExcludedFromFees[address(_tokenDistributor)] = true;

        _numToSell = 10 * tokenUnit;
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
        uint256 balance = balanceOf(from);
        require(balance >= amount, "BNE");

        bool takeFee;
        if (_isExcludedFromFees[from] || _isExcludedFromFees[to]) {
            _tokenTransfer(from, to, amount, false);
            return;
        }

        bool isAddLP;

        if (to == _mainPair) {
            isAddLP = _isAddLiquidity(amount);
            if(isAddLP){
                lpRewardProcessor.addHolder(from);
                _tokenTransfer(from, to, amount, false);
                return;
            }
        }

        if ((_swapPairList[from] || _swapPairList[to])) {
            require(0 < _startTradeTime,"not open");
            takeFee = true;

            if (block.timestamp < _startTradeTime + 15) {
                _funTransfer(from, to, amount);
                return;
            }
        }

        _tokenTransfer(from, to, amount, takeFee);

        if(!_swapPairList[to] &&  _startTradeTime + 1800 > block.timestamp){
            require(balanceOf(to) <= _walletLimitAMount,"exceed wallet limit!");
        }

        if(_swapPairList[from]){
            lpRewardProcessor.processReward(200000);
        }else if(_swapPairList[to]){
            shibNftRewrad.distribute(_xleashNftRewardCount);
        }else {
            xleashNftReward.distribute(_shibNftRewradCount);
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

        uint bal = IERC20(tokenOther).balanceOf(address(_mainPair));
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

        uint bal = IERC20(tokenOther).balanceOf(address(_mainPair));
        isRemove = r > bal;
    }
    function _funTransfer(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount = tAmount * 99 / 100;
        _takeTransfer(
            sender,
            fundAddress,
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
        uint256 senderBalance = _balances[sender];
        senderBalance -= tAmount;
        _balances[sender] = senderBalance;
        uint256 feeAmount;

        if (takeFee) {
            bool isSell;
            uint256 swapFeeAmount;
            uint256 funFeeAmount;
            if (_swapPairList[sender]) {//Buy
                swapFeeAmount = tAmount * (_buyLPDividendFee + _buyDestroyFee + _buyNFTFee + _buyXshibNFTFee) / 10000;
            } else if (_swapPairList[recipient]) {//Sell
                isSell = true;
                if(_startTradeTime + 1800 > block.timestamp){
                    funFeeAmount = tAmount * 3000 / 10000;
                }else {
                    swapFeeAmount = tAmount * (_sellLPDividendFee + _sellDestroyFee + _sellNFTFee + _sellXshibNFTFee) / 10000;
                }
            }

            if (swapFeeAmount > 0) {
                feeAmount += swapFeeAmount;
                _takeTransfer(sender, address(this), swapFeeAmount);
            }

            if (funFeeAmount > 0) {
                feeAmount += funFeeAmount;
                _takeTransfer(sender, fundAddress, funFeeAmount);
            }

            if (isSell && !inSwap) {
                uint256 contractTokenBalance = _balances[address(this)];
                uint256 numToSell = _numToSell;
                if (contractTokenBalance >= numToSell) {
                    swapTokenForFund(numToSell);
                }
            }
        }

        _takeTransfer(sender, recipient, tAmount - feeAmount);
    }

     function swapTokenForFund(uint256 tokenAmount) public lockTheSwap {
        uint256 totalFee = _buyLPDividendFee + _buyDestroyFee + _buyNFTFee + _buyXshibNFTFee 
                           + _sellDestroyFee + _sellLPDividendFee + _sellNFTFee + _sellXshibNFTFee;

        uint256 swapAmount4XSHI = (tokenAmount *(_buyDestroyFee + _sellDestroyFee)) / totalFee;

        if (swapAmount4XSHI > 0) {
            address[] memory _path = new address[](3);
            _path[0] = address(this);
            _path[1] = _usdt;
            _path[2] = Xshib;
            try
                _swapRouter
                    .swapExactTokensForTokensSupportingFeeOnTransferTokens(
                        swapAmount4XSHI,
                        0,
                        _path,
                        0x000000000000000000000000000000000000dEaD,
                        block.timestamp
                    )
            {} catch {
                emit Failed_swapExactTokensForTokensSupportingFeeOnTransferTokens(
                    0
                );
            }
        }

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _usdt;
        try
            _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
                tokenAmount - swapAmount4XSHI,
                0,
                path,
                address(_tokenDistributor),
                block.timestamp
            )
        {} catch {
            emit Failed_swapExactTokensForTokensSupportingFeeOnTransferTokens(
                0
            );
        }

        uint256 usdtFee = _buyLPDividendFee + _buyNFTFee + _buyXshibNFTFee 
                            + _sellLPDividendFee + _sellNFTFee + _sellXshibNFTFee;

        IERC20 USDT = IERC20(_usdt);

        uint256 usdtBanlance = USDT.balanceOf(address(_tokenDistributor));

        uint256 gouyingNftRewardAmount = (usdtBanlance *(_buyNFTFee + _sellNFTFee)) / usdtFee;

        if(gouyingNftRewardAmount > 0){
            USDT.transferFrom(address(_tokenDistributor), address(xleashNftReward), gouyingNftRewardAmount);
        }

        uint256 shibNftRewardAmount = (usdtBanlance *(_buyXshibNFTFee + _sellXshibNFTFee)) / usdtFee;

        if(shibNftRewardAmount > 0){
            USDT.transferFrom(address(_tokenDistributor), address(shibNftRewrad), shibNftRewardAmount);
        }

        uint256 leftAmout =usdtBanlance - gouyingNftRewardAmount - shibNftRewardAmount;

        if(leftAmout > 0){
            USDT.transferFrom(address(_tokenDistributor), address(lpRewardProcessor), leftAmout);
        }

    }

    event Failed_swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 value
    );

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
        _isExcludedFromFees[addr] = true;
    }

    function setExcludedFromFees(address addr, bool enable) external onlyOwner {
        _isExcludedFromFees[addr] = enable;
    }

    function batchSetExcludedFromFees(address [] memory addr, bool enable) external onlyOwner {
        for (uint i = 0; i < addr.length; i++) {
            _isExcludedFromFees[addr[i]] = enable;
        }
    }

    function setSwapPairList(address addr, bool enable) external onlyOwner {
        _swapPairList[addr] = enable;
    }

    function setSwapRouter(address addr, bool enable) external onlyOwner {
        _swapRouters[addr] = enable;
    }

   function claimBalance() external {
        payable(fundAddress).transfer(address(this).balance);
    }

    function claimToken(
        address token,
        uint256 amount,
        address to
    ) external onlyFunder {
        IERC20(token).transfer(to, amount);
    }

    modifier onlyFunder() {
        require(_owner == msg.sender || fundAddress == msg.sender, "!Funder");
        _;
    }

    receive() external payable {}

    function startTrade() external onlyOwner {
        require(0 == _startTradeTime, "T");
        _startTradeTime = block.timestamp;
    }

    function setNumToSell(uint256 amount) external onlyOwner {
        _numToSell = amount;
    }

    function setXleashAddress(address _addr)external  onlyOwner{
        xleashNftReward = new NFTReward(_addr, msg.sender);
    }

    function setXshibAddress(address _addr)external  onlyOwner{
        shibNftRewrad =  new NFTReward(_addr, msg.sender);
    }

    function addHolderBatch(address[] memory addList) external onlyOwner{
        for(uint256 i = 0; i < addList.length; i++){
            lpRewardProcessor.addHolder(addList[i]);
        }
    }

    function setHolderRewardCondition(uint256 amount) external onlyOwner {
        lpRewardProcessor.setHolderRewardCondition(amount);
    }

    function setExcludeHolder(address addr, bool enable) external onlyOwner {
        lpRewardProcessor.setExcludeHolder(addr, enable);
    }

    function withdrawTo(address destination, uint256 amount) external onlyOwner {
        lpRewardProcessor.withdrawTo(destination,  amount);
    }

    function setLpManager(address _addr) external onlyOwner{
        lpRewardProcessor.setLpManager(_addr);
    }

    function setxleashNftRewardCount(uint256 _count) external onlyOwner {
        _xleashNftRewardCount = _count;
    }

    function setShibNftRewradCount(uint256 _count) external onlyOwner {
        _shibNftRewradCount = _count;
    }

}

contract Xleash is AbsToken {
    constructor() AbsToken(
    //SwapRouter
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),
    //USDT
        address(0x55d398326f99059fF775485246999027B3197955),
    //Receive
        address(0xf27845e3223C4f8A93B454e4CcF3713F2ADA0000)
    ){

    }
}


interface IERC721 {
    function totalSupply() external view returns (uint256);
    function ownerOf(uint256 tokenId) external view returns (address);
}

contract NFTReward is Ownable {
    IERC20 public USDT = IERC20(0x55d398326f99059fF775485246999027B3197955);
    IERC721 public nftToken;

    uint256 public lastProcessedTokenIndex = 0;
    uint256 public rewardPerOwner;
    uint256 public minDistributeAmount = 1e18;
    uint256 public startTokenId = 1;

    mapping(uint256 => bool) public isExcluded;
    uint256[] public excludedNftIds;

    constructor(address _nftAddress,address _owner) {
        nftToken = IERC721(_nftAddress);
        transferOwnership(_owner);
    }

    function addExcludedNftId(uint256 _tokenId) external onlyOwner {
        require(!isExcluded[_tokenId], "Token ID already excluded");
        isExcluded[_tokenId] = true;
        excludedNftIds.push(_tokenId);
    }

    function removeExcludedNftId(uint256 _tokenId) external onlyOwner {
        require(isExcluded[_tokenId], "Token ID is not excluded");
        isExcluded[_tokenId] = false;

        // Remove the tokenId from excludedNftIds array
        for (uint256 i = 0; i < excludedNftIds.length; i++) {
            if (excludedNftIds[i] == _tokenId) {
                excludedNftIds[i] = excludedNftIds[excludedNftIds.length - 1];
                excludedNftIds.pop();
                break;
            }
        }
    }

    function distribute(uint256 numberOfTokens) external {
        if (rewardPerOwner == 0) {
            uint256 totalNFT = nftToken.totalSupply() - excludedNftIds.length;
            if(totalNFT == 0) {
                return;
            }

            uint256 totalUSDT = USDT.balanceOf(address(this));
            if(totalUSDT < minDistributeAmount){
                return;
            }
            rewardPerOwner = totalUSDT / totalNFT;
            lastProcessedTokenIndex = 0;
        }

        for (uint256 i = 0; i < numberOfTokens && lastProcessedTokenIndex < nftToken.totalSupply(); i++) {
            if (!isExcluded[lastProcessedTokenIndex]) {
                address nftOwner = nftToken.ownerOf(lastProcessedTokenIndex + startTokenId);
                if(nftOwner==address(0))continue ;
                USDT.transfer(nftOwner, rewardPerOwner);
            }
            lastProcessedTokenIndex++;
        }

        // Reset if finished
        if (lastProcessedTokenIndex >= nftToken.totalSupply()) {
            rewardPerOwner = 0;
        }
    }

    function setMinDistributeAmount(uint256 newValue) external  onlyOwner{
        minDistributeAmount = newValue;
    }

    function withdrawUSDT(address _to) external onlyOwner {
        uint256 totalUSDT = USDT.balanceOf(address(this));
        require(USDT.transfer(_to, totalUSDT), "Withdrawal failed");
    }

    // Optional: Change NFT address if needed
    function changeNFTAddress(address _newAddress) external onlyOwner {
        nftToken = IERC721(_newAddress);
    }
}

interface ILpManager{
    function userBalances(address user) external returns (uint256);
}

contract LPRewardProcessor is Ownable {
    address[] public holders;
    mapping(address => uint256) holderIndex;
    mapping(address => bool) excludeHolder;

    uint256 private currentIndex;
    uint256 private holderRewardCondition = 1e18;
    uint256 private progressRewardBlock;
    address private _usdt;
    address private _mainPair;

    ILpManager public lpManager = ILpManager(0x2B5A8F624e6D961851c3d890710ea0e1b1531780); 

    constructor(address usdt, address mainPair) {
        _usdt = usdt;
        _mainPair = mainPair;
    }

    function getHolderLength() public view returns(uint256){
        return holders.length;
    }

    function addHolder(address adr) external  onlyOwner{
        uint256 size;
        assembly {size := extcodesize(adr)}
        if (size > 0) {
            return;
        }
        if (0 == holderIndex[adr]) {
            if (0 == holders.length || holders[0] != adr) {
                holderIndex[adr] = holders.length;
                holders.push(adr);
            }
        }
    }

    function processReward(uint256 gas) external {
        if (progressRewardBlock + 200 > block.number) {
            return;
        }

        IERC20 USDT = IERC20(_usdt);
        uint256 balance = USDT.balanceOf(address(this));
        if (balance < holderRewardCondition) {
            return;
        }

        _distributeReward(USDT, balance, gas);
        progressRewardBlock = block.number;
    }

    function processRewardWithoutCondition(uint256 gas) public {
        IERC20 USDT = IERC20(_usdt);
        uint256 balance = USDT.balanceOf(address(this));
        if (balance == 0) {
            return;
        }
        _distributeReward(USDT, balance, gas);
    }

    function _distributeReward(IERC20 USDT, uint256 balance, uint256 gas) private {
        IERC20 holdToken = IERC20(_mainPair);
        uint holdTokenTotal = holdToken.totalSupply();

        address shareHolder;
        uint256 tokenBalance;
        uint256 amount;

        uint256 shareholderCount = holders.length;
        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();

        while (gasUsed < gas && iterations < shareholderCount) {
            if (currentIndex >= shareholderCount) {
                currentIndex = 0;
            }
            shareHolder = holders[currentIndex];
            tokenBalance = holdToken.balanceOf(shareHolder) + lpManager.userBalances(shareHolder);
            if (tokenBalance > 0 && !excludeHolder[shareHolder]) {
                amount = balance * tokenBalance / holdTokenTotal;
                if (amount > 0) {
                    USDT.transfer(shareHolder, amount);
                }
            }

            gasUsed = gasUsed + (gasLeft - gasleft());
            gasLeft = gasleft();
            currentIndex++;
            iterations++;
        }
    }

    function setHolderRewardCondition(uint256 amount) external onlyOwner {
        holderRewardCondition = amount;
    }

    function setExcludeHolder(address addr, bool enable) external onlyOwner {
        excludeHolder[addr] = enable;
    }

    function setLpManager(address _addr) external onlyOwner{
        lpManager = ILpManager(_addr);
    }

    function withdrawTo(address destination, uint256 amount) external onlyOwner {
        require(IERC20(_usdt).transfer(destination, amount), "Transfer failed");
    }
}