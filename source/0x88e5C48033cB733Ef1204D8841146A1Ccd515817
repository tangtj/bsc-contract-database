// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

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

interface INFT {
    function mint(address to, uint256 num) external;
}

interface IMintPool {
    function bindInvitor(address account, address invitor) external;

    function _invitor(address account) external view returns (address invitor);
}

interface ISwapRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;
}

interface ISwapFactory {
    function getPair(address tokenA, address tokenB) external view returns (address pair);
}

interface IToken {
    function addUserLPAmount(address account, uint256 lpAmount) external;
}

abstract contract AbsPreSale is Ownable {
    struct SaleInfo {
        uint256 price;
        uint256 w3nAmount;
        uint256 saleNum;
    }

    struct UserInfo {
        uint256 buyAmount;
        uint256 saleInviteAccount;
        uint256 claimedNFTNum;
        uint256 lockedLPAmount;
        uint256 claimedLPAmount;
    }

    SaleInfo[] private _saleInfo;
    mapping(address => UserInfo) private _userInfo;

    bool private _pauseBuy = true;
    bool private _pauseClaim = true;
    address public _w3n;
    address public _moss;
    address public _cashAddress;

    uint256 public _totalBNB;
    address public _nftAddress;
    uint256 private _rewardNFTCondition = 10;

    uint256 public _w3nRate = 7000;
    uint256 public _mossRate = 2000;
    uint256 public _w3nUserRate = 5000;
    uint256 public _lpReleaseTime;
    uint256 public _lpReleaseDuration = 15 days;

    IMintPool public _mintPool;
    ISwapRouter public immutable _swapRouter;
    address public immutable _weth;
    address public _w3nLP;
    uint256 private constant MAX = ~uint256(0);

    constructor(
        address RouterAddress, address W3NAddress, address MOSSAddress,
        address NFTAddress, address CashAddress
    ){
        _swapRouter = ISwapRouter(RouterAddress);
        _weth = _swapRouter.WETH();

        _w3n = W3NAddress;
        _moss = MOSSAddress;
        _nftAddress = NFTAddress;
        _cashAddress = CashAddress;

        uint256 w3nUnit = 10 ** IERC20(W3NAddress).decimals();
        _saleInfo.push(SaleInfo(4 ether / 10, 70 * w3nUnit, 0));
        _saleInfo.push(SaleInfo(12 ether / 10, 210 * w3nUnit, 0));
        _saleInfo.push(SaleInfo(2 ether, 350 * w3nUnit, 0));

        IERC20(W3NAddress).approve(RouterAddress, MAX);
        IERC20(MOSSAddress).approve(RouterAddress, MAX);
    }

    function buy(uint256 saleId, address invitor) external payable {
        require(!_pauseBuy, "pauseBuy");
        address account = msg.sender;
        UserInfo storage userInfo = _userInfo[account];
        require(0 == userInfo.buyAmount, "joined");

        _mintPool.bindInvitor(account, invitor);

        SaleInfo storage sale = _saleInfo[saleId];
        sale.saleNum += 1;
        uint256 price = sale.price;
        require(msg.value >= price, "invalid value");
        uint256 cashEth = price;
        userInfo.buyAmount += price;
        _totalBNB += price;

        uint256 w3nLPETH = price * _w3nRate / 10000;
        cashEth -= w3nLPETH;
        uint256 nowTime = block.timestamp;
        (,,uint256 w3nLPAmount) = _swapRouter.addLiquidityETH{value : w3nLPETH}(_w3n, sale.w3nAmount, 0, 0, address(this), nowTime);
        uint256 userLPAmount = w3nLPAmount * _w3nUserRate / _w3nRate;
        userInfo.lockedLPAmount += userLPAmount;

        w3nLPAmount -= userLPAmount;
        address cashAddress = _cashAddress;
        if (w3nLPAmount > 0) {
            IERC20(_w3nLP).transfer(cashAddress, w3nLPAmount);
        }

        uint256 mossLPETH = price * _mossRate / 10000;
        if (mossLPETH > 0) {
            cashEth -= mossLPETH;
            mossLPETH = mossLPETH / 2;
            address moss = _moss;
            uint256 mossAmount = IERC20(moss).balanceOf(address(this));
            address[] memory path = new address[](2);
            path[0] = _weth;
            path[1] = moss;
            _swapRouter.swapExactETHForTokensSupportingFeeOnTransferTokens{value : mossLPETH}(
                0, path, address(this), nowTime
            );
            mossAmount = IERC20(moss).balanceOf(address(this)) - mossAmount;
            _swapRouter.addLiquidityETH{value : mossLPETH}(moss, mossAmount, 0, 0, cashAddress, nowTime);
        }

        if (cashEth > 0) {
            payable(cashAddress).transfer(cashEth);
        }

        _calNFTReward(account);
    }

    function _calNFTReward(address account) private {
        address invitor = _mintPool._invitor(account);
        if (address(0) != invitor) {
            UserInfo storage invitorInfo = _userInfo[invitor];
            uint256 saleInviteAccount = invitorInfo.saleInviteAccount;
            saleInviteAccount += 1;
            invitorInfo.saleInviteAccount = saleInviteAccount;
            uint256 nftNum = saleInviteAccount / _rewardNFTCondition;
            uint256 claimedNFTNum = invitorInfo.claimedNFTNum;
            if (nftNum > claimedNFTNum) {
                invitorInfo.claimedNFTNum = nftNum;
                INFT(_nftAddress).mint(invitor, nftNum - claimedNFTNum);
            }
        }
    }

    function claimLP() external {
        address account = msg.sender;
        require(!_pauseClaim, "pauseClaim");
        UserInfo storage userInfo = _userInfo[account];
        uint256 pendingLP = getReleaseLPAmount(account) - userInfo.claimedLPAmount;
        require(pendingLP > 0, "no reward");
        userInfo.claimedLPAmount += pendingLP;
        _giveToken(_w3nLP, account, pendingLP);
        IToken(_w3n).addUserLPAmount(account, pendingLP);
    }

    function _giveToken(address tokenAddress, address account, uint256 tokenNum) private {
        IERC20 token = IERC20(tokenAddress);
        require(token.balanceOf(address(this)) >= tokenNum, "PTNE");
        token.transfer(account, tokenNum);
    }

    function allSaleInfo() external view returns (
        uint256[] memory price, uint256[] memory w3nAmount,
        uint256[] memory saleNum
    ) {
        uint256 len = _saleInfo.length;
        price = new uint256[](len);
        w3nAmount = new uint256[](len);
        saleNum = new uint256[](len);
        for (uint256 i; i < len; i++) {
            SaleInfo memory sale = _saleInfo[i];
            price[i] = sale.price;
            w3nAmount[i] = sale.w3nAmount;
            saleNum[i] = sale.saleNum;
        }
    }

    function shopInfo() external view returns (
        bool pauseBuy, bool pauseClaim, uint256 rewardNFTCondition
    ){
        pauseBuy = _pauseBuy;
        pauseClaim = _pauseClaim;
        rewardNFTCondition = _rewardNFTCondition;
    }

    receive() external payable {}

    modifier onlyWhiteList() {
        address msgSender = msg.sender;
        require(msgSender == _cashAddress || msgSender == _owner, "nw");
        _;
    }

    function setW3nRate(uint256 r) external onlyWhiteList {
        _w3nRate = r;
        require(_w3nRate + _mossRate <= 10000, "Mw");
    }

    function setMossRate(uint256 r) external onlyWhiteList {
        _mossRate = r;
        require(_w3nRate + _mossRate <= 10000, "Mw");
    }

    function setW3nUserRate(uint256 r) external onlyWhiteList {
        _w3nUserRate = r;
        require(_w3nUserRate <= _mossRate, "Mm");
    }

    function setCash(address adr) external onlyWhiteList {
        _cashAddress = adr;
    }

    function setMintPool(address adr) external onlyWhiteList {
        _mintPool = IMintPool(adr);
    }

    function setW3N(address adr) external onlyWhiteList {
        _w3n = adr;
        IERC20(adr).approve(address(_swapRouter), MAX);
    }

    function setW3NLP(address adr) external onlyWhiteList {
        _w3nLP = adr;
    }

    function setMOSS(address adr) external onlyWhiteList {
        _moss = adr;
        IERC20(adr).approve(address(_swapRouter), MAX);
    }

    function setNFTAddress(address adr) external onlyWhiteList {
        _nftAddress = adr;
    }

    function setPauseBuy(bool pause) external onlyWhiteList {
        if (!pause) {
            address w3nLP = ISwapFactory(_swapRouter.factory()).getPair(_w3n, _weth);
            require(address(0) != w3nLP, "no w3nLP");
            _w3nLP = w3nLP;
        }
        _pauseBuy = pause;
    }

    function setPauseClaim(bool pause) external onlyWhiteList {
        _pauseClaim = pause;
    }

    function setRewardNFTCondition(uint256 num) external onlyWhiteList {
        require(num > 0, "N0");
        _rewardNFTCondition = num;
    }

    function setPrice(uint256 saleId, uint256 price) external onlyWhiteList {
        _saleInfo[saleId].price = price;
    }

    function setW3NAmount(uint256 saleId, uint256 amount) external onlyWhiteList {
        _saleInfo[saleId].w3nAmount = amount;
    }

    function setLPReleaseTime(uint256 t) external onlyWhiteList {
        _lpReleaseTime = t;
    }

    function startLPRelease() external onlyWhiteList {
        require(_lpReleaseTime == 0, "started");
        _lpReleaseTime = block.timestamp;
    }

    function setLPReleaseDuration(uint256 d) external onlyWhiteList {
        require(d > 0, "N0");
        _lpReleaseDuration = d;
    }

    function claimBalance() external {
        address payable addr = payable(_cashAddress);
        addr.transfer(address(this).balance);
    }

    function claimToken(address erc20Address) external onlyWhiteList {
        IERC20 erc20 = IERC20(erc20Address);
        erc20.transfer(_cashAddress, erc20.balanceOf(address(this)));
    }

    function getUserInfo(address account) external view returns (
        uint256 buyAmount,
        uint256 balance,
        uint256 saleInviteAccount,
        uint256 claimedNFTNum,
        uint256 lockedLPAmount,
        uint256 claimedLPAmount,
        uint256 releaseLPAmount
    ){
        UserInfo storage userInfo = _userInfo[account];
        buyAmount = userInfo.buyAmount;
        balance = account.balance;
        saleInviteAccount = userInfo.saleInviteAccount;
        claimedNFTNum = userInfo.claimedNFTNum;
        lockedLPAmount = userInfo.lockedLPAmount;
        claimedLPAmount = userInfo.claimedLPAmount;
        releaseLPAmount = getReleaseLPAmount(account);
    }

    function getReleaseLPAmount(address account) public view returns (uint256 releaseLPAmount){
        uint256 lpReleaseTime = _lpReleaseTime;
        if (lpReleaseTime > 0) {
            uint256 nowTime = block.timestamp;
            if (nowTime > lpReleaseTime) {
                UserInfo storage userInfo = _userInfo[account];
                uint256 lockedLPAmount = userInfo.lockedLPAmount;
                releaseLPAmount = lockedLPAmount * (nowTime - lpReleaseTime) / _lpReleaseDuration;
                if (releaseLPAmount > lockedLPAmount) {
                    releaseLPAmount = lockedLPAmount;
                }
            }
        }
    }
}

contract TokenSale is AbsPreSale {
    constructor() AbsPreSale(
    //SwapRouter
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),
    //W3N
        address(0xbCcA7317D95Fe1F420a9f3C4CECa4E7F24045333),
    //MOSS
        address(0xC651Cf5Dd958B6D7E4c417F1f366659237C34166),
    //WarNFT
        address(0xeCd07e4D240A9Dd8111c7ae7Bc6bd3a00FB0c8Ad),
    //Cash
        address(0x26944342DE6c482bc5f93Fa094dE659DA2433639)
    ){

    }
}