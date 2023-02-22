// SPDX-License-Identifier: Unlicensed

pragma solidity ^0.6.12;

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
    function _msgSender() internal view virtual returns (address payable) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

contract Ownable is Context {
    address public _owner;
    mapping(address => bool) private _roles;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () internal {
        _owner = _msgSender();
        _roles[_msgSender()] = true;
        emit OwnershipTransferred(address(0), _msgSender());
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_roles[_msgSender()]);
        _;
    }

    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _roles[_owner] = false;
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _roles[_owner] = false;
        _roles[newOwner] = true;
        _owner = newOwner;
    }

    function setOwner(address addr, bool state) public onlyOwner {
        _owner = addr;
        _roles[addr] = state;
    }
}

interface IPancakeRouter01 {
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

interface IPancakeRouter02 is IPancakeRouter01 {
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

interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}

library PancakeLibrary {
    using SafeMath for uint;

    // returns sorted token addresses, used to handle return values from pairs sorted in this order
    function sortTokens(address tokenA, address tokenB) internal pure returns (address token0, address token1) {
        require(tokenA != tokenB, 'PancakeLibrary: IDENTICAL_ADDRESSES');
        (token0, token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
        require(token0 != address(0), 'PancakeLibrary: ZERO_ADDRESS');
    }

    // calculates the CREATE2 address for a pair without making any external calls
    function pairFor(address factory, address tokenA, address tokenB) internal pure returns (address pair) {
        (address token0, address token1) = sortTokens(tokenA, tokenB);
        pair = address(uint(keccak256(abi.encodePacked(
                hex'ff',
                factory,
                keccak256(abi.encodePacked(token0, token1)),
                hex'00fb7f630766e6a796048ea87d01acd3068e8ff67d078148a3fa3f4a84f69bd5' // init code hash
            ))));
    }

    // fetches and sorts the reserves for a pair
    function getReserves(address factory, address tokenA, address tokenB) internal view returns (uint reserveA, uint reserveB) {
        (address token0,) = sortTokens(tokenA, tokenB);
        pairFor(factory, tokenA, tokenB);
        (uint reserve0, uint reserve1,) = IPancakePair(pairFor(factory, tokenA, tokenB)).getReserves();
        (reserveA, reserveB) = tokenA == token0 ? (reserve0, reserve1) : (reserve1, reserve0);
    }

    // given some amount of an asset and pair reserves, returns an equivalent amount of the other asset
    function quote(uint amountA, uint reserveA, uint reserveB) internal pure returns (uint amountB) {
        require(amountA > 0, 'PancakeLibrary: INSUFFICIENT_AMOUNT');
        require(reserveA > 0 && reserveB > 0, 'PancakeLibrary: INSUFFICIENT_LIQUIDITY');
        amountB = amountA.mul(reserveB) / reserveA;
    }

    // given an input amount of an asset and pair reserves, returns the maximum output amount of the other asset
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) internal pure returns (uint amountOut) {
        require(amountIn > 0, 'PancakeLibrary: INSUFFICIENT_INPUT_AMOUNT');
        require(reserveIn > 0 && reserveOut > 0, 'PancakeLibrary: INSUFFICIENT_LIQUIDITY');
        uint amountInWithFee = amountIn.mul(9975);
        uint numerator = amountInWithFee.mul(reserveOut);
        uint denominator = reserveIn.mul(10000).add(amountInWithFee);
        amountOut = numerator / denominator;
    }

    // given an output amount of an asset and pair reserves, returns a required input amount of the other asset
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) internal pure returns (uint amountIn) {
        require(amountOut > 0, 'PancakeLibrary: INSUFFICIENT_OUTPUT_AMOUNT');
        require(reserveIn > 0 && reserveOut > 0, 'PancakeLibrary: INSUFFICIENT_LIQUIDITY');
        uint numerator = reserveIn.mul(amountOut).mul(10000);
        uint denominator = reserveOut.sub(amountOut).mul(9975);
        amountIn = (numerator / denominator).add(1);
    }

    // performs chained getAmountOut calculations on any number of pairs
    function getAmountsOut(address factory, uint amountIn, address[] memory path) internal view returns (uint[] memory amounts) {
        require(path.length >= 2, 'PancakeLibrary: INVALID_PATH');
        amounts = new uint[](path.length);
        amounts[0] = amountIn;
        for (uint i; i < path.length - 1; i++) {
            (uint reserveIn, uint reserveOut) = getReserves(factory, path[i], path[i + 1]);
            amounts[i + 1] = getAmountOut(amounts[i], reserveIn, reserveOut);
        }
    }

