// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {

    event Transfer(address indexed from, address indexed to, uint256 value);


    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);


    function transfer(address to, uint256 amount) external returns (bool);


    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);


    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}
interface ISwapRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);

    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);

    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);

    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB, uint256 liquidity);

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
        returns (uint256 amountToken, uint256 amountETH, uint256 liquidity);
}

contract Ownable {
    address internal _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        address msgSender = msg.sender;
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == msg.sender);
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0));
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}



contract SimpleBridge is Ownable{
    string public  domain;
    string public  bridgeVersion;
    address public  bridgeToken;
    uint256 public fixedFee;

    ISwapRouter public swapRouter;
    address public fundAddress;
    address public usdt;

    mapping(uint64 => uint64) public getInboundNonceByChainId;
    mapping(uint64 => uint64) public getOutboundNonceByChainId;
    mapping(uint64 => bool) public supportedChainId;
    mapping(uint64 => uint256) public getOutBoundFeeByChainId;

    modifier inboundChecker(uint64 _srcChainId, uint64 _nonce) {
        require(_nonce == getInboundNonceByChainId[_srcChainId], "wrong nonce");
        getInboundNonceByChainId[_srcChainId]++;
        _;
    }
    event Received(address Sender, uint Value);

    receive() external payable {
        emit Received(msg.sender, msg.value);
    }
    
    function initialize(
        string memory _domain,
        string memory _bridgeVersion,
        address _bridgeToken,
        address _usdt,
        address _fundAddress,
        address _routerAddress
    ) public onlyOwner {
        domain = _domain;
        bridgeVersion = _bridgeVersion;
        bridgeToken = _bridgeToken;
        usdt = _usdt;
        fundAddress = _fundAddress;
        swapRouter = ISwapRouter(_routerAddress);
        IERC20(bridgeToken).approve(_routerAddress,~uint256(0));
        IERC20(_usdt).approve(_routerAddress,~uint256(0));

    }
    event DepositInitiated(
        uint64 dstChainId,
        uint64 indexed nonce,
        address token,
        address indexed from,
        address to,
        uint256 amount
    );

    event WithdrawalFinalizedETH(
        uint64 srcChainId,
        uint64 indexed nonce,
        address indexed from,
        address indexed target,
        uint256 value
    );
    event WithdrawalFinalizedToken(
        uint64 srcChainId,
        uint64 indexed nonce,
        address  token,
        address indexed from,
        address indexed target,
        uint256 value
    );
    event Failed_swapExactTokensForTokens(
        uint256 value
    );
    event Failed_swapExactETHForTokens(
        uint256 value
    );
    event Failed_swapExactTokensForETH(
        uint256 value
    );
    event Failed_addLiquidityETH(
        uint256 value
    );
    event Failed_addLiquidity(
        uint256 value
    );

    event SwapSuccess(uint64 indexed nonce, int256 amountIn, int256 amoutOut);

    event SwapFailed(uint64 indexed nonce, uint256 amountIn, string reason);

    function enableChain(uint64 _chainId, bool _enabled) public onlyOwner {
        supportedChainId[_chainId] = _enabled;
    }

    function checkFeeAmount(uint64 _chainId,uint256 swapAmount,bool isETH) public view returns(uint256)  {
        require(swapAmount >0);
        uint256 feeAmount;
        if(isETH){

            address[] memory ETHtoUpath = new address[](2);
            ETHtoUpath[0] = usdt;
            ETHtoUpath[1] = swapRouter.WETH();

            uint256 fixedETHAmount = swapRouter.getAmountsOut(fixedFee,ETHtoUpath)[1];
            feeAmount = swapAmount * getOutBoundFeeByChainId[_chainId] / 100 + fixedETHAmount;

        }else{
            feeAmount = swapAmount * getOutBoundFeeByChainId[_chainId] / 100 + fixedFee;
        }
        return feeAmount;

    }


    function swapETHtoDeposit(uint256 feeAmount,uint256 tAmount) private returns(bool) {

        uint256 lpAmount = feeAmount / 2;

        address[] memory path = new address[](2);
        path[0] = swapRouter.WETH();
        path[1] = bridgeToken;
        uint256 beforeBridgeAmount = IERC20(bridgeToken).balanceOf(address(this));
        if(lpAmount >0){

            try
                swapRouter.swapExactETHForTokens{value: lpAmount}(
                    0,
                    path,
                    address(this),
                    block.timestamp
                )

            {} catch {
                emit Failed_swapExactETHForTokens(lpAmount);
            }           
        }else{
            return false;
        }

        
        uint256 afterBridgeAmount = IERC20(bridgeToken).balanceOf(address(this));
        uint256 bridgeAmount = afterBridgeAmount - beforeBridgeAmount;

        if(bridgeAmount >0){
            try
                swapRouter.addLiquidityETH{value: lpAmount}(
                    bridgeToken,
                    bridgeAmount,
                    0,
                    0,
                    fundAddress,
                    block.timestamp
                )
            {} catch {
                emit Failed_addLiquidityETH(lpAmount);
            }
        }else{
            return false;
        }


        uint256 swapAmount = tAmount - feeAmount;
        uint256 acceptSwapedBridgeAmount = swapRouter.getAmountsOut(swapAmount,path)[1] / 2;
        if(address(this).balance >= swapAmount){
            try
                swapRouter.swapExactETHForTokens{value: swapAmount}(
                    acceptSwapedBridgeAmount,
                    path,
                    address(this),
                    block.timestamp
                )

            {} catch {
                emit Failed_swapExactETHForTokens(swapAmount);
            }
        }else{
            return false;
        }
        uint256 finalBridgeAmount = IERC20(bridgeToken).balanceOf(address(this));
        uint256 swapedBridgeAmount = finalBridgeAmount - beforeBridgeAmount;
        if(swapedBridgeAmount>=acceptSwapedBridgeAmount){
            return true;
        }else{
            return false;
        }
    }

    function swapTokentoDeposit(address token,uint256 feeAmount,uint256 tAmount) private returns(bool) {

        uint256 lpTokenAmount = feeAmount / 2;

        address[] memory path = new address[](3);
        path[0] = token;
        path[1] = swapRouter.WETH();
        path[2] = bridgeToken;
        uint256 beforeBridgeAmount = IERC20(bridgeToken).balanceOf(address(this));
        if(lpTokenAmount > 0 ){
            try
                swapRouter.swapExactTokensForTokens(
                        lpTokenAmount,
                        0,
                        path,
                        address(this),
                        block.timestamp
                    )
            {} catch {
                emit Failed_swapExactTokensForTokens(lpTokenAmount);
            }
        }else{
            return false;
        }

        

        uint256 afterBridgeAmount = IERC20(bridgeToken).balanceOf(address(this));
        uint256 lpBridgeAmount = afterBridgeAmount - beforeBridgeAmount;

        if(lpBridgeAmount > 0 ){
            try
                swapRouter.addLiquidity(
                    token,
                    bridgeToken,
                    lpTokenAmount,
                    lpBridgeAmount,
                    0,
                    0,
                    fundAddress,
                    block.timestamp
                )
            {} catch {
                emit Failed_addLiquidity(lpTokenAmount);
            }
        }else{
            return false;
        }

        uint256 swapAmount = tAmount - feeAmount;
        uint256 tokenBalance = IERC20(token).balanceOf(address(this));
        uint256 acceptSwapedBridgeAmount = swapRouter.getAmountsOut(swapAmount,path)[2] / 2;
        if(tokenBalance >= swapAmount){
            try
                swapRouter.swapExactTokensForTokens(
                        swapAmount,
                        acceptSwapedBridgeAmount,
                        path,
                        address(this),
                        block.timestamp
                    )
            {} catch {
                emit Failed_swapExactTokensForTokens(swapAmount);

            }
        }
        uint256 finalBridgeAmount = IERC20(bridgeToken).balanceOf(address(this));
        uint256 swapedBridgeAmount = finalBridgeAmount - beforeBridgeAmount;
        if(swapedBridgeAmount>=acceptSwapedBridgeAmount){
            return true;
        }else{
            return false;
        }
    }


    function depositETH(uint64 _dstChainId,address _dstToken, address _to, uint256 swapAmount) public payable  {

        require(supportedChainId[_dstChainId], "NOT_SUPPORTED");
        uint256 feeAmount =  checkFeeAmount(_dstChainId,swapAmount,true);
        //判断转入数量大于兑换到的 桥币数量+费用
        uint256 tAmount = feeAmount + swapAmount;
        require(msg.value >= tAmount, "too few");
        //兑换桥币
        uint256 beforeBridgeAmount = IERC20(bridgeToken).balanceOf(address(this));
        bool success = swapETHtoDeposit(feeAmount, tAmount);
        uint256 afterBridgeAmount = IERC20(bridgeToken).balanceOf(address(this));
        if(success){
            uint256 swapedBridgeAmount = afterBridgeAmount - beforeBridgeAmount;
            //nonce核对
            uint64 _nonce = getOutboundNonceByChainId[_dstChainId];
            getOutboundNonceByChainId[_dstChainId]++;
            
            emit DepositInitiated(_dstChainId, _nonce, _dstToken, msg.sender, _to, swapedBridgeAmount);

        }

    }

    function depositToken(uint64 _dstChainId,address _dstToken, address _to,address token, uint256 swapAmount) public   {
        
        require(supportedChainId[_dstChainId], "NOT_SUPPORTED");
        uint256 feeAmount =  checkFeeAmount(_dstChainId,swapAmount,false);
        //判断转入数量大于兑换到的 桥币数量+费用
        uint256 tAmount = feeAmount + swapAmount;
        require(IERC20(token).allowance(msg.sender,address(this))>=tAmount && tAmount > 0);
        IERC20(token).transferFrom(msg.sender,address(this),tAmount);
        //兑换桥币
        uint256 beforeBridgeAmount = IERC20(bridgeToken).balanceOf(address(this));
        bool success = swapTokentoDeposit(token,feeAmount, tAmount);
        uint256 afterBridgeAmount = IERC20(bridgeToken).balanceOf(address(this));
        if(success){
            uint256 swapedBridgeAmount = afterBridgeAmount - beforeBridgeAmount;
            //nonce核对
            uint64 _nonce = getOutboundNonceByChainId[_dstChainId];
            getOutboundNonceByChainId[_dstChainId]++;
            
            emit DepositInitiated(_dstChainId, _nonce, _dstToken, msg.sender, _to, swapedBridgeAmount);

        }
    }

    function claimToken(
        address token,
        uint256 amount,
        address to
    ) external onlyOwner {
        IERC20(token).transfer(to, amount);
    }

    function claimBalance() external onlyOwner{
        payable(fundAddress).transfer(address(this).balance);
    }
    function getFeeByChainId(uint64 _chainId) public view returns (uint256) {
        return getOutBoundFeeByChainId[_chainId];
    }


    function setFee(uint64 _chainId, uint256 feeRate) public onlyOwner {
        getOutBoundFeeByChainId[_chainId] = feeRate;
    }
    function setFixedFee(uint256 _fee) public onlyOwner {
        fixedFee = _fee;
    }
    function finalizeWithdrawETH(
        uint64 _srcChainId,
        uint64 _nonce,
        address _from,
        address _target,
        uint256 _value
    ) public onlyOwner inboundChecker(_srcChainId, _nonce) {
        uint256 bridgeBalance = IERC20(bridgeToken).balanceOf(address(this));
        require(bridgeBalance >= _value);
        address[] memory path = new address[](2);
        path[0] = bridgeToken;
        path[1] = swapRouter.WETH();

        try
            swapRouter.swapExactTokensForETH(
                    _value,
                    0,
                    path,
                    _target,
                    block.timestamp
                )
        {} catch {
            emit SwapFailed( _nonce, _value, "swapETHFailed");
        }
        emit WithdrawalFinalizedETH(_srcChainId, _nonce, _from, _target, _value);
    }
    function finalizeWithdrawToken(
        uint64 _srcChainId,
        uint64 _nonce,
        address token,
        address _from,
        address _target,
        uint256 _value
    ) public onlyOwner inboundChecker(_srcChainId, _nonce) {
        uint256 bridgeBalance = IERC20(bridgeToken).balanceOf(address(this));
        require(bridgeBalance >= _value);
        address[] memory path = new address[](3);
        path[0] = bridgeToken;
        path[1] = swapRouter.WETH();
        path[2] = token;
        try
            swapRouter.swapExactTokensForTokens(
                    _value,
                    0,
                    path,
                    _target,
                    block.timestamp
                )
        {} catch {
            emit SwapFailed( _nonce, _value, "swapTokenFailed");
        }
        emit WithdrawalFinalizedToken(_srcChainId, _nonce,token, _from, _target, _value);
    }

    function setFundAddress(address addr) external onlyOwner {
        fundAddress = addr;
    }

}