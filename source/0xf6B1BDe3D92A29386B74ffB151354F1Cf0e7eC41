// SPDX-License-Identifier: MIT
pragma solidity 0.8.17;


// CAUTION
// This version of SafeMath should only be used with Solidity 0.8 or later,
// because it relies on the compiler's built in overflow checks.
library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }
}


// Interface of the ERC20 standard as defined in the EIP.
interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address from, address to) external view returns (uint256);
    function approve(address to, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}


// safe transfer
library TransferHelper {
    function safeApprove(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('approve(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x095ea7b3, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: APPROVE_FAILED');
    }

    function safeTransfer(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FAILED');
    }

    function safeTransferFrom(address token, address from, address to, uint value) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FROM_FAILED');
    }

    function safeTransferETH(address to, uint value) internal {
        (bool success,) = to.call{value:value}(new bytes(0));
        // (bool success,) = to.call.value(value)(new bytes(0));
        require(success, 'TransferHelper: ETH_TRANSFER_FAILED');
    }
}


// PriceUSDTCalcuator interface
interface IPriceUSDTCalcuator {
    function tokenPriceUSDT(address token) external view returns(uint256);
    function lpPriceUSDT(address lp) external view returns(uint256);
}

// CardNFT interface
interface ICardNFT {
    function card(address account) external view returns(uint256);
    function mintCard(address account, uint256 grade) external;
    function burnCard(address account) external;
    function cardEteState(address account) external view returns(uint256,uint256);
    function changeCardEteState(address account, uint256 limit, uint256 taked) external;
}

// crystal interface
interface ICrystal {
    function crystalPriceUSDT() external view returns (uint256);
    function mintCrystal(address account, uint256 amount) external returns (bool);
    function burnCrystal(address account, uint256 amount) external returns (bool);
}


// owner
abstract contract Ownable {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, 'owner error');
        _;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        if (newOwner != address(0)) {
            owner = newOwner;
        }
    }
}


abstract contract ReentrancyGuard {
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;
    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    modifier nonReentrant() {
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");
        _status = _ENTERED;
        _;
        _status = _NOT_ENTERED;
    }
}


