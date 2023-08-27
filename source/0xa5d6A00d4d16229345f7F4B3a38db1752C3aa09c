// SPDX-License-Identifier: MIT

pragma solidity ^0.8.1;

library Address {
    function isContract(address account) internal view returns (bool) {
        // This method relies on extcodesize/address.code.length, which returns 0
        // for contracts in construction, since the code is only stored at the end
        // of the constructor execution.

        return account.code.length > 0;
    }
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{value: amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }
    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, "Address: low-level call failed");
    }
    function functionCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, errorMessage);
    }
    function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
    }
    function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
        return functionStaticCall(target, data, "Address: low-level static call failed");
    }
    function functionStaticCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        (bool success, bytes memory returndata) = target.staticcall(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
    }
    function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionDelegateCall(target, data, "Address: low-level delegate call failed");
    }
    function functionDelegateCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
    }
    function verifyCallResultFromTarget(
        address target,
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        if (success) {
            if (returndata.length == 0) {
                // only check isContract if the call was successful and the return data is empty
                // otherwise we already know that it was a contract
                require(isContract(target), "Address: call to non-contract");
            }
            return returndata;
        } else {
            _revert(returndata, errorMessage);
        }
    }
    function verifyCallResult(
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal pure returns (bytes memory) {
        if (success) {
            return returndata;
        } else {
            _revert(returndata, errorMessage);
        }
    }
    function _revert(bytes memory returndata, string memory errorMessage) private pure {
        // Look for revert reason and bubble it up if present
        if (returndata.length > 0) {
            // The easiest way to bubble the revert reason is using memory via assembly
            /// @solidity memory-safe-assembly
            assembly {
                let returndata_size := mload(returndata)
                revert(add(32, returndata), returndata_size)
            }
        } else {
            revert(errorMessage);
        }
    }
}

pragma solidity ^0.8.0;

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    constructor() {
        _transferOwnership(_msgSender());
    }
    modifier onlyOwner() {
        _checkOwner();
        _;
    }
    function owner() public view virtual returns (address) {
        return _owner;
    }
    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface IERC20Permit {
    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;
    function nonces(address owner) external view returns (uint256);
    function DOMAIN_SEPARATOR() external view returns (bytes32);
}

library SafeERC20 {
    using Address for address;
    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }
    function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }
    function safeApprove(IERC20 token, address spender, uint256 value) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        require(
            (value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }
    function safeIncreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 oldAllowance = token.allowance(address(this), spender);
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, oldAllowance + value));
    }
    function safeDecreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        unchecked {
            uint256 oldAllowance = token.allowance(address(this), spender);
            require(oldAllowance >= value, "SafeERC20: decreased allowance below zero");
            _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, oldAllowance - value));
        }
    }
    function forceApprove(IERC20 token, address spender, uint256 value) internal {
        bytes memory approvalCall = abi.encodeWithSelector(token.approve.selector, spender, value);

        if (!_callOptionalReturnBool(token, approvalCall)) {
            _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, 0));
            _callOptionalReturn(token, approvalCall);
        }
    }
    function safePermit(
        IERC20Permit token,
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) internal {
        uint256 nonceBefore = token.nonces(owner);
        token.permit(owner, spender, value, deadline, v, r, s);
        uint256 nonceAfter = token.nonces(owner);
        require(nonceAfter == nonceBefore + 1, "SafeERC20: permit did not succeed");
    }
    function _callOptionalReturn(IERC20 token, bytes memory data) private {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves. We use {Address-functionCall} to perform this call, which verifies that
        // the target address contains contract code and also asserts for success in the low-level call.

        bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
        require(returndata.length == 0 || abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
    }

    function _callOptionalReturnBool(IERC20 token, bytes memory data) private returns (bool) {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves. We cannot use {Address-functionCall} here since this should return false
        // and not revert is the subcall reverts.

        (bool success, bytes memory returndata) = address(token).call(data);
        return
            success && (returndata.length == 0 || abi.decode(returndata, (bool))) && Address.isContract(address(token));
    }
}

pragma solidity >=0.5.0;

interface IUniswapV2Factory {
    event PairCreated(
        address indexed token0,
        address indexed token1,
        address pair,
        uint256
    );

    function feeTo() external view returns (address);

    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB)
        external
        view
        returns (address pair);

    function allPairs(uint256) external view returns (address pair);

    function allPairsLength() external view returns (uint256);

    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);

    function setFeeTo(address) external;

    function setFeeToSetter(address) external;
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

pragma solidity >=0.6.2;

