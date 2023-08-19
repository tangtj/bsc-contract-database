/**
 *Submitted for verification at BscScan.com on 2023-06-22
*/

/**
 *Submitted for verification at Etherscan.io on 2023-06-21
*/

/**
 *Submitted for verification at BscScan.com on 2023-06-15
*/

/**
 *Submitted for verification at BscScan.com on 2023-06-15
*/

/**
 *Submitted for verification at BscScan.com on 2023-06-14
*/

/**
 *Submitted for verification at BscScan.com on 2023-06-08
*/

/**
 *Submitted for verification at BscScan.com on 2023-04-25
*/

/**
 *Submitted for verification at BscScan.com on 2023-03-14
*/

pragma solidity ^0.8.6;

// SPDX-License-Identifier: Unlicensed
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

contract Ownable {
    address public _owner;
    function owner() public view returns (address) {
        return _owner;
    }
     modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    }

    function changeOwner(address newOwner) public onlyOwner {
        _owner = newOwner;
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
        return c;
    }
}
interface IUniswapV2Pair {
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
interface ISwapFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}
interface ISwapRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external ;
}
contract Token is IERC20, Ownable {
    using SafeMath for uint256;

    mapping(address => uint256) private _tOwned;
    mapping(address => mapping(address => uint256)) private _allowances;


    uint256 private constant MAX = ~uint256(0);
    uint256 private _tTotal;
    uint256 private _rTotal;
    uint256 private _tFeeTotal;

    string private _name;
    string private _symbol;
    uint256 private _decimals;

    uint256 public _holdFee = 10;
    uint256 public _deadFee = 5;
    address private _deadAddress =
        address(0x000000000000000000000000000000000000dEaD);

    uint256 public _devFee = 28;
    address private _devAddress ;
    address public  _deployer;
    uint256 public _nodeFee = 20;
    address[] public _nodeAddress ;

    address public invitePoolAddr;
    address  public nftNodePoolAddr;
    address  public nftStationPoolAddr;
    mapping(address => bool) public swapPairList;
    address public usdtAddr;
    ISwapRouter private _swapRouter;

    uint256 public _baseFee = 10;
    address[] public _baseAddress;

    uint256 public _inviterFee = 27;

    address public uniswapV2Pair;

    constructor(address tokenOwner,address marketAddress, address invitePool, address nftNodePool, address nftStationPool,address USDTAddress,address RouterAddress) {
        _name = "CHATGPT-AI";
        _symbol = "GPTAI";
        _decimals = 18;

        _devAddress = marketAddress;
        _tTotal = 21000000 * 10**_decimals;

        _tOwned[tokenOwner] = _tTotal;

        _owner = msg.sender;
        _deployer = msg.sender;

        invitePoolAddr = address(invitePool);
        nftNodePoolAddr = address(nftNodePool);
        nftStationPoolAddr = address(nftStationPool);
        ISwapRouter swapRouter = ISwapRouter(RouterAddress);
        address usdt = USDTAddress;
        usdtAddr = usdt;
    
        _swapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;
        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        address usdtPair = swapFactory.createPair(address(this), usdt);
        require(IUniswapV2Pair(usdtPair).token1() == address(this), "invalid token address");
        uniswapV2Pair = usdtPair;

        swapPairList[usdtPair] = true;
        address mainPair = swapFactory.createPair(address(this), swapRouter.WETH());
        swapPairList[mainPair] = true;

        emit Transfer(address(0), tokenOwner, _tTotal);
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint256) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _tTotal;
    }
    function balanceOf(address account) public view override returns (uint256) {
        return _tOwned[account];
    }

    function transfer(address recipient, uint256 amount)
        public
        override
        returns (bool)
    {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender)
        public
        view
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount)
        public
        override
        returns (bool)
    {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            msg.sender,
            _allowances[sender][msg.sender].sub(
                amount,
                "ERC20: transfer amount exceeds allowance"
            )
        );
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue)
        public
        virtual
        returns (bool)
    {
        _approve(
            msg.sender,
            spender,
            _allowances[msg.sender][spender].add(addedValue)
        );
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        virtual
        returns (bool)
    {
        _approve(
            msg.sender,
            spender,
            _allowances[msg.sender][spender].sub(
                subtractedValue,
                "ERC20: decreased allowance below zero"
            )
        );
        return true;
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");

        bool takeFee = true;

        if (!swapPairList[to] && !swapPairList[from]) {
            takeFee = false;
        }
        bool isAdd = false;
        bool isDel = false;
        if (swapPairList[to]) {
            (isAdd,) = getLPStatus(from,to);
        }else if(swapPairList[from]){
            (,isDel) = getLPStatus(from,to);
        }else{
        }
        if (isAdd || isDel){
            takeFee = false;
        } 
        if (isDel) {
            amount = amount * 8000 / 10000;
        }

        _tokenTransfer(from, to, amount, takeFee);
    }

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee
    ) private {
        _tOwned[sender] = _tOwned[sender].sub(tAmount);
        uint256 rate;
        if (takeFee) {
            _takeTransfer(
                sender,
                _deadAddress,
                tAmount.div(1000).mul(_deadFee)
            );

            _takeTransfer(
                sender,
                _devAddress,
                tAmount.div(1000).mul(_devFee)
            );

            _takeTransfer(sender, invitePoolAddr, tAmount.div(1000).mul(_inviterFee));
            _takeTransfer(sender, nftNodePoolAddr, tAmount.div(1000).mul(_nodeFee));
            _takeTransfer(sender, nftStationPoolAddr, tAmount.div(1000).mul(_baseFee));

            uint256 nodebaseFee = 0;
            nodebaseFee = nodebaseFee + _nodeFee;
            nodebaseFee = nodebaseFee + _baseFee;
            rate =  _devFee + _deadFee + _inviterFee + nodebaseFee;
        }


        uint256 recipientRate = 1000 - rate;
        _tOwned[recipient] = _tOwned[recipient].add(tAmount.div(1000).mul(recipientRate));

        emit Transfer(sender, recipient, tAmount.div(1000).mul(recipientRate));
    }
   
    function getLPStatus(address from,address to) internal view  returns (bool isAdd,bool isDel){
        IUniswapV2Pair pair;
        address token = address(this);
        if(swapPairList[to]){
            pair = IUniswapV2Pair(to);
        }else{
            pair = IUniswapV2Pair(from);
        }
        isAdd = false;
        isDel = false;
        address token0 = pair.token0();
        address token1 = pair.token1();
        (uint r0,uint r1,) = pair.getReserves();
        uint bal1 = IERC20(token1).balanceOf(address(pair));
        uint bal0 = IERC20(token0).balanceOf(address(pair));
        if (swapPairList[to]) {
            if (token0 == token) {
                if (bal1 > r1) {
                    uint change1 = bal1 - r1;
                    isAdd = change1 > 1000;
                }
            } else {
                if (bal0 > r0) {
                    uint change0 = bal0 - r0;
                    isAdd = change0 > 1000;
                }
            }
        }else {
            if (token0 == token) {
                if (bal1 < r1 && r1 > 0) {
                    uint change1 = r1 - bal1;
                    isDel = change1 > 0;
                }
            } else {
                if (bal0 < r0 && r0 > 0) {
                    uint change0 = r0 - bal0;
                    isDel = change0 > 0;
                }
            }
        }
        return (isAdd,isDel);
    }

    function _takeTransfer(
        address sender,
        address to,
        uint256 tAmount
    ) private {
        _tOwned[to] = _tOwned[to].add(tAmount);
        emit Transfer(sender, to, tAmount);
    }
}

contract CHATGPTAI is Token {

    constructor() Token(
        address(0xB6feDc8193BC87A9f1a1f958719Bf1c6AFc0B092),//owner
        address(0x587f1F9D3D56357dEC34cADC5b874484D31c5725) ,// market
         address(0xBE96c739f265f647361111B76AEb0f3205B47EAA) ,// invitePool
         address(0xe3416CcF8fA4A5A631A0a34A0E8ceACc41e376df) ,// nftNodePool
         address(0x5E31Ee7c1eE610473c439b6e6E3fe20a7d7270cb) ,// nftStationPool
        address(0x55d398326f99059fF775485246999027B3197955),// USDT
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E) // PancakeSwap: Router v2
    ){

    }
}
// 0xCf97c40825dd2138B84033547E0b74b301054f71
// address(0xc632079f98dBA60003b06DC5a735E75f5BCe185B),// USDT
        // address(0xD99D1c33F9fC3444f8101754aBC46c52416550D1) // PancakeSwap: Router v2
        //         address(0x10ED43C718714eb63d5aA57B78B54704E256024E), // PancakeSwap: Router v2
        // address(0x55d398326f99059fF775485246999027B3197955), // USDT