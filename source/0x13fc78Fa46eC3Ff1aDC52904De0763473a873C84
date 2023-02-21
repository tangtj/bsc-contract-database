// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
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
        // assert(a == b * c + a % b);
        // There is no case in which this doesn't hold
        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

contract Recv {
    IERC20 public token520 = IERC20(msg.sender);
    IERC20 public usdt;

    constructor ()  {
        usdt = IERC20(0x55d398326f99059fF775485246999027B3197955);
    }

    function withdraw() public {
        uint256 usdtBalance = usdt.balanceOf(address(this));
        if (usdtBalance > 0) {
            usdt.transfer(address(token520), usdtBalance);
        }
        uint256 token520Balance = token520.balanceOf(address(this));
        if (token520Balance > 0) {
            token520.transfer(address(token520), token520Balance);
        }
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

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

interface IERC20Metadata is IERC20 {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);
}

interface IPancakeFactory {
    event PairCreated(
        address indexed token0,
        address indexed token1,
        address pair,
        uint256
    );

    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);
}

interface IPancakeRouter {
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

    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountETH);

    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountETH);

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

interface IPancakePair {
    function token0() external view returns (address);

    function token1() external view returns (address);

    function swap(
        uint256 amount0Out,
        uint256 amount1Out,
        address to,
        bytes calldata data
    ) external;

    function sync() external;

    function getReserves()
        external
        view
        returns (
            uint112 reserve0,
            uint112 reserve1,
            uint32 blockTimestampLast
        );
}



contract Ownable is Context {
    address private _owner;
    address private _op;
    address private _dever;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        _op = msgSender;
        _dever = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    modifier onlyOp() {
        require(_op == _msgSender(), "Ownable: caller is not the op");
        _;
    }

    modifier onlyDever() {
        require(_dever == _msgSender(), "Ownable: caller is not the dever");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }

    function transferOp(address newOp) public virtual onlyOwner {
        require(
            newOp != address(0),
            "Ownable: new op is the zero address"
        );
        _op = newOp;
    }
}

library Address {
  
    function isContract(address account) internal view returns (bool) {
        // This method relies in extcodesize, which returns 0 for contracts in
        // construction, since the code is only stored at the end of the
        // constructor execution.

        // According to EIP-1052, 0x0 is the value returned for not-yet created accounts
        // and 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470 is returned
        // for accounts without code, i.e. `keccak256('')`
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        // solhint-disable-next-line no-inline-assembly
        assembly { codehash := extcodehash(account) }
        return (codehash != 0x0 && codehash != accountHash);
    }
}


interface StarLightHDiv {
    function getBuyAmount(address sender,address recipient,uint256 amount) external returns (uint256);
    function distributeReward(uint256 amount) external;
    function proc(address addr)  external ;
}