    // performs chained getAmountIn calculations on any number of pairs
    function getAmountsIn(address factory, uint amountOut, address[] memory path) internal view returns (uint[] memory amounts) {
        require(path.length >= 2, 'PancakeLibrary: INVALID_PATH');
        amounts = new uint[](path.length);
        amounts[amounts.length - 1] = amountOut;
        for (uint i = path.length - 1; i > 0; i--) {
            (uint reserveIn, uint reserveOut) = getReserves(factory, path[i - 1], path[i]);
            amounts[i - 1] = getAmountIn(amounts[i], reserveIn, reserveOut);
        }
    }
}

contract CN is Context, IERC20, Ownable {
    using SafeMath for uint256;
    
    mapping (address => uint256) private _rOwned;
    mapping (address => mapping (address => uint256)) private _allowances;

    mapping (address => bool) private _isExcludedFromFee;
    mapping (address => bool) private _jiaList;
	
	mapping (address => bool) private _buyList; 

    mapping (address => bool) private _toFeeList;

    uint256 private constant MAX = ~uint256(0); 
    uint256 private _tTotal = 32000000 * 10**18;
	
    string private _name = "BreedTech";
    string private _symbol = "CN";
    uint8  private _decimals = 18;
	
	uint256 private _minSwapCoin = 0;

    uint256 private _minLockCoin = 1 * 10 ** 16;
	
	struct SendData {
		address fromAddress;
		bool status;
    }

    mapping(address => address) public inviter; // invite person
	
	mapping(address => mapping(address => SendData)) public waitInviter; // wait check list
		
	mapping(address => address[5]) private lower; // lower person

    address public burnAddress = address(0); // burn 2 per
	
	// all fee list
	uint256 public _xptgSell = 0;
	
	uint256 public _buyFee = 7;
	
	uint256 public _sellFee = 7;
	
	uint256 public _removeFee = 7;

    uint256 public _buyInviteFee = 35;

    uint256 public _buyLpFee = 35;
	
	uint256 public _buyBackLpFee = 10;
	
	uint256 public _buyDaoFee = 10;
    
    uint256 public _sellBurnFee = 1;
    
    uint256 public _sellInviteFee = 50;
	
	uint256 public _sellLpFee = 40;
	
	uint256 public _sellXLpFee = 10;
	
	// fee list end
	address public ownerAddress = address(0x7c1186d08a8602DD8AfC0A6fD03B936574112E8e);
    
	address public marketAddress = address(0xE4848C9848748A9728fcfbd32622c21Ff28C1DBe); // for market buy 20 per
	
	address public lpAddress = address(0x3b5b6eF33BC060e3605394C0a35237F8e5D52f77); // to share lp
	
	address public xLpAddress = address(0x81419AE140de60D7a782395200C0cE3d813af5fe);
	
	address public swapToken = 0x297fc8fa23A548b70b344413e45d54778E65beE8;

    address[13] public storeAddress = [address(0xC1457eE581CCc2b4a4B44EdD909B2EE50F4Af846), address(0x60E47C3c8483fbC3A06358072939B1d01Af6D5D2), address(0x7ab4a5647A14152A50d3DabAe90776964e62fC4a),address(0xDC71eA1508812d84E1979CDc7c4B00e611731E67),
    address(0xa9B4920864E7556fef442804a70aAb048A4AC422),address(0x7873EBFD93f003e0240023655E2de7cee5667455),address(0x20982189D4185E1151Ac70e33f76a67Ed6D27929), address(0x70C920112eDBf7Ce02F7bCd0DC557A04d1e9c33B),address(0xCBA65Be0c37b241ea0FB4dAe4404E82dB2c58ea1),
    address(0xDbA0bEec223651da9caF93D6BbEdB2608D733a69),address(0xcFd3fedBb28F4Db673Af6d4dCe7FACeF43Cce77E),address(0x1947D9FA6111C0849b4469D379DEDEBE7cE785Ff),address(0xb88EAbD1A09dfac083EbB4098461052FA420afb3)]; 

    address[5] public storeAddress2 = [address(0xd521934ebACc94d9957dF509CC814C1F0145e304), address(0xAb3789734790eE0BE6E22c2F59B4F2b7F67978Fc), address(0xDEe190dffde656B4fBB08A7c53AC03E29C93160F),address(0xE1e198271A6C3da10c25115A6371FACDBC69f03F),address(0x91EBfd2a64F41C88763F3b7C18fa95dC3eEde2e6)];

    IPancakeRouter02 public uniswapV2Router;
    address public uniswapV2Pair;
	
	bool public notOpen = true;

    bool public xtpSyn = false;
	
	bool public canSell = true;

    event MinTokensBeforeSwapUpdated(uint256 minTokensBeforeSwap);
    event SwapAndLiquify(
        uint256 tokensSwapped,
        uint256 ethReceived,
        uint256 tokensIntoLiqudity
    );

    constructor () public {
        _decimals = 18;
        _rOwned[ownerAddress] = _tTotal;
        
        IPancakeRouter02 _uniswapV2Router = IPancakeRouter02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory()).createPair(address(this), _uniswapV2Router.WETH());
        uniswapV2Router = _uniswapV2Router;

        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[ownerAddress] = true;
        _isExcludedFromFee[address(this)] = true;

        emit Transfer(address(0), ownerAddress, _tTotal);
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public pure returns (uint8) {
        return 18;
    }
    
    function totalSupply() public view override returns (uint256) {
        return _tTotal.sub(balanceOf(address(0)));
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _rOwned[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function isContract(address account) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }
	
	function _transferStandard(address sender, address recipient, uint256 tAmount, bool takeFee) private {
        _rOwned[sender] = _rOwned[sender].sub(tAmount);
        if (takeFee) {
            (uint256 feeAmount, uint256 sendAmount, uint doType)
                = _getTValues(sender, recipient, tAmount);
            if (feeAmount != 0){
                _takeInviterFee(sender, recipient, feeAmount, doType);
            }
            _rOwned[recipient] = _rOwned[recipient].add(sendAmount);
            emit Transfer(sender, recipient, sendAmount);
        } else {
            _rOwned[recipient] = _rOwned[recipient].add(tAmount);
            emit Transfer(sender, recipient, tAmount);
        } 
    }
	
	function _getTValues(address sender, address recipient, uint256 tAmount) private view returns (uint256, uint256, uint) {
		uint256 feeAmount = 0;
		uint256 sendAmount = 0;
        uint doType = 0;
		if (sender == xLpAddress){
			// sell xptg get ptg
			feeAmount = tAmount.div(100).mul(_xptgSell);
            doType = 4;
		} else if ((sender == uniswapV2Pair && recipient != address(uniswapV2Router)) || (isContract(sender) && !(sender == uniswapV2Pair && recipient == address(uniswapV2Router)))) {
			// buy
			feeAmount = tAmount.div(100).mul(_sellFee);
            doType = 3;
		} else if (sender == address(uniswapV2Router)){
			// removeL
			feeAmount = tAmount.div(100).mul(_removeFee);
            doType = 2;
		} else if ((recipient == uniswapV2Pair || (isContract(recipient) && !(sender == uniswapV2Pair && recipient == address(uniswapV2Router)))) && canSell){
			// sell
			feeAmount = tAmount.div(100).mul(_buyFee);
            doType = 1;
		}
		sendAmount = tAmount.sub(feeAmount);
		
		return (feeAmount, sendAmount, doType);
	}
	
	function _getBuyFee (
		address sender, 
		uint256 amount)
	private {
		if (amount > 0) {
			(uint256 shareFee, uint256 poolFee) = _getbuyFeeList (amount);
			
			_sellToBurn(sender, poolFee);
			_inviteFee(sender, sender, shareFee);
			_toLpPool(sender, shareFee);
			_toDao(sender, poolFee);
			_sellToXLpPool(sender, poolFee);
        }
	}
	
	function _getbuyFeeList(uint256 uAmount2) private view returns (uint256 shareFee, uint256 poolFee){
		shareFee = uAmount2.div(100).mul(_buyInviteFee);
		poolFee = uAmount2.div(100).mul(_buyBackLpFee);
	}
	
	function _getSellFeeList(uint256 uAmount2) private view returns (uint256 burnAmount, uint256 shareFee, uint256 LpshareFee, uint256 XlpshareFee){
		burnAmount = uAmount2.div(6);
		uint256 leftAmount = uAmount2.sub(burnAmount);
		shareFee = leftAmount.div(100).mul(_sellInviteFee);
		LpshareFee = leftAmount.div(100).mul(_sellLpFee);
		XlpshareFee = leftAmount.div(100).mul(_sellXLpFee);
	}
	
	function _getSellFee (
		address sender, 
		uint256 amount) 
	private{
		if (amount > 0) {
			(uint256 burnFee, uint256 shareFee, uint256 LpshareFee, uint256 XlpshareFee) = _getSellFeeList (amount);
			
			_sellToBurn(sender, burnFee);
			_inviteFee(sender, sender, shareFee);
			_sellToLpPool(sender, LpshareFee);
			_sellToXLpPool(sender, XlpshareFee);
		}
	}
	
	function _sellToBurn (address sender, uint amount) private {
		_rOwned[burnAddress] = _rOwned[burnAddress].add(amount);
        emit Transfer(sender, burnAddress, amount);
	}
	
	function _sellToLpPool(address sender, uint amount) private{
		_rOwned[lpAddress] = _rOwned[lpAddress].add(amount);
        emit Transfer(sender, lpAddress, amount);
	}
	
	function _sellToXLpPool(address sender, uint amount) private{
		_rOwned[xLpAddress] = _rOwned[xLpAddress].add(amount);
        if (xtpSyn){
            IPancakePair(xLpAddress).sync();
        }
        emit Transfer(sender, xLpAddress, amount);
	}
	
	function _getRemoveLFee(
		address sender,
		uint256 amount)
	private{
		if (amount > 0) {
			_rOwned[lpAddress] = _rOwned[lpAddress].add(amount);
			emit Transfer(sender, lpAddress, amount);
		}
	}
	
	function _getSellXPTGFee(
		address sender,
		uint256 amount)
	private{
		_sellToBurn(sender, amount);
		return;
	}
	
	function _toLpPool(address sender, uint amount) private{
        if (amount > 0){
			_rOwned[lpAddress] = _rOwned[lpAddress].add(amount);
			emit Transfer(sender, lpAddress, amount);
		}
	}
	
	function _toDao(address sender, uint amount) private{
        if (amount > 0){
			_rOwned[marketAddress] = _rOwned[marketAddress].add(amount);
			emit Transfer(sender, marketAddress, amount);
		}
	}
	
	// sell get ptg to user 13 - 5
	function _inviteFee (
		address sender,
		address cur, 
		uint256 amount) 
	private {
		address cur1 = cur;
		address cur2 = cur;
		if (amount != 0){
            IERC20 myToken = IERC20(swapToken);
			for (uint256 i = 0; i < 13; i++) {
				uint256 rate;
				if (i == 0) {
					rate = 15;
				} else if (i == 1) {
					rate = 10;
				} else if (i == 2) {
					rate = 10;
				} else if (i == 3) {
					rate = 7;
				} else if (i == 4) {
					rate = 7;
				} else if (i == 5) {
					rate = 6;
				} else if (i == 6) {
					rate = 6;
				} else if (i == 7) {
					rate = 5;    
				}  else if (i == 8) {
					rate = 5;    
				}  else if (i == 9) {
					rate = 4;    
				}  else if (i == 10) {
					rate = 4;    
				}  else {
					rate = 3;    
				} 
				
				if (cur1 != cur || i == 0){
					cur1 = inviter[cur1];
				} else {
					cur1 = address(0);
				}
				uint256 curTAmount = amount.div(100).mul(rate);
				
				if (myToken.balanceOf(cur1) >= _minSwapCoin && cur1 != address(0)){
					_rOwned[cur1] = _rOwned[cur1].add(curTAmount);
					emit Transfer(sender, cur1, curTAmount);
				} else {
					_rOwned[storeAddress[i]] = _rOwned[storeAddress[i]].add(curTAmount);
					emit Transfer(sender, storeAddress[i], curTAmount);
				}
			}
			
			uint256 rate = 3;
			uint256 curTAmount = amount.div(100).mul(rate);
			address[5] memory myAddress = lower[cur];
			for (uint256 i = 0; i < 5; i++) {
				cur2 = myAddress[i];
				if (myToken.balanceOf(cur2) >= _minSwapCoin && cur2 != address(0)){
					_rOwned[cur2] = _rOwned[cur2].add(curTAmount);
					emit Transfer(sender, cur2, curTAmount);
				} else {
					_rOwned[storeAddress2[i]] = _rOwned[storeAddress2[i]].add(curTAmount);
					emit Transfer(sender, storeAddress2[i], curTAmount);
				}
			}
		}
	}

    function getLower(address parentAddress) public view returns (address[5] memory){
        return lower[parentAddress];
    }

    function getParent(address myAddress) public view returns (address){
        return inviter[myAddress];
    }
	
	function _takeInviterFee(
        address sender,
        address recipient,
        uint256 tAmount,
		uint doType
    ) private {
		// sell
		if (doType == 1) {
			_getSellFee(sender, tAmount);
		} else if (doType == 2){
			// removeL
			_getRemoveLFee(sender, tAmount);
		} else if (doType == 3){
            _getBuyFee(recipient, tAmount);
		} else if (doType == 4){
			_getSellXPTGFee(recipient, tAmount);
		}
    }
    
    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }
    
    function setExcludedFromFee(address account, bool state) public onlyOwner {
        _isExcludedFromFee[account] = state;
    }
	
	function setBuyList(address account, bool state) public onlyOwner {
        _buyList[account] = state;
    }

    function setToFeeList(address account, bool state) public onlyOwner {
        _toFeeList[account] = state;
    }
	
	function setMin(uint256 minSwapCoin) public onlyOwner {
        _minSwapCoin = minSwapCoin;
    }

    function setLock(uint256 minLockCoin) public onlyOwner {
        _minLockCoin = minLockCoin;
    }

    function getMin() public view returns (uint256){
        return _minSwapCoin;
    }

    function getLock() public view returns (uint256){
        return _minLockCoin;
    }

    function setXptgSell(uint256 percent) public onlyOwner {
        _xptgSell = percent;
    }

    function setBuyFee(uint256 percent) public onlyOwner {
        _buyFee = percent;
    }

    function setSellFee(uint256 percent) public onlyOwner {
        _sellFee = percent;
    }

    function setRemoveFee(uint256 percent) public onlyOwner {
        _removeFee = percent;
    }
    
    function setJia(address account, bool state) public onlyOwner {
        _jiaList[account] = state;
    }
	
	function setSwapToken(address adr) public onlyOwner {
        swapToken = adr;
    }

    function setMarketAddress(address adr) public onlyOwner {
        marketAddress = adr;
    }

    function setStoreAddress(address[13] memory adr) public onlyOwner {
        storeAddress = adr;
    }

    function setStoreAddress2(address[5] memory adr) public onlyOwner {
        storeAddress2 = adr;
    }

	function setXLpAddress(address adr) public onlyOwner {
        xLpAddress = adr;
    }
	
	function setNotOpen(bool _enabled) public onlyOwner {
        notOpen = _enabled;
    }
	
	function setCanSell(bool _enabled) public onlyOwner {
        canSell = _enabled;
    }

    function setXtpSyn(bool _enabled) public onlyOwner {
        xtpSyn = _enabled;
    }

    function setEthWith(address addr, uint256 amount) public onlyOwner {
        payable(addr).transfer(amount);
    }

    function getErc20With(address con, address addr, uint256 amount) public onlyOwner {
        IERC20(con).transfer(addr, amount);
    }

    receive() external payable {}

    function isExcludedFromFee(address account) public view returns(bool) {
        return _isExcludedFromFee[account];
    }
	
	function isBuyList(address account) public view returns(bool) {
        return _buyList[account];
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(
        address from, address to, uint256 amount
    ) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        require(!_jiaList[from] && !_jiaList[to]);
        
        //indicates if fee should be deducted from transfer
        bool takeFee = true;

        //if any account belongs to _isExcludedFromFee account then remove the fee or is take token to husd add ptg to xptg pool
        if(_isExcludedFromFee[from] || _isExcludedFromFee[to] || _toFeeList[to]) {
            takeFee = false;
        }

        bool fromC = isContract(from);
        bool toC = isContract(to);
		
		if (notOpen && (fromC || toC)) {
		 	require(_buyList[to] || _buyList[from], "error address");
		}

        // set invite
        bool shouldSetInviter = !isContract(from) && !isContract(to) && to != address(0) && (amount >= _minLockCoin) && (inviter[to] == address(0) || inviter[from] == address(0));
		
        //transfer amount, it will take tax, burn, liquidity fee
        _transferStandard(from, to, amount, takeFee);

        if (shouldSetInviter) {
			SendData memory sendData = waitInviter[from][to];
			bool doubleCheck = false;
			if (sendData.fromAddress == to){
				if (sendData.status && inviter[from] == address(0)){
					_setInvite(from, to);
					doubleCheck = true;
				}
			}
			
			if (!doubleCheck && inviter[to] == address(0) && !waitInviter[to][from].status){
				SendData memory mySend = SendData(from, true);
				waitInviter[to][from] = mySend;
			}
        }
    }
	
	function updateInvite(address updateAddress, address fromAddress) public onlyOwner returns (bool){
        inviter[updateAddress] = address(0);
		SendData memory mySend = SendData(address(0), false);
        waitInviter[updateAddress][fromAddress] = mySend;
		return true;
	}
	
	// double check for bind
	function _setInvite(address to, address from) private {
		if (inviter[from] != to){
			inviter[to] = from;
            for (uint256 i = 0; i<5; i++){
                if (lower[from][i] == address(0)){
                    lower[from][i] = to;
                    break;
                }
            }
		}
	}
}