// ete is mos
// new..
contract LockPositionsV2 is Ownable, ReentrancyGuard {
    using SafeMath for uint256;


    address public cardNFT;                                // card contract address.
    address public priceUSDTCalcuator;                     // price usdt calculator contract address.
    address public crystal;
    address public ete;                                    // ete address.
    uint256 private _tenOtherPayTokenNumbers = 10e18;      // 10 other token.
    address public otherPayToken;                          // lock need pay other token.

    mapping(uint256 => lockMsg) public lockMsgOf;  // v1-vn lock get star card.
    struct lockMsg{
        address lockTokenOrLp;    // lock token or lp.
        bool isLp;                // is lp, need get price usdt so is token or lp.
        address gainToken;        // gain token address.(lick xete)
        bool isOpen;              // is open.
    }
    mapping(uint256 => payMsg) public payMsgOf;  // pay and burn get card
    struct payMsg {
        address token;    // token address.
        bool isLp;        // is lp.
        bool isBurn;      // is burn.
        bool isOpen;      // is open.
    }
    mapping(address => bool) public priceIsUsdt; // price is usdt.(like xete, sete, bete, cete, ...)
    uint256 public lockMsgOfCount;   // lock pool count.
    uint256 public payMsgOfCount;    // apy pool count.
   

    // lock price usdt amount, only star card (2).
    uint256 public lockPriceUSDT;
    // usdt buy card and burn ete get card of need usdt amount.
    uint256 public buyPriceUSDT;
    // account -> (xete or bete) -> bool
    // account lock LP gain eteCardToken record. every version is once.
    mapping(address => mapping(address => bool)) public lockGainEteCardTokenRecord; 
    // get crystal multiple ratio mul. default 100%
    uint256 public crystalMul = 100;
    uint256 public burnCardEarnMul = 3;    // card 3 mul rean then burn card.


    constructor(
        address cardNFT_,
        address priceUSDTCalcuator_,
        address crystal_,
        address mos_,
        address otherPayToken_
    ) {
        cardNFT = cardNFT_;
        priceUSDTCalcuator = priceUSDTCalcuator_;
        crystal = crystal_;
        ete = mos_;
        otherPayToken = otherPayToken_;

        // default lock price usdt.
        lockPriceUSDT = 2000*(1e18);
        // default usdt buy and burn ete get card usdt amount.
        buyPriceUSDT = 1000*(1e18);
    }


    event LockingGainCard(uint256 pool, address account, uint256 lpAmount, uint256 crystalAmount, uint256 gainAmount);
    event PayGainCard(address account, address token, uint256 amount);


    function setCardNFT(address newCardNFT) public onlyOwner {
        cardNFT = newCardNFT;
    }
    function setPriceUSDTCalcuator(address newPriceUSDTCalcuator) public onlyOwner {
        priceUSDTCalcuator = newPriceUSDTCalcuator;
    }
    function setCrystal(address newCrystal) public onlyOwner {
        crystal = newCrystal;
    }
    function setEte(address newEte) public onlyOwner {
        ete = newEte;
    }
    function setOtherPayToken(address newOtherPayToken) public onlyOwner {
        otherPayToken = newOtherPayToken;
    }

    function setLockPriceUSDT(uint256 newPriceUSDT) public onlyOwner {
        require(newPriceUSDT > 0, "price error");
        lockPriceUSDT = newPriceUSDT;
    }
    function setBuyPriceUSDT(uint256 newPriceUSDT) public onlyOwner {
        require(newPriceUSDT > 0, "price error");
        buyPriceUSDT = newPriceUSDT;
    }

    function setCrystalMul(uint256 newCrystalMul) public onlyOwner {
        crystalMul = newCrystalMul;
    }
    function setBurnCardEarnMul(uint256 _burnCardEarnMul) public onlyOwner {
        burnCardEarnMul = _burnCardEarnMul;
    }

    // add price eq usdt of token
    function addPriceIsUsdt(address _token, bool _isPriceUsdt) public onlyOwner {
        require(_token != address(0), "zero address error 0");
        priceIsUsdt[_token] = _isPriceUsdt;
    }

    // add lock msg
    function addLockMsg(address _lockTokenOrLp, bool _isLp, address _gainToken, bool _isOpen) public onlyOwner {
        require(_lockTokenOrLp != address(0), "zero address error 0");
        require(_gainToken != address(0), "zero address error 1");
        require(priceIsUsdt[_gainToken], "gain token address error 2");  // is true.(like xete)

        lockMsgOfCount++;
        lockMsgOf[lockMsgOfCount] = lockMsg({
            lockTokenOrLp: _lockTokenOrLp,
            isLp: _isLp,
            gainToken: _gainToken,
            isOpen: _isOpen
        });
    }

    // add pay msg
    function addPayMsg(address _token, bool _isLp, bool _isBurn, bool _isOpen) public onlyOwner {
        require(_token != address(0), "zero address error 0");
        
        payMsgOfCount++;
        payMsgOf[payMsgOfCount] = payMsg({
            token: _token,
            isLp: _isLp,
            isBurn: _isBurn,
            isOpen: _isOpen
        });
    }

    // change lock smg 
    function changeLockMsg(uint256 _index, address _lockTokenOrLp, bool _isLp, address _gainToken, bool _isOpen) public onlyOwner {
        require(_index > 0 && _index <= lockMsgOfCount, "index error");
        require(_lockTokenOrLp != address(0), "zero address error 0");
        require(_gainToken != address(0), "zero address error 1");
        require(priceIsUsdt[_gainToken], "gain token address error 2");  // is true.(like xete)

        lockMsgOf[_index] = lockMsg({
            lockTokenOrLp: _lockTokenOrLp,
            isLp: _isLp,
            gainToken: _gainToken,
            isOpen: _isOpen
        });
    }
    
    // change pay smg 
    function changePayMsg(uint256 _index, address _token, bool _isLp, bool _isBurn, bool _isOpen) public onlyOwner {
        require(_index > 0 && _index <= payMsgOfCount, "index error");
        require(_token != address(0), "zero address error 0");

        payMsgOf[_index] = payMsg({
            token: _token,
            isLp: _isLp,
            isBurn: _isBurn,
            isOpen: _isOpen
        });
    }

    // get all lock
    function getAllLockMsg() external view returns(lockMsg[] memory allLockMsg) {
        uint256 _count = lockMsgOfCount; // save gas.
        allLockMsg = new lockMsg[](_count);
        for(uint256 i = 1; i <= _count; i++) {
            allLockMsg[i-1] = lockMsgOf[i];
        }
    }

    // get pay lock
    function getAllPayMsg() external view returns(payMsg[] memory allPayMsg) {
        uint256 _count = payMsgOfCount; // save gas.
        allPayMsg = new payMsg[](_count);
        for(uint256 i = 1; i <= _count; i++) {
            allPayMsg[i-1] = payMsgOf[i];
        }
    }

    // token -> usdt
    function calculateTokenAmountEqUsdtAmount(address _tokenOrLp, uint256 _tokenAmount, bool _isLp) public view returns(uint256) {
        if(priceIsUsdt[_tokenOrLp]) {
            // if price eq usdt.
            return _tokenAmount;
        }else if(_isLp){
            // is lp
            uint256 lpPrice = IPriceUSDTCalcuator(priceUSDTCalcuator).lpPriceUSDT(_tokenOrLp);
            uint256 usdtAmount = _tokenAmount.mul(lpPrice).div(1e18);
            return usdtAmount;
        }else {
            // is token
            uint256 tokenPrice = IPriceUSDTCalcuator(priceUSDTCalcuator).tokenPriceUSDT(_tokenOrLp);
            uint256 usdtAmount = _tokenAmount.mul(tokenPrice).div(1e18);
            return usdtAmount;
        }
    }

    // usdt -> token
    function calculateUsdtAmountEqTokenAmount(address _tokenOrLp, uint256 _usdtAmount, bool _isLp) public view returns(uint256) {
        if(priceIsUsdt[_tokenOrLp]) {
            // if price eq usdt.
            return _usdtAmount;
        }else if(_isLp){
            // is lp
            uint256 lpPrice = IPriceUSDTCalcuator(priceUSDTCalcuator).lpPriceUSDT(_tokenOrLp); // 5e18
            uint256 lpAmount = _usdtAmount.mul(1e18).div(lpPrice);
            return lpAmount;
        }else {
            // is token
            uint256 tokenPrice = IPriceUSDTCalcuator(priceUSDTCalcuator).tokenPriceUSDT(_tokenOrLp);
            uint256 tokenAmount = _usdtAmount.mul(1e18).div(tokenPrice);
            return tokenAmount;
        }
    }

    // lock before get
    function lockLpGainCardBefore(address _account, uint256 _index, bool _isGainCrystal) public view returns(uint256 lpAmount, uint256 crystalAmount, uint256 gainAmount) {
        lockMsg memory _lockMsg = lockMsgOf[_index];
        lpAmount = calculateUsdtAmountEqTokenAmount(_lockMsg.lockTokenOrLp, lockPriceUSDT, _lockMsg.isLp);
        if(_isGainCrystal) {
            uint256 crystalPrice = ICrystal(crystal).crystalPriceUSDT();
            crystalAmount = lockPriceUSDT.mul(1e18).div(crystalPrice).mul(crystalMul).div(100);
        }
        gainAmount = lockGainEteCardTokenRecord[_account][_lockMsg.gainToken] ? 0 : lockPriceUSDT;
    }

    // lock lp get card and eteCardToken and crystal.
    // @card: only 2=star card.
    function lockLpGainCard(uint256 _index, bool _isGainCrystal) public nonReentrant {
        address account = msg.sender;
        lockMsg memory _lockMsg = lockMsgOf[_index];
        require(_lockMsg.isOpen, "not open");
        require(!isContract(account), "not user1");
        require(tx.origin == account, 'not user2');

        // pay lp and other token
        (uint256 lpAmount, uint256 crystalAmount, uint256 gainAmount) = lockLpGainCardBefore(account, _index, _isGainCrystal);
        require(lpAmount > 0, "lp amount is zero");
        TransferHelper.safeTransferFrom(_lockMsg.lockTokenOrLp, account, address(this), lpAmount);
        // is gain crystal
        if(_isGainCrystal) {
            TransferHelper.safeTransferFrom(otherPayToken, account, address(this), _tenOtherPayTokenNumbers);
            ICrystal(crystal).mintCrystal(account, crystalAmount);
        }
        
        ICardNFT(cardNFT).mintCard(account, 2);
        if(gainAmount > 0) {
            lockGainEteCardTokenRecord[account][_lockMsg.gainToken] = true;
            TransferHelper.safeTransfer(_lockMsg.gainToken, account, gainAmount); // xete
        }

        // add card ete limit.
        _addCardEteLimit(account, lockPriceUSDT);
        // emit
        emit LockingGainCard(_index, account, lpAmount, crystalAmount, gainAmount);
    }

    // pay usdt buy card.
    function payGainCard(uint256 _index) public nonReentrant {
        address account = msg.sender;
        payMsg memory _payMsg = payMsgOf[_index];
        require(_payMsg.isOpen, "not open");
        require(!isContract(account), "not user1");
        require(tx.origin == account, 'not user2');

        // pay amount
        uint256 payAmount = calculateUsdtAmountEqTokenAmount(_payMsg.token, buyPriceUSDT, _payMsg.isLp);
        address toAddress = _payMsg.isBurn ? address(0) : address(this);
        require(payAmount > 0, "pay amount is zero");
        TransferHelper.safeTransferFrom(_payMsg.token, account, toAddress, payAmount);
        ICardNFT(cardNFT).mintCard(account, 2);

         // add card ete limit.
        _addCardEteLimit(account, buyPriceUSDT);
        // emit
        emit PayGainCard(account, _payMsg.token, payAmount);  
    }

    // add card ete limit.
    function _addCardEteLimit(address _account, uint256 _usdtAmount) private {
        uint256 eteAmount = calculateUsdtAmountEqTokenAmount(ete, _usdtAmount, false);
        uint256 eteLimit = eteAmount.mul(burnCardEarnMul);

        ICardNFT(cardNFT).changeCardEteState(_account, eteLimit, 0);
    }

    // take token
    function takeToken(address token, address to, uint256 value) external onlyOwner {
        require(to != address(0), "zero address error");
        require(value > 0, "value zero error");
        TransferHelper.safeTransfer(token, to, value);
    }

    function isContract(address account) internal view returns (bool) {
        return account.code.length > 0;
    }

}