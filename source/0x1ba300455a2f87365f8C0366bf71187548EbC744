/**
 *Submitted for verification at BscScan.com on 2023-09-09
*/

// SPDX-License-Identifier: MIT
// File: @openzeppelin/contracts/security/ReentrancyGuard.sol
// OpenZeppelin Contracts (last updated v4.9.0) (security/ReentrancyGuard.sol)

pragma solidity ^0.8.0;

abstract contract ReentrancyGuard {

    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

   
    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }
    function _nonReentrantBefore() private {
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");
        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }
    function _nonReentrantAfter() private {
    
        _status = _NOT_ENTERED;
    }

    function _reentrancyGuardEntered() internal view returns (bool) {
        return _status == _ENTERED;
    }
}
// File: @openzeppelin/contracts/utils/Context.sol
// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)
pragma solidity ^0.8.0;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

// File: @openzeppelin/contracts/security/Pausable.sol
// OpenZeppelin Contracts (last updated v4.7.0) (security/Pausable.sol)
pragma solidity ^0.8.0;
abstract contract Pausable is Context {

    event Paused(address account);
    event Unpaused(address account);
    bool private _paused;
    constructor() {
        _paused = false;
    }
    modifier whenNotPaused() {
        _requireNotPaused();
        _;
    }
    modifier whenPaused() {
        _requirePaused();
        _;
    }
    function paused() public view virtual returns (bool) {
        return _paused;
    }
    function _requireNotPaused() internal view virtual {
        require(!paused(), "Pausable: paused");
    }

    function _requirePaused() internal view virtual {
        require(paused(), "Pausable: not paused");
    }

    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(_msgSender());
    }

    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(_msgSender());
    }
}
// File: @openzeppelin/contracts/token/ERC20/IERC20.sol
// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)
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
// File: @openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol
// OpenZeppelin Contracts v4.4.1 (token/ERC20/extensions/IERC20Metadata.sol)
pragma solidity ^0.8.0;
interface IERC20Metadata is IERC20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
}
// File: contracts/Astrobirdz.sol
pragma solidity ^0.8.0;
library SafeMath {
    function tryAdd(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }
    function trySub(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }
    function tryMul(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            if (a == 0) return (true, 0);
            uint256 c = a * b;
            if (c / a != b) return (false, 0);
            return (true, c);
        }
    }
    function tryDiv(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }
    function tryMod(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }
    function add(uint256 a, uint256 b) internal pure returns (uint256) {return a + b; }
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {return a - b;}
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {return a * b; }
    function div(uint256 a, uint256 b) internal pure returns (uint256) {return a / b;}
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {return a % b; }
    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }
    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }
    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}
interface IPancakeFactory {
    event PairCreated(
        address indexed token0,
        address indexed token1,
        address pair,
        uint);
    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);
    function getPair(
        address tokenA,
        address tokenB
        )
        external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);
    function createPair(
        address tokenA,
        address tokenB
        )
        external returns (address pair);
    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}