contract STARLIGHT is Context, IERC20, IERC20Metadata, Ownable {
    using SafeMath for uint256;
    string private _name;
    string private _symbol;
    uint256 private _totalSupply;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    
    mapping(address => address) public inviter;
    mapping(address => bool)    public inviterBlack;
    mapping(address => uint256) public inviteValidNum;
    mapping(address => uint256) public inviterLockTime;
    
    mapping(address => bool) public isNodes;
    mapping(address => bool) public isExcludedFromFee;
    mapping(address => bool) public inviteNeedActive;
    mapping(address => bool) public isInviteActive;
    mapping(address => bool) public isValidAddr;
    mapping(address => bool) public isVentureAddr;
    mapping(address => uint256) public ventureAmount;
    mapping(address => uint256) public ventureReleaseTimes;
    mapping(address => bool) private isDeliver;
    mapping(address => bool) private isWhiteAddr;
    mapping(address => uint256) private WhiteLimit;
    mapping(address => bool) public isBot;
    mapping(address => uint256) public botIndex;
    address[] public botAddrs;
    address[] public nodes;
    bool public    isLaunch = false;
    uint256 public startTime;
    uint256 public inviterRequireLockTime;
    address public _pair;
    uint256 public referThreshold = 0;

    address public recvAddress;
    address public techAddress;
    address public daoAddress;
    address public creationAddress;
    address public lpDivAddress;
    address public sysWAddress;
    address public sysAddress; 
    address public sysMAddress  = address(0x0000000000000000000000000000000000000adD);
    address public luck2Address = address(0x0000000000000000000000000000000000000099);
    address public luck3Address = address(0x0000000000000000000000000000000000000999);
    address public luck4Address = address(0x0000000000000000000000000000000000009999);
    address public  deadAddress = address(0x000000000000000000000000000000000000dEaD);
    address public _router      = address(0x10ED43C718714eb63d5aA57B78B54704E256024E);
    address public usdt         = address(0x55d398326f99059fF775485246999027B3197955);
    mapping(uint256 => address[]) public rounds;
    mapping(uint256 => mapping(address => uint256)) public rNodeRewards;
    mapping(uint256 => address[]) public roundNodes;
    mapping(uint256 => mapping(address => bool)) public roundNodePush;
    mapping(uint256 => mapping(address => bool)) public isRoundValid;
    mapping(uint256 => address[]) public hourAddrs;
    uint256 public hourStartTime;
    uint256 public roundIndex = 0;
    uint256 private enterCount = 0;
    uint256 public superNodeNum = 21;
    StarLightHDiv slhd;
    Recv   RECV;
    //buy fees 10%
    uint256  public referFee = 3;
    uint256  public nodeFee = 2;
    uint256  public superNodeFee = 1;
    uint256  public holdDivFee = 3;
    uint256  public techFee = 1;
    uint256  public priFee = 20;
    //sell fees 10%
    uint256 public lucky2Fee = 1;
    uint256 public lucky3Fee = 2;
    uint256 public lucky4Fee = 3;
    uint256 public hourFee = 1;
    uint256 public daoFee = 1;
    uint256 public lpDivFee = 1;
    uint256 public destroyFee = 1;
    address[] public buyluck2Addrs;
    address[] public buyluck3Addrs;
    address[] public buyluck4Addrs;
    uint256[] public buyluck2FAmounts;
    uint256[] public buyluck3FAmounts;
    uint256[] public buyluck4FAmounts;
    uint256[] public buyluck2Num;
    uint256[] public buyluck3Num;
    uint256[] public buyluck4Num;
    uint256[] public luck2Index;
    uint256[] public luck3Index;
    uint256[] public luck4Index;
    uint256[] public luck2RAmount;
    uint256[] public luck3RAmount;
    uint256[] public luck4RAmount;
    
    uint256 public luck2FeeTotal;
    uint256 public luck3FeeTotal;
    uint256 public luck4FeeTotal;

    bool private swapping;

    uint256 public haveRRThreshold =  100 * 10**18;

    uint256 public buyBase = 10**18;

    uint256 public whiteThres = 1000 * buyBase;

    uint256 public inviteActiveUsdt = 100 * buyBase;

    uint256 public validAddrUsdt = 100 * buyBase;

    uint256 public hourAddrUsdt = 300 * buyBase;

    uint256 public hourtime = 1 hours;
    uint256 public superNodeTime = 14 days;
    uint256 public inviteValidThres = 100;
    uint256 public hourNum = 3;

    uint256 public superNodeStartTime = 0;
    bool public swapAndLiquifyEnabled = true;
    bool whiteEnable = false;
    uint256 whiteEnableTime;

    uint256 constant  oneDay = 1 days;
    uint256 releaseEndTime = 0;
    uint256 cReleaseEndTime = 0 ;
    uint256 cReleaseTime = 0 ;
    uint256 releaseStartTime = 0 ;

    modifier transferCounter {
        enterCount = enterCount.add(1);
        _;
        enterCount = enterCount.sub(1, "transfer counter");
    }
    event SwapAndLiquify(
        uint256 tokensSwapped,
        uint256 ethReceived
    );

    constructor(address _recvAddress, address _techAddress ,address _daoAddress,address _creationAddress,address _lpDivAddress,address _sysWAddress,address _sysAddress) {
        _name = "LIGHT";
        _symbol = "LIGHT";
        recvAddress =  _recvAddress;
        techAddress =  _techAddress;
        daoAddress =  _daoAddress;
        creationAddress = _creationAddress;
        lpDivAddress = _lpDivAddress;
        sysWAddress = _sysWAddress;
        sysAddress = _sysAddress;
        _mint(recvAddress, 75000000 * 10**18);
        _mint(sysMAddress,25000000 * 10**18);
        IPancakeRouter router = IPancakeRouter(_router);
        _pair = IPancakeFactory(router.factory()).createPair(
            address(this),
            usdt
        );
        RECV = new Recv();
        _approve(address(this), address(_router), uint256(0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF));
        isExcludedFromFee[_router] = true;
        isExcludedFromFee[address(this)] = true;
        isExcludedFromFee[recvAddress] = true;
        isExcludedFromFee[techAddress] = true;
        isExcludedFromFee[daoAddress] = true;
        isExcludedFromFee[creationAddress] = true;
        isExcludedFromFee[lpDivAddress] = true;  
        isExcludedFromFee[sysWAddress] = true;
        isExcludedFromFee[sysAddress] = true;  
        inviterRequireLockTime = 60;
    }

    function name() public view virtual override returns (string memory) {
        return _name;
    }

    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual override returns (uint8) {
        return 18;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function transfer(address recipient, uint256 amount)
        public
        virtual
        override
        returns (bool)
    {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount)
        public
        virtual
        override
        returns (bool)
    {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);
        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(
            currentAllowance >= amount,
            "ERC20: transfer amount exceeds allowance"
        );
        unchecked {
            _approve(sender, _msgSender(), currentAllowance - amount);
        }
        return true;
    }

    function _mint(address account, uint256 amount) internal virtual {
        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);
    }

    function setHaveRRThreshold(uint256 amount) public onlyOwner{
        require(amount <= 100000000 * 10**18);
        haveRRThreshold = amount;
    }

    function setPriFee(uint256 _priFee) public onlyOwner{
        priFee = _priFee;
    }

    function setSLHD(address _slhd) public onlyOwner{
        if(address(slhd) != address(0)) setDeliver(address(slhd),false);
        slhd = StarLightHDiv(_slhd);
        setDeliver(address(slhd),true);
    }

    function Launch() public onlyOwner {
        require(!isLaunch);
        isLaunch = true;
        startTime = block.timestamp;
        superNodeStartTime = block.timestamp;
        hourStartTime = block.timestamp;
        cReleaseTime = block.timestamp.sub(oneDay);
        releaseStartTime = block.timestamp.sub(oneDay);
        releaseEndTime = releaseStartTime.add(20 * oneDay);
        cReleaseEndTime = cReleaseTime.add(365 * oneDay);
        
    }

    function addBot(address account) private {
        if (!isBot[account]){
         isBot[account] = true;
         botIndex[account] = botAddrs.length;
         botAddrs.push(account);
        }
    }

    function exBot(address account) external onlyOwner {
        if(isBot[account]){
         isBot[account] = false;
         botAddrs[botIndex[account]] = botAddrs[botAddrs.length-1];
         botAddrs.pop();
        }
    }

    function setReferThreshold(uint256 thres) public onlyOwner{
        require(thres <= 100000000 * 10**18);
        referThreshold = thres;
    }

    function setWhiteAddress(address[] memory accounts, bool isWL) public onlyOwner {
        for(uint256 i = 0 ; i <accounts.length;i++){
            isWhiteAddr[accounts[i]] = isWL;
        }
    }

    function setWhiteEnable(uint256 time) public onlyOwner {
        require(!whiteEnable);
        whiteEnable = true;
        whiteEnableTime = time;
    }

    function isWhiteAddress(address account) public view returns (bool) {
        return isWhiteAddr[account];
    }

    function setVentureAddress(address[] memory accounts,uint256[] memory amounts) public onlyOwner {
        for(uint256 i = 0 ; i <accounts.length;i++){
            isVentureAddr[accounts[i]] = amounts[i] > 0;
            ventureAmount[accounts[i]] = amounts[i];
        }
    }

    function isVentureAddress(address account) public view returns (bool) {
        return isVentureAddr[account];
    }

    function setInviterBlackAddress(address[] memory accounts, bool isIB) public onlyOwner {
         for(uint256 i = 0 ; i <accounts.length;i++){
            inviterBlack[accounts[i]] = isIB;
         }
    }

    function setSuperNodeNum(uint256 num) public onlyOwner {
        superNodeNum = num;
    }

    function setHourNum(uint256 num) public onlyOwner {
        hourNum = num;
    }

    function setDeliver(address _deliverAddr,bool _isD) public onlyOwner {
        isDeliver[_deliverAddr] = _isD;
    }

    function resetInviter(address[] memory accounts, address _inviter) public onlyOwner {
        require(!Address.isContract(_inviter));
        for(uint256 i = 0 ; i <accounts.length;i++){
            if(!Address.isContract(accounts[i])){
                address addr = _inviter;
                while(addr != accounts[i] && addr != address(0)) addr = inviter[addr];
                if(addr != accounts[i]){
                    inviter[accounts[i]] = _inviter;
                    inviterLockTime[accounts[i]] = block.timestamp;
                } 
            }
        }
    }

    function setInviteNeedActive(address[] memory accounts, bool isIT) public onlyOwner {
                for(uint256 i = 0 ; i <accounts.length;i++){
                    if(!Address.isContract(accounts[i]) && !isValidAddr[accounts[i]])
                        inviteNeedActive[accounts[i]] = isIT;
                }
    }

    function setInviter(address[] memory accounts, address _inviter,uint256 amount,bool isRInviter) public onlyOwner {
        uint256 total = 0;
        uint256 i ;
        require(!Address.isContract(_inviter));
        for(i = 0 ; i <accounts.length;i++){
            if(inviter[accounts[i]] == address(0) && accounts[i] != _inviter && !Address.isContract(accounts[i])){
                inviter[accounts[i]] = _inviter;
                inviterLockTime[accounts[i]] = block.timestamp; 
                inviteNeedActive[accounts[i]] = true;
                _basicTransfer(msg.sender,accounts[i],amount);
                total++;
            }
        }
        if(inviter[_inviter] == address(0)){
            inviter[_inviter] = sysAddress;
            inviterLockTime[_inviter] = block.timestamp;
            inviteNeedActive[_inviter] = true;
        }
        if(isRInviter && total > 0)
            _basicTransfer(msg.sender,_inviter,amount.mul(total).div(20));
    }

    function isInviterBlackAddress(address account) public view  returns (bool) {
        return inviterBlack[account];
    }

    function RoundLen(uint256 rIndex) public view  returns (uint256) {
        return rounds[rIndex].length;
    }

    function nodeLen() public view  returns (uint256) {
        return nodes.length;
    }

    function RoundNodeLen(uint256 rIndex) public view  returns (uint256) {
        return roundNodes[rIndex].length;
    }

    function RoundAddr(uint256 rIndex,uint256 index) public view  returns (address) {
        return rounds[rIndex][index];
    }


    function rescueToken(address tokenAddress, uint256 tokens)
    public
    onlyOwner
    returns (bool success)
    {
        return IERC20(tokenAddress).transfer(msg.sender, tokens);
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function balanceOf(address account)
        public
        view
        virtual
        override
        returns (uint256)
    {
        if(isVentureAddr[account] && releaseEndTime != 0 && ventureReleaseTimes[account] < releaseEndTime){
            uint256 lastReleaseTime = ventureReleaseTimes[account];
            if(lastReleaseTime == 0)
                lastReleaseTime = releaseStartTime; 
            uint256 time = block.timestamp > releaseEndTime? releaseEndTime : block.timestamp;
            return _balances[account].add(time.sub(lastReleaseTime).div(oneDay).mul(ventureAmount[account]).mul(5 * 10**18));
        }
        if(account == creationAddress && cReleaseEndTime != 0 && cReleaseTime < cReleaseEndTime){
            uint256 time = block.timestamp > cReleaseEndTime ? cReleaseEndTime : block.timestamp;
            uint256 addBal = time.sub(cReleaseTime).div(oneDay).mul(5000000 * 10**18).div(365);
            return _balances[account].add(addBal);
        }
        return _balances[account];
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual transferCounter {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(!isBot[sender],"the bot address");
        require(amount > 0);
        if(isDeliver[sender] ||isDeliver[recipient]){
             _basicTransfer(sender, recipient, amount);
            return;
        }
        if(isVentureAddr[sender] && ventureReleaseTimes[sender] < releaseEndTime){
            uint256 lastBalance = _balances[sender];
            uint256 newBalance = balanceOf(sender);
            if(newBalance > lastBalance){
                ventureReleaseTimes[sender] = block.timestamp > releaseEndTime? releaseEndTime : block.timestamp;
               _basicTransfer(sysMAddress,sender, newBalance.sub(lastBalance));    
            }
        }
        if(sender == creationAddress && cReleaseTime < cReleaseEndTime){
            uint256 lastBalance = _balances[sender];
            uint256 newBalance = balanceOf(sender);
            if(newBalance > lastBalance){
                cReleaseTime = block.timestamp > cReleaseEndTime? cReleaseEndTime : block.timestamp;
               _basicTransfer(sysMAddress,sender, newBalance.sub(lastBalance));    
            }
        }
        bool shouldSetInviter = !Address.isContract(recipient) && !Address.isContract(sender) && balanceOf(recipient) == 0 && inviter[recipient] == address(0) && !inviterBlack[sender];   
        if (isExcludedFromFee[sender] || isExcludedFromFee[recipient]) {
            _basicTransfer(sender, recipient, amount);
        } else {
            if(sender == _pair || recipient == _pair){
                if(!isLaunch)
                {
                    require( whiteEnable && block.timestamp >= whiteEnableTime && (isWhiteAddress(sender)||isWhiteAddress(recipient)) ,"swap not open");
                     if(sender == _pair && block.timestamp <= whiteEnableTime.add(15)){
                        addBot(recipient);
                    }
                }
            }
            if (sender == _pair) {
                _buyTransfer(sender,recipient,amount);
            }
            else if(recipient == _pair){
                _sellTransfer(sender,recipient,amount);
            } 
            else {
                require(isInviteActive[sender] || !inviteNeedActive[sender]);
                _basicTransfer(sender,recipient,amount);
            }
        }
         if (shouldSetInviter && amount >= referThreshold) {
             inviter[recipient] = sender;
             inviterLockTime[recipient] = block.timestamp; 
         }
         if(enterCount == 1){
               if(address(slhd)!=address(0)){
                    if(!Address.isContract(sender) && !isExcludedFromFee[sender])
                        slhd.proc(sender);
                    if(!Address.isContract(recipient) && !isExcludedFromFee[recipient])
                        slhd.proc(recipient);
               }
         }

    }

    function _sellTransfer(
         address sender,
        address recipient,
        uint256 amount
    )internal returns (bool){

            require(!inviteNeedActive[sender] || isInviteActive[sender]);

            uint256 share = amount.div(100);
            _basicTransfer(sender,daoAddress,share.mul(daoFee));
            uint256 curHour = block.timestamp.sub(hourStartTime).div(hourtime);
            if(hourAddrs[curHour].length == 0)
                _basicTransfer(sender,sysAddress,share.mul(hourFee));
            else{
                uint hourAFee = share.mul(hourFee).div(hourAddrs[curHour].length);
                for(uint256 i = 0;i<hourAddrs[curHour].length;i++){
                    _basicTransfer(sender,hourAddrs[curHour][i],hourAFee);
                }
            }
            _basicTransfer(sender,luck2Address,share.mul(lucky2Fee));
            luck2FeeTotal = luck2FeeTotal.add(share.mul(lucky2Fee));
            _basicTransfer(sender,luck3Address,share.mul(lucky3Fee));
            luck3FeeTotal = luck3FeeTotal.add(share.mul(lucky3Fee));
            _basicTransfer(sender,luck4Address,share.mul(lucky4Fee));
            luck4FeeTotal = luck4FeeTotal.add(share.mul(lucky4Fee));
            _basicTransfer(sender,lpDivAddress,share.mul(lpDivFee));
            _basicTransfer(sender,deadAddress,share.mul(destroyFee));
            uint256 _sellFee = daoFee+hourFee +lpDivFee + lucky2Fee+lucky3Fee+lucky4Fee+destroyFee;
            _basicTransfer(sender,recipient,share.mul(100-_sellFee));
            return true;
    }


    function _nodeProc(address node) internal returns (bool){
         if(roundNodes[roundIndex].length < superNodeNum){
                uint256 i ;
                for( i = 0; i< roundNodes[roundIndex].length; i++){
                    if(roundNodes[roundIndex][i] == node){
                        break;
                    }
                }
                if(i == roundNodes[roundIndex].length){
                    roundNodes[roundIndex].push(node);
                    roundNodePush[roundIndex][node] = true;
                }
        }else if(roundNodes[roundIndex][0] != node){

            uint256 minReward  = rNodeRewards[roundIndex][roundNodes[roundIndex][0]];
            uint256 minRIndex = 0;
            uint256 i;
            for(i = 1; i< superNodeNum; i++){
                if(roundNodes[roundIndex][i] == node){
                        break;
                }
                if(minReward > rNodeRewards[roundIndex][roundNodes[roundIndex][i]]){
                    minReward = rNodeRewards[roundIndex][roundNodes[roundIndex][i]];
                    minRIndex = i;
                }

            }
            if(i == superNodeNum && minReward < rNodeRewards[roundIndex][node]){
                 roundNodePush[roundIndex][roundNodes[roundIndex][minRIndex]] = false;
                 roundNodePush[roundIndex][node] = true;
                 roundNodes[roundIndex][minRIndex] = node;
                 
            }
        }
        return true;
    }

    function _buyTransfer(
         address sender,
        address recipient,
        uint256 amount
    )internal returns (bool){

            uint256 buyAmount = getBuyAmount(amount);
            if(address(slhd) != address(0))
                buyAmount = slhd.getBuyAmount(sender,recipient,amount);
            require( isLaunch || (!isLaunch && isWhiteAddress(recipient) && WhiteLimit[recipient].add(buyAmount) <= whiteThres));
            if(!isLaunch){
                WhiteLimit[recipient] = WhiteLimit[recipient].add(buyAmount);
            }
            if(!isInviteActive[recipient] && buyAmount >= inviteActiveUsdt){
                inviteNeedActive[recipient] = true;
                isInviteActive[recipient] = true;
            }
            if (inviter[recipient] == address(0)) {
                    inviter[recipient] = sysAddress; 
                    inviterLockTime[recipient] = block.timestamp;
                 } else {
                        if (
                            inviterLockTime[recipient] >
                            block.timestamp - inviterRequireLockTime &&
                            inviter[recipient] != sysAddress
                        ) {
                            
                            inviter[recipient] = sysAddress;
                            inviterLockTime[recipient] = block.timestamp;
                        }
            }
            if(!Address.isContract(recipient)){
                uint256 num = buyAmount.div(buyBase);
                if(num<10){

                }else if(num < 100 ){
                        buyluck2Addrs.push(recipient);
                        buyluck2FAmounts.push(luck2FeeTotal);
                        buyluck2Num.push(num);
                }else if (num <1000 ){
                        buyluck3Addrs.push(recipient);
                        buyluck3FAmounts.push(luck3FeeTotal);
                        buyluck3Num.push(num);
                }else if (num<10000){
                        buyluck4Addrs.push(recipient);
                        buyluck4FAmounts.push(luck4FeeTotal);
                        buyluck4Num.push(num);
                }
            }
            
            if(buyAmount >= hourAddrUsdt){
                uint256 curHour = block.timestamp.sub(hourStartTime).div(hourtime);
                if( !Address.isContract(recipient) && hourAddrs[curHour].length < hourNum){
                    hourAddrs[curHour].push(recipient);
                }
            }
            if(buyAmount >= validAddrUsdt){
               
                if(!isValidAddr[recipient] && !Address.isContract(recipient)){
                    isValidAddr[recipient] = true;
                    address ref = inviter[recipient];
                    if(ref != address(0) && !inviterBlack[ref]){
                        inviteValidNum[ref]++;
                        if(inviteValidNum[ref] == inviteValidThres){
                            nodes.push(ref);
                            isNodes[ref] = true;
                        }
                    }
                }
                if(superNodeStartTime != 0 && superNodeStartTime.add(superNodeTime) < block.timestamp){
                    superNodeStartTime = block.timestamp;
                    roundIndex++;
                }
                
                rounds[roundIndex].push(recipient);
                if(isNodes[recipient] && !isRoundValid[roundIndex][recipient]){
                    _nodeProc(recipient);
                }
                if(!isRoundValid[roundIndex][recipient])
                    isRoundValid[roundIndex][recipient] = true;
                
            }

            uint256 share = amount.div(100);
            address refer = inviter[recipient];
            if(refer != sysAddress && !inviterBlack[refer] && balanceOf(refer) >= haveRRThreshold)
             {
                _basicTransfer(sender,refer,share.mul(referFee));
             }else{
                _basicTransfer(sender,sysAddress,share.mul(referFee));
             } 
             while(refer != sysAddress){
                if(isNodes[refer] && !inviterBlack[refer] && isValidAddr[refer] && isInviteActive[refer] && balanceOf(refer) >= haveRRThreshold){
                   _basicTransfer(sender,refer,share.mul(nodeFee));
                    rNodeRewards[roundIndex][refer] += share.mul(nodeFee);
                    if(isRoundValid[roundIndex][refer]){
                        _nodeProc(refer);
                    }
                    break;
                }
                refer = inviter[refer];
             }
             if(refer == sysAddress){
                _basicTransfer(sender,sysAddress,share.mul(nodeFee));
             }
             if(roundIndex > 0){
                refer = inviter[recipient];
                while(refer != sysAddress){
                    if(roundNodePush[roundIndex-1][refer] && isRoundValid[roundIndex][refer]){
                        _basicTransfer(sender,refer,share.mul(superNodeFee));
                        break;
                    }
                    refer = inviter[refer];
                }
             }
             if(refer == sysAddress || roundIndex == 0){
                _basicTransfer(sender,sysAddress,share.mul(superNodeFee));
             }
             _basicTransfer(sender,techAddress,share.mul(techFee)); 
             if(address(slhd) == address(0)){
                _basicTransfer(sender,sysAddress,share.mul(holdDivFee));
             }else{
                 _basicTransfer(sender,address(slhd),share.mul(holdDivFee));
                 slhd.distributeReward(share.mul(holdDivFee));
             }
             uint256 _buyFee = referFee + nodeFee + techFee + superNodeFee + holdDivFee;
             if(!isLaunch){
                 _buyFee = _buyFee+priFee;
                 _basicTransfer(sender,sysWAddress,share.mul(priFee));
             }
             _basicTransfer(sender,recipient,amount.sub(share.mul(_buyFee)));
             return true;
    }

    function _basicTransfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal returns (bool) {
        _balances[sender] = _balances[sender].sub(amount,"Insufficient Balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function hasLuckNum(uint256 num,uint256 _start,uint256 _end) public  view returns(bool,uint256){
        require (num >= 10 && num < 10000 && _start < _end);
        bool find = false;
        uint256 start = 0;
        uint256 end = 0;
        if(num  < 10){

        }else if(num < 100){
            if(luck2Index.length > 0)
                start = luck2Index[luck2Index.length-1]+1;
            if(start < _start)
                start = _start;
            end = buyluck2Num.length;
            if(_end < end)
                end = _end;
            while(start < end){
                if(num == buyluck2Num[start]){
                   find = true;
                   break; 
                }
                start++;
            }

        }else if(num < 1000){
            if(luck3Index.length > 0)
                start = luck3Index[luck3Index.length-1]+1;
            if(start < _start)
                start = _start;
            end = buyluck3Num.length;
            if(_end < end)
                end = _end;
            while(start < end){
                if(num == buyluck3Num[start]){
                   find = true;
                   break; 
                }
                start++;
            }

        }else if (num < 10000){
            if(luck4Index.length > 0)
                start = luck4Index[luck4Index.length-1]+1;
            if(start < _start)
                start = _start;
            end = buyluck4Num.length;
            if(_end < end)
                end = _end;
            while(start < end){
                if(num == buyluck4Num[start]){
                   find = true;
                   break; 
                }
                start++;
            }
        }
        return (find,start);
    }

    function getLuck234Length() external view returns(uint256,uint256,uint256){
        return (buyluck2Addrs.length,buyluck3Addrs.length,buyluck4Addrs.length);
    }

    function setLuckNum(uint256 num,uint256 _start,uint256 _end) external onlyOp returns(bool,uint256){
        require (num >= 10 && num < 10000 && _start < _end);
        (bool find ,uint256 start) = hasLuckNum(num,_start,_end);
        require(find);
        if(num <10){

        }else if(num < 100){
            if(find){
                luck2Index.push(start);
                uint256 luckDAmount = 0;
                if(luck2Index.length == 1){
                    luckDAmount = buyluck2FAmounts[start];
                    
                }else{
                    luckDAmount = luck2RAmount[luck2RAmount.length-1].add(buyluck2FAmounts[luck2Index[luck2Index.length-1]].sub(buyluck2FAmounts[luck2Index[luck2Index.length-2]]));    
                }
                if(luckDAmount > 0){
                    uint256 luckReward = luckDAmount.mul(50).div(100);
                    luck2RAmount.push(luckDAmount.sub(luckReward));
                     _basicTransfer(luck2Address,buyluck2Addrs[start],luckReward);
                }
            }
        }else if(num < 1000){
            if(find){
                luck3Index.push(start);
                uint256 luckDAmount = 0;
                if(luck3Index.length == 1){
                    luckDAmount = buyluck3FAmounts[start];
                    
                }else{
                    luckDAmount = luck3RAmount[luck3RAmount.length-1].add(buyluck3FAmounts[luck3Index[luck3Index.length-1]].sub(buyluck3FAmounts[luck3Index[luck3Index.length-2]]));    
                }
                if(luckDAmount > 0){
                    uint256 luckReward = luckDAmount.mul(50).div(100);
                    luck3RAmount.push(luckDAmount.sub(luckReward));
                     _basicTransfer(luck3Address,buyluck3Addrs[start],luckReward);
                }
            }

        }else if (num < 10000){
            if(find){
                luck4Index.push(start);
                 uint256 luckDAmount = 0;
                 if(luck4Index.length == 1){
                    luckDAmount = buyluck4FAmounts[start];
                    
                }else{
                    luckDAmount = luck4RAmount[luck4RAmount.length-1].add(buyluck4FAmounts[luck4Index[luck4Index.length-1]].sub(buyluck4FAmounts[luck4Index[luck4Index.length-2]]));    
                }
                if(luckDAmount > 0){
                    uint256 luckReward = luckDAmount.mul(50).div(100);
                    luck4RAmount.push(luckDAmount.sub(luckReward));
                     _basicTransfer(luck4Address,buyluck4Addrs[start],luckReward);
                }
            }
        }
        return (find,start);
    }

    function getBuyAmount(uint256 amount) internal view returns (uint256) {
            uint256 buyAmount = 0;
            (uint256 reserve0, uint256 reserve1, ) = IPancakePair(_pair).getReserves();
            if (reserve1 > 0 && address(this) != IPancakePair(_pair).token0()) {
                buyAmount = reserve0.mul(amount).div(reserve1.add(amount));
            }
            if (reserve0 > 0 &&  address(this) != IPancakePair(_pair).token1()) {
                buyAmount = reserve1.mul(amount).div(reserve0.add(amount));
            }
            buyAmount = buyAmount.mul(1000).div(996);
            return buyAmount;
    }


    function setIsExcludedFromFee(address[] memory accounts, bool newValue)
        public
        onlyOwner
    {
        for(uint256 i = 0;i<accounts.length;i++)
            isExcludedFromFee[accounts[i]] = newValue;
    }

    function setInviteActiveUsdt(uint256 _inviteActiveUsdt) public onlyOwner{
        inviteActiveUsdt = _inviteActiveUsdt;
    }

    function setHourAddrUsdt(uint256 _hourAddrUsdt) public onlyOwner{
        hourAddrUsdt = _hourAddrUsdt;
    }

     function setValidAddrUsdt(uint256 _validAddrUsdt) public onlyOwner{
        validAddrUsdt = _validAddrUsdt;
    }
}