interface IUniswapV2Router01 {
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

    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountA, uint256 amountB);

    function removeLiquidityETHWithPermit(
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
    ) external returns (uint256 amountToken, uint256 amountETH);

    function swapExactTokensForTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapTokensForExactTokens(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapExactETHForTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable returns (uint256[] memory amounts);

    function swapTokensForExactETH(
        uint256 amountOut,
        uint256 amountInMax,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapExactTokensForETH(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapETHForExactTokens(
        uint256 amountOut,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable returns (uint256[] memory amounts);

    function quote(
        uint256 amountA,
        uint256 reserveA,
        uint256 reserveB
    ) external pure returns (uint256 amountB);

    function getAmountOut(
        uint256 amountIn,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountOut);

    function getAmountIn(
        uint256 amountOut,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountIn);

    function getAmountsOut(uint256 amountIn, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);

    function getAmountsIn(uint256 amountOut, address[] calldata path)
        external
        view
        returns (uint256[] memory amounts);
}
interface IUniswapV2Router02 is IUniswapV2Router01 {
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
pragma solidity 0.8.18;

interface IUniswapV2Caller {
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        address router,
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        uint256 deadline
    ) external;
}

contract LORDZ is IERC20, Ownable {
    using SafeERC20 for IERC20;
    struct Args {
        string name;
        string symbol;
        uint8 decimals;
        uint256 totalSupply;
        uint256 maxWallet;
        uint256 maxTransactionAmount;
        address marketingWallet;
        address mainRouter;
        address baseTokenForMarket; 
        address treasuryWallet;       
        bool isMarketingFeeBaseToken;
        uint24 sellLiquidityFee;
        uint24 buyLiquidityFee;
        uint24 sellMarketingFee;
        uint24 buyMarketingFee;
        uint24 sellTreasuryFee;
        uint24 buyTreasuryFee;
        uint24 sellRewardFee;
        uint24 buyRewardFee;
        address uniswapV2Caller;
    }
    IUniswapV2Caller public uniswapV2Caller;
    address public baseTokenForMarket;
    uint8 private _decimals;
    mapping(address => uint256) private _rOwned;
    mapping(address => uint256) private _tOwned;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _isExcluded;
    address[] private _excluded;
    uint256 private constant MAX = ~uint256(0);
    uint256 private _tTotal;
    uint256 private _rTotal;
    uint256 private _tRwardFeeTotal;
    string private _name;
    string private _symbol;

    uint256 private _rewardFee;
    uint256 private _previousRewardFee;

    uint256 private _liquidityFee;
    uint256 private _previousLiquidityFee;

    uint256 private _marketingFee;
    uint256 private _previousMarketingFee;

    uint256 private _treasuryFee;
    uint256 private _previousTreasuryFee;

    bool private inSwapAndLiquify;
    uint24 public sellRewardFee;
    uint24 public buyRewardFee;
    uint24 public sellLiquidityFee;
    uint24 public buyLiquidityFee;

    uint24 public sellMarketingFee;
    uint24 public buyMarketingFee;

    uint24 public sellTreasuryFee;
    uint24 public buyTreasuryFee;

    address public marketingWallet;
    bool public isMarketingFeeBaseToken;
    address public treasuryWallet;

    uint256 public minAmountToTakeFee;
    uint256 public maxWallet;
    uint256 public maxTransactionAmount;


    IUniswapV2Router02 public mainRouter;
    address public mainPair;

    mapping(address => bool) public isExcludedFromFee;
    mapping(address => bool) public isExcludedFromMaxTransactionAmount;
    mapping(address => bool) public automatedMarketMakerPairs;

    uint256 private _liquidityFeeTokens;
    uint256 private _marketingFeeTokens;
    uint256 private _treasuryFeeTokens;

    event UpdateLiquidityFee(
        uint24 newSellLiquidityFee,
        uint24 newBuyLiquidityFee,
        uint24 oldSellLiquidityFee,
        uint24 oldBuyLiquidityFee
    );
    event UpdateMarketingFee(
        uint24 newSellMarketingFee,
        uint24 newBuyMarketingFee,
        uint24 oldSellMarketingFee,
        uint24 oldBuyMarketingFee
    );
    event UpdateTreasuryFee(
        uint24 newSellTreasuryFee,
        uint24 newBuyTreasuryFee,
        uint24 oldSellTreasuryFee,
        uint24 oldBuyTreasuryFee
    );
    event UpdateRewardFee(
        uint24 newSellRewardFee,
        uint24 newBuyRewardFee,
        uint24 oldSellRewardFee,
        uint24 oldBuyRewardFee
    );  
    event UpdateMarketingWallet(
        address indexed newMarketingWallet,
        bool newIsMarketingFeeBaseToken,
        address indexed oldMarketingWallet,
        bool oldIsMarketingFeeBaseToken
    );
    event UpdateTreasuryWallet(
        address indexed newTreasuryWallet,
        address indexed oldTreasuryWallet
    );
    event MainRouterUpdated(address mainRouter, address mainPair, address baseTokenForMarket);

    event UpdateMinAmountToTakeFee(uint256 newMinAmountToTakeFee, uint256 oldMinAmountToTakeFee);
    event SetAutomatedMarketMakerPair(address indexed pair, bool value);
    event ExcludedFromFee(address indexed account, bool isEx);
    event SwapAndLiquify(uint256 tokensForLiquidity, uint256 baseTokenForLiquidity);
    event MarketingFeeTaken(
        uint256 marketingFeeTokens,
        uint256 marketingFeeBaseTokenSwapped
    );
    event TreasuryFeeTaken(
        uint256 amount
    );
    event ExcludedFromMaxTransactionAmount(address indexed account, bool isExcluded);
    event UpdateMaxWallet(uint256 newMaxWallet, uint256 oldMaxWallet);
    event UpdateMaxTransactionAmount(uint256 newMaxTransactionAmount, uint256 oldMaxTransactionAmount);
    constructor(
        Args memory args
    ) {
        uniswapV2Caller = IUniswapV2Caller(args.uniswapV2Caller);
        baseTokenForMarket=args.baseTokenForMarket;
        _decimals = args.decimals;
        _name = args.name;
        _symbol = args.symbol;
        _tTotal = args.totalSupply;
        _rTotal = (MAX - (MAX % _tTotal));
        _rOwned[_msgSender()] = _rTotal;
        marketingWallet = args.marketingWallet;
        treasuryWallet = args.treasuryWallet;
        isMarketingFeeBaseToken = args.isMarketingFeeBaseToken;
        emit UpdateMarketingWallet(
            marketingWallet,
            isMarketingFeeBaseToken,
            address(0),
            false
        );
        emit UpdateTreasuryWallet(
            treasuryWallet,
            address(0)
        );
        mainRouter = IUniswapV2Router02(args.mainRouter);
        if(baseTokenForMarket != mainRouter.WETH()){            
            IERC20(baseTokenForMarket).approve(address(mainRouter), MAX);            
        }
        _approve(address(this), address(uniswapV2Caller), MAX);
        _approve(address(this), address(mainRouter), MAX);

        mainPair = IUniswapV2Factory(mainRouter.factory()).createPair(
            address(this),
            baseTokenForMarket
        );
        emit MainRouterUpdated(address(mainRouter), mainPair, baseTokenForMarket);
      
        sellLiquidityFee = args.sellLiquidityFee;
        buyLiquidityFee = args.buyLiquidityFee;
        emit UpdateLiquidityFee(
            sellLiquidityFee,
            buyLiquidityFee,
            0,
            0
        );
        sellMarketingFee = args.sellMarketingFee;
        buyMarketingFee = args.buyMarketingFee;
        emit UpdateMarketingFee(
            sellMarketingFee,
            buyMarketingFee,
            0,
            0
        );
        sellTreasuryFee = args.sellTreasuryFee;
        buyTreasuryFee = args.buyTreasuryFee;
        emit UpdateTreasuryFee(
            sellTreasuryFee,
            buyTreasuryFee,
            0,
            0
        );
        sellRewardFee = args.sellRewardFee;
        buyRewardFee = args.buyRewardFee;
        emit UpdateRewardFee(
            sellRewardFee,
            buyRewardFee,
            0,
            0
        );
        minAmountToTakeFee = args.totalSupply/10000;
        emit UpdateMinAmountToTakeFee(minAmountToTakeFee, 0);
        require(args.maxTransactionAmount>=args.totalSupply / 10000, "maxTransactionAmount >= total supply / 10000");
        require(args.maxWallet>=args.totalSupply / 10000, "maxWallet >= total supply / 10000");
        maxWallet=args.maxWallet;
        emit UpdateMaxWallet(maxWallet, 0);
        maxTransactionAmount=args.maxTransactionAmount;
        emit UpdateMaxTransactionAmount(maxTransactionAmount, 0);
        _isExcluded[address(0xdead)] = true;
        _excluded.push(address(0xdead));
        _isExcluded[address(this)] = true;
        _excluded.push(address(this));
        _isExcluded[treasuryWallet] = true;
        _excluded.push(treasuryWallet);

        isExcludedFromFee[address(this)] = true;
        isExcludedFromFee[marketingWallet] = true;
        isExcludedFromFee[treasuryWallet] = true;
        isExcludedFromFee[_msgSender()] = true;
        isExcludedFromFee[address(0xdead)] = true;
        isExcludedFromMaxTransactionAmount[address(0xdead)]=true;
        isExcludedFromMaxTransactionAmount[address(this)]=true;
        isExcludedFromMaxTransactionAmount[marketingWallet]=true;
        isExcludedFromMaxTransactionAmount[treasuryWallet]=true;
        isExcludedFromMaxTransactionAmount[_msgSender()]=true;
        _setAutomatedMarketMakerPair(mainPair, true);
        emit Transfer(address(0), _msgSender(), args.totalSupply);
    }

    function updateMainPair(
        address _mainRouter,
        address _baseTokenForMarket
    ) external onlyOwner
    {
        baseTokenForMarket = _baseTokenForMarket;
        if(address(mainRouter) != _mainRouter){
            _approve(address(this), _mainRouter, MAX);
            mainRouter = IUniswapV2Router02(_mainRouter);
        } 
        mainPair = IUniswapV2Factory(mainRouter.factory()).createPair(
            address(this),
            baseTokenForMarket
        );
        if(baseTokenForMarket != mainRouter.WETH()){            
            IERC20(baseTokenForMarket).safeApprove(address(mainRouter), MAX);
        }
        emit MainRouterUpdated(address(mainRouter), mainPair, baseTokenForMarket);
        _setAutomatedMarketMakerPair(mainPair, true);

    }
 
    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 amount
    ) private {
        (           
            // uint256 tTransferAmount,
            uint256 tRwardFee,
            uint256 tLiquidity,
            uint256 tMarketing,
            uint256 tTreasury
        ) = _getTValues(amount);
        uint256 tTransferAmount = amount - tRwardFee - tLiquidity - tMarketing - tTreasury;
        uint256 currentRate = _getRate();       
        {
            //marketing & liquidity & treasury fee collected in the token contract
            _liquidityFeeTokens = _liquidityFeeTokens+tLiquidity;
            _marketingFeeTokens = _marketingFeeTokens+tMarketing;
            _treasuryFeeTokens = _treasuryFeeTokens+tTreasury;
            uint256 tTotalFee = tLiquidity+tMarketing+tTreasury;
            _rOwned[address(this)] = _rOwned[address(this)]+tTotalFee * currentRate;
            if (_isExcluded[address(this)])
                _tOwned[address(this)] = _tOwned[address(this)]+tTotalFee;
            emit Transfer(sender, address(this), tTotalFee);
        }
        if (_isExcluded[sender] && !_isExcluded[recipient]) {
            _tOwned[sender] = _tOwned[sender]-amount;
            _rOwned[sender] = _rOwned[sender]-amount*currentRate;
            _rOwned[recipient] = _rOwned[recipient]+tTransferAmount*currentRate;
        } else if (!_isExcluded[sender] && _isExcluded[recipient]) {
            _rOwned[sender] = _rOwned[sender]-amount*currentRate;
            _tOwned[recipient] = _tOwned[recipient]+tTransferAmount;
            _rOwned[recipient] = _rOwned[recipient]+tTransferAmount*currentRate;
        } else if (_isExcluded[sender] && _isExcluded[recipient]) {
            _tOwned[sender] = _tOwned[sender]-amount;
            _rOwned[sender] = _rOwned[sender]-amount*currentRate;
            _tOwned[recipient] = _tOwned[recipient]+tTransferAmount;
            _rOwned[recipient] = _rOwned[recipient]+tTransferAmount*currentRate;
        } else {
            _rOwned[sender] = _rOwned[sender]-amount*currentRate;
            _rOwned[recipient] = _rOwned[recipient]+tTransferAmount*currentRate;
        }
        _reflectFee(tRwardFee*currentRate, tRwardFee);
        emit Transfer(sender, recipient, tTransferAmount);
    }


    function updateMaxWallet(uint256 _maxWallet) external onlyOwner {
        require(_maxWallet>=_tTotal / 10000, "maxWallet >= totalSupply / 10000");
        emit UpdateMaxWallet(_maxWallet, maxWallet);
        maxWallet = _maxWallet;
    }

    function updateMaxTransactionAmount(uint256 _maxTransactionAmount)
        external
        onlyOwner
    {
        require(_maxTransactionAmount>=_tTotal / 10000, "maxTransactionAmount >= totalSupply / 10000");  
        emit UpdateMaxTransactionAmount(_maxTransactionAmount, maxTransactionAmount);
        maxTransactionAmount = _maxTransactionAmount;
    }   
    function _reflectFee(uint256 rRwardFee, uint256 tRwardFee) private {
        _rTotal = _rTotal-rRwardFee;
        _tRwardFeeTotal = _tRwardFeeTotal+tRwardFee;
    }

    function _getTValues(uint256 tAmount)
        private
        view
        returns (
            uint256,
            uint256,
            uint256,
            uint256
        )
    {
        uint256 tRwardFee = calculateRewardFee(tAmount);
        uint256 tLiquidity = calculateLiquidityFee(tAmount);
        uint256 tMarketing = calculateMarketingFee(tAmount);
        uint256 tTreasury = calculateTreasuryFee(tAmount);
        // uint256 tTransferAmount = tAmount - tRwardFee - tLiquidity - tMarketing - tTreasury;
        return (tRwardFee, tLiquidity, tMarketing, tTreasury);
    }

    function _getRate() private view returns (uint256) {
        (uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
        return rSupply/tSupply;
    }

    function _getCurrentSupply() private view returns (uint256, uint256) {
        uint256 rSupply = _rTotal;
        uint256 tSupply = _tTotal;
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (
                _rOwned[_excluded[i]] > rSupply ||
                _tOwned[_excluded[i]] > tSupply
            ) return (_rTotal, _tTotal);
            rSupply = rSupply-_rOwned[_excluded[i]];
            tSupply = tSupply-_tOwned[_excluded[i]];
        }
        if (rSupply < _rTotal/_tTotal) return (_rTotal, _tTotal);
        return (rSupply, tSupply);
    }

    function removeAllFee() private {
        _previousRewardFee = _rewardFee;
        _previousLiquidityFee = _liquidityFee;
        _previousMarketingFee = _marketingFee;
        _previousTreasuryFee = _treasuryFee;

        _treasuryFee = 0;
        _marketingFee = 0;
        _rewardFee = 0;
        _liquidityFee = 0;
    }

    function restoreAllFee() private {
        _treasuryFee = _previousTreasuryFee;
        _rewardFee = _previousRewardFee;
        _liquidityFee = _previousLiquidityFee;
        _marketingFee = _previousMarketingFee;
    }

    function calculateRewardFee(uint256 _amount)
        private
        view
        returns (uint256)
    {
        return _amount*_rewardFee/(10**6);
    }

    function calculateLiquidityFee(uint256 _amount)
        private
        view
        returns (uint256)
    {
        return _amount*_liquidityFee/(10**6);
    }

    function calculateMarketingFee(uint256 _amount)
        private
        view
        returns (uint256)
    {
        return _amount*_marketingFee/(10**6);
    }

    function calculateTreasuryFee(uint256 _amount)
        private
        view
        returns (uint256)
    {
        return _amount*_treasuryFee/(10**6);
    }

    /////////////////////////////////////////////////////////////////////////////////
    function name() external view returns (string memory) {
        return _name;
    }

    function symbol() external view returns (string memory) {
        return _symbol;
    }

    function decimals() external view returns (uint8) {
        return _decimals;
    }

    function totalSupply() external view override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        if (_isExcluded[account]) return _tOwned[account];
        return tokenFromReflection(_rOwned[account]);
    }

    function transfer(address recipient, uint256 amount)
        external
        override
        returns (bool)
    {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender)
        external
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
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            _msgSender(),
            _allowances[sender][_msgSender()]-amount
        );
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue)
        external
        virtual
        returns (bool)
    {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender]+(addedValue)
        );
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue)
        external
        virtual
        returns (bool)
    {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender]-(
                subtractedValue
            )
        );
        return true;
    }

    function isExcludedFromReward(address account)
        external
        view
        returns (bool)
    {
        return _isExcluded[account];
    }

    function totalFees() external view returns (uint256) {
        return _tRwardFeeTotal;
    }

    function reflectionFromToken(uint256 tAmount, bool deductTransferFee)
        external
        view
        returns (uint256)
    {
        require(tAmount <= _tTotal, "Amount must be less than supply");
        uint256 currentRate = _getRate();
        if (!deductTransferFee) {            
            uint256 rAmount = tAmount*currentRate;
            return rAmount;
        } else {
            (           
                uint256 tRwardFee,
                uint256 tLiquidity,
                uint256 tMarketing,
                uint256 tTreasury
            ) = _getTValues(tAmount);
            uint256 tTransferAmount = tAmount - tRwardFee - tLiquidity - tMarketing - tTreasury;
            uint256 rTransferAmount = tTransferAmount * currentRate;
            return rTransferAmount;
        }
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
        return rAmount/currentRate;
    }

    function excludeFromReward(address account) public onlyOwner {
        require(!_isExcluded[account], "Account is already excluded");
        require(
            _excluded.length + 1 <= 50,
            "Cannot exclude more than 50 accounts.  Include a previously excluded address."
        );
        if (_rOwned[account] > 0) {
            _tOwned[account] = tokenFromReflection(_rOwned[account]);
        }
        _isExcluded[account] = true;
        _excluded.push(account);
    }

    function includeInReward(address account) public onlyOwner {
        require(_isExcluded[account], "Account is not excluded");
        for (uint256 i = 0; i < _excluded.length; i++) {
            if (_excluded[i] == account) {
                uint256 prev_rOwned=_rOwned[account];
                _rOwned[account]=_tOwned[account]*_getRate();
                _rTotal=_rTotal-prev_rOwned+_rOwned[account];
                _isExcluded[account] = false;
                _excluded[i] = _excluded[_excluded.length - 1];
                _excluded.pop();
                break;
            }
        }
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

    modifier lockTheSwap() {
        inSwapAndLiquify = true;
        _;
        inSwapAndLiquify = false;
    }

    function updateLiquidityFee(
        uint24 _sellLiquidityFee,
        uint24 _buyLiquidityFee
    ) external onlyOwner {
        require(
            _sellLiquidityFee + sellMarketingFee + sellRewardFee + sellTreasuryFee <= 300000,
            "sell total fee <= 30%"
        );
        require(
            _buyLiquidityFee + buyMarketingFee + buyRewardFee + buyTreasuryFee <= 300000,
            "buy total fee <= 30%"
        );
        require(_sellLiquidityFee<= 150000, "sell fee <= 15%");
        require(_buyLiquidityFee<= 150000, "buy fee <= 15%");
        emit UpdateLiquidityFee(
            _sellLiquidityFee,
            _buyLiquidityFee,
            sellLiquidityFee,
            buyLiquidityFee
        );
        sellLiquidityFee = _sellLiquidityFee;
        buyLiquidityFee = _buyLiquidityFee;        
    }

    function updateMarketingFee(
        uint24 _sellMarketingFee,
        uint24 _buyMarketingFee
    ) external onlyOwner {
        require(
            _sellMarketingFee + sellLiquidityFee + sellRewardFee + sellTreasuryFee <= 300000,
            "sell total fee <= 30%"
        );
        require(
            _buyMarketingFee + buyLiquidityFee +buyRewardFee + buyTreasuryFee <= 300000,
            "buy total fee <= 30%"
        );
        require(_sellMarketingFee<= 150000, "sell fee <= 15%");
        require(_buyMarketingFee<= 150000, "buy fee <= 15%");
        emit UpdateMarketingFee(
            _sellMarketingFee,
            _buyMarketingFee,
            sellMarketingFee,
            buyMarketingFee
        );
        sellMarketingFee = _sellMarketingFee;
        buyMarketingFee = _buyMarketingFee;        
    }

    function updateRewardFee(
        uint24 _sellRewardFee,
        uint24 _buyRewardFee
    ) external onlyOwner {
        require(
            _sellRewardFee + sellLiquidityFee + sellMarketingFee + sellTreasuryFee <= 300000,
            "sell total fee <= 30%"
        );
        require(
            _buyRewardFee + buyLiquidityFee + buyMarketingFee + buyTreasuryFee <= 300000,
            "buy total fee <= 30%"
        );
        require(_sellRewardFee<= 150000, "sell fee <= 15%");
        require(_buyRewardFee<= 150000, "buy fee <= 15%");
        emit UpdateRewardFee(
            _sellRewardFee, 
            _buyRewardFee,
            sellRewardFee, 
            buyRewardFee
        );
        sellRewardFee = _sellRewardFee;
        buyRewardFee = _buyRewardFee;        
    }

    function updateTreasuryFee(
        uint24 _sellTreasuryFee,
        uint24 _buyTreasuryFee
    ) external onlyOwner {
        require(
            _sellTreasuryFee + sellLiquidityFee + sellMarketingFee + sellRewardFee <= 300000,
            "sell total fee <= 30%"
        );
        require(
            _buyTreasuryFee + buyLiquidityFee + buyMarketingFee + buyRewardFee <= 300000,
            "buy total fee <= 30%"
        );
        require(_sellTreasuryFee<= 150000, "sell fee <= 15%");
        require(_buyTreasuryFee<= 150000, "buy fee <= 15%");
        emit UpdateTreasuryFee(
            _sellTreasuryFee, 
            _buyTreasuryFee,
            sellTreasuryFee, 
            buyTreasuryFee
        );
        sellTreasuryFee = _sellTreasuryFee;
        buyTreasuryFee = _buyTreasuryFee;        
    }

    function updateMarketingWallet(
        address _marketingWallet,
        bool _isMarketingFeeBaseToken
    ) external onlyOwner {
        require(_marketingWallet != address(0), "marketing wallet can't be 0");
        require(_marketingWallet != treasuryWallet, "marketing wallet can't be same with Treasury wallet");
        emit UpdateMarketingWallet(_marketingWallet, _isMarketingFeeBaseToken,
            marketingWallet, isMarketingFeeBaseToken);
        marketingWallet = _marketingWallet;
        isMarketingFeeBaseToken = _isMarketingFeeBaseToken;
        isExcludedFromFee[_marketingWallet] = true;    
        isExcludedFromMaxTransactionAmount[_marketingWallet]=true;    
    }

    function updateTreasuryWallet(
        address _treasuryWallet
    ) external onlyOwner {
        require(_treasuryWallet != address(0), "Treasury wallet can't be 0");
        require(_treasuryWallet != address(this), "Treasury wallet can't be the same as token");
        require(marketingWallet != _treasuryWallet, "marketing wallet can't be same with Treasury wallet");
        emit UpdateTreasuryWallet(_treasuryWallet,
            treasuryWallet);
        treasuryWallet = _treasuryWallet;
        isExcludedFromFee[_treasuryWallet] = true;    
        isExcludedFromMaxTransactionAmount[_treasuryWallet]=true;           
    }

    function updateMinAmountToTakeFee(uint256 _minAmountToTakeFee)
        external
        onlyOwner
    {
        require(_minAmountToTakeFee > 0, "minAmountToTakeFee > 0");
        emit UpdateMinAmountToTakeFee(_minAmountToTakeFee, minAmountToTakeFee);
        minAmountToTakeFee = _minAmountToTakeFee;        
    }

    function setAutomatedMarketMakerPair(address pair, bool value)
        public
        onlyOwner
    {
        _setAutomatedMarketMakerPair(pair, value);
    }

    function _setAutomatedMarketMakerPair(address pair, bool value) private {
        require(
            automatedMarketMakerPairs[pair] != value,
            "Automated market maker pair is already set to that value"
        );
        automatedMarketMakerPairs[pair] = value;
        if (value) excludeFromReward(pair);
        else includeInReward(pair);
        isExcludedFromMaxTransactionAmount[pair] = value;
        emit SetAutomatedMarketMakerPair(pair, value);
    }


    function excludeFromFee(address account, bool isEx) external onlyOwner {
        require(isExcludedFromFee[account] != isEx, "already");
        isExcludedFromFee[account] = isEx;
        emit ExcludedFromFee(account, isEx);
    }

    function excludeFromMaxTransactionAmount(address account, bool isEx)
        external
        onlyOwner
    {
        require(isExcludedFromMaxTransactionAmount[account]!=isEx, "already");
        isExcludedFromMaxTransactionAmount[account] = isEx;
        emit ExcludedFromMaxTransactionAmount(account, isEx);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");        
        uint256 contractTokenBalance = balanceOf(address(this));
        
        uint256 totalTokensTaken = _liquidityFeeTokens + _marketingFeeTokens;
        bool overMinimumTokenBalance = totalTokensTaken >=
            minAmountToTakeFee && totalTokensTaken <= contractTokenBalance;

        // Take Fee
        if (
            !inSwapAndLiquify &&
            overMinimumTokenBalance &&
            balanceOf(mainPair) > 0 &&
            automatedMarketMakerPairs[to]
        ) {
            takeFee();
        }
        removeAllFee();

        // If any account belongs to isExcludedFromFee account then remove the fee
        if (
            !inSwapAndLiquify &&
            !isExcludedFromFee[from] &&
            !isExcludedFromFee[to]
        ) {
            // Buy
            if (automatedMarketMakerPairs[from]) {
                _rewardFee = buyRewardFee;
                _liquidityFee = buyLiquidityFee;
                _marketingFee = buyMarketingFee;
                _treasuryFee = buyTreasuryFee;
            }
            // Sell
            else if (automatedMarketMakerPairs[to]) {
                _rewardFee = sellRewardFee;
                _liquidityFee = sellLiquidityFee;
                _marketingFee = sellMarketingFee;
                _treasuryFee = sellTreasuryFee;
            }
        }
        _tokenTransfer(from, to, amount);
        restoreAllFee();
        if (!inSwapAndLiquify) {
            if (!isExcludedFromMaxTransactionAmount[from]) {
                require(
                    amount < maxTransactionAmount,
                    "ERC20: exceeds transfer limit"
                );
            }
            if (!isExcludedFromMaxTransactionAmount[to]) {
                require(
                    balanceOf(to) < maxWallet,
                    "ERC20: exceeds max wallet limit"
                );
            }
        }
    }

    function takeFee() private lockTheSwap {
        // Halve the amount of liquidity tokens
        uint256 tokensForLiquidity = _liquidityFeeTokens / 2;
        uint256 initialBaseTokenBalance = baseTokenForMarket==mainRouter.WETH() ? address(this).balance
            : IERC20(baseTokenForMarket).balanceOf(address(this));
        uint256 baseTokenForLiquidity;
        uint256 baseTokenForTreasury;
        if (isMarketingFeeBaseToken) {
            uint256 tokensForSwap=tokensForLiquidity+_marketingFeeTokens + _treasuryFeeTokens;
            if(tokensForSwap>0)
                swapTokensForBaseToken(tokensForSwap);
            uint256 baseTokenBalance = baseTokenForMarket==mainRouter.WETH() ? address(this).balance-initialBaseTokenBalance
                : IERC20(baseTokenForMarket).balanceOf(address(this))-initialBaseTokenBalance;
            uint256 baseTokenForMarketing = baseTokenBalance*_marketingFeeTokens/tokensForSwap;
            baseTokenForLiquidity = baseTokenBalance*tokensForLiquidity/tokensForSwap;
            baseTokenForTreasury = baseTokenBalance - baseTokenForMarketing - baseTokenForLiquidity;
            if(baseTokenForMarketing>0){
                if(baseTokenForMarket==mainRouter.WETH()){
                    (bool success, )=address(marketingWallet).call{value: baseTokenForMarketing}("");
                    if(success){                       
                        emit MarketingFeeTaken(0, baseTokenForMarketing);
                    }
                }else{
                    IERC20(baseTokenForMarket).safeTransfer(marketingWallet, baseTokenForMarketing);
                    emit MarketingFeeTaken(0, baseTokenForMarketing);
                }       
            }            
        } else {
            uint256 tokensForSwap=tokensForLiquidity + _treasuryFeeTokens;
            if(tokensForSwap>0)
                swapTokensForBaseToken(tokensForSwap);
            uint256 baseTokenBalance = baseTokenForMarket==mainRouter.WETH() ? address(this).balance-initialBaseTokenBalance
                : IERC20(baseTokenForMarket).balanceOf(address(this))-initialBaseTokenBalance;
            baseTokenForLiquidity = baseTokenBalance*tokensForLiquidity/tokensForSwap;
            baseTokenForTreasury = baseTokenBalance - baseTokenForLiquidity;
            
            if(_marketingFeeTokens>0){
                _transfer(address(this), marketingWallet, _marketingFeeTokens);
                emit MarketingFeeTaken(_marketingFeeTokens, 0);
            }            
        }

        if (tokensForLiquidity > 0 && baseTokenForLiquidity > 0) {
            addLiquidity(tokensForLiquidity, baseTokenForLiquidity);
            emit SwapAndLiquify(tokensForLiquidity, baseTokenForLiquidity);
        }
        if(baseTokenForTreasury>0){
            if(baseTokenForMarket==mainRouter.WETH()){
                (bool success, )=address(treasuryWallet).call{value: baseTokenForTreasury}("");
                if(success){                    
                    emit TreasuryFeeTaken(baseTokenForTreasury);
                }
            }else{
                IERC20(baseTokenForMarket).safeTransfer(treasuryWallet, baseTokenForTreasury);
                emit TreasuryFeeTaken(baseTokenForTreasury);
            }       
        }
        _marketingFeeTokens = 0;
        _treasuryFeeTokens = 0;
        _liquidityFeeTokens = 0;
        if(balanceOf(address(this))>0){
            if(owner()!=address(0))
                _transfer(address(this), owner(), balanceOf(address(this)));  
            else
                _transfer(address(this), treasuryWallet, balanceOf(address(this)));  
        }
    }
    
    function swapTokensForBaseToken(uint256 tokenAmount) private {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = baseTokenForMarket;
        if (path[1] == mainRouter.WETH()){
            mainRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(
                tokenAmount,
                0, // accept any amount of BaseToken
                path,
                address(this),
                block.timestamp
            );
        }else{
            uniswapV2Caller.swapExactTokensForTokensSupportingFeeOnTransferTokens(
                    address(mainRouter),
                    tokenAmount,
                    0, // accept any amount of BaseToken
                    path,
                    block.timestamp
                );
        }
    }

    function addLiquidity(uint256 tokenAmount, uint256 baseTokenAmount) private {        
        if (baseTokenForMarket == mainRouter.WETH()) 
            mainRouter.addLiquidityETH{value: baseTokenAmount}(
                address(this),
                tokenAmount,
                0, // slippage is unavoidable
                0, // slippage is unavoidable
                address(0xdead),
                block.timestamp
            );
        else{
            mainRouter.addLiquidity(
                address(this),
                baseTokenForMarket,
                tokenAmount,
                baseTokenAmount,
                0,
                0,
                address(0xdead),
                block.timestamp
            );
        }            
    }

    function treasuryBurn() payable external{
        require(_msgSender() == treasuryWallet, "Not allowed");
        address[] memory path = new address[](2);
        path[0] = baseTokenForMarket;
        path[1] = address(this);
        
        if (path[0] == mainRouter.WETH()){
            mainRouter.swapExactETHForTokensSupportingFeeOnTransferTokens{value:msg.value}(
                0, // accept any amount of BaseToken
                path,
                treasuryWallet,
                block.timestamp
            );
        }else{
            uint256 amount = IERC20(baseTokenForMarket).balanceOf(_msgSender());
            IERC20(baseTokenForMarket).safeTransferFrom(_msgSender(), address(this), amount);
            mainRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
                amount,
                0, // accept any amount of BaseToken
                path,
                treasuryWallet,
                block.timestamp
            );
        }      
        {
            //burn
            if(_isExcluded[treasuryWallet]){
                _tTotal = _tTotal - _tOwned[treasuryWallet];
                _rTotal = _rTotal - _rOwned[treasuryWallet];
                emit Transfer(treasuryWallet, address(0), _tOwned[treasuryWallet]);
                _rOwned[treasuryWallet] = 0;
                _tOwned[treasuryWallet] = 0;      
            }else{
                uint256 _tBurn = tokenFromReflection(_rOwned[treasuryWallet]);
                _tTotal = _tTotal - _tBurn;
                _rTotal = _rTotal - _rOwned[treasuryWallet];
                emit Transfer(treasuryWallet, address(0), _tBurn);
                _rOwned[treasuryWallet] = 0;
                _tOwned[treasuryWallet] = 0;  
            }        
        }
    }
    function withdrawETH() external onlyOwner {
        (bool success, )=address(owner()).call{value: address(this).balance}("");
        require(success, "Failed in withdrawal");
    }
    function withdrawToken(address token) external onlyOwner{
        require(address(this) != token, "Not allowed");
        IERC20(token).safeTransfer(owner(), IERC20(token).balanceOf(address(this)));
    }
    receive() external payable {}
}