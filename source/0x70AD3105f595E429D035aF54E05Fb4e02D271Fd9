// SPDX-License-Identifier: Unlicensed

pragma solidity 0.8.18;

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

contract Ownable is Context {
    address public _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        _owner = _msgSender();
        emit OwnershipTransferred(address(0), _msgSender());
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require (_msgSender() == _owner);
        _;
    }

    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

interface IPancakePair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);
    function PERMIT_TYPEHASH() external pure returns (bytes32);
    function nonces(address owner) external view returns (uint);

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;

    event Mint(address indexed sender, uint amount0, uint amount1);
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint);
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function kLast() external view returns (uint);

    function mint(address to) external returns (uint liquidity);
    function burn(address to) external returns (uint amount0, uint amount1);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function skim(address to) external;
    function sync() external;

    function initialize(address, address) external;
}

interface IW3swapRouter01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

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
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

interface IW3swapRouter02 is IW3swapRouter01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountETH);
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}



library calSome{
    using SafeMath for uint256;
    function getFee(uint amount, uint per) internal pure returns(uint){
        return amount.mul(per).div(10000);
    }

    function rand(uint256 _length) internal view returns(uint256) {
        uint256 random = uint256(keccak256(abi.encodePacked(block.difficulty, block.timestamp)));
        return random%_length;
    }

    function getBackPer(uint minNum, uint maxNum, uint nowNum, uint _addOnce, uint changeDay) 
    internal pure returns (uint){
        uint getDay = nowNum.add(changeDay.mul(_addOnce));
        uint backPer = 0;
        if (changeDay == 0){
            return 0;
        } else {
            if (getDay > maxNum){
                uint days1 = maxNum.sub(nowNum).div(_addOnce).add(1);
                uint addScore1 = days1.mul(days1.sub(1).mul(_addOnce)).div(2);
                backPer = days1.mul(nowNum).add(addScore1);
                uint days2 = changeDay.sub(days1);
                if (days2 > 0){
                    uint addScore2 = days2.mul(days2.sub(1).mul(_addOnce)).div(2);
                    backPer = backPer.add(days2.mul(minNum).add(addScore2));
                }
            } else {
                uint addScore = changeDay.mul(changeDay.sub(1).mul(_addOnce)).div(2);
                backPer = changeDay.mul(nowNum).add(addScore);
            }
            if (backPer >= 10000){
                backPer = 10000;
            }
            return backPer;
        }
    }

    function getTruePer(uint minNum, uint maxNum, uint nowNum, uint _addOnce, uint changeDay) 
    internal pure returns (uint){
        uint getDay = nowNum.add(changeDay.mul(_addOnce));
        if (getDay > maxNum){
            uint days1 = maxNum.sub(nowNum).add(1).div(_addOnce);
            uint needDay = maxNum.sub(minNum).add(1).div(_addOnce);
            uint days2 = changeDay.sub(days1).mod(needDay);
            getDay = minNum.add(days2.mul(_addOnce));
        }
        return getDay;
    }
}

interface levelDo {
    function getUserTrueLevel(address userAddress) external view returns(uint);
    function getUserLevel(address searchAddress) external view returns (uint);
    function getLevelBack(uint level) external view returns(uint);
    function getLevelPer(uint level) external view returns(uint);
    function getLevelNum(uint level) external view returns(uint);
    function getHaveBuy(address userAddress) external view returns(uint);
    function updateTeamScore(address _yourAddress, uint _amount) external;
    function getParent(address userAddress) external view returns(address);
}

