// SPDX-License-Identifier: SimPL-2.0

pragma solidity ^0.8.0;

interface IERC20 {
    // 返回存在的代币数量
    function totalSupply() external view returns (uint256);

    // 返回 account 拥有的代币数量
    function balanceOf(address account) external view returns (uint256);

    // 将 amount 代币从调用者账户移动到 recipient
    // 返回一个布尔值表示操作是否成功
    // 发出 {Transfer} 事件
    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    // 返回 spender 允许 owner 通过 {transferFrom}消费剩余的代币数量
    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    // 调用者设置 spender 消费自己amount数量的代币
    function approve(address spender, uint256 amount) external returns (bool);

    // 将amount数量的代币从 sender 移动到 recipient ，从调用者的账户扣除 amount
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);
    function toBurn(uint _amount) external;
    // 当value数量的代币从一个form账户移动到另一个to账户
    event Transfer(address indexed from, address indexed to, uint256 value);
    // 当调用{approve}时，触发该事件
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

interface IRouter {
    function WETH() external view returns (address);

    function getAmountsOut(uint256 amountIn, address[] memory path)
        external
        view
        returns (uint256[] memory amounts);

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

    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB);

    function removeLiquidityETH(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountToken, uint256 amountETH);

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

interface LRDespositeInterface {
    function desposite(uint256 amounts) external;
    function userMount() external view returns (uint256);
}

contract LRDesposite{

    address public owner;
    IRouter public router;
    address public _usdt = 0xA579fda72b2F56BE94805b27dE316acf5CF3Fb8d;
    address public _LR =  0x708A572473a1bef18E2a5d0eb010785131AA3632;
    address public _router = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
    address public _collected = 0xCf17185A23754D1A3dB05530b2A8D5231B1aFb80;

    uint256 public startNum = 100 * 10 ** 18;
    uint256 public userMount = 0;

    mapping(address => bool) public users;


    event userDesposite(address user,uint256 amounts);

    constructor() payable{
        owner = msg.sender;
        router = IRouter(_router); 
    }

    receive() external payable {}

    modifier isOnwer(){
        require(msg.sender == owner);
        _;
    }

    function setOwner(address own) public isOnwer{
        owner = own;
    }

    function setUsdtAddress(address adr) public isOnwer{
        _usdt = adr;
    }

    function setLRAddress(address adr) public isOnwer{
        _LR = adr;
    }

    function setCollectedAddress(address adr) public isOnwer{
        _collected = adr;
    }

    function getStartNum(uint256 _num) public {
        startNum = _num;
    }

    function setStartRouterContract(address adr) public isOnwer{
        _router = adr;
        router = IRouter(_router); 
    }

    function getAmountOut(uint256 usdt_, address tokenOut)
        public
        view
        returns (uint256)
    {   
        require(_usdt != address(0),"usdr undefind");
        address[] memory path = new address[](2);
        path[0] = address(_usdt);
        path[1] = tokenOut == address(0) ? router.WETH() : tokenOut;
        uint256[] memory amounts = router.getAmountsOut(usdt_, path);
        return amounts[1];
    }

    function desposite(uint256 amounts) public {

        require(amounts >= startNum,"start number is insufficient");
        if(users[msg.sender] == false){
            users[msg.sender] = true;
            userMount ++;
        }
        IERC20(_usdt).transferFrom(msg.sender,address(this),amounts);
        IERC20(_usdt).transfer(_collected,amounts/2);
        IERC20(_usdt).approve(_router,amounts/2);
        address[] memory path = new address[](2);
        path[0] = address(_usdt);
        path[1] = address(_LR);
        router.swapExactTokensForTokensSupportingFeeOnTransferTokens(amounts/2,0,path,address(this),block.timestamp + (10 minutes));
        uint256 ownToken = IERC20(_LR).balanceOf(address(this));
        IERC20(_LR).toBurn(ownToken);
        if(msg.sender != address(this)){
            emit userDesposite(msg.sender,amounts);
        }
        
    }

    function getLRBalance() public view returns(uint256){
        return IERC20(_LR).balanceOf(address(this));
    }

    function getUSDTBalance() public view returns(uint256){
        return IERC20(_usdt).balanceOf(address(this));
    }

    function toBuyUsdtUseMyLR() public {

        uint256 value = IERC20(_LR).balanceOf(address(this));
        IERC20(_LR).approve(_router,value);
        address[] memory path = new address[](2);
        path[0] = address(_LR);
        path[1] = address(_usdt);
        router.swapExactTokensForTokensSupportingFeeOnTransferTokens(value,0,path,address(this),block.timestamp + (10 minutes));
        uint256 usdtValue = IERC20(_usdt).balanceOf(address(this));
        IERC20(_usdt).approve(_router,usdtValue);
        // this.desposite(usdtValue);

    }

    function toUseMyAllUsdtDesposite() public {
        uint256 value = IERC20(_usdt).balanceOf(address(this));
        IERC20(_usdt).transfer(_collected,value/2);
        IERC20(_usdt).approve(_router,value/2);
        address[] memory path = new address[](2);
        path[0] = address(_usdt);
        path[1] = address(_LR);
        router.swapExactTokensForTokensSupportingFeeOnTransferTokens(value/2,0,path,address(this),block.timestamp + (10 minutes));
        uint256 ownToken = IERC20(_LR).balanceOf(address(this));
        IERC20(_LR).toBurn(ownToken);
    }

    function toNotificationEvent(address adr ,uint256 amounts) public  {
        require(msg.sender == _LR,"sender is not LRToken");
        // uint256 value = IERC20(_LR).balanceOf(address(this));
        // // IERC20(_LR).approve(_router,value);
        // address[] memory path = new address[](2);
        // path[0] = address(_LR);
        // path[1] = address(_usdt);
        // router.swapExactTokensForTokensSupportingFeeOnTransferTokens(value,0,path,address(this),block.timestamp + (10 minutes));
        // uint256 usdtValue = IERC20(_usdt).balanceOf(address(this));
        // IERC20(_usdt).approve(_router,usdtValue);

        emit userDesposite(adr,amounts);
        
    }

    

}