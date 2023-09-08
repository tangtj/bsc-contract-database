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

abstract contract AbsPreSale is Ownable {
    struct SaleInfo {
        uint256 price;
        uint256 saleNum;
        address cashAddress;
    }

    struct UserInfo {
        bool isActive;
        uint256 buyUsdtAmount;
        uint256 inviteUsdt;
    }

    address private _usdtAddress;

    SaleInfo[] private _saleInfo;
    mapping(address => UserInfo) private _userInfo;

    bool private _pauseBuy = false;

    uint256 private _totalUsdt;
    uint256 private _totalInviteUsdt;

    mapping(address => address) public _invitor;
    mapping(address => address[]) public _binder;
    mapping(uint256 => uint256) public _inviteFee;
    mapping(uint256 => uint256) public _inviteFeeCondition;
    uint256 private constant _inviteLen = 3;
    mapping(uint256 => address[]) public _saleAccounts;
    mapping(uint256 => mapping(address => uint256)) public _teamNum;

    constructor(
        address UsdtAddress
    ){
        _usdtAddress = UsdtAddress;
        _inviteFee[0] = 500;
        _inviteFee[1] = 200;
        _inviteFee[2] = 300;

        _inviteFeeCondition[0] = 1;
        _inviteFeeCondition[1] = 3;
        _inviteFeeCondition[2] = 4;

        uint256 usdtUnit = 10 ** IERC20(UsdtAddress).decimals();
        _saleInfo.push(SaleInfo(100 * usdtUnit, 0, address(0xF6F0A6c98E060502f72E708AAC56648a311dd840)));
        _saleInfo.push(SaleInfo(300 * usdtUnit, 0, address(0xb8f9f0e8c0fa57Fc240f752c5C3324e1ed43318b)));
        _saleInfo.push(SaleInfo(1000 * usdtUnit, 0, address(0x1635c627BF7EC3E57238Ac517512586457800EE9)));
        _saleInfo.push(SaleInfo(3000 * usdtUnit, 0, address(0x8C7F5b129c6ad3ab34f475Bf287315D230459a48)));
        _saleInfo.push(SaleInfo(5000 * usdtUnit, 0, address(0x67ca006FE631d3B459d702bD8273D367a3Aa0dFc)));
    }

    function buy(uint256 saleId, address invitor) external {
        require(!_pauseBuy, "pauseBuy");

        address account = msg.sender;
        _bindInvitor(account, invitor);

        SaleInfo storage sale = _saleInfo[saleId];
        sale.saleNum += 1;

        uint256 price = sale.price;

        UserInfo storage userInfo = _userInfo[account];
        require(0 == userInfo.buyUsdtAmount, "bought");
        userInfo.buyUsdtAmount += price;

        _totalUsdt += price;

        address usdtAddress = _usdtAddress;

        uint256 len = _inviteLen;
        address current = account;
        uint256 totalInviteUsdt;
        for (uint256 i; i < len; ++i) {
            invitor = _invitor[current];
            if (address(0) == invitor) {
                break;
            }

            if (_binder[invitor].length >= _inviteFeeCondition[i]) {
                uint256 inviteAmount = price * _inviteFee[i] / 10000;
                totalInviteUsdt += inviteAmount;
                _userInfo[invitor].inviteUsdt += inviteAmount;
                _takeToken(usdtAddress, account, invitor, inviteAmount);
            }

            current = invitor;
        }

        _takeToken(usdtAddress, account, sale.cashAddress, price - totalInviteUsdt);
        _totalInviteUsdt += totalInviteUsdt;
        _saleAccounts[saleId].push(account);
    }

    function _bindInvitor(address account, address invitor) private {
        UserInfo storage user = _userInfo[account];
        if (!user.isActive) {
            if (_userInfo[invitor].isActive) {
                _invitor[account] = invitor;
                _binder[invitor].push(account);
                for (uint256 i; i < _inviteLen;) {
                    if (address(0) == invitor) {
                        break;
                    }
                    _teamNum[i][invitor] += 1;
                    invitor = _invitor[invitor];
                unchecked{
                    ++i;
                }
                }
            }
            user.isActive = true;
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
        uint256[] memory prices, uint256[] memory saleNum, address[] memory cashAddress
    ) {
        uint256 len = _saleInfo.length;
        prices = new uint256[](len);
        saleNum = new uint256[](len);
        cashAddress = new address[](len);
        for (uint256 i; i < len; i++) {
            SaleInfo storage sale = _saleInfo[i];
            prices[i] = sale.price;
            saleNum[i] = sale.saleNum;
            cashAddress[i] = sale.cashAddress;
        }
    }

    function shopInfo() external view returns (
        address usdtAddress, uint256 usdtDecimals, string memory usdtSymbol,
        bool pauseBuy, uint256 totalUsdt, uint256 totalInviteUsdt
    ){
        usdtAddress = _usdtAddress;
        usdtDecimals = IERC20(usdtAddress).decimals();
        usdtSymbol = IERC20(usdtAddress).symbol();
        pauseBuy = _pauseBuy;
        totalUsdt = _totalUsdt;
        totalInviteUsdt = _totalInviteUsdt;
    }

    function getUserInfo(address account) external view returns (
        uint256 usdtBalance, uint256 usdtAllowance,
        uint256 buyUsdtAmount, uint256 inviteUsdt,
        bool isActive, address invitor,
        uint256 binder0Length, uint256 binder1Length, uint256 binder2Length
    ){
        usdtBalance = IERC20(_usdtAddress).balanceOf(account);
        usdtAllowance = IERC20(_usdtAddress).allowance(account, address(this));
        UserInfo storage userInfo = _userInfo[account];
        buyUsdtAmount = userInfo.buyUsdtAmount;
        inviteUsdt = userInfo.inviteUsdt;
        isActive = userInfo.isActive;
        invitor = _invitor[account];
        binder0Length = _teamNum[0][account];
        binder1Length = _teamNum[1][account];
        binder2Length = _teamNum[2][account];
    }

    function getBinderLength(address account) public view returns (uint256){
        return _binder[account].length;
    }

    function getSaleAccountLength(uint256 i) public view returns (uint256){
        return _saleAccounts[i].length;
    }

    function getSaleAccounts(uint256 i) public view returns (address[] memory accounts){
        accounts = _saleAccounts[i];
    }

    receive() external payable {}

    function addSale(uint256 usdtAmount, address cash) external onlyOwner {
        _saleInfo.push(SaleInfo(usdtAmount, 0, cash));
    }

    function setPrice(uint256 saleId, uint256 price) external onlyOwner {
        _saleInfo[saleId].price = price;
    }

    function setTokenAmount(uint256 saleId, address cash) external onlyOwner {
        _saleInfo[saleId].cashAddress = cash;
    }

    function setUsdtAddress(address adr) external onlyOwner {
        _usdtAddress = adr;
    }

    function setPauseBuy(bool pause) external onlyOwner {
        _pauseBuy = pause;
    }

    function setInviteFee(uint256 i, uint256 fee) external onlyOwner {
        _inviteFee[i] = fee;
    }

    function setInviteFeeCondition(uint256 i, uint256 c) external onlyOwner {
        _inviteFeeCondition[i] = c;
    }

    function claimBalance(address to, uint256 amount) external onlyOwner {
        safeTransferETH(to, amount);
    }

    function claimToken(address token, address to, uint256 amount) external onlyOwner {
        _giveToken(token, to, amount);
    }
}

contract TokenSale is AbsPreSale {
    constructor() AbsPreSale(
    //USDT
        address(0x55d398326f99059fF775485246999027B3197955)
    ){

    }
}