contract zhou is Context, Ownable {
    using SafeMath for uint256;
    using SafeMath for uint8;

    uint coinMin = 100;
    uint coinMax = 200;
    uint coinAddOnce = 1;

    mapping (address => uint) userStore;

    mapping (address => StoreCell) storeRelase;
    mapping (address => StoreCell) coinRealse;


    mapping (address => uint) userCanGet;

    mapping (address => uint) userLevelCanGet; 

    mapping (address => bool) hasSafe;

    mapping (address => uint) breakTime;

    mapping (address => mapping(uint => uint[])) userBuyList1;
    mapping (address => mapping(uint => uint[])) userBuyList2;
    mapping (address => mapping(uint => uint)) userBuyNum;

    mapping (uint => StoreLog) storeInfo;

    mapping (address => uint) myScore;

    mapping (address => uint) teamScore;

    mapping(uint => uint) levelAmount;

    uint relaseTime = 0;
 
    mapping (address => mapping(uint => uint)) userHasGet;

    uint[2] randomNum = [382, 1000];

    address pairAddress = 0xcE53bEdACbD5c221EcB16862E98Fe56c9fa55591;

    address contract1 = 0x5568E93c30717315A6F02634D52F7fE313bB270E;

    address leaderAddress = 0x3b911fBd76dA4b01D2ff75B0610d45Ff14182697;

    address public usdtContract = address(0x55d398326f99059fF775485246999027B3197955);

    IW3swapRouter02 public router1 = IW3swapRouter02(0x10ED43C718714eb63d5aA57B78B54704E256024E);

    address public LevelAddress = address(0x10549D7CB4ff4CC641bc357268bF8890975970b4);

    levelDo public levelContract = levelDo(LevelAddress);

    uint _buyId = 0;

    uint256 public maxLength = 50;

    uint dayOnce = 82;

    uint[2] chooseInfo3 = [50, 80];

    mapping (address => uint) userAcitive;

    uint staticuserAcitive = 1000e18;

    mapping (address => uint) useruseActive;

    mapping (address => uint) usercangActive;

    mapping (address => uint) usersafeActive;

    mapping (address => uint) hasBack;

    mapping (address => uint) bindNum;

    uint[] maxCoin = [500e18, 800e18, 1300e18, 2100e18, 3400e18, 5500e18, 8900e18, 14400e18, 23300e18];

    mapping (uint => mapping(uint => uint[])) lastLog;

    uint safeBack = 100;

    uint addOnce = 1;

    struct StoreLog{
        uint _id;
        address myAddress;
        uint amount;
        uint needDay;
        uint belongType;
        uint backPer;
        uint storeAmount;
        uint useAmount;
        uint status;
        uint safeAmount;
        uint storeTime;
        uint belongTimes;
    }

    struct StoreCell{
        uint amount;
        uint lastLog;
        uint lastPercent;
        uint hasRealse;
        uint canRealse;
    }

    uint minCoin = 10e18;

    struct TypeInfo{
        uint amount;
        uint256 nowDay;
        uint256 nowBuy;
        uint256 minCoin;
        uint256 maxCoin;
        uint256 startDay;
        uint256 willBack;
        uint startTime;
        uint dayCanBuy;
        uint8[2] chooseDay;
        uint nowTimes;
        uint nowGroup;
        uint status;
    }

    event buyOrderEvent(
        address indexed userAddress,
        uint getDays,
        uint percent
    );

    uint[4] startMin = [13e22, 5e22, 3e22, 0];

    uint[6] shareAll = [6180, 3000, 300, 240, 150, 130];

    uint[3] safeNeed = [50, 130, 340];

    uint timeIncr = 61200;
    // uint timeIncr = 0;
    uint systemDays = 1 days;
    uint needAddTime = 1 hours;

    uint mustPer = 3820;
    uint otherPer = 6180;

    modifier onlyLevel() {
        require (_msgSender() == LevelAddress);
        _;
    }

    mapping (uint => TypeInfo) allRealse;

    function getLevelCan(address userAddress, uint level) public view returns(uint, uint){
        return (levelAmount[level], userHasGet[userAddress][level]);
    }

    function getStoreInfo (address searchAddress) public view returns (uint, uint, uint, uint, uint, uint){
        return (storeRelase[searchAddress].amount, storeRelase[searchAddress].lastLog, storeRelase[searchAddress].lastPercent, storeRelase[searchAddress].hasRealse, userStore[searchAddress], storeRelase[searchAddress].canRealse);
    }

    function getCoinInfo (address searchAddress) public view returns (uint, uint, uint, uint, bool[8] memory, uint){
        return (coinRealse[searchAddress].amount, coinRealse[searchAddress].lastLog, coinRealse[searchAddress].lastPercent, coinRealse[searchAddress].hasRealse, myHasRelase[searchAddress], coinRealse[searchAddress].canRealse);
    }

    function getUserActive(address userAddress) public view returns(uint, uint, uint, uint, uint, uint){
        return (userAcitive[userAddress], usercangActive[userAddress], staticuserAcitive, useruseActive[userAddress], usersafeActive[userAddress], userCanGet[userAddress]);
    }

    constructor ()  {
        relaseTime = block.timestamp;
        uint nowDay = relaseTime.div(systemDays);
        allRealse[0] = TypeInfo(0, nowDay, 0, minCoin, maxCoin[0], 3,  0, relaseTime, 0, [3, 5], 1,  0, 0);
        allRealse[1] = TypeInfo(0, nowDay, 0, minCoin, maxCoin[0], 8,  0, relaseTime, 0, [8, 13], 1, 0, 0);
        allRealse[2] = TypeInfo(0, nowDay, 0, minCoin, maxCoin[0], 21, 0, relaseTime, 0, [21, 34], 1,0,0);
        allRealse[3] = TypeInfo(0, nowDay, 0, minCoin, maxCoin[0], 1,  0, relaseTime, 0, [2, 1], 1, 0, 0);
    }

    function getInfo(uint8 searchType) public view returns(uint, uint, uint, uint, uint, uint, uint, uint){
        TypeInfo memory yourType = allRealse[searchType];
        uint thisAmount = 0;
        if (searchCan && msg.sender == owner()){
            thisAmount = yourType.amount;
        }
        return (yourType.nowBuy, yourType.minCoin, yourType.maxCoin, yourType.dayCanBuy, yourType.status, yourType.nowTimes, yourType.nowGroup, thisAmount);
    }

    function buyOrder(uint chooseType, bool useStore, uint amount, bool buySafe) public{
        address userAddress = msg.sender;
        uint userneedStore = 0;
        uint needAmount = amount;
        bool canBuyOrder = canBuy(chooseType, amount, userAddress, buySafe);
        require(canBuyOrder, "c1");
        uint[2] memory nowBack= getNowBackPer(false, 0, chooseType);

        if (chooseType == 3){
            require(!hasSafe[userAddress], "c2");
            userneedStore = amount.div(10);
            require(userStore[userAddress] >= userneedStore, "c3");
            userStore[userAddress] = userStore[userAddress].sub(userneedStore);
            insertLog(1, userAddress, 10, userneedStore, true, userAddress);

            calStore(chooseType, amount, userAddress, buySafe, userneedStore, needAmount, nowBack);
        } else {
            if (useStore){
                needAmount = calSome.getFee(amount, mustPer);
                userneedStore = amount.sub(needAmount);
                require(userStore[userAddress] >= userneedStore, "c3");
                userStore[userAddress] = userStore[userAddress].sub(userneedStore);
                insertLog(1, userAddress, 11, userneedStore, true, userAddress);
            }

            uint pairAmount = calSome.getFee(amount, shareAll[5]);
            transferToThis(userAddress, needAmount.sub(pairAmount));

            allRealse[chooseType].nowBuy = allRealse[chooseType].nowBuy.add(amount);
            calStore(chooseType, amount, userAddress, buySafe, userneedStore, needAmount, nowBack);

            takeShare(chooseType, msg.sender, amount);
            takeAll(chooseType, msg.sender, amount);
               
            sendToPair(pairAmount);
            sendToType(3, calSome.getFee(amount, shareAll[4]));
            sendToType(chooseType, calSome.getFee(amount, shareAll[1]));
            
            levelContract.updateTeamScore(msg.sender, amount);
        }
    }

    function calStore(uint chooseType, uint amount, address userAddress, bool buySafe, uint userneedStore, uint needAmount, uint[2] memory nowBack) private {
        uint thisBuyId = ++_buyId;
        uint nowTime = block.timestamp;
        uint times = allRealse[chooseType].nowTimes;
        uint giveAmount = 0;
        
        if (chooseType == 3){
            allRealse[chooseType].nowBuy = allRealse[chooseType].nowBuy.add(amount);
            uint thisGive = calSome.getFee(amount, nowBack[0].mul(nowBack[1]));
            allRealse[chooseType].willBack = allRealse[chooseType].willBack.add(thisGive);
        
            hasSafe[userAddress] = true;
        } else {
            if (buySafe){
                uint needSafeAmount = safeNeed[chooseType];
                giveAmount = calSome.getFee(amount, needSafeAmount);
                transferToThis(userAddress, giveAmount);
                sendToType(3, giveAmount);
            }
        }

        StoreLog memory yourStore = StoreLog(thisBuyId, userAddress, amount, nowBack[0], chooseType, nowBack[1], userneedStore, needAmount, 0, giveAmount, nowTime, times);
        storeInfo[thisBuyId] = yourStore;
        userBuyList1[userAddress][chooseType].push(thisBuyId);

        userBuyNum[userAddress][chooseType] = userBuyNum[userAddress][chooseType].add(1);

        emit buyOrderEvent(userAddress, nowBack[0], nowBack[1]);
    }

    function transferToThis(address userAddress, uint amount) private{
        require(IERC20(usdtContract).transferFrom(userAddress, address(this), amount) == true, "c4");
    }
    
    uint[] needIncr = [0, 51, 51];
    uint[] indexNum = [51, 102, 123];
    uint[] indexPer = [50, 80, 80];

    function getFirst(uint nowDay, uint _addOnce) private view returns (uint){
        uint leftDay = nowDay.mod(dayOnce);
        uint backPer = 50;
        for (uint i=0 ; i< indexNum.length; i++){
            if (leftDay < indexNum[i]){
                uint needAdd = leftDay.sub(needIncr[i]);
                backPer = indexPer[i].add(_addOnce.mul(needAdd));
                break;
            } else {
                leftDay = leftDay.sub(needIncr[i]);
            }
        }
        return backPer;
    }
    
    function takeShare(uint chooseType, address shareAddress, uint amount) private{
        address parentAddress = levelContract.getParent(shareAddress);
        uint leftAmount = 240;
        uint cengBack = 20;
        uint nowI = 1;
        uint reBack = 0;
        for (uint i=0; i < maxLength; i++){
            if (leftAmount == 0 || parentAddress == address(0)){
                break;
            }
            uint level = levelContract.getUserTrueLevel(parentAddress);
            if (level >= 6 || (level >= 5 && nowI<= 7) || (level >= 4 && nowI<= 6) || (level >= 3 && nowI<= 5) || (level >= 2 && nowI<= 4) || (level >= 1 && nowI<= 3)){
                uint getAmount = calSome.getFee(amount, cengBack);

                uint addback = giveActive(getAmount, parentAddress, 201, shareAddress);
                reBack = reBack.add(addback);
                nowI = nowI.add(1);
                leftAmount = leftAmount.sub(cengBack);
            }
            parentAddress = levelContract.getParent(parentAddress);
        }
        if (leftAmount > 0 || reBack > 0){
            sendToType(chooseType, calSome.getFee(amount, leftAmount).add(reBack));
        }
    }

    function takeAll(uint chooseType, address shareAddress, uint amount) private{
        uint lastBack = 0;
        uint reBack = 0;
        uint lingAll = 300;
        address parentAddress = levelContract.getParent(shareAddress);
        
        for (uint i=0; i < maxLength; i++){
            if (lastBack >= lingAll || parentAddress == address(0)){
                break;
            }
            uint level = levelContract.getUserLevel(parentAddress);
            if (level > 0){
                uint backYour = levelContract.getLevelBack(level);
                if (backYour > lastBack){
                    uint getAmount = calSome.getFee(amount, backYour.sub(lastBack));
                    lastBack = backYour;
                    uint addback = giveActive(getAmount, parentAddress, 202, shareAddress);
                    reBack = reBack.add(addback);
                }
            }
            parentAddress = levelContract.getParent(parentAddress);
        }
        if (lastBack < lingAll || reBack > 0){
            sendToType(chooseType, calSome.getFee(amount, lingAll.sub(lastBack)).add(reBack));
        }
    }

    function giveActive(uint getAmount, address userAddress, uint backType, address shareAddress) private returns (uint reback){
        uint userActivitNumber = userAcitive[userAddress].add(staticuserAcitive);
        uint useruseActiveNumber = useruseActive[userAddress];
        require (useruseActiveNumber < userActivitNumber, "c");
        if (useruseActiveNumber.add(getAmount) >= userActivitNumber){
            uint reBackAmount = userActivitNumber.sub(useruseActiveNumber);
            reback = getAmount.sub(reBackAmount);
            getAmount = reBackAmount;
        }

        insertLog(2, userAddress, backType, getAmount, true, shareAddress);
        userCanGet[userAddress] = userCanGet[userAddress].add(getAmount);
        useruseActive[userAddress] = useruseActive[userAddress].add(getAmount);
    }

    function getTrueDay(uint nowTime,uint endTime,uint needDay) private view returns(uint){
        (,uint afterDay) = getSubTime(nowTime, endTime);
        return needDay > afterDay? needDay.sub(afterDay): 0;
    }

    function rebackOrder(uint orderId, bool chooseCoin) public {
        StoreLog memory store = storeInfo[orderId];
        address userAddress = msg.sender;
        require(userAddress == store.myAddress, "c5");
        uint needDay = store.storeTime.add(store.needDay.mul(systemDays));
        uint nowTime = block.timestamp;
        uint belongType = store.belongType;
        uint nowTimes = allRealse[belongType].nowTimes;

        require(nowTime >= needDay && store.status == 0, "c6");
        uint trueDay = getTrueDay(nowTime, needDay, store.needDay);
        storeInfo[orderId].status = 1;

        if (belongType == 3){
            uint backAmount = calSome.getFee(store.amount, store.backPer.mul(store.needDay));
            allRealse[belongType].willBack = allRealse[belongType].willBack.sub(backAmount);
            hasBack[userAddress] = hasBack[userAddress].add(backAmount);
            hasSafe[userAddress] = false;
            if (chooseCoin){
                addFunction(true, 1, userAddress, backAmount);
                addFunction(false, 1, userAddress, backAmount.mul(5));
                allRealse[belongType].amount = allRealse[belongType].amount.sub(calSome.getFee(backAmount, mustPer));
                cSendToPair(backAmount);
            } else {
                allRealse[belongType].amount = allRealse[belongType].amount.sub(backAmount);
                transferFromContract(101, backAmount, userAddress);
            }
        } else {
            if (nowTimes > store.belongTimes) {
                if (belongType != 3){
                    uint storeAmount = store.storeAmount;
                    breakTime[userAddress] = breakTime[userAddress].add(1);
                    if (storeAmount == 0){
                        uint trueAmount = calSome.getFee(store.amount, otherPer);
                        transferFromContract(102, trueAmount, userAddress);
                        if (store.safeAmount > 0){
                            usercangActive[userAddress] = usercangActive[userAddress].add(store.amount);
                            usersafeActive[userAddress] = usersafeActive[userAddress].add(store.safeAmount);
                        }
                    }
                    addFunction(false, 4, userAddress, store.amount.mul(5));
                }
            } else {
                bool hasBreak = false;
                if (belongType != 3){
                    uint useAmount = store.useAmount;
                    if (store.storeAmount > 0){
                        if (allRealse[belongType].amount <= useAmount){
                            hasBreak = true;
                            checkBack(useAmount, allRealse[belongType].amount, userAddress, store.safeAmount);
                            addFunction(false, 4, userAddress, store.amount.mul(5));
                            breakType(belongType);
                        } else {
                            allRealse[belongType].amount = allRealse[belongType].amount.sub(useAmount);
                            transferFromContract(102, useAmount, userAddress);
                        }
                    } else {
                        uint trueAmount = calSome.getFee(store.amount, mustPer);
                        if (allRealse[belongType].amount <= trueAmount){
                            hasBreak = true;
                            checkBack(trueAmount, allRealse[belongType].amount, userAddress, store.safeAmount);
                            transferFromContract(102, calSome.getFee(store.amount, otherPer), userAddress);

                            addFunction(false, 4, userAddress, store.amount.mul(5));
                            breakType(belongType);
                        } else {
                            allRealse[belongType].amount = allRealse[belongType].amount.sub(trueAmount);
                            transferFromContract(102, useAmount, userAddress);
                        }
                    }
                }
                uint backAmount = calSome.getFee(store.amount, store.backPer.mul(trueDay));
                if (!hasBreak){
                    if (chooseCoin){
                        addFunction(true, 1, userAddress, backAmount);
                        addFunction(false, 1, userAddress, backAmount.mul(5));
                        allRealse[belongType].amount = allRealse[belongType].amount.sub(calSome.getFee(backAmount, mustPer));
                        cSendToPair(backAmount);
                    } else {
                        if (allRealse[belongType].amount <= backAmount){
                            transferFromContract(103, allRealse[belongType].amount, userAddress);
                            breakType(belongType);
                        } else {
                            allRealse[belongType].amount = allRealse[belongType].amount.sub(backAmount);
                            transferFromContract(103, backAmount, userAddress);
                        }
                    }
                }
            }
        }
        userBuyList2[userAddress][belongType].push(orderId);
        deleteOne(userAddress, orderId, belongType);
    }

    function deleteOne(address searchAddress, uint orderId, uint chooseType) private {
        uint length = userBuyList1[searchAddress][chooseType].length;
        for(uint i = 0; i < length; i++){
            if (userBuyList1[searchAddress][chooseType][i] == orderId){
                delete userBuyList1[searchAddress][chooseType][i];
                userBuyNum[searchAddress][chooseType] = userBuyNum[searchAddress][chooseType].sub(1);
                break;
            }
        }
    }

    function getSubTime(uint nowTime, uint logTime) private view returns(uint, uint){
        uint nowDay = nowTime.div(systemDays);
        uint logDay = logTime.div(systemDays);
        if (nowDay <= logDay){
            return (nowDay, 0);
        } else {
            return (nowDay, nowDay.sub(logDay));
        }
    }

    function getMyRealse(bool coin) public view returns (uint, uint, uint){
        StoreCell memory myStore = coinRealse[msg.sender];
        if (!coin){
            myStore = storeRelase[msg.sender];
        }
        uint amount = myStore.amount;

        uint nowTime = block.timestamp;
        uint logTime = myStore.lastLog;
        (,uint changeDay2) = getSubTime(nowTime, logTime);
        uint lastPercent = myStore.lastPercent;
        uint backPer = calSome.getBackPer(coinMin, coinMax, lastPercent, coinAddOnce, changeDay2);
        uint addBack = calSome.getFee(amount, backPer);

        uint backAmount = addBack.add(myStore.canRealse);
        if (backAmount > 0){
            if (backAmount >= amount){
                backAmount = amount;
            }
        }
        uint backCoin = 0;
        if (coin){
            uint giveRelease = getTokenPrice(1e18)[1];
            backCoin = backAmount.mul(1e18).div(giveRelease);
        }
        return (backAmount, addBack, backCoin);
    }

    function getCoin(bool coin) public {
        uint nowTime = block.timestamp;
        (uint backAmount, uint addBack, uint backCoin) = getMyRealse(coin);
        uint nowBack = getCoinBack();

        if (backAmount > 0){
            if (!coin){
                storeRelase[msg.sender].canRealse = 0;
                storeRelase[msg.sender].amount = storeRelase[msg.sender].amount.sub(addBack);
                storeRelase[msg.sender].lastPercent = nowBack;
                storeRelase[msg.sender].lastLog = nowTime;
                storeRelase[msg.sender].hasRealse = storeRelase[msg.sender].hasRealse.add(backAmount);
                userStore[msg.sender] = userStore[msg.sender].add(backAmount);
            } else {
                coinRealse[msg.sender].canRealse = 0;
                coinRealse[msg.sender].amount = coinRealse[msg.sender].amount.sub(addBack);
                coinRealse[msg.sender].lastPercent = nowBack;
                coinRealse[msg.sender].lastLog = nowTime;
                coinRealse[msg.sender].hasRealse = coinRealse[msg.sender].hasRealse.add(backAmount);
                tranferCoin(backCoin, msg.sender);
            }
            insertLog(coin?0:1, msg.sender, 7, backAmount, true, msg.sender);
        }
    }

    function checkBack(uint amount, uint leftAmount, address yourAddress, uint safeAmount) private{
        if (amount >= leftAmount){
            uint breakAmount = amount.sub(leftAmount);
            breakTime[yourAddress] = breakTime[yourAddress].add(1);
            transferFromContract(102, leftAmount, yourAddress);
            if (safeAmount > 0){
                usercangActive[yourAddress] = usercangActive[yourAddress].add(breakAmount.mul(10000).div(mustPer));
                usersafeActive[msg.sender] = usersafeActive[msg.sender].add(safeAmount);
            }
        }
    }

    function breakType(uint belongType) private {
        allRealse[belongType].status = 1;
        allRealse[belongType].amount = 0;
    }

    function buyAcitve(uint amount) public {
        uint leftAmount = 100;
        for (uint i = 6; i <= 10; i++){
            uint thisNum = levelContract.getLevelNum(i);
            if (thisNum > 0){
                leftAmount = leftAmount.sub(10);
                levelAmount[i] = levelAmount[i].add(amount.div(10).div(thisNum));
            }
        }
        require(IERC20(usdtContract).transferFrom(msg.sender, address(leaderAddress), amount.mul(leftAmount).div(100)) == true, "c4");

        uint yourLevel = levelContract.getUserLevel(msg.sender);
        
        uint getS = amount.mul(levelContract.getLevelPer(yourLevel));
        userAcitive[msg.sender] = userAcitive[msg.sender].add(getS);
        addFunction(false, 6, msg.sender, amount.mul(5));
    }

    function resetType(uint chooseType) private {
        allRealse[chooseType].startTime = block.timestamp;
        allRealse[chooseType].nowTimes = allRealse[chooseType].nowTimes.add(1);
        allRealse[chooseType].status = 0;
        allRealse[chooseType].maxCoin = maxCoin[0];
    }

    function canBuy(uint chooseType, uint amount, address userAddress, bool buySafe) private returns (bool){
        TypeInfo memory chooseInfo = allRealse[chooseType];
        (uint nowDay,) = getDayCal(chooseInfo.startTime, block.timestamp);
        uint thisDay = chooseInfo.nowDay;
        uint dayBuy = chooseInfo.nowBuy;
        uint dayCanBuy = chooseInfo.dayCanBuy;

        require(block.timestamp >= 1688806800 && amount >= chooseInfo.minCoin && amount <= chooseInfo.maxCoin, "c7");
        //require(amount >= chooseInfo.minCoin && amount <= chooseInfo.maxCoin, "c7");

        if (chooseType == 3){
            require(amount <= usercangActive[userAddress].sub(hasBack[userAddress]), "c8");
            uint feeAmount = calSome.getFee(usercangActive[userAddress], mustPer);
            uint canback = feeAmount.add(usersafeActive[userAddress]);

            if (nowDay > thisDay || chooseInfo.dayCanBuy == 0){
                dayBuy = 0;
                allRealse[chooseType].nowDay = nowDay;
                allRealse[chooseType].nowBuy = 0;
                dayCanBuy = calSome.getFee(chooseInfo.amount, mustPer);
            }
            require (canback > hasBack[userAddress], "c9");
            uint maxGet = getMax(amount, chooseInfo.chooseDay[0]);
            require (chooseInfo.willBack.add(maxGet) <= dayCanBuy, "c1");
            return true;
        } else {
            require (allRealse[chooseType].status != 1 || nowDay > thisDay, "c10");

            if (nowDay > thisDay || chooseInfo.dayCanBuy == 0){
                uint lastNum = 0;
                bool newS = false;
                if (chooseInfo.status == 1){
                    resetType(chooseType);
                    newS = true;
                    chooseInfo.nowGroup = 0;
                    chooseInfo.nowTimes = chooseInfo.nowTimes.add(1);
                }
                
                allRealse[chooseType].nowGroup = chooseInfo.nowGroup.add(1);
                dayCanBuy = getDaysBack(chooseInfo.nowGroup, chooseType);

                lastLog[chooseType][chooseInfo.nowTimes].push(dayCanBuy);

                if (chooseInfo.dayCanBuy > chooseInfo.nowBuy && !newS){
                    lastNum = chooseInfo.dayCanBuy.sub(chooseInfo.nowBuy);
                }
                allRealse[chooseType].dayCanBuy = dayCanBuy.add(lastNum);
                dayBuy = 0;
                allRealse[chooseType].nowDay = nowDay;
                allRealse[chooseType].nowBuy = 0;
            }

            uint month = chooseInfo.nowGroup.div(30);
            if (month >= maxCoin.length) {
                allRealse[chooseType].maxCoin = maxCoin[maxCoin.length.sub(1)];
            } else {
                allRealse[chooseType].maxCoin = maxCoin[month];
            }

            require (buySafe || nowDay.mul(systemDays).add(timeIncr).add(needAddTime) < block.timestamp, "c11");

            return dayBuy.add(amount) <= dayCanBuy;
        }
    }

    function getMax(uint amount, uint maxBack) private view returns (uint maxGet){
        maxGet = calSome.getFee(amount, safeBack.mul(maxBack));
    }

    function getNowBackPer(bool giveDay, uint chooseDay, uint chooseType) private view returns (uint[2] memory ){
        TypeInfo memory chooseInfo = allRealse[chooseType];
        uint backDay = chooseInfo.chooseDay[0];
        uint randomM = calSome.rand(randomNum[1]);
        if (randomM > randomNum[0]){
            backDay = chooseInfo.chooseDay[1];
        }
        uint backPer = 50;
        if (chooseType == 3){
            backPer = safeBack;
        } else {
            uint calDays = 0;
            if (giveDay){
                calDays = chooseDay;
            } else {
                calDays = chooseInfo.nowGroup.sub(1);
            }
            if (chooseType == 1 || chooseType == 0){
                backPer = getFirst(calDays, addOnce);
            } else {
                backPer = calSome.getTruePer(chooseInfo3[0], chooseInfo3[1], chooseInfo3[0], addOnce, calDays);
            }
        }
        return [backDay, backPer];
    }


    function getCoinBack() private view returns(uint){
        uint nowTime = block.timestamp;
        (,uint changeDay) = getSubTime(nowTime, relaseTime);
        uint backPer = calSome.getTruePer(coinMin, coinMax, coinMin, coinAddOnce, changeDay);
        return backPer;
    }

    function addFunction(bool coin, uint addType, address addAddress, uint addAmount) private {
        StoreCell memory myStore = coinRealse[addAddress];
        if (!coin){
            myStore = storeRelase[addAddress];
        }

        uint amount = myStore.amount;
        uint nowTime = block.timestamp;
        uint nowPer = getCoinBack();
        if (amount > 0){
            uint logTime = myStore.lastLog;
            (,uint changeDay) = getSubTime(nowTime, logTime);
            uint lastPercent = myStore.lastPercent;

            uint nowBack = nowPer;
            uint backPer = calSome.getBackPer(coinMin, coinMax, lastPercent, coinAddOnce, changeDay);
            uint addBack = calSome.getFee(amount, backPer);

            if (addBack.add(myStore.canRealse) >= amount){
                myStore.canRealse = amount;
            } else {
                myStore.canRealse = myStore.canRealse.add(addBack);
            }
            myStore.amount = myStore.amount.sub(addBack).add(addAmount);
            myStore.lastPercent = nowBack;
        } else {
            myStore.amount = addAmount;
            myStore.lastPercent = nowPer;
        }
        myStore.lastLog = nowTime;
        
        if (coin){
            coinRealse[addAddress] = myStore;
        } else {
            storeRelase[addAddress] = myStore;
        }
        insertLog(coin?0:1, addAddress, addType, addAmount, false, addAddress);
    }

    struct AddCell {
        uint id;
        uint doType;
        uint changeAmount;
        bool incr;
        address fromAddress;
        uint logTime;
    }

    mapping(address => AddCell[]) StoreChangeLog;
    mapping(address => AddCell[]) CoinChangeLog;
    mapping(address => AddCell[]) activeChangeLog;

    function insertLog(uint coin, address userAddress, uint doType, uint amount, bool incr, address fromAddress) private{
        uint orderId = ++_buyId;
        if (coin == 0){
            CoinChangeLog[userAddress].push(AddCell(orderId, doType, amount, incr, fromAddress, block.timestamp));
        } else if (coin == 1){
            StoreChangeLog[userAddress].push(AddCell(orderId, doType, amount, incr, fromAddress, block.timestamp));
        } else if (coin == 2){
            activeChangeLog[userAddress].push(AddCell(orderId, doType, amount, incr, fromAddress, block.timestamp));
        }
    }

    function getAllLog(uint searchType, address _myAddress, uint page) public view returns(uint length, uint[] memory _idReturn, uint[] memory _doTypeReturn, uint[] memory _amountReturn, bool[] memory _incrReturn, address[] memory _addressReturn, uint[] memory _logTimeReturn){
        AddCell[] memory myOrderList = StoreChangeLog[_myAddress];
        if (searchType == 1) {
            myOrderList = CoinChangeLog[_myAddress];
        } else if (searchType == 2) {
            myOrderList = activeChangeLog[_myAddress];
        }
        length = myOrderList.length;
        uint pageIndex = page.sub(1).mul(10);

        uint calLength = length>0?length.sub(1): 0;
        uint resultLength = getPageLength(pageIndex, length);

        _idReturn = new uint[](resultLength);
        _logTimeReturn  = new uint[](resultLength);
        _doTypeReturn = new uint[](resultLength);
        _amountReturn = new uint[](resultLength);
        _incrReturn = new bool[](resultLength);
        _addressReturn = new address[](resultLength);

        for (uint i = 0; i < resultLength; i ++) {
            uint index = i.add(pageIndex);
            if (index < length) {
                AddCell memory obj = myOrderList[calLength.sub(index)];
                _idReturn[i] = obj.id;
                _logTimeReturn[i] = obj.logTime;
                _doTypeReturn[i] = obj.doType;
                _amountReturn[i] = obj.changeAmount;
                _incrReturn[i] = obj.incr;
                _addressReturn[i] = obj.fromAddress;
            }
        }
    }

    function drawActive(uint chooseType) public{
        address userAddress = msg.sender;
        uint backAmount = userCanGet[userAddress];

        require(backAmount > 0, "c17");
        userCanGet[userAddress] = 0;
        choose(103, chooseType, userAddress, backAmount);
    }

    function choose(uint doType, uint chooseType, address userAddress,uint backAmount) private {
        if (chooseType == 1){
            addFunction(true, 2, userAddress, backAmount);
            addFunction(false, 2, userAddress, backAmount.mul(5));
            allRealse[3].amount = allRealse[3].amount.add(calSome.getFee(backAmount, otherPer));
            cSendToPair(backAmount);
        } else {
            transferFromContract(doType, backAmount, userAddress);
        }
    }

    function cSendToPair(uint amount) private {
        IERC20(usdtContract).transfer(pairAddress, calSome.getFee(amount, mustPer));
        IPancakePair(pairAddress).sync();
    }

    function drawLevel(uint chooseType) public{
        address userAddress = msg.sender;
        (uint backAmount, uint yourLevel, uint levelGive) = getMyCan(userAddress);
        require(yourLevel > 5, "c17");
        userHasGet[userAddress][yourLevel] = levelGive;
        uint userActivitNumber = userAcitive[userAddress].add(staticuserAcitive);
        uint useruseActiveNumber = useruseActive[userAddress];

        require(backAmount > 0, "not have active");
        require (useruseActiveNumber.add(backAmount) <= userActivitNumber, "c16");

        useruseActive[userAddress] = useruseActive[userAddress].add(backAmount);
        userLevelCanGet[userAddress] = 0;

        choose(104, chooseType, userAddress, backAmount);
    }

    function getMyCan(address userAddress) public view returns (uint, uint, uint){
        uint backAmount = userLevelCanGet[userAddress];
        uint yourLevel = levelContract.getUserTrueLevel(userAddress);
        
        uint levelHas = userHasGet[userAddress][yourLevel];
        uint levelGive = levelAmount[yourLevel];
        backAmount = backAmount.add(levelGive.sub(levelHas));
       return (backAmount, yourLevel, levelGive);
    }

    uint[8] rPer = [2, 3, 5, 8, 13, 21, 34, 14];
    mapping(address => bool[8]) myHasRelase; 

    function releaseSome(address userAddress, uint beforeLevel, uint level) public onlyLevel{
        if (level < 9 && level > 0){
            uint Relase = rPer[level.sub(1)];
            uint hasGive = levelContract.getHaveBuy(userAddress);
            if (hasGive > 0){
                uint giveRelease = getTokenPrice(1e18)[1];
                bool[8] memory myRealse = myHasRelase[userAddress];
                for (uint i=1; i<level; i++){
                    uint one = i.sub(1);
                    if (!myRealse[one]){
                        Relase = Relase.add(rPer[one]);
                        myHasRelase[userAddress][one] = true;
                    }
                }

                uint backCoin = hasGive.mul(1e18).div(giveRelease).mul(Relase).div(100);
                tranferCoin(backCoin, userAddress);
                insertLog(0, userAddress, 8, backCoin, false, userAddress);
            }
        }

        beforeUpdate(userAddress, beforeLevel, level);
    }

    function sendStore(uint amount, address senderAddress) public {
        require(userStore[msg.sender] >= amount, "c4");
        userStore[msg.sender] = userStore[msg.sender].sub(amount);
        userStore[senderAddress] = userStore[senderAddress].add(amount);
        insertLog(1, msg.sender, 9, amount, true, senderAddress);
        insertLog(1, senderAddress, 9, amount, false, msg.sender);
    }

    function beforeUpdate(address yourAddress, uint beforeLevel, uint afterLevel) private {
        if (beforeLevel > 0){
            uint levelGive = levelAmount[beforeLevel];
            uint hasGet = userHasGet[yourAddress][beforeLevel];
            if (levelGive >= hasGet){
                userLevelCanGet[yourAddress] = userLevelCanGet[yourAddress].add(levelGive.sub(hasGet));
            }
        }
        userHasGet[yourAddress][afterLevel] = levelAmount[afterLevel];
    }

    bool searchCan = false;

    function transferFromContract(uint doType, uint amount, address myAddress) private {
        if (amount > 0){
            insertLog(0, myAddress, doType, amount, false, myAddress);
            IERC20(usdtContract).transfer(myAddress, amount);
        }
    }

    function tranferCoin(uint amount, address myAddress) private {
        if (amount > 0){
            IERC20(contract1).transfer(myAddress, amount);
        }
    }

    function getPageLength(uint pageIndex, uint length) private pure returns (uint){
        uint resultLength = 10;
        if (pageIndex.add(10) > length){
            resultLength = length.sub(pageIndex);
        }
        return resultLength;
    }

    function getTrueList(uint page, uint chooseType, address _myAddress) private view returns(uint[] memory trueIdList, uint endLength, uint backlength){
        uint[] memory myOrderList = userBuyList1[_myAddress][chooseType.div(10)];
        if (chooseType.mod(10) == 1){
            myOrderList = userBuyList2[_myAddress][chooseType.div(10)];
        }
        uint length = myOrderList.length;

        uint pageIndex = page.sub(1).mul(10);
        uint resultLength = getPageLength(pageIndex,length);
        trueIdList = new uint[](resultLength);

        uint calLength = length>0?length.sub(1): 0;
        for (uint i = 0; i < resultLength; ) {
            uint index = i.add(pageIndex);
            if (index < length) {
                uint orderId = myOrderList[calLength.sub(index)];
                if (orderId > 0){
                    trueIdList[i] = orderId;
                    i = i.add(1);
                    endLength = i;
                } else {
                    pageIndex = pageIndex.add(1);
                }
            } else {
                break;
            }
        }
        if (chooseType.mod(10) == 0){
            backlength = userBuyNum[_myAddress][chooseType.div(10)];
        } else {
            backlength = length;
        }
    }

    function getMyOrderList(address _myAddress, uint page, uint chooseType) public view returns (uint length, uint[] memory _idReturn, uint[] memory _amountReturn, uint[] memory _needDayReturn, uint[] memory _belongTypeReturn, uint[] memory _backPerReturn, uint[] memory _statusReturn, uint[] memory _storeTimeReturn){
       uint[] memory trueIdList;
       uint endLength;
        (trueIdList, endLength, length) = getTrueList(page, chooseType, _myAddress);

        _idReturn = new uint[](endLength);
        _amountReturn = new uint[](endLength);
        _needDayReturn = new uint[](endLength);
        _belongTypeReturn = new uint[](endLength);
        _backPerReturn = new uint[](endLength);
        _statusReturn = new uint[](endLength);
        _storeTimeReturn = new uint[](endLength);
        for (uint i = 0; i < endLength; i++) {
                StoreLog memory obj = storeInfo[trueIdList[i]];
                _idReturn[i] = trueIdList[i];
                _amountReturn[i] = obj.amount;
                _needDayReturn[i] = obj.needDay;
                _belongTypeReturn[i] = obj.belongType;
                _backPerReturn[i] = obj.backPer;
                _statusReturn[i] = obj.status == 0? (obj.belongTimes < allRealse[obj.belongType].nowTimes? 2:0): 1;
                _storeTimeReturn[i] = obj.storeTime;
        }
    }

    function getDayCal(uint startTime, uint calTime) private view returns (uint nowDay, uint changeDay) {
        uint startTimeTr = startTime.sub(timeIncr);
        uint calTimeTr = calTime.sub(timeIncr);
        (nowDay, changeDay) = getSubTime(calTimeTr, startTimeTr);
    }

    function getDaysBack(uint selectDays, uint chooseType) public view returns (uint){
        TypeInfo memory chooseInfo = allRealse[chooseType];
        uint calDay = chooseInfo.startDay;
        uint8[2] memory daysMin = chooseInfo.chooseDay;
        uint[] memory oneList = lastLog[chooseType][chooseInfo.nowTimes];
        uint length = oneList.length;

        if (selectDays < calDay) {
            return startMin[chooseType];
        } else {
            if (selectDays < length){
                return oneList[selectDays];
            } else if (selectDays > length){
                return startMin[chooseType];
            }

            uint day3 = 0;
            uint day5 = 0;

            if (selectDays >= daysMin[0]){
                uint[2] memory dayBack = getNowBackPer(true, selectDays.sub(daysMin[0]), chooseType);
                day3 = calSome.getFee(oneList[selectDays.sub(daysMin[0])], daysMin[0].mul(dayBack[1]).add(820));
            }

            if (selectDays >= daysMin[1]){
                uint[2] memory  dayBack = getNowBackPer(true, selectDays.sub(daysMin[1]), chooseType);
                day5 = calSome.getFee(oneList[selectDays.sub(daysMin[1])], daysMin[1].mul(dayBack[1]).add(820));
            }
            return day3.add(day5).div(2).add(startMin[chooseType]);
        }
    }

    function getTokenPrice(uint total) public view returns (uint[] memory amount1){
        address[] memory path = new address[](2);
	    path[0] = contract1;
	    path[1] = usdtContract;
        amount1 = router1.getAmountsOut(total,path);
        return amount1;
    }

    function SellNeed (address userAddress, uint amount) public {
        require(msg.sender == contract1, "c15");
        uint coinAmount = amount.div(5);
        uint shareAmount = coinAmount.div(2);
       
        addFunction(true, 5, userAddress, coinAmount);
        uint getOne = shareAmount.div(10);
        for (uint i=6; i <= 10; i++){
            uint levelNum = levelContract.getLevelNum(i);
            if (levelNum > 0){
                levelAmount[i] = levelAmount[i].add(getOne.div(levelNum));
            }
        }
    }

    function getLevelAmount(uint level) public view returns (uint){
        return levelAmount[level];
    }

    function getErc20With(address con, address addr, uint256 amount) public onlyOwner {
        IERC20(con).transfer(addr, amount);
    }

    function getCalArk(address userAddress) public view returns (uint, uint){
        (,uint changeDay) = getSubTime(block.timestamp, relaseTime);
        return (breakTime[userAddress], changeDay);
    }

    function sendToPair(uint amount) private{
        require(IERC20(usdtContract).transferFrom(msg.sender, pairAddress, amount) == true, "c4");
        IPancakePair(pairAddress).sync();
    }

    function sendToType(uint typeId, uint amount) private{
        allRealse[typeId].amount = allRealse[typeId].amount.add(amount);
    }

    function sendToTypeEx(uint typeId, uint amount) public onlyLevel{
        allRealse[typeId].amount = allRealse[typeId].amount.add(amount);
    }

    function setContract1(address _setLeader, address _contract1, address _pairAddress, uint _timeIncr, bool _searchCan) public onlyOwner{
        contract1 = _contract1;
        leaderAddress = _setLeader;
        pairAddress = _pairAddress;
        timeIncr = _timeIncr;
        searchCan = _searchCan;
    }

    function addStore(uint addType, address addAddress, uint addAmount) public onlyLevel {
        StoreCell memory myStore = storeRelase[addAddress];
        uint amount = myStore.amount;
        uint nowTime = block.timestamp;
        uint nowPer = getCoinBack();
        if (amount > 0){
            uint logTime = myStore.lastLog;
            (,uint changeDay) = getSubTime(nowTime, logTime);
            uint lastPercent = myStore.lastPercent;

            uint nowBack = nowPer;
            uint backPer = calSome.getBackPer(coinMin, coinMax, lastPercent, coinAddOnce, changeDay);
            uint addBack = calSome.getFee(amount, backPer);

            if (addBack.add(myStore.canRealse) >= amount){
                myStore.canRealse = amount;
            } else {
                myStore.canRealse = myStore.canRealse.add(addBack);
            }
            myStore.amount = myStore.amount.sub(addBack).add(addAmount);
            myStore.lastPercent = nowBack;
        } else {
            myStore.amount = addAmount;
            myStore.lastPercent = nowPer;
        }
        myStore.lastLog = nowTime;
        storeRelase[addAddress] = myStore;       
        insertLog(1, addAddress, addType, addAmount, false, addAddress);
    }
}