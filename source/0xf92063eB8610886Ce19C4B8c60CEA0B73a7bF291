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
        require(_owner == msg.sender, "!owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "new 0");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

interface INFT {
    function mint(address to, uint256 num) external;
}

abstract contract AbsPreSale is Ownable {
    struct SaleInfo {
        uint256 price;
        uint256 tokenAmount;
        uint256 qty;
        uint256 saleNum;
        bool rewardNFT;
    }

    struct UserInfo {
        bool isActive;
        uint256 buyUsdtAmount;
        uint256 buyTokenAmount;
        uint256 claimedAmount;
        uint256 inviteUsdt;
    }

    address private _usdtAddress;
    address public _cashAddress;
    address private _tokenAddress;

    SaleInfo[] private _saleInfo;
    mapping(address => UserInfo) private _userInfo;

    bool private _pauseBuy = false;
    bool private _pauseClaim = true;

    uint256 private _totalUsdt;
    uint256 private _totalToken;
    uint256 private _totalInviteUsdt;

    mapping(address => address) public _invitor;
    mapping(address => address[]) public _binder;
    mapping(uint256 => uint256) public _inviteFee;
    uint256 private constant _inviteLen = 5;
    address private _defaultInvitor;

    uint256 public _releaseStartTime;
    uint256 public _releaseMarginDuration = 7 days;
    uint256 public _maxReleaseNum = 10;

    address public _nftAddress;

    constructor(
        address UsdtAddress, address TokenAddress, address NFTAddress,
        address CashAddress, address DefaultInvitor
    ){
        _tokenAddress = TokenAddress;
        _usdtAddress = UsdtAddress;
        _nftAddress = NFTAddress;
        _cashAddress = CashAddress;
        _defaultInvitor = DefaultInvitor;
        _userInfo[DefaultInvitor].isActive = true;
        _inviteFee[0] = 1000;
        _inviteFee[1] = 800;
        _inviteFee[2] = 600;
        _inviteFee[3] = 300;
        _inviteFee[4] = 100;

        uint256 usdtUnit = 10 ** IERC20(UsdtAddress).decimals();
        uint256 tokenUnit = 10 ** IERC20(TokenAddress).decimals();

        _saleInfo.push(SaleInfo(100 * usdtUnit, 500 * tokenUnit, 100000000, 0, false));
        _saleInfo.push(SaleInfo(500 * usdtUnit, 3000 * tokenUnit, 100000000, 0, false));
        _saleInfo.push(SaleInfo(1000 * usdtUnit, 7000 * tokenUnit, 100000000, 0, false));
        _saleInfo.push(SaleInfo(3000 * usdtUnit, 24000 * tokenUnit, 100000000, 0, false));
        _saleInfo.push(SaleInfo(5000 * usdtUnit, 50000 * tokenUnit, 100000000, 0, true));
    }

    function buy(uint256 saleId, address invitor) external {
        require(!_pauseBuy, "pauseBuy");

        address account = msg.sender;
        _bindInvitor(account, invitor);

        SaleInfo storage sale = _saleInfo[saleId];
        require(sale.qty > sale.saleNum, "no qty");
        sale.saleNum += 1;

        uint256 price = sale.price;
        uint256 tokenAmount = sale.tokenAmount;

        UserInfo storage userInfo = _userInfo[account];
        userInfo.buyTokenAmount += tokenAmount;
        userInfo.buyUsdtAmount += price;

        _totalUsdt += price;
        _totalToken += tokenAmount;

        address usdtAddress = _usdtAddress;
        _takeToken(usdtAddress, account, address(this), price);

        if (sale.rewardNFT) {
            INFT(_nftAddress).mint(account, 1);
        }

        uint256 len = _inviteLen;
        address current = account;
        uint256 totalInviteUsdt;
        for (uint256 i; i < len; ++i) {
            invitor = _invitor[current];
            if (address(0) == invitor) {
                break;
            }
            uint256 inviteAmount = price * _inviteFee[i] / 10000;
            totalInviteUsdt += inviteAmount;
            _userInfo[invitor].inviteUsdt += inviteAmount;
            _giveToken(usdtAddress, invitor, inviteAmount);
            current = invitor;
        }

        _giveToken(usdtAddress, _cashAddress, price - totalInviteUsdt);
        _totalInviteUsdt += totalInviteUsdt;
    }

    function _bindInvitor(address account, address invitor) private {
        UserInfo storage user = _userInfo[account];
        if (!user.isActive) {
            require(address(0) != invitor, "invitor 0");
            require(_userInfo[invitor].isActive, "invitor !Active");
            _invitor[account] = invitor;
            _binder[invitor].push(account);
            user.isActive = true;
        }
    }

    function claim() external {
        require(!_pauseClaim, "pauseClaim");
        address account = msg.sender;
        (uint256 pendingReward,) = getPendingReward(account);
        if (0 == pendingReward) {
            return;
        }
        _giveToken(_tokenAddress, account, pendingReward);
        UserInfo storage userInfo = _userInfo[account];
    unchecked{
        userInfo.claimedAmount += pendingReward;
    }
    }

    function getReleaseAmount(uint256 amount) public view returns (uint256 releaseAmount){
        uint256 startTime = _releaseStartTime;
        if (0 == startTime) {
            return 0;
        }
        uint256 blockTime = block.timestamp;
        if (startTime > blockTime) {
            return 0;
        }
        uint256 duration = blockTime - startTime;
        uint256 releaseNum = duration / _releaseMarginDuration;
        releaseNum += 1;
        releaseAmount = amount * releaseNum / _maxReleaseNum;
        if (releaseAmount > amount) {
            releaseAmount = amount;
        }
    }

    function _giveToken(address tokenAddress, address account, uint256 amount) private {
        if (0 == amount) {
            return;
        }
        IERC20 token = IERC20(tokenAddress);
        require(token.balanceOf(address(this)) >= amount, "PTNE");
        safeTransfer(tokenAddress, account, amount);
    }

    function _takeToken(address tokenAddress, address from, address to, uint256 tokenNum) private {
        IERC20 token = IERC20(tokenAddress);
        require(token.balanceOf(address(from)) >= tokenNum, "TNE");
        safeTransferFrom(tokenAddress, from, to, tokenNum);
    }

    function safeTransfer(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TF');
    }

    function safeTransferETH(address to, uint value) internal {
        (bool success,) = to.call{value : value}(new bytes(0));
        require(success, 'ETF');
    }

    function safeTransferFrom(address token, address from, address to, uint value) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TFF');
    }

    function allSaleInfo() external view returns (
        uint256[] memory prices, uint256[] memory tokenNum,
        uint256[] memory qtyNum, uint256[] memory saleNum, bool[] memory rewardNft
    ) {
        uint256 len = _saleInfo.length;
        prices = new uint256[](len);
        tokenNum = new uint256[](len);
        qtyNum = new uint256[](len);
        saleNum = new uint256[](len);
        rewardNft = new bool[](len);
        for (uint256 i; i < len; i++) {
            SaleInfo storage sale = _saleInfo[i];
            prices[i] = sale.price;
            tokenNum[i] = sale.tokenAmount;
            qtyNum[i] = sale.qty;
            saleNum[i] = sale.saleNum;
            rewardNft[i] = sale.rewardNFT;
        }
    }

    function shopInfo() external view returns (
        address usdtAddress, uint256 usdtDecimals, string memory usdtSymbol,
        address tokenAddress, uint256 tokenDecimals, string memory tokenSymbol,
        bool pauseBuy, bool pauseClaim,
        uint256 totalUsdt, uint256 totalToken, uint256 totalInviteUsdt,
        address defaultInvitor
    ){
        usdtAddress = _usdtAddress;
        usdtDecimals = IERC20(usdtAddress).decimals();
        usdtSymbol = IERC20(usdtAddress).symbol();
        tokenAddress = _tokenAddress;
        tokenDecimals = IERC20(tokenAddress).decimals();
        tokenSymbol = IERC20(tokenAddress).symbol();
        pauseBuy = _pauseBuy;
        pauseClaim = _pauseClaim;
        totalUsdt = _totalUsdt;
        totalToken = _totalToken;
        totalInviteUsdt = _totalInviteUsdt;
        defaultInvitor = _defaultInvitor;
    }

    function getUserInfo(address account) external view returns (
        uint256 pendingReward, uint256 releaseAmount,
        uint256 usdtBalance, uint256 usdtAllowance,
        uint256 buyUsdtAmount, uint256 buyTokenAmount,
        uint256 claimedAmount, uint256 inviteUsdt,
        bool isActive, address invitor
    ){
        (pendingReward, releaseAmount) = getPendingReward(account);
        usdtBalance = IERC20(_usdtAddress).balanceOf(account);
        usdtAllowance = IERC20(_usdtAddress).allowance(account, address(this));
        UserInfo storage userInfo = _userInfo[account];
        buyUsdtAmount = userInfo.buyUsdtAmount;
        buyTokenAmount = userInfo.buyTokenAmount;
        claimedAmount = userInfo.claimedAmount;
        inviteUsdt = userInfo.inviteUsdt;
        isActive = userInfo.isActive;
        invitor = _invitor[account];
    }

    function getPendingReward(address account) public view returns (uint256 pendingReward, uint256 releaseAmount){
        UserInfo storage userInfo = _userInfo[account];
        uint256 claimedAmount = userInfo.claimedAmount;
        releaseAmount = getReleaseAmount(userInfo.buyTokenAmount);
        if (releaseAmount > claimedAmount) {
            pendingReward = releaseAmount - claimedAmount;
        }
    }

    function getBinderLength(address account) public view returns (uint256){
        return _binder[account].length;
    }

    receive() external payable {}

    function addSale(uint256 usdtAmount, uint256 tokenAmount, uint256 qty, bool rewardNft) external onlyOwner {
        _saleInfo.push(SaleInfo(usdtAmount, tokenAmount, qty, 0, rewardNft));
    }

    function setPrice(uint256 saleId, uint256 price) external onlyOwner {
        _saleInfo[saleId].price = price;
    }

    function setTokenAmount(uint256 saleId, uint256 tokenAmount) external onlyOwner {
        _saleInfo[saleId].tokenAmount = tokenAmount;
    }

    function setQty(uint256 saleId, uint256 qty) external onlyOwner {
        _saleInfo[saleId].qty = qty;
    }

    function setRewardNft(uint256 saleId, bool rewardNft) external onlyOwner {
        _saleInfo[saleId].rewardNFT = rewardNft;
    }

    function startClaim() external onlyOwner {
        _pauseClaim = false;
        _releaseStartTime = block.timestamp;
    }

    function setReleaseStartTime(uint256 time) external onlyOwner {
        _releaseStartTime = time;
    }

    function setReleaseMarginDuration(uint256 duration) external onlyOwner {
        require(duration > 0, "0");
        _releaseMarginDuration = duration;
    }

    function setTokenAddress(address adr) external onlyOwner {
        _tokenAddress = adr;
    }

    function setUsdtAddress(address adr) external onlyOwner {
        _usdtAddress = adr;
    }

    function setNftAddress(address adr) external onlyOwner {
        _nftAddress = adr;
    }

    function setCashAddress(address adr) external onlyOwner {
        _cashAddress = adr;
    }

    function setPauseBuy(bool pause) external onlyOwner {
        _pauseBuy = pause;
    }

    function setPauseClaim(bool pause) external onlyOwner {
        _pauseClaim = pause;
    }

    //
    function setInviteFee(uint256 i, uint256 fee) external onlyOwner {
        _inviteFee[i] = fee;
    }

    function claimBalance(address to, uint256 amount) external onlyOwner {
        safeTransferETH(to, amount);
    }

    function claimToken(address token, address to, uint256 amount) external onlyOwner {
        _giveToken(token, to, amount);
    }

    function setDefaultInvitor(address adr) external onlyOwner {
        _defaultInvitor = adr;
        _userInfo[adr].isActive = true;
    }
}

contract BearSale is AbsPreSale {
    constructor() AbsPreSale(
    //USDT
        address(0x55d398326f99059fF775485246999027B3197955),
    //Bear
        address(0x5DcFAce60eb1cD5501c291Cb8A6DFb13437bBea8),
    //NFT
        address(0xdB9bC4Cc36eeCD376716d11632C7dfdc3813996e),
    //Cash
        address(0x18E63Ebf9e915d231488b5563a2f6d721FefE7d2),
    //DefalutInvitor
        address(0x18E63Ebf9e915d231488b5563a2f6d721FefE7d2)
    ){

    }
}