interface IPancakePair {
    event Approval(
        address indexed owner,
        address indexed spender,
        uint value);
    event Transfer(
        address indexed from,
        address indexed to,
        uint value);
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
    function permit(
        address owner,
        address spender,
        uint value,
        uint deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
        ) external;
    event Mint(
        address indexed sender,
        uint amount0,
        uint amount1
    );
    event Burn(
        address indexed sender,
        uint amount0,
        uint amount1,
        address indexed to
    );
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
contract Astrobirdz is
    IERC20,
    IERC20Metadata,
    Pausable,
    ReentrancyGuard
{
    address private _owner;
    using SafeMath for uint256;

    mapping(address => uint256) private _balances;
    mapping(address => bool) private blacklist;

    uint256 private _totalTax ;
    uint256 private _teamTax = 100;
    uint256 private _marketingTax = 200;
    address private _teamAddress  = 0x251177e4626C45A1e8567E8dA6A946e26BA73E54; // Wallet Address for Team Tax
    address private _marketingAddress = 0x8B2837fcaFAdD80d24C159dD910C201F06eC45Ce; // Wallet Address for Marketing Tax
    uint256 private _maxTxPercent = 500;

    uint256 private _maxTxAmount;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;  // 1b tokens for distribution
    string private _name;
    string private _symbol;

    mapping (address => bool) private _isExcludedFromFees;
    mapping (address => bool) private _isExcludedFromLimits;

    address constant public DEAD = 0x000000000000000000000000000000000000dEaD;

    IPancakeRouter02 public pancakeRouter;
    address public pancakePair;
    mapping(address => bool) public automatedMarketMakerPairs;
    bool public swapEnabled = true;
    event SwapBackTaxSuccess(
        uint256 tokenAmount,
        uint256 ethAmountReceived,
        bool success
    );
    error NotEnoughTokensBeforeSwap();
    bool private swapping;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
  
  constructor()
{
    _name = "Astrobirdz";
    _symbol = "ABZ";
    _owner = msg.sender;
    uint256 _Supply = 1_000_000_000 * 10 ** 18;
    _mint(_owner, _Supply);
    IPancakeRouter02 _pancakeRouter = IPancakeRouter02(
        0x10ED43C718714eb63d5aA57B78B54704E256024E
    );
    pancakeRouter = _pancakeRouter;
    setExcludedFromLimits(address(_pancakeRouter), true);

    pancakePair = IPancakeFactory(_pancakeRouter.factory())
        .createPair(address(this), _pancakeRouter.WETH());
    setExcludedFromLimits(address(pancakePair), true);
    _setAutomatedMarketMakerPair(address(pancakePair), true);
    setExcludedFromFees(address(this), true);
    setExcludedFromFees(pancakePair, true);
    _isExcludedFromFees[_owner] = true;
    _isExcludedFromFees[_marketingAddress] = true;
    _isExcludedFromFees[_teamAddress] = true;
    _isExcludedFromFees[address(this)] = true;
    _isExcludedFromFees[DEAD] = true;
    _isExcludedFromLimits[_owner] = true;
    _isExcludedFromLimits[address(this)] = true;
    _maxTxAmount = (_totalSupply.mul(_maxTxPercent)).div(10000);
    _totalTax = _teamTax.add(_marketingTax);
}
    receive() external payable {}
    fallback() external payable {}
    modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
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
    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }
    function transfer(address recipient, uint256 amount) whenNotPaused external override returns (bool) {
        // require(!isBlacklisted(msg.sender), "BlackListed Address!!!");
        return _transfer(msg.sender, recipient, amount);
    }
    function allowance(address owner_, address spender) public view virtual override returns (uint256) {
        return _allowances[owner_][spender];
    }
    function approve(address spender, uint256 amount) whenNotPaused public virtual override returns (bool) {
        address owner_ = _msgSender();
        _approve(owner_, spender, amount);
        return true;
    }
    function transferFrom(address from, address to, uint256 amount) whenNotPaused public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }
    function increaseAllowance(address spender, uint256 addedValue) whenNotPaused public virtual returns (bool) {
        address owner_ = _msgSender();
        _approve(owner_, spender, allowance(owner_, spender) + addedValue);
        return true;
    }
    function decreaseAllowance(address spender, uint256 subtractedValue) whenNotPaused public virtual returns (bool) {
        address owner_ = _msgSender();
        uint256 currentAllowance = allowance(owner_, spender);
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(owner_, spender, currentAllowance - subtractedValue);
        }

        return true;
    }
    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply = _totalSupply.add(amount);
        unchecked {
            // Overflow not possible: balance + amount is at most totalSupply + amount, which is checked above.
            _balances[account] = _balances[account].add(amount);
        }
        emit Transfer(address(0), account, amount);

        _afterTokenTransfer(address(0), account, amount);
    }
    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance.sub(amount);
            // Overflow not possible: amount <= accountBalance <= totalSupply.
            _totalSupply =_totalSupply.sub(amount);
        }

        emit Transfer(account, address(0), amount);

        _afterTokenTransfer(account, address(0), amount);
    }
    function burn( uint256 amount) whenNotPaused external{
        _burn(msg.sender, amount);
    }
    function _approve(address owner_, address spender, uint256 amount) internal virtual {
        require(owner_ != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner_][spender] = amount;
        emit Approval(owner_, spender, amount);
    }
    function _spendAllowance(address owner_, address spender, uint256 amount) internal virtual {
        uint256 currentAllowance = allowance(owner_, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            unchecked {
                _approve(owner_, spender, currentAllowance - amount);
            }
        }
    }
    function _beforeTokenTransfer(address from, address to, uint256 amount) internal virtual {}
    function _afterTokenTransfer(address from, address to, uint256 amount) internal virtual {}
    function pause() public onlyOwner {
        _pause();
    }
    function unpause() public onlyOwner {
        _unpause();
    }

    function setTax(
        uint256 _newTeamTax,
        uint256 _newMarketingTax)
        whenNotPaused
        public
        onlyOwner
    {
        _teamTax = _newTeamTax;
        _marketingTax = _newMarketingTax;
        _totalTax = _teamTax + _marketingTax;
    }
    function setTeamAddress(
        address _newAddress)
        whenNotPaused
        public
        onlyOwner
    {
        require(_newAddress != address(0), "Zero Address Given");
        _teamAddress = _newAddress;
    }
    function setMarketingAddress( address _newAddress)
        whenNotPaused
        public
        onlyOwner
    {
        require(_newAddress != address(0), "Zero Address Given");
        _marketingAddress = _newAddress;
    }
    function totalTax() public view returns (uint256) {
        return _totalTax;
    }
    function teamTax() public view returns (uint256) {
        return _teamTax;
    }
    function marketingTax() public view returns (uint256) {
        return _marketingTax;
    }
    function teamAddress() public view returns (address) {
        return _teamAddress;
    }
    function marketingAddress() public view returns (address) {
        return _marketingAddress;
    }
    function owner() public view returns (address) {
        return _owner;
    }
    function transferOwnership(address newOwner) external onlyOwner {
        require(newOwner != address(0), "Call renounceOwnership to transfer owner to the zero address.");
        require(newOwner != DEAD, "Call renounceOwnership to transfer owner to the zero address.");
        _isExcludedFromFees[_owner] = false;
        _isExcludedFromLimits[_owner] = false;
 
        _isExcludedFromFees[newOwner] = true;
        _isExcludedFromLimits[newOwner] = true;

        if(balanceOf(_owner) > 0) {
            _finalizeTransfer(_owner, newOwner, balanceOf(_owner), false);
        }
        
        _owner = newOwner;
        emit OwnershipTransferred(_owner, newOwner);
        
    }
    function renounceOwnership() public virtual onlyOwner {
        _isExcludedFromFees[_owner] = false;
        _isExcludedFromLimits[_owner] = false;
        _owner = address(0);
        emit OwnershipTransferred(_owner, address(0));
    }
    function isExcludedFromLimits(address account) public view returns (bool) {
        return _isExcludedFromLimits[account];
    }
    function isExcludedFromFees(address account) public view returns (bool) {
        return _isExcludedFromFees[account];
    }
    function setExcludedFromLimits(address account, bool enabled) whenNotPaused public onlyOwner {
        require(account != address(0), "Zero Address Given");
        _isExcludedFromLimits[account] = enabled;
    }
    function setExcludedFromFees(address account, bool enabled) whenNotPaused public onlyOwner {
        require(account != address(0), "Zero Address Given");
        _isExcludedFromFees[account] = enabled;
    }
    //Pancake Swap Functions
    function updateSwapEnabled(bool enabled) external onlyOwner {
        swapEnabled = enabled;
    }
    function setAutomatedMarketMakerPair(
        address pair,
        bool value
    ) public onlyOwner {
        require(
            pair != pancakePair,
            "The pair cannot be removed from automatedMarketMakerPairs"
        );
        _setAutomatedMarketMakerPair(pair, value);
    }
   function addLiquidity(uint256 _tokenAmount) external payable onlyOwner {
    require(pancakePair == address(0), "LP pair already exists"); // Add this check

    //PancakeSwap Router Address BSC Mainnet: 0x10ED43C718714eb63d5aA57B78B54704E256024E
    //PancakeSwap Router Address on BSC Testnet: 0xD99D1c33F9fC3444f8101754aBC46c52416550D1
    IPancakeRouter02 _pancakeRouter = IPancakeRouter02(
        0x10ED43C718714eb63d5aA57B78B54704E256024E
    );
    pancakeRouter = _pancakeRouter;
    setExcludedFromLimits(address(_pancakeRouter), true);
    _approve(owner(), address(pancakeRouter), _tokenAmount);
    pancakeRouter.addLiquidityETH{value: msg.value}(
        address(this), //token address
        _tokenAmount, // liquidity amount
        0, // slippage is unavoidable
        0, // slippage is unavoidable
        owner(), // LP tokens are sent to the owner
        block.timestamp
    );
}
    function _setAutomatedMarketMakerPair(address pair, bool value) private {
        automatedMarketMakerPairs[pair] = value;
    }
    function swapTokensForEth(uint256 tokenAmount) private {
        // generate the uniswap pair path of token -> weth
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = pancakeRouter.WETH();
        _approve(address(this), address(pancakeRouter), tokenAmount);
        // make the swap
        pancakeRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // accept any amount of ETH
            path,
            address(this),
            block.timestamp
        );
    }
