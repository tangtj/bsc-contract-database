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
   modifier onlyowner() {
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

    mapping(address => uint256) private _rOwned;
    mapping(address => uint256) private _tOwned;
    mapping(address => mapping(address => uint256)) private _allowances;


    uint256 private constant MAX = ~uint256(0);
    uint256 private _tTotal;
    uint256 private _rTotal;
    uint256 private _tFeeTotal;

    mapping (address => bool) private _iclud;
    address[] private _ecd;

    string private _name;
    string private _symbol;
    uint256 private _decimals;

    uint256 public _holdFee = 10;
    uint256 public _deadFee = 5;
    address private _deadAddress =
        address(0x000000000000000000000000000000000000dEaD);

    uint256 public _devFee = 18;
    address private _devAddress ;
    address public  _deployer;
    uint256 public _nodeFee = 20;
    address[] public _nodeAddress ;
    mapping(address => bool) private _closeNode;

    address public _invitePool;
    address  public _nftNodePool;
    address  public _nftStationPool;
    mapping(address => bool) public _swapPairList;
    address public _usdt;
    ISwapRouter private _swapRouter;


    uint256 public _baseFee = 10;
    address[] public _baseAddress ;
    mapping(address => bool) private _closeBase;


    address private devAddress;

    uint256 public _inviterFee = 27;

    // mapping(address => address) public inviter;

    address public uniswapV2Pair;

    constructor(address tokenOwner,address marketAddress, address invitePool, address nftNodePool, address nftStationPool,address USDTAddress,address RouterAddress) {
        _name = "CHATGPT-AI";
        _symbol = "GPTA";
        _decimals = 18;

        devAddress = tokenOwner;
        _devAddress = marketAddress;
        _tTotal = 21000000 * 10**_decimals;
        _rTotal = (MAX - (MAX % _tTotal));

        _rOwned[tokenOwner] = _rTotal;

        _owner = msg.sender;
        _deployer = msg.sender;

        _invitePool = address(invitePool);
        _nftNodePool = address(nftNodePool);
        _nftStationPool = address(nftStationPool);
        ISwapRouter swapRouter = ISwapRouter(RouterAddress);
        address usdt = USDTAddress;
        _usdt = usdt;
    
        _swapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;
        ISwapFactory swapFactory = ISwapFactory(swapRouter.factory());
        address usdtPair = swapFactory.createPair(address(this), usdt);
        require(IUniswapV2Pair(usdtPair).token1() == address(this), "invalid token address");
        uniswapV2Pair = usdtPair;

        _swapPairList[usdtPair] = true;
        address mainPair = swapFactory.createPair(address(this), swapRouter.WETH());
        _swapPairList[mainPair] = true;

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
        if (_iclud[account]) return _tOwned[account];
        return tokenFromReflection(_rOwned[account]);
    }
    function iEFR(address account) public view returns (bool) {
        return _iclud[account];
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

    function totalFees() public view returns (uint256) {
        return _tFeeTotal;
    }

    function tokenFromReflection(uint256 rAmount)
        public
        view
        returns (uint256)
    {
        require(
            rAmount <= _rTotal,
            "Amount must be less than total reflections"
        );
        uint256 currentRate = _getRate();
        return rAmount.div(currentRate);
    }


    function ecdFR(address account) public onlyOwner() {
        _ecdFR(account);
    }
    function _ecdFR(address account) private {
        require(!_iclud[account], "Account is already excluded");
        if(_rOwned[account] > 0) {
            _tOwned[account] = tokenFromReflection(_rOwned[account]);
        }
        _iclud[account] = true;
        _ecd.push(account);
    }

    function icdIR(address account) external onlyOwner() {
        _icdIR(account);
    }

    function _icdIR(address account) private {
        require(_iclud[account], "Account is already excluded");
        for (uint256 i = 0; i < _ecd.length; i++) {
            if (_ecd[i] == account) {
                _ecd[i] = _ecd[_ecd.length - 1];
                _tOwned[account] = 0;
                _iclud[account] = false;
                _ecd.pop();
                break;
            }
        }
    }

    //to recieve ETH from uniswapV2Router when swaping
    receive() external payable {}

    function _getRate() private view returns (uint256) {
        (uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
        return rSupply.div(tSupply);
    }

    function _getCurrentSupply() private view returns (uint256, uint256) {
        uint256 rSupply = _rTotal;
        uint256 tSupply = _tTotal;
        for (uint256 i = 0; i < _ecd.length; i++) {
            if (_rOwned[_ecd[i]] > rSupply || _tOwned[_ecd[i]] > tSupply) return (_rTotal, _tTotal);
            rSupply = rSupply.sub(_rOwned[_ecd[i]]);
            tSupply = tSupply.sub(_tOwned[_ecd[i]]);
        }
        if (rSupply < _rTotal.div(_tTotal)) return (_rTotal, _tTotal);
        return (rSupply, tSupply);
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

        if (!_swapPairList[to] && !_swapPairList[from]) {
            takeFee = false;
        }
        bool isAdd = false;
        bool isDel = false;
        if (_swapPairList[to]) {
            (isAdd,) = getLPStatus(from,to);
        }else if(_swapPairList[from]){
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

        uint256 currentRate = _getRate();
        uint256 rAmount = tAmount.mul(currentRate);
        _rOwned[sender] = _rOwned[sender].sub(rAmount);
        if(_iclud[sender]){
            _tOwned[sender] = _tOwned[sender].sub(tAmount);
        }
        uint256 rate;
        if (takeFee) {

            _takeTransfer(
                sender,
                _deadAddress,
                tAmount.div(1000).mul(_deadFee),
                currentRate
            );


            _takeTransfer(
                sender,
                _devAddress,
                tAmount.div(1000).mul(_devFee),
                currentRate
            );

            _reflectFee(rAmount.div(1000).mul(_holdFee),tAmount.div(1000).mul(_holdFee));
            // _takeInviterFee(sender, recipient, tAmount, currentRate);
            _takeTransfer(sender, _invitePool, tAmount.div(1000).mul(_inviterFee), currentRate);
            _takeTransfer(sender, _nftNodePool, tAmount.div(1000).mul(_nodeFee), currentRate);
            _takeTransfer(sender, _nftStationPool, tAmount.div(1000).mul(_baseFee), currentRate);

            uint256 nodebaseFee = 0;
            nodebaseFee = nodebaseFee + _nodeFee;
            nodebaseFee = nodebaseFee + _baseFee;
            rate =  _devFee + _deadFee + _inviterFee + nodebaseFee + _holdFee;
        }


        uint256 recipientRate = 1000 - rate;
        _rOwned[recipient] = _rOwned[recipient].add(
            rAmount.div(1000).mul(recipientRate)
        );

        if(_iclud[recipient]){
            _tOwned[recipient] = _tOwned[recipient].add(tAmount.div(1000).mul(recipientRate));
        }

        upDateBal(sender,recipient);
        emit Transfer(sender, recipient, tAmount.div(1000).mul(recipientRate));
    }
    function upDateBal(address sender,address recipient)internal{
        if (sender != _invitePool && sender != _nftNodePool && sender != _nftStationPool &&
            recipient != _invitePool &&recipient !=_nftNodePool && recipient != _nftStationPool) {
            if(balanceOf(sender)  < 1000 * 10 **_decimals ){

                if(!_iclud[sender]){
                    _ecdFR(sender);
                }
            }
            if(balanceOf(recipient)  < 1000 * 10 **_decimals ){

                if(!_iclud[recipient]){
                    _ecdFR(recipient);
                }
            }
            if(balanceOf(sender)  >= 1000 * 10 **_decimals ){
                if(_iclud[sender]){
                    _icdIR(sender);
                }
            }
            if(balanceOf(recipient)  >= 1000 * 10 **_decimals ){
                if(_iclud[recipient]){
                    _icdIR(recipient);
                }
            }
        }
        
    }

    function getLPStatus(address from,address to) internal view  returns (bool isAdd,bool isDel){
        IUniswapV2Pair pair;
        address token = address(this);
        if(_swapPairList[to]){
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
        if (_swapPairList[to]) {
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
        uint256 tAmount,
        uint256 currentRate
    ) private {

        uint256 rAmount = tAmount.mul(currentRate);

        _rOwned[to] = _rOwned[to].add(rAmount);
        emit Transfer(sender, to, tAmount);
    }

    function _reflectFee(uint256 rFee, uint256 tFee) private {
        _rTotal = _rTotal.sub(rFee);
        _tFeeTotal = _tFeeTotal.add(tFee);
    }
}

contract CHATGPTAI is Token {

    constructor() Token(
        address(0x6b597ec327c6acfD0768a5800389fDFb2D13164E),//owner
        address(0x05CBeF6B1E877684D0e2BA3dab7f977D10C2551c) ,// market
         address(0xDb4Bf56437c975AB1c17de093B5ff9bc04920939) ,// invitePool
         address(0x6a88Cd3db816B68E41EA6Cc620061d65a01d9CFC) ,// nftNodePool
         address(0x866a09ddCe900b2e95b5D8eAda30515B329794F2) ,// nftStationPool
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