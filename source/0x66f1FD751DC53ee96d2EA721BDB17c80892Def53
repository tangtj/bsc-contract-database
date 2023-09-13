// SPDX-License-Identifier: MIT

pragma solidity ^0.8.6;


interface DMRouter{
    function userBalances(address user) external returns (uint256);
}

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

    function WETH() external pure returns (address);

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
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
library TransferHelper {
    function safeApprove(
        address token,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('approve(address,uint256)')));
        (bool success, bytes memory data) = token.call(
            abi.encodeWithSelector(0x095ea7b3, to, value)
        );
        require(
            success && (data.length == 0 || abi.decode(data, (bool))),
            "TransferHelper: APPROVE_FAILED"
        );
    }

    function safeTransfer(
        address token,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(
            abi.encodeWithSelector(0xa9059cbb, to, value)
        );
        require(
            success && (data.length == 0 || abi.decode(data, (bool))),
            "TransferHelper: TRANSFER_FAILED"
        );
    }

    function safeTransferFrom(
        address token,
        address from,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(
            abi.encodeWithSelector(0x23b872dd, from, to, value)
        );
        require(
            success && (data.length == 0 || abi.decode(data, (bool))),
            "TransferHelper: TRANSFER_FROM_FAILED"
        );
    }

    function safeTransferETH(address to, uint256 value) internal {
        (bool success, ) = to.call{value: value}(new bytes(0));
        require(success, "TransferHelper: ETH_TRANSFER_FAILED");
    }
}

contract TokenDistributor is Ownable {
    mapping(address=>bool) public operator;
    constructor (address USDT) {
        operator[owner()] = true;
        IERC20(USDT).approve(msg.sender, uint(~uint256(0)));
    }
    function withdrawTokens(address tokenAddress, uint256 amount) external {
        require(operator[msg.sender],"not allow");
        IERC20 token = IERC20(tokenAddress);
        require(token.transfer(msg.sender, amount), "Token transfer failed");
    }

    function withdrawETH(uint256 amountOut) external  {
        require(operator[msg.sender],"not allow");
        TransferHelper.safeTransferETH(msg.sender, amountOut);
    }

    function setOperator(address _addressA,bool t) external{
        require(operator[msg.sender],"not allow");
        operator[_addressA] = t;
    }

}
interface ISwapPair {
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function token0() external view returns (address);

    function sync() external;

    function totalSupply() external view returns (uint);
}

contract DianMaoToken is  IERC20, Ownable {

    mapping(address=>uint) public lpHolderIndex;
    address[] public lpHolders;
    uint256 public lpCurrentIndex;

    mapping(address => uint256) public _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    string private _name;
    string private _symbol;
    uint8 private _decimals=18;


    uint256 private _tTotal;

    ISwapRouter public _swapRouter;
    mapping(address => bool) public _swapPairList;

    address public USDT;


    uint256 private constant MAX = ~uint256(0);
    TokenDistributor public _lpTokenDistributor;
    TokenDistributor public _DynamicRewardsTokenDistributor;
    TokenDistributor public _BottomPoolTokenDistributor;
    DMRouter public dMRouter;


    address public _mainPair;


    address public fundAddress;
    address public FoundationAddress;
    address public LPAddress;
    address public DynamicRewardsAddress;
    address public BottomPoolAddress;
    uint256 public LP_fee = 1;
    uint256 public Foundation_fee = 1;
    uint256 public Dynamic_fee = 1;
    uint256 public Bottom_fee = 1;
    uint256 public BottomPoolLimit = 1000 ** _decimals;
    uint256 public buyLimit=20;
    uint256 public startTradeBlock;


    mapping(address => bool) public _feeWhiteList;
    mapping(address => bool) public blacklistedUsers;
    mapping(address => bool) public _reWardExcludeList;


    uint256 public refAmount=(1 * 10**_decimals)/10000; //判定发送多少金额算绑定关系
    mapping(address => address) public referees; // 存储推荐关系
    mapping(address => uint256) public pendingRewards; // 存储待领取的奖励
    uint256 public lastClaimTime; // 记录上次领取奖励的时间
    address[] public rewardsUsersList;
    mapping(address=>uint) rewardsUsersListndex;
    bool public rewardsEnable;


    address public admin;

    modifier onlyAdmin() {
        require(msg.sender == admin, "Not authorized: Only admin can call this");
        _;
    }


    function setDmRouter(address _dmRouter) external onlyOwner{
        dMRouter = DMRouter(_dmRouter);
    }

    function setReferee(address _referee) external onlyOwner{
        require(referees[msg.sender] == address(0), "Referee already set!");
        referees[msg.sender] = _referee;
    }

    function setRefAmount(uint256 _newAmount)  external onlyOwner{
        refAmount = _newAmount;
    }

    function setadmin(address _admin) external onlyOwner{
        admin = _admin;
    }


    function setrewardsEnable(bool _rewardsEnable) external onlyOwner{
        rewardsEnable = _rewardsEnable;
    }


    function setBottomPoolLimit(uint256 _BottomPoolLimit) external onlyOwner{
        BottomPoolLimit = _BottomPoolLimit;
    }

    function setBuyLimit(uint256 _buyLimit) external onlyOwner{
        buyLimit = _buyLimit;
    }

    bool private inSwap;
    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }



    constructor (
        address RouterAddress,uint256 Supply,address _USDT){
        USDT = _USDT;
        // address USDT= 0x3b389783982bc58c15312871de7fbfec9ce65e39; // TODO 测试
        // = 0x55d398326f99059fF775485246999027B3197955;  // 线上

        fundAddress = msg.sender;
        FoundationAddress = msg.sender;
        admin =  msg.sender;

        _lpTokenDistributor = new TokenDistributor(USDT);
        _DynamicRewardsTokenDistributor = new TokenDistributor(USDT);
        _BottomPoolTokenDistributor = new TokenDistributor(USDT);

        LPAddress = address(_lpTokenDistributor);
        DynamicRewardsAddress = address(_DynamicRewardsTokenDistributor);
        BottomPoolAddress = address(_BottomPoolTokenDistributor);


        _name = "DianMao";
        _symbol = "DMC";
        _tTotal = Supply; //5000000000000000000000
        _balances[msg.sender] = _tTotal;
        emit Transfer(address(0), msg.sender, _tTotal);

        ISwapRouter swapRouter = ISwapRouter(RouterAddress);
        _swapRouter = swapRouter; // 测试 0xcc7adc94f3d80127849d2b41b6439b7cf1eb4ae0
        // 0x10ED43C718714eb63d5aA57B78B54704E256024E
        _allowances[address(this)][address(swapRouter)] = MAX;
        _allowances[LPAddress][address(this)] = MAX;
        _allowances[DynamicRewardsAddress][address(this)] = MAX;
        _allowances[BottomPoolAddress][address(this)] = MAX;

        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        address mainPair = swapFactory.createPair(address(this), USDT);
        _mainPair = mainPair;

        // 判断是否流动性必须的前置条件
        require(USDT < address(this),"USDT<address(this)");

        _feeWhiteList[LPAddress] = true;
        _feeWhiteList[DynamicRewardsAddress] = true;
        _feeWhiteList[msg.sender] = true;
        _feeWhiteList[address(this)] = true;
        IERC20(USDT).approve(RouterAddress,MAX);
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
        uint256 balance = _balances[account];
        return balance;
    }

    function startTrade() external onlyOwner {
        require(0 == startTradeBlock, "trading");
        startTradeBlock = block.number;
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

    function addReWardExcludeListBatch(address[] calldata userAddresses1) external onlyOwner {
        for (uint i = 0; i < userAddresses1.length; i++) {
            _reWardExcludeList[userAddresses1[i]] = true;
        }
    }

    function removeReWardExcludeListBatch(address[] calldata userAddresses1) external onlyOwner {
        for (uint i = 0; i < userAddresses1.length; i++) {
            _reWardExcludeList[userAddresses1[i]] = false;
        }
    }


    function addBlacklistBatch(address[] calldata userAddresses1) external onlyOwner {
        for (uint i = 0; i < userAddresses1.length; i++) {
            blacklistedUsers[userAddresses1[i]] = true;
        }
    }

    function removeBlacklistBatch(address[] calldata userAddresses1) external onlyOwner {
        for (uint i = 0; i < userAddresses1.length; i++) {
            blacklistedUsers[userAddresses1[i]] = false;
        }
    }
    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {

        require(!blacklistedUsers[from], "blacklistedUsers not allowed for this token");
        if(_feeWhiteList[from]  || _feeWhiteList[to] || inSwap){
            _baseTransfer(from,to,amount);
            return;
        }

        bool isAddLP;
        bool isRemoveLP;
        bool isBuy;
        bool isSell;

        // 判断行为类型 是添加流动性还是移除流动性
        if (to == _mainPair) {
            isAddLP = _isAddLiquidity(amount);
            isSell = !isAddLP;

        } else if (from == _mainPair) {
            isRemoveLP = _isRemoveLiquidity();
            isBuy = !isRemoveLP;
        }

        // 判断单笔限购10枚！
        if(isBuy){
            require(0 < startTradeBlock, "!Trading");
            require(amount<=buyLimit*(10 ** _decimals),"buy chaochu 10 deci");
        }

        uint256 totalFee;
        // 扣税
        if (isBuy) {
            _baseTransfer(from, LPAddress, (amount * LP_fee) / 100);
            _baseTransfer(from, FoundationAddress, (amount * Foundation_fee) / 100);
            _baseTransfer(from, DynamicRewardsAddress, (amount * Dynamic_fee) / 100);
            totalFee += (amount * LP_fee) / 100 + (amount * Foundation_fee) / 100 + (amount * Dynamic_fee) / 100;
        }

        if (isSell || isRemoveLP) {
            _baseTransfer(from, LPAddress, (amount * LP_fee) / 100);
            _baseTransfer(from, FoundationAddress, (amount * Foundation_fee) / 100);
            _baseTransfer(from, BottomPoolAddress, (amount * Bottom_fee) / 100);
            uint256 contractTokenBalance = balanceOf(BottomPoolAddress);
            if (contractTokenBalance > BottomPoolLimit) {
                swapTokenForFund(contractTokenBalance);
            }
            totalFee += (amount * LP_fee) / 100 + (amount * Foundation_fee) / 100 + (amount * Bottom_fee) / 100;
        }

        //将剩下的 amount 转给 to
        _baseTransfer(from,to,amount - totalFee);

        // LP合约分红本币
        if (from != address(_lpTokenDistributor)) {
            if (isAddLP) {
                addLPHolder(from);
            }else{
                processLPReward(500000); // _lpTokenDistributor
            }
        }

        // 判断是否需要绑定关系
        if(!isBuy && !isSell && !isAddLP && !isRemoveLP && amount == refAmount){
            bindDynamicReward(from, to);
        }
        // 计算应得分红
        if(isAddLP){
            distributeDynamicReward(from, amount);
            return;
        }
        // 触发推荐人分红
        if(rewardsEnable && block.timestamp - lastClaimTime >= 48 hours){
            claimRewardsForUsers(0,rewardsUsersList.length);
        }

    }

    function swapTokenForFund(uint256 lpAmount) public lockTheSwap {
        uint256 tokenAmount = lpAmount/2;
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = USDT;

        _baseTransfer(BottomPoolAddress, address(this), lpAmount);
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(BottomPoolAddress),
            block.timestamp
        );

        uint256 lpUsdt = IERC20(USDT).balanceOf(BottomPoolAddress);
        IERC20(USDT).transferFrom(BottomPoolAddress, address(this), lpUsdt);

        if (lpAmount > 0 && lpUsdt > 0) {
            _swapRouter.addLiquidity(
                address(this), address(USDT), lpAmount, lpUsdt, 0, 0, fundAddress, block.timestamp
            );
        }
    }

    function addLPHolder(address adr) private {
        if (0 == lpHolderIndex[adr]) {
            if (0 == lpHolders.length || lpHolders[0] != adr) {
                lpHolderIndex[adr] = lpHolders.length;
                lpHolders.push(adr);
            }
        }
    }


    function addLPHolderExt(address adr) public onlyOwner {
        addLPHolder(adr);
    }

    function processLPReward(uint256 gas) public {
        IERC20 token = IERC20(address(this));
        uint256 balance = token.balanceOf(address(_lpTokenDistributor));
        if (balance == 0) {
            return;
        }
        IERC20 holdToken = IERC20(_mainPair);
        uint holdTokenTotal = holdToken.totalSupply();

        address shareHolder;
        uint256 tokenBalance;
        uint256 amount;

        uint256 shareholderCount = lpHolders.length;
        uint256 gasUsed = 0;
        uint256 iterations = 0;
        uint256 gasLeft = gasleft();

        while (gasUsed < gas && iterations < shareholderCount) {
            if (lpCurrentIndex >= shareholderCount) {
                lpCurrentIndex = 0;
            }
            shareHolder = lpHolders[lpCurrentIndex];
            if(!_reWardExcludeList[shareHolder]){
                tokenBalance = holdToken.balanceOf(shareHolder) + dMRouter.userBalances(shareHolder);
                if (tokenBalance > 0) {
                    amount = balance * tokenBalance / holdTokenTotal;
                    if (amount > 0) {
                        token.transferFrom(address(_lpTokenDistributor),shareHolder, amount);
                    }
                }
                gasUsed = gasUsed + (gasLeft - gasleft());
                gasLeft = gasleft();
            }
            lpCurrentIndex++;
            iterations++;
        }
    }

    function _isAddLiquidity(uint256 amount) public view returns (bool isAdd){
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0, uint256 r1,) = mainPair.getReserves();

        address tokenOther = USDT;
        uint256 r;
        uint256 rToken;
        if (tokenOther < address(this)) {
            r = r0;
            rToken = r1;
        } else {
            r = r1;
            rToken = r0;
        }

        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        if (rToken == 0) {
            isAdd = bal > r;
        } else {
            isAdd = bal > r + r * amount / rToken / 2;
        }
    }

    function _isRemoveLiquidity() public  view returns (bool isRemove){
        ISwapPair mainPair = ISwapPair(_mainPair);
        (uint r0,uint256 r1,) = mainPair.getReserves();

        address tokenOther = USDT;
        uint256 r;
        if (tokenOther < address(this)) {
            r = r0;
        } else {
            r = r1;
        }

        uint bal = IERC20(tokenOther).balanceOf(address(mainPair));
        isRemove = r >= bal;
    }

    function _baseTransfer(
        address sender,
        address to,
        uint256 tAmount
    ) private {
        _balances[to] = _balances[to] + tAmount;
        _balances[sender] = _balances[sender] - tAmount;
        emit Transfer(sender, to, tAmount);
    }


    function setFundAddress(address _fundAddress) public onlyOwner{
        fundAddress = _fundAddress;
    }
    function setLPAddress(address lpAddress) public onlyOwner{
        LPAddress = lpAddress;
    }

    function setFoundationAddress(address foundationAddress) public onlyOwner{
        FoundationAddress = foundationAddress;
    }

    function setDynamicRewardsAddress(address dynamicRewardsAddress) public onlyOwner{
        DynamicRewardsAddress = dynamicRewardsAddress;
    }


    function setLPFee(uint256 fee) public onlyOwner{
        LP_fee = fee;
    }

    function setFoundationFee(uint256 fee) public onlyOwner{
        Foundation_fee = fee;
    }

    function setDynamicFee(uint256 fee) public onlyOwner{
        Dynamic_fee = fee;
    }

    function setBottomFee(uint256 fee) public onlyOwner {
        Bottom_fee = fee;
    }

    function addToFeeWhiteList(address user) public onlyOwner{
        _feeWhiteList[user] = true;
    }

    function removeFromFeeWhiteList(address user) public onlyOwner{
        _feeWhiteList[user] = false;
    }

    function bindDynamicReward(address from,address user) private {
        // 绑定关系
        if(referees[user]==address(0)){
            referees[user] = from;
            if(rewardsUsersListndex[from]==0 || (rewardsUsersList.length>0 && rewardsUsersList[0]!=from)){
                rewardsUsersListndex[user] = rewardsUsersList.length;
                rewardsUsersList.push(from);
            }
        }
    }
    function distributeDynamicReward(address user, uint256 amount) private {
        // 计算奖励
        address firstReferee = referees[user];
        if (firstReferee != address(0)) {
            uint256 firstReward = (amount * 5) / 100; // 一代直推5%
            pendingRewards[firstReferee] += firstReward;

            address secondReferee = referees[firstReferee];
            if (secondReferee != address(0)) {
                uint256 secondReward = (amount * 3) / 100; // 二代间推3%
                pendingRewards[secondReferee] += secondReward;
            }
        }
    }


    function claimRewardsForUsers(uint256 startIndex, uint256 endIndex) private {
        require(startIndex < endIndex && endIndex <= rewardsUsersList.length, "Invalid index values");

        for (uint256 i = startIndex; i < endIndex; i++) {
            address userAddress = rewardsUsersList[i];

            uint256 reward = pendingRewards[userAddress];
            if(reward > 0) {
                pendingRewards[userAddress]=0;
                _transfer(address(_DynamicRewardsTokenDistributor), userAddress, reward);
            }
        }
        lastClaimTime = block.timestamp;
    }


    function claimRewardsForUsersExternal(uint256 startIndex, uint256 endIndex) public onlyAdmin {
        claimRewardsForUsers(startIndex,endIndex);
    }



    function setAdmin(address _admin) external onlyOwner {
        admin = _admin;
    }

    function setOperator(address _admin) external onlyOwner {
        _lpTokenDistributor.setOperator(_admin,true);
        _DynamicRewardsTokenDistributor.setOperator(_admin,true);
        _BottomPoolTokenDistributor.setOperator(_admin,true);
    }
}