function _transfer(address from, address to, uint256 amount) internal returns (bool) {
    require(from != address(0), "ERC20: transfer from the zero address");
    require(to != address(0), "ERC20: transfer to the zero address");
    require(amount > 0, "Transfer amount must be greater than zero");
    uint256 fromBalance = balanceOf(from);
    require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");

    bool takeFee = true;  // Default to taking fees

    // Check if sender is excluded from fees
    if (_isExcludedFromFees[from] && _isExcludedFromFees[to]) {
        takeFee = false;
    }

    return _finalizeTransfer(from, to, amount, takeFee);
}

    function _finalizeTransfer(
        address from,
        address to,
        uint256 amount,
        bool takeFee)
        internal
        returns (bool) {
        uint256 amountReceived = amount;
        if (swapEnabled &&
            !swapping &&
            (automatedMarketMakerPairs[from] || automatedMarketMakerPairs[to]) &&
            takeFee) {
            swapping = true;
            amountReceived = takeTaxes(from, amount);
            swapping = false;
        }
        uint256 fromBalance = balanceOf(from);
        unchecked {
            _balances[from] = fromBalance.sub(amount);
            // Overflow not possible: the sum of all balances is capped by totalSupply, and the sum is preserved by
            // decrementing then incrementing.
            _balances[to] = _balances[to].add(amountReceived);
        }
        emit Transfer(from, to, amountReceived);
        return true;
    }
    function takeTaxes(address from, uint256 amount) internal returns (uint256) {
        bool success;
        uint256 teamTaxFee = (amount.mul(teamTax())).div(10000);
        uint256 marketingTaxFee = (amount.mul(marketingTax())).div(10000);
        _balances[address(this)] = (_balances[address(this)].add(teamTaxFee)).add(marketingTaxFee);
        //_balances[_teamAddress] = _balances[_teamAddress] + teamTaxFee;
        //_balances[_marketingAddress] = _balances[_marketingAddress] + marketingTaxFee;
        uint256 amountToSwapForETH = _balances[address(this)];
        swapTokensForEth(amountToSwapForETH);
        uint256 amountEthToSend = address(this).balance;
        uint256 amountToTeam = amountEthToSend.div(3);
        uint256 amountToMarketing = amountEthToSend.sub(amountToTeam);
        (success, ) = address(marketingAddress()).call{value: amountToMarketing}("");
        (success, ) = address(teamAddress()).call{value: amountToTeam}("");     
        emit SwapBackTaxSuccess(amountToSwapForETH, amountEthToSend, success);
        emit Transfer(from, _teamAddress, teamTaxFee);
        emit Transfer(from, _marketingAddress, marketingTaxFee);
        return (amount.sub(teamTaxFee)).sub(marketingTaxFee);
    }
}