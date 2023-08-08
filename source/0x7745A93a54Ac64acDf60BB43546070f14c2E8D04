// SPDX-License-Identifier: MIT

pragma solidity 0.7.6;



library TransferHelper {
    function safeTransferETH(address to, uint value) internal {
        (bool success,) = to.call{value:value}(new bytes(0));
        require(success, 'TransferHelper: ETH_TRANSFER_FAILED');
    }
}

library SafeMathInt {
    int256 private constant MIN_INT256 = int256(1) << 255;
    int256 private constant MAX_INT256 = ~(int256(1) << 255);

    function mul(int256 a, int256 b) internal pure returns (int256) {
        int256 c = a * b;

        require(c != MIN_INT256 || (a & MIN_INT256) != (b & MIN_INT256));
        require((b == 0) || (c / b == a));
        return c;
    }

    function div(int256 a, int256 b) internal pure returns (int256) {
        require(b != -1 || a != MIN_INT256);

        return a / b;
    }

    function sub(int256 a, int256 b) internal pure returns (int256) {
        int256 c = a - b;
        require((b >= 0 && c <= a) || (b < 0 && c > a));
        return c;
    }

    function add(int256 a, int256 b) internal pure returns (int256) {
        int256 c = a + b;
        require((b >= 0 && c >= a) || (b < 0 && c < a));
        return c;
    }

    function abs(int256 a) internal pure returns (int256) {
        require(a != MIN_INT256);
        return a < 0 ? -a : a;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address who) external view returns (uint256);

    function allowance(address owner, address spender) external view returns (uint256);

    function transfer(address to, uint256 value) external returns (bool);

    function approve(address spender, uint256 value) external returns (bool);

    function transferFrom(address from, address to, uint256 value) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);

}

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
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

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b != 0);
        return a % b;
    }
}

interface InterfaceLP {
    function sync() external;
}

library Roles {
    struct Role {
        mapping (address => bool) bearer;
    }

    function add(Role storage role, address account) internal {
        require(!has(role, account), "Roles: account already has role");
        role.bearer[account] = true;
    }

    function remove(Role storage role, address account) internal {
        require(has(role, account), "Roles: account does not have role");
        role.bearer[account] = false;
    }

    function has(Role storage role, address account) internal view returns (bool) {
        require(account != address(0), "Roles: account is the zero address");
        return role.bearer[account];
    }
}

abstract contract ERC20Detailed is IERC20 {
    string private _name;
    string private _symbol;
    uint8 private _decimals;

    constructor(
        string memory _tokenName,
        string memory _tokenSymbol,
        uint8 _tokenDecimals
    ) {
        _name = _tokenName;
        _symbol = _tokenSymbol;
        _decimals = _tokenDecimals;
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint8) {
        return _decimals;
    }
}

interface IDEXRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    )
    external
    returns (
        uint256 amountA,
        uint256 amountB,
        uint256 liquidity
    );

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
    external
    payable
    returns (
        uint256 amountToken,
        uint256 amountETH,
        uint256 liquidity
    );

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

interface IDEXFactory {
    function createPair(address tokenA, address tokenB)
    external
    returns (address pair);
}

contract Ownable {
    address private _owner;

    event OwnershipRenounced(address indexed previousOwner);

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _owner = msg.sender;
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(msg.sender == _owner, "Not owner");
        _;
    }

    function renounceOwnership() public onlyOwner {
        emit OwnershipRenounced(_owner);
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public onlyOwner {
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal {
        require(newOwner != address(0));
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}
contract WhitelistedRole is Ownable {
    using Roles for Roles.Role;

    event WhitelistedAdded(address indexed account);
    event WhitelistedRemoved(address indexed account);

    Roles.Role private _whitelisteds;
    
    constructor(){
        _addWhitelisted(msg.sender);
    }

    modifier onlyWhitelisted() {
        require(isWhitelisted(msg.sender), "WhitelistedRole: caller does not have the role");
        _;
    }

   

    function isWhitelisted(address account) public view returns (bool) {
        return _whitelisteds.has(account);
    }


    function addWhitelisted(address account) public onlyWhitelisted {
        _addWhitelisted(account);
    }


    function removeWhitelisted(address account) public onlyWhitelisted {
        _removeWhitelisted(account);
    }

    function trenounceWhitelisted() public {
        _removeWhitelisted(msg.sender);
    }

    function _addWhitelisted(address account) internal {
        _whitelisteds.add(account);
        emit WhitelistedAdded(account);
    }

    function _removeWhitelisted(address account) internal {
        _whitelisteds.remove(account);
        emit WhitelistedRemoved(account);
    }

}

contract YOMPTOKEN is ERC20Detailed, Ownable, WhitelistedRole {
    using SafeMath for uint256;
    using SafeMathInt for int256;

    bool public initialDistributionFinished = false;
    bool public swapEnabled = true;
    bool public autoRebase = true;
    bool public feesOnNormalTransfers = false;
    
    uint256 public rebaseFrequency = 900;
    uint256 public nextRebase = block.timestamp + 900; //15nmis
    uint256 public lastRebase;

    mapping(address => bool) _isFeeExempt;
    address[] public _markerPairs;
    mapping (address => bool) public automatedMarketMakerPairs;

    uint256 public constant MAX_FEE_RATE = 20;
    uint256 private constant MAX_REBASE_FREQUENCY = 1800;
    uint256 private constant DECIMALS = 18;
    uint256 private constant MAX_UINT256 = ~uint256(0);
    uint256 private constant INITIAL_FRAGMENTS_SUPPLY =  10**6 * 10**DECIMALS; //1Million supply
    uint256 private constant TOTAL_GONS = MAX_UINT256 - (MAX_UINT256 % INITIAL_FRAGMENTS_SUPPLY);
    uint256 private constant MIN_SUPPLY = 1 * 10**DECIMALS;

    address private constant DEAD = 0x000000000000000000000000000000000000dEaD;
    address private constant ZERO = 0x0000000000000000000000000000000000000000;

    address     public KingReceiver = 0xA0c151669971b11648730A4B7a94A34F7916CC00;
    address[12] public DisciplesReceiver;
    address     public treasuryReceiver = 0xA0c151669971b11648730A4B7a94A34F7916CC00;
    address     public YOMPGod = 0xA0c151669971b11648730A4B7a94A34F7916CC00;
    
    uint256 public constant KingReceiverPart        = 33;
    uint256 public constant DisciplesReceiverPart   = 24;
    uint256 public constant treasuryReceiverPart    = 28;
    uint256 public constant YOMPGodPart             = 15;

   
    uint256 public startTime = 0;
    uint256 public first7  = 0; // startTime.add(7 * 1 days);
    uint256 public first30 = 0; //startTime.add(30 * 1 days);
    uint256 public first90 = 0; //startTime.add(90 * 1 days);
    uint256 public first180= 0; //startTime.add(180 * 1 days);
    uint256 public first364= 0; //startTime.add(364 * 1 days);

    uint256 public first1825= 0; //startTime.add(1825 * 1 days);
    uint256 public first3650= 0; //startTime.add(3650 * 1 days);

    uint256[7] public percentagesList = [100, 50, 25, 10, 5, 2, 1];
    uint256[7] public timesList = [first7, first30, first90, first180, first364, first1825, first3650];
   
    IDEXRouter public router;
    address public pair;

    uint256 public constant twentyFour = 24;
    uint256 public constant hour = (1 hours);
  
    uint256 public totalBuyFee = 15;
    uint256 public totalSellFee = 15;
    uint256 public constant feeDenominator = 100;

   bool inSwap;
  
    modifier swapping() {
        inSwap = true;
        _;
        inSwap = false;
    }

     modifier validRecipient(address to) {
        require(to != address(0x0));
        _;
    }

    uint256 private _totalSupply;
    uint256 private _gonsPerFragment;
    uint256 private gonSwapThreshold = (TOTAL_GONS * 10) / 10000;

    mapping(address => uint256) private _gonBalances;
    mapping(address => mapping(address => uint256)) private _allowedFragments;

    constructor() ERC20Detailed("YOMP", "YOMP", uint8(DECIMALS)) {
        router = IDEXRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E); //mainnet
        pair = IDEXFactory(router.factory()).createPair(address(this), router.WETH());
        
        _allowedFragments[address(this)][address(router)] = uint256(-1);
        _allowedFragments[address(this)][pair] = uint256(-1);
        _allowedFragments[address(this)][address(this)] = uint256(-1);
        
        setAutomatedMarketMakerPair(pair, true);
        
        _totalSupply = INITIAL_FRAGMENTS_SUPPLY;
        _gonBalances[msg.sender] = TOTAL_GONS;
        _gonsPerFragment = TOTAL_GONS.div(_totalSupply);

        _isFeeExempt[treasuryReceiver] = true;
        _isFeeExempt[address(this)] = true;
        _isFeeExempt[msg.sender] = true;

        startTime = block.timestamp;
        lastRebase = startTime;

        first7  = startTime.add(7 * 1 days);
        first30 = startTime.add(30 * 1 days);
        first90 = startTime.add(90 * 1 days);
        first180= startTime.add(180 * 1 days);
        first364= startTime.add(364 * 1 days);

        first1825= startTime.add(1825 * 1 days);
        first3650= startTime.add(3650 * 1 days);

        timesList = [first7, first30, first90, first180, first364, first1825, first3650];

        emit Transfer(address(0x0), msg.sender, _totalSupply);
    } 

    receive() external payable {
    }

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }

    function allowance(address owner_, address spender) external view override returns (uint256){
        
        return _allowedFragments[owner_][spender];
    }

    function balanceOf(address who) public view override returns (uint256) {
        return _gonBalances[who].div(_gonsPerFragment);
    }

    function checkFeeExempt(address _addr) external view returns (bool) {
        require(_addr != address(0), "checking zero address");
        return _isFeeExempt[_addr];
    }


    function checkSwapThreshold() external view returns (uint256) {
        return gonSwapThreshold.div(_gonsPerFragment);
    }

    function shouldRebase() internal view returns (bool) {
        return nextRebase <= block.timestamp;
    }

    function shouldTakeFee(address from, address to) internal view returns (bool) {
        if(_isFeeExempt[from] || _isFeeExempt[to]){ 
            return false;
        }else if (feesOnNormalTransfers){
            return true;
        }else{
            return (automatedMarketMakerPairs[from] || automatedMarketMakerPairs[to]); //check if its from any swap pair
        }
    }

    function shouldSwapBack() internal view returns (bool) {
        return
        !automatedMarketMakerPairs[msg.sender] &&
        !inSwap &&
        swapEnabled &&
        totalBuyFee.add(totalSellFee) > 0 &&
        _gonBalances[address(this)] >= gonSwapThreshold;
    }

    function getCirculatingSupply() public view returns (uint256) {
        return (TOTAL_GONS.sub(_gonBalances[DEAD]).sub(_gonBalances[ZERO])).div(_gonsPerFragment);
    }

    function getLiquidityBacking(uint256 accuracy) public view returns (uint256){
        uint256 liquidityBalance = 0;
        for(uint i = 0; i < _markerPairs.length; i++){
            liquidityBalance.add(balanceOf(_markerPairs[i]).div(10 ** 9));
        }
        return accuracy.mul(liquidityBalance.mul(2)).div(getCirculatingSupply().div(10 ** 9));
    }

    function isOverLiquified(uint256 target, uint256 accuracy) public view returns (bool){
        return getLiquidityBacking(accuracy) > target;
    }

    function manualSync() public {
        for(uint i = 0; i < _markerPairs.length; i++){
            InterfaceLP(_markerPairs[i]).sync();
        }
    }

    function transfer(address to, uint256 value) external override validRecipient(to) returns (bool){
        _transferFrom(msg.sender, to, value);
        return true;
    }

    function _basicTransfer(address from, address to, uint256 amount) internal returns (bool) {
        uint256 gonAmount = amount.mul(_gonsPerFragment);
        _gonBalances[from] = _gonBalances[from].sub(gonAmount);
        _gonBalances[to] = _gonBalances[to].add(gonAmount);

        emit Transfer(from, to, amount);

        return true;
    }

    function _transferFrom(address sender, address recipient, uint256 amount) internal returns (bool) {
        require(amount > 0, "Cannot transfer amount zero");
        bool excludedAccount = _isFeeExempt[sender] || _isFeeExempt[recipient];

        require(initialDistributionFinished || excludedAccount, "Trading not started");

        if (inSwap) {
            return _basicTransfer(sender, recipient, amount);
        }

        uint256 gonAmount = amount.mul(_gonsPerFragment);

        if (automatedMarketMakerPairs[recipient] && shouldSwapBack() ) { //check if sell transaction and shoulswapback
            swapBack();
        }

        _gonBalances[sender] = _gonBalances[sender].sub(gonAmount);

        uint256 gonAmountReceived = shouldTakeFee(sender, recipient) ? takeFee(sender, recipient, gonAmount) : gonAmount;
        _gonBalances[recipient] = _gonBalances[recipient].add(gonAmountReceived);

        emit Transfer(
            sender,
            recipient,
            gonAmountReceived.div(_gonsPerFragment)
        );

        if(shouldRebase() && autoRebase) {
            _rebase();

            if(!automatedMarketMakerPairs[sender]) manualSync();
        }

        return true;
    }

    function transferFrom(address from, address to, uint256 value) external override validRecipient(to) returns (bool) {
        if (_allowedFragments[from][msg.sender] != uint256(-1)) {
            _allowedFragments[from][msg.sender] = _allowedFragments[from][
            msg.sender
            ].sub(value, "Insufficient Allowance");
        }

        _transferFrom(from, to, value);
        return true;
    }

    function _swapTokensForBNB(uint256 tokenAmount) private {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = router.WETH();

        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );

        uint256 totalBNB = address(this).balance; //1bnb
        
        uint256 KingReceiverValue = totalBNB.mul(KingReceiverPart).div(feeDenominator); //1(33)/100 = .33
        TransferHelper.safeTransferETH(KingReceiver, KingReceiverValue);

        uint256 treasuryReceiverValue = totalBNB.mul(treasuryReceiverPart).div(feeDenominator); //1(28)/100 = .28
        TransferHelper.safeTransferETH(treasuryReceiver, treasuryReceiverValue);

        uint256 YOMPGodValue = totalBNB.mul(YOMPGodPart).div(feeDenominator); //1(15)/100 = .15
        TransferHelper.safeTransferETH(YOMPGod, YOMPGodValue);

        uint256 DisciplesReceiverValue = totalBNB.sub(KingReceiverValue.add(treasuryReceiverValue).add(YOMPGodValue)); // 1- (.33+.28+.15) = 0.24
        for(uint i=0; i<12; i++){
            TransferHelper.safeTransferETH(DisciplesReceiver[i], uint256(DisciplesReceiverValue.div(12))); //0.02
        }
        
    }

    function swapBack() internal swapping {
        uint256 contractTokenBalance = _gonBalances[address(this)].div(_gonsPerFragment); //1000

        if(contractTokenBalance > 0){
            _swapTokensForBNB(contractTokenBalance);
        }

        emit SwapBack(contractTokenBalance, treasuryReceiver);
    }

    function takeFee(address sender, address recipient, uint256 gonAmount) internal returns (uint256){
        uint256 _realFee = totalBuyFee;
        if(automatedMarketMakerPairs[recipient]) _realFee = totalSellFee;

        uint256 feeAmount = gonAmount.mul(_realFee).div(feeDenominator);

        _gonBalances[address(this)] = _gonBalances[address(this)].add(feeAmount);
        emit Transfer(sender, address(this), feeAmount.div(_gonsPerFragment));

        return gonAmount.sub(feeAmount);
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) external returns (bool){
        uint256 oldValue = _allowedFragments[msg.sender][spender];
        if (subtractedValue >= oldValue) {
            _allowedFragments[msg.sender][spender] = 0;
        } else {
            _allowedFragments[msg.sender][spender] = oldValue.sub(
                subtractedValue
            );
        }
        emit Approval(
            msg.sender,
            spender,
            _allowedFragments[msg.sender][spender]
        );
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) external returns (bool){
        _allowedFragments[msg.sender][spender] = _allowedFragments[msg.sender][
        spender
        ].add(addedValue);
        emit Approval(
            msg.sender,
            spender,
            _allowedFragments[msg.sender][spender]
        );
        return true;
    }

    function approve(address spender, uint256 value) external override returns (bool){
        _allowedFragments[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    
    function _rebase() private {
        if(!inSwap) {
           uint256 epoch = block.timestamp;
           uint256 lastRebaseVal = lastRebase;
           
           uint256 NowIndex = getTimeRange(epoch); //0
            uint256 startIndex = getTimeRange(lastRebase); // 0
            uint256 totalPRC=0;

            uint256 timeToUse = lastRebase;
            uint256 times=0;
            for(uint i=startIndex; i<=NowIndex; i++){

                if(epoch > timesList[i]) { timeToUse = timesList[i]; }
                else timeToUse = epoch;

                uint256 diff = (timeToUse.sub(lastRebaseVal)).div(rebaseFrequency); //1
                times = times.add(diff);

                totalPRC = totalPRC.add(uint256(percentagesList[i].mul(diff))); //1

                lastRebaseVal = timesList[i];
            }

            int256 supplyDelta = int256(INITIAL_FRAGMENTS_SUPPLY.mul(totalPRC).div(100).div(100).div(twentyFour.mul(hour)).mul(rebaseFrequency)); 

            lastRebase = lastRebase.add(times.mul(rebaseFrequency));
            nextRebase = lastRebase.add(rebaseFrequency); // next Rebasing should not happen before 15mins
            
            coreRebase(supplyDelta);
            
        }
    }

    function coreRebase(int256 supplyDelta) private returns (uint256) {
        uint256 epoch = block.timestamp;

        if (supplyDelta == 0) {
            emit LogRebase(epoch, _totalSupply);
            return _totalSupply;
        }

        if (supplyDelta < 0) {
            _totalSupply = _totalSupply.add(uint256(-supplyDelta));
        } else {
            _totalSupply = _totalSupply.sub(uint256(supplyDelta));
        }

        if (_totalSupply < MIN_SUPPLY) {
            _totalSupply = MIN_SUPPLY;
        }

        _gonsPerFragment = TOTAL_GONS.div(_totalSupply);

        emit LogRebase(epoch, _totalSupply);
        return _totalSupply;
    }

    function getTimeRange(uint256 timeNow) public view returns (uint256){
        //uint256 timeNow = block.timestamp;

        if(timeNow < first7){
            return 0;
        }
        else if(timeNow >= first7 && timeNow < first30){
            return 1;
        }
        else if(timeNow >= first30 && timeNow < first90){
            return 2;
        }
        else if(timeNow >= first90 && timeNow < first180){
            return 3;
        }
        else if(timeNow >= first180 && timeNow < first364){
            return 4;
        }
        else if(timeNow >= first364 && timeNow < first1825){
            return 5;
        }
        else if(timeNow >= first1825 && timeNow < first3650){
            return 6;
        }
        else return 0;
    }


    function setAutomatedMarketMakerPair(address _pair, bool _value) public onlyWhitelisted {
        require(automatedMarketMakerPairs[_pair] != _value, "Value already set");

        automatedMarketMakerPairs[_pair] = _value;

        if(_value){
            _markerPairs.push(_pair);
        }else{
            require(_markerPairs.length > 1, "Required 1 pair");
            for (uint256 i = 0; i < _markerPairs.length; i++) {
                if (_markerPairs[i] == _pair) {
                    _markerPairs[i] = _markerPairs[_markerPairs.length - 1];
                    _markerPairs.pop();
                    break;
                }
            }
        }

        emit SetAutomatedMarketMakerPair(_pair, _value);
    }

    function setInitialDistributionFinished(bool _value) external onlyOwner {
        require(initialDistributionFinished != _value, "Not changed");
        initialDistributionFinished = _value;
    }

    function setFeeExempt(address _addr, bool _value) external onlyWhitelisted {
        require(_isFeeExempt[_addr] != _value, "Not changed");
        _isFeeExempt[_addr] = _value;
    }

    function setSwapBackSettings(bool _enabled, uint256 _num, uint256 _denom) external onlyWhitelisted {
        swapEnabled = _enabled;
        gonSwapThreshold = TOTAL_GONS.div(_denom).mul(_num);
    }

    function setFeeReceivers(address _KingReceiver, address _treasuryReceiver, address[12] memory _DisciplesReceiver , address _YOMPGod) external onlyWhitelisted {
        require(
            _KingReceiver != address(0) &&
            _treasuryReceiver != address(0) &&
            _YOMPGod != address(0)
            ,
            "Wrong Address"
        );
        KingReceiver = _KingReceiver;
        treasuryReceiver = _treasuryReceiver;
        DisciplesReceiver = _DisciplesReceiver;
        YOMPGod = _YOMPGod;
    }

    function setFees(uint256 _totalBuyFee, uint256 _totalSellFee) external onlyWhitelisted {
        require(
            _totalBuyFee <= MAX_FEE_RATE,
            "wrong buy fees"
        );

        require(
            _totalSellFee <= MAX_FEE_RATE,
            "wrong sell fees"
        );

        totalBuyFee = _totalBuyFee;
        totalSellFee = _totalSellFee;
    }

    function clearStuckBalance(address _receiver) external onlyWhitelisted {
        uint256 balance = address(this).balance;
        payable(_receiver).transfer(balance);
    }

    function rescueToken(address tokenAddress, uint256 tokens) external onlyWhitelisted returns (bool success){
        return ERC20Detailed(tokenAddress).transfer(msg.sender, tokens);
    }

    function setAutoRebase(bool _autoRebase) external onlyWhitelisted {
        require(autoRebase != _autoRebase, "Not changed");
        autoRebase = _autoRebase;
    }

    function setRebaseFrequency(uint256 _rebaseFrequency) external onlyWhitelisted {
        require(_rebaseFrequency <= MAX_REBASE_FREQUENCY, "Too high");
        rebaseFrequency = _rebaseFrequency;
    }


    function setFeesOnNormalTransfers(bool _enabled) external onlyWhitelisted {
        require(feesOnNormalTransfers != _enabled, "Not changed");
        feesOnNormalTransfers = _enabled;
    }

    function setNextRebase(uint256 _nextRebase) external onlyWhitelisted { //if you want to toggle rebasing just set time to 20years later
        nextRebase = _nextRebase;
    }

    event SwapBack(uint256 contractTokenBalance,address treasuryReceiver);
    event SwapAndLiquify(uint256 tokensSwapped, uint256 bnbReceived, uint256 tokensIntoLiqudity);
    event LogRebase(uint256 indexed epoch, uint256 totalSupply);
    event SetAutomatedMarketMakerPair(address indexed pair, bool indexed value);
}