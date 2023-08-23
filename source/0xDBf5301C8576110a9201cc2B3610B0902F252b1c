pragma solidity ^0.8.19;

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

contract TokenDistributor {
    constructor (address token) {
        IERC20(token).approve(msg.sender, uint(~uint256(0)));
    }
}

contract Token is IERC20, Ownable {
    using SafeMath for uint256;

    uint256 totalFee;

    uint256 private _tTotal;

    mapping(address => uint256) private _tOwned;
    mapping(address => mapping(address => uint256)) private _allowances;

    mapping (address => bool) public eff;

    // uint256 public swapTokensAtAmount = totalSupply.mul(2).div(10**6); // 0.002%
    uint256 public swapTokensAtAmount = 1e18;

    bool private swapping;

    string private _name;
    string private _symbol;
    uint256 private _decimals;

    address private _deadAddress =
        address(0x000000000000000000000000000000000000dEaD);

    uint256 public communityFee = 10;
    uint256 public devFee = 10;
    uint256 public foundationFee = 20;
    uint256 public marketFee = 10;
    uint256 public nftFee = 20;

    address private _communityPoolAddr;
    address private _devPoolAddr;
    address private _foundationPoolAddr;
    address private _marketPoolAddr;
    address private _nftPoolAddr;

    uint256 public trasferFee = 200;

    uint256 public feeRate = 70;

    mapping(address => bool) public ammPairs;
    address public usdtAddr;
    ISwapRouter private _swapRouter;
    address public uniswapV2Pair;

    TokenDistributor private _tokenDistributor;

    event SwapUsdt(address indexed path0, address indexed path1, address indexed to, uint256 amount, uint256 value);

    constructor(address holder, address cpa, address dpa, address fpa, address mpa, address npa, address USDTAddress, address RouterAddress) {
        _name = "ETXU";
        _symbol = "ETXU";
        _decimals = 18;

        _tTotal = 96800000 * 10**_decimals;
        _tOwned[holder] = _tTotal;

        _owner = msg.sender;

        _communityPoolAddr = cpa;
        _devPoolAddr = dpa;
        _foundationPoolAddr = fpa;
        _marketPoolAddr = mpa;
        _nftPoolAddr = npa;

        ISwapRouter swapRouter = ISwapRouter(RouterAddress);
        usdtAddr = USDTAddress;
    
        _swapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = ~uint256(0);
        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        address usdtPair = swapFactory.createPair(address(this), usdtAddr);
        require(IUniswapV2Pair(usdtPair).token1() == address(this), "invalid token address");
        uniswapV2Pair = usdtPair;
        ammPairs[usdtPair] = true;
        address mainPair = swapFactory.createPair(address(this), swapRouter.WETH());
        ammPairs[mainPair] = true;

        _tokenDistributor = new TokenDistributor(usdtAddr);

        eff[address(this)] = true;
        eff[holder] = true;
        eff[_owner] = true;
        eff[address(_swapRouter)] = true;

        emit Transfer(address(0), holder, _tTotal);
    }

    function setEFF( address _eAddress) external onlyOwner{
        eff[_eAddress] = true;
    }

    function setFaEFF( address _eAddress) external onlyOwner{
        eff[_eAddress] = false;
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

        uint256 contractTokenBalance = balanceOf(address(this));

        bool canSwap = contractTokenBalance >= swapTokensAtAmount;

        if( canSwap &&
            !swapping &&
            !ammPairs[from] &&
            from != owner() &&
            to != owner()
        ) {
            swapping = true;
            swapUSDT(contractTokenBalance);
            IERC20 USDT = IERC20(usdtAddr);
            uint256 usdtBalance = USDT.balanceOf(address(_tokenDistributor));
            emit SwapUsdt(address(this), usdtAddr, address(_tokenDistributor), totalFee, usdtBalance);
            USDT.transferFrom(address(_tokenDistributor), _communityPoolAddr, usdtBalance.mul(communityFee).div(70));
            USDT.transferFrom(address(_tokenDistributor), _devPoolAddr, usdtBalance.mul(devFee).div(70));
            USDT.transferFrom(address(_tokenDistributor), _foundationPoolAddr, usdtBalance.mul(foundationFee).div(70));
            USDT.transferFrom(address(_tokenDistributor), _marketPoolAddr, usdtBalance.mul(marketFee).div(70));
            USDT.transferFrom(address(_tokenDistributor), _nftPoolAddr, usdtBalance.mul(nftFee).div(70));
            swapping = false;
        }

        bool takeFee = !swapping;

        bool isTrans = false;
        bool isAdd = false;
        bool isDel = false;
        if (ammPairs[to]) {
            (isAdd,) = getLPStatus(from, to);
        }else if(ammPairs[from]){
            (,isDel) = getLPStatus(from, to);
        }else{
            if (!ammPairs[from] && !ammPairs[to]) {
                isTrans = true;
            }
        }
        if (isAdd || isDel || eff[from] || eff[to]){
            takeFee = false;
        }

        _tokenTransfer(from, to, amount, takeFee, isTrans);
    }

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee,
        bool isTrans
    ) private {
        _tOwned[sender] = _tOwned[sender].sub(tAmount);

        uint256 toRate = 1000;
        if (takeFee) {
            if (isTrans) {
                uint256 tBurn = tAmount.mul(trasferFee).div(1000);
                _take(sender, _deadAddress, tBurn);
                toRate = toRate - trasferFee;
            } else {
                uint256 fee = tAmount.mul(feeRate).div(1000);
                _take(sender, address(this), fee);
                toRate = toRate - feeRate;
            }
        }

        _tOwned[recipient] = _tOwned[recipient].add(tAmount.mul(toRate).div(1000));
        emit Transfer(sender, recipient, tAmount.mul(toRate).div(1000));
    }

    function swapUSDT(uint256 tokenAmount) private {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = usdtAddr;

        _approve(address(this), address(_swapRouter), tokenAmount);
        _swapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(_tokenDistributor),
            block.timestamp
        );
    }
    
    function getLPStatus(address from,address to) internal view  returns (bool isAdd,bool isDel){
        IUniswapV2Pair pair;
        address token = address(this);
        if(ammPairs[to]){
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
        if (ammPairs[to]) {
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

    function _take(address from, address to, uint256 tValue) private {
        _tOwned[to] = _tOwned[to].add(tValue);
        emit Transfer(from, to, tValue);
    }

    function _takeTransfer(address from, address to, uint256 tAmount) private {
        _tOwned[from] = _tOwned[from].sub(tAmount);
        _tOwned[to] = _tOwned[to].add(tAmount);
        emit Transfer(from, to, tAmount);
    }
}

contract ETXU is Token {

    constructor() Token(
        address(0x2203C61Ddedb7a3c2B374F258e75130a3EBA10c7),//holder
        address(0xEd974c82B4Af45E6890643095c57b7Cd1dD64f01) ,// community
         address(0xa1c382F582FA0169F5CfA7ecbc9C68E376beB7E3) ,// dev
         address(0x7CFF48f196B4232AA83EAf18D70374Eb204a6CA2) ,// foundation
         address(0x44d4987A4890DD6366854daF409DD183f2CA3be6) ,// narket
         address(0x54F593e645f02a6Aa73Dfd2845655b56c787E698) ,// nft
        address(0x55d398326f99059fF775485246999027B3197955),// USDT
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E) // PancakeSwap: Router v2
    ){

    }
}

// address(0xc632079f98dBA60003b06DC5a735E75f5BCe185B),// USDT
// address(0xD99D1c33F9fC3444f8101754aBC46c52416550D1) // PancakeSwap: Router v2
// address(0x10ED43C718714eb63d5aA57B78B54704E256024E), // PancakeSwap: Router v2
// address(0x55d398326f99059fF775485246999027B3197955), // USDT