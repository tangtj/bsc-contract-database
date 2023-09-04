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
        uint256 qty;
    }

    struct UserInfo {
        uint256 buyAmount;
        uint256 lockedLPAmount;
        uint256 claimedLPAmount;
    }

    SaleInfo private _saleInfo;
    mapping(address => UserInfo) private _userInfo;

    bool private _pauseBuy = true;
    bool private _pauseClaim = true;
    address public _w3n;
    address public _moss;
    address public _cashAddress;

    uint256 public _totalBNB;
    address public _nftAddress;

    uint256 public _w3nRate = 7000;
    uint256 public _mossRate = 2000;
    uint256 public _w3nUserRate = 5000;
    uint256 public _lpReleaseTime;
    uint256 public _lpReleaseDuration = 15 days;

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
        _saleInfo.price = 2 ether;
        _saleInfo.w3nAmount = 350 * w3nUnit;
        _saleInfo.qty = 100;

        IERC20(W3NAddress).approve(RouterAddress, MAX);
        IERC20(MOSSAddress).approve(RouterAddress, MAX);
    }

    function buy() external payable {
        require(!_pauseBuy, "pauseBuy");
        address account = msg.sender;
        UserInfo storage userInfo = _userInfo[account];
        require(0 == userInfo.buyAmount, "joined");

        SaleInfo storage sale = _saleInfo;
        require(sale.saleNum < sale.qty, "no qty");
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

        INFT(_nftAddress).mint(account, 1);
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

    function shopInfo() external view returns (
        bool pauseBuy, bool pauseClaim,
        uint256 price, uint256 w3nAmount, uint256 saleNum, uint256 qty
    ){
        pauseBuy = _pauseBuy;
        pauseClaim = _pauseClaim;
        price = _saleInfo.price;
        w3nAmount = _saleInfo.w3nAmount;
        saleNum = _saleInfo.saleNum;
        qty = _saleInfo.qty;
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

    function setPrice(uint256 price) external onlyWhiteList {
        _saleInfo.price = price;
    }

    function setQty(uint256 qty) external onlyWhiteList {
        _saleInfo.qty = qty;
    }

    function setW3NAmount(uint256 amount) external onlyWhiteList {
        _saleInfo.w3nAmount = amount;
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
        uint256 lockedLPAmount,
        uint256 claimedLPAmount,
        uint256 releaseLPAmount
    ){
        UserInfo storage userInfo = _userInfo[account];
        buyAmount = userInfo.buyAmount;
        balance = account.balance;
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

contract NFTSale is AbsPreSale {
    constructor() AbsPreSale(
    //SwapRouter
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E),
    //W3N
        address(0xbCcA7317D95Fe1F420a9f3C4CECa4E7F24045333),
    //MOSS
        address(0xC651Cf5Dd958B6D7E4c417F1f366659237C34166),
    //GenesisNFT
        address(0x71B826342BB00339c7993fcF70b75F26f5895421),
    //Cash
        address(0x26944342DE6c482bc5f93Fa094dE659DA2433639)
    ){

    }
}