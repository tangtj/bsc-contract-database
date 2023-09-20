// File: contracts\base\token\ERC20\IERC20.sol

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.4;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address recipient, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

// File: contracts\base\token\ERC20\extensions\IERC20Metadata.sol

/**
 * @dev Interface for the optional metadata functions from the ERC20 standard.
 *
 * _Available since v4.1._
 */
interface IERC20Metadata is IERC20 {
    /**
     * @dev Returns the name of the token.
     */
    function name() external view returns (string memory);

    /**
     * @dev Returns the symbol of the token.
     */
    function symbol() external view returns (string memory);

    /**
     * @dev Returns the decimals places of the token.
     */
    function decimals() external view returns (uint8);
}

// File: contracts\base\utils\Context.sol

/*
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

// File: contracts\base\access\Ownable.sol


/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner account will be the one that deploys the contract. This
 * can later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function onOwnershipTransferred(address previousOwner, address newOwner) internal virtual { }
}

// File: contracts\base\token\ERC20\PancakeSwap\IPancakeRouter01.sol

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

// File: contracts\base\token\ERC20\PancakeSwap\IPancakeRouter02.sol


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

// File: contracts\base\token\ERC20\PancakeSwap\IPancakeFactory.sol


interface IPancakeFactory {
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

// File: contracts\base\access\ReentrancyGuard.sol

/**
 * @dev Contract module that helps prevent reentrant calls to a function.
 *
 * Inheriting from `ReentrancyGuard` will make the {nonReentrant} modifier
 * available, which can be applied to functions to make sure there are no nested
 * (reentrant) calls to them.
 *
 * Note that because there is a single `nonReentrant` guard, functions marked as
 * `nonReentrant` may not call one another. This can be worked around by making
 * those functions `private`, and then adding `external` `nonReentrant` entry
 * points to them.
 *
 * TIP: If you would like to learn more about reentrancy and alternative ways
 * to protect against it, check out our blog post
 * https://blog.openzeppelin.com/reentrancy-after-istanbul/[Reentrancy After Istanbul].
 */
abstract contract ReentrancyGuard {
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.

    // The values being non-zero value makes deployment a bit more expensive,
    // but in exchange the refund on every call to nonReentrant will be lower in
    // amount. Since refunds are capped to a percentage of the total
    // transaction's gas, it is best to keep them low in cases like this one, to
    // increase the likelihood of the full refund coming into effect.
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor () {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and make it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        // On the first call to nonReentrant, _notEntered will be true
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;

        _;

        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }

    modifier isHuman() {
        require(tx.origin == msg.sender, "sorry humans only");
        _;
    }
}

// File: contracts\StellarDiamond.sol


/*
Notes: Usage of SafeMath is not required as of solidity 0.8.0 due to built-in safety checks
*/
contract StellarDiamond is Context, IERC20Metadata, Ownable, ReentrancyGuard {
	uint256 private constant MAX = ~uint256(0);
	
	// Main token properties
	string private constant _name = "Stellar Diamond";
    string private constant _symbol = "STELLAR";
    uint8 private constant _decimals = 9;
	uint256 private constant _distributionFee = 2; //2% of each transaction will be distributed to all holders
	uint256 private constant _liquidityFee = 4; //4% of each transaction will be added as liquidity
	uint256 private constant _rewardFee = 4; //4% of each transaction will be used for BNB reward pool
	uint256 private constant _poolFee = _rewardFee + _liquidityFee; //The total fee to be taken and added to the pool, this includes both the liquidity fee and the reward fee

	uint256 private constant _totalTokens = 1000000000000000 * 10**_decimals;	//1 quadrillion total supply
	mapping (address => uint256) private _balances; //The balance of each address.  This is before applying distribution rate.  To get the actual balance, see balanceOf() method
	mapping (address => mapping (address => uint256)) private _allowances;

	//Indicates the amount of distribution available. Min value is _totalTokens. This is divisible by _totalTokens without any remainder
    uint256 private _totalDistributionAvailable = (MAX - (MAX % _totalTokens)); 

	// Fees & Rewards
	address private constant _burnWallet = 0x000000000000000000000000000000000000dEaD; //The address that keeps track of all tokens burned
	uint256 private _totalFeesDistributed; // The total fees distributed (in number of tokens)
	uint256 private _totalFeesPooled; // The total fees pooled (in number of tokens)
	uint256 private constant _tokenSwapThreshold = _totalTokens / 100000; //There should be at least 0.001% of the total supply in the contract before triggering a swap
	mapping (address => bool) private _addressesExcludedFromFees; // The list of addresses that do not pay a fee for transactions
	mapping(address => uint256) public _nextAvailableClaimDate; // The next available reward claim date for each address
	uint256 _rewardCyclePeriod = 1 days; // The duration of the reward cycle (e.g. can claim rewards once a day)
	uint256 _rewardCycleExtensionThreshold = 20; // If someone receives more than 20% of their balance in a transaction, their cycle date will increase accordingly
	uint256 _totalBNBLiquidityAddedFromFees; // The total number of BNB added to the pool through fees

	// Charity
	address payable private _charityAddress = payable(0xb6266d43F3E319e884E31075a36fDE8ceAeEf1C8); // A percentage of the BNB pool will go to the charity address
	uint256 private _charityThreshold = 1 ether; // The minimum number of BNB reward before triggering a charity call.  This means if reward is low, it will not contribute to charity
	uint256 private _charityPercentage = 10; // In case charity is triggerred, this is the percentage to take out from the reward transaction

	// Transaction limit
	uint256 private constant _maxTransactionAmount = _totalTokens / 10000; // Only up to 0.01% of total supply can be exchanged at once
	mapping (address => bool) private _addressesExcludedFromTransactionLimit; // The list of addresses that are not affected by the transaction limit

	address private _pancakeSwapRouterAddress;
	IPancakeRouter02 private _pancakeswapV2Router;
    address private _pancakeswapV2Pair;

	event Swapped(uint256 tokensSwapped, uint256 bnbReceived, uint256 tokensIntoLiqudity, uint256 bnbIntoLiquidity);
	event BNBClaimed(address recipient, uint256 bnbReceived, uint256 nextAvailableClaimDate);
	
	constructor (address routerAddress) {
        _balances[_msgSender()] = _totalDistributionAvailable;
		
		// Exclude addresses from fees
        _addressesExcludedFromFees[address(this)] = true;
		_addressesExcludedFromFees[owner()] = true;

		// Exclude addresses from transaction limits
		_addressesExcludedFromTransactionLimit[owner()] = true;
		_addressesExcludedFromTransactionLimit[address(this)] = true;
		_addressesExcludedFromTransactionLimit[_burnWallet] = true;
        
		// Initialize pancake router
		_pancakeSwapRouterAddress = routerAddress; //0x10ed43c718714eb63d5aa57b78b54704e256024e
		_pancakeswapV2Router = IPancakeRouter02(_pancakeSwapRouterAddress);
        _pancakeswapV2Pair = IPancakeFactory(_pancakeswapV2Router.factory()).createPair(address(this), _pancakeswapV2Router.WETH());
		

        emit Transfer(address(0), _msgSender(), _totalTokens);

		// Allow pancakeSwap to spend the tokens of the address, no matter the amount
		doApprove(address(this), _pancakeSwapRouterAddress, MAX);
    }

	function balanceOf(address account) public view override returns (uint256) {
		// Apply reflection
        uint256 currentRate =  getCurrentDistributionRate();
        return _balances[account] / currentRate;
    }
	

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        doTransfer(_msgSender(), recipient, amount);
        return true;
    }
    

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        doTransfer(sender, recipient, amount);
        doApprove(sender, _msgSender(), _allowances[sender][_msgSender()] - amount);
        return true;
    }
	

	function approve(address spender, uint256 amount) public override returns (bool) {
        doApprove(_msgSender(), spender, amount);
        return true;
    }
    
	
    function doTransfer(address sender, address recipient, uint256 amount) private {
        require(sender != address(0), "Transfer from the zero address is not allowed");
        require(recipient != address(0), "Transfer to the zero address is not allowed");
        require(amount > 0, "Transfer amount must be greater than zero");
		
		
        if (!_addressesExcludedFromTransactionLimit[sender] && !_addressesExcludedFromTransactionLimit[recipient]) {
            require(amount <= _maxTransactionAmount, "Transfer amount exceeds the maximum allowed");
		}

		// Perform a swap if needed
		executeSwapIfNeeded(sender, recipient);

		// Extend the reward cycle according to the amount bought, this is done so that users do not abuse the cycle (buy before it ends & sell after they claim the reward)
		_nextAvailableClaimDate[recipient] += calculateRewardCycleExtension(balanceOf(recipient), amount);
		
		// Calculate distribution & pool rates
		(uint256 distributionFeeRate, uint256 poolFeeRate) = calculateFeeRates(sender, recipient);
		
		uint256 distributionAmount = amount * distributionFeeRate / 100;
		uint256 poolAmount = amount * poolFeeRate / 100;
		uint256 transferAmount = amount - distributionAmount - poolAmount;

		// Update balances
		updateBalances(sender, recipient, amount, distributionAmount, poolAmount);

		// Update total fees
        _totalFeesDistributed += distributionAmount;
		_totalFeesPooled += poolAmount;

        emit Transfer(sender, recipient, transferAmount); 
    }


	function updateBalances(address sender, address recipient, uint256 amount, uint256 distributionAmount, uint256 poolAmount) private {
		uint256 currentRate = getCurrentDistributionRate();

		// Calculate amount to be sent by sender
		uint256 sentAmount = amount * currentRate;
		
		// Calculate amount to be received by recipient
		uint256 rDistributionAmount = distributionAmount * currentRate;
		uint256 rPoolAmount = poolAmount * currentRate;
		uint256 receivedAmount = sentAmount - rDistributionAmount - rPoolAmount;

		// Update balances
        _balances[sender] -= sentAmount;
        _balances[recipient] += receivedAmount;
		
		// Add pool to contract
		_balances[address(this)] += rPoolAmount;
		
		// Update the distribution available.  By doing so, we're reducing the reflection rate therefore everyone's balance goes up accordingly
		_totalDistributionAvailable -= rDistributionAmount;

		// Since we burned a big portion of the tokens during contract creation, the burn wallet will also receive a cut from the distribution
	}
	

	function doApprove(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "Cannot approve from the zero address");
        require(spender != address(0), "Cannot approve to the zero address");

        _allowances[owner][spender] = amount;
		
        emit Approval(owner, spender, amount);
    }

    
	//Returns the current reflection rate, which is _totalDistributionAvailable/_totalTokens
	//This means that it starts high and goes down as time goes on (total distribution tokens available decreases).  Min value is 1, indicating no distribution.
	function getCurrentDistributionRate() public view returns(uint256) {
		if (_totalDistributionAvailable < _totalTokens) {
			return 1;
		}
		
        return _totalDistributionAvailable / _totalTokens;
    }
	
	function calculateFeeRates(address sender, address recipient) private view returns(uint256, uint256) {
		bool applyFees = !_addressesExcludedFromFees[sender] && !_addressesExcludedFromFees[recipient];
		if (applyFees)
		{
		    return (_distributionFee, _poolFee);
		}

		return (0, 0);
	}

	
	function executeSwapIfNeeded(address sender, address recipient) private {
		// Check if it's time to swap for liquidity & reward pool
		uint256 tokensAvailableForSwap = balanceOf(address(this));
		if (tokensAvailableForSwap >= _tokenSwapThreshold) {

			// Limit to threshold & max transaction amount
			tokensAvailableForSwap = _tokenSwapThreshold;
			if (tokensAvailableForSwap > _maxTransactionAmount)
			{
				tokensAvailableForSwap = _maxTransactionAmount;
			}

			// Make sure that we are not stuck in a loop (Swap only once)
			bool isToPancakeSwap = sender == address(this) && recipient == _pancakeswapV2Pair;
			if (!isToPancakeSwap && sender != _pancakeswapV2Pair) {
				executeSwap(tokensAvailableForSwap);
			}
		}
	}
	

	function executeSwap(uint256 amount) private {
		// The amount includes both the liquidity and the reward tokens, find the correct portion for each one so that they are allocated accordingly
		// Inverse rates to avoid floating point result
		uint256 liquidityRatio = _poolFee / _liquidityFee;
		uint256 rewardRatio = _poolFee / _rewardFee;
		
		uint256 tokensReservedForLiquidity = amount / liquidityRatio;
		uint256 tokensReservedForReward = amount / rewardRatio;

		// For the liquidity portion, half of it will be swapped for BNB and the other half will be used to add the BNB into the liquidity
		uint256 tokensToSwapForLiquidity = tokensReservedForLiquidity / 2;
		uint256 tokensToAddAsLiquidity = tokensToSwapForLiquidity;

		// Swap both reward tokens and liquidity tokens for BNB
		uint256 tokensToSwap = tokensReservedForReward + tokensToSwapForLiquidity;
		uint256 bnbSwapped = swapTokensForBNB(tokensToSwap);
		
		// Calculate what portion of the swapped BNB is for liquidity and supply it using the other half of the token liquidity portion.  The remaining BNBs in the contract represent the reward pool
		uint256 bnbLiquidityRatio = tokensToSwap / tokensToSwapForLiquidity;
		uint256 bnbToBeAddedToLiquidity = bnbSwapped / bnbLiquidityRatio;
        _pancakeswapV2Router.addLiquidityETH{value: bnbToBeAddedToLiquidity}(address(this), tokensToAddAsLiquidity, 0, 0, owner(), block.timestamp + 360);

        emit Swapped(tokensToSwap, bnbSwapped, tokensToAddAsLiquidity, bnbToBeAddedToLiquidity);
    }
	
	
	// This function swaps a {tokenAmount} of stellar tokens for BNB and returns the total amount of BNB received
	function swapTokensForBNB(uint256 tokenAmount) private  returns(uint256) {
		uint256 initialBalance = address(this).balance;
		
		// Generate pair for Stellar -> WBNB
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _pancakeswapV2Router.WETH();

		// Swap
        _pancakeswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(tokenAmount, 0, path, address(this), block.timestamp + 360);
		
		// Return the amount received
		return address(this).balance - initialBalance;
    }


	function claimReward() isHuman nonReentrant public {
        require(_nextAvailableClaimDate[msg.sender] <= block.timestamp, 'Claim date for this address has not passed yet');
        require(balanceOf(msg.sender) >= 0, 'The address must own STELLAR before claiming a reward');

        uint256 reward = calculateBNBReward(msg.sender);

        // If reward is over the charity threshold
        if (reward >= _charityThreshold) {

            // Use a percentage of it to transfer it to charity wallet
            uint256 charityamount = reward * _charityPercentage / 100;
            (bool success, ) = _charityAddress.call{ value: charityamount }("");
            require(success, "Charity transaction failed");    
            
            reward = reward - charityamount;
        }

        // Update the next claim date
        _nextAvailableClaimDate[msg.sender] = block.timestamp + getRewardCyclePeriod();
        emit BNBClaimed(msg.sender, reward, _nextAvailableClaimDate[msg.sender]);

		// Send the reward to the caller
        (bool sent,) = msg.sender.call{value : reward}("");
        require(sent, 'Reward transaction failed');
    }

    function setClaimDate(address addr, uint256 date) public onlyOwner {
        _nextAvailableClaimDate[addr] = date;
    }
    
    function setCharityThreshold(uint256 threshold) public onlyOwner {
        _charityThreshold = threshold;
    }
    
    function setCharityAddress(address payable addr) public onlyOwner {
        _charityAddress = addr;
    }

	function calculateRewardCycleExtension(uint256 balance, uint256 amount) public view returns (uint256) {
		uint256 basePeriod = getRewardCyclePeriod();

        if (balance == 0) {
            return block.timestamp + basePeriod;
        }

		uint256 rate = amount / balance * 100;

        if (rate >= _rewardCycleExtensionThreshold) {
            uint256 extension = basePeriod * rate / 100;

            if (extension >= basePeriod) {
                extension = basePeriod;
            }

            return extension;
        }

        return 0;
    }


	function calculateBNBReward(address ofAddress) public view returns (uint256) {
        uint256 holdersAmount = getTotalAmountOfTokensHeld();

		uint256 balance = balanceOf(ofAddress);
		uint256 bnbPool =  address(this).balance;

		return bnbPool * balance / holdersAmount;
    }


	function getRewardCyclePeriod() public view returns (uint256) {
		return _rewardCyclePeriod;
    }


	function getTotalAmountOfTokensHeld() public view returns (uint256) {
		return _totalTokens - balanceOf(address(0)) - balanceOf(_burnWallet) - balanceOf(_pancakeswapV2Pair);
	}


	function name() public override pure returns (string memory) {
        return _name;
    }


    function symbol() public override pure returns (string memory) {
        return _symbol;
    }


    function totalSupply() public override pure returns (uint256) {
        return _totalTokens;
    }
	

	function decimals() public override pure returns (uint8) {
        return _decimals;
    }
	

	function totalFeesDistributed() public view returns (uint256) {
        return _totalFeesDistributed;
    }
	

	function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }
	
	
	function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        doApprove(_msgSender(), spender, _allowances[_msgSender()][spender] + addedValue);
        return true;
    }

	
    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        doApprove(_msgSender(), spender, _allowances[_msgSender()][spender] - subtractedValue);
        return true;
    }

	
	function onOwnershipTransferred(address previousOwner, address newOwner) internal override {
		// This is to make sure that once ownership is renounced, the original owner is no longer excluded from fees
		_addressesExcludedFromFees[previousOwner] = false;
		_addressesExcludedFromFees[newOwner] = true;
	}

	function getAmountUntilSwap() public view returns (uint256) {
		uint256 balance = balanceOf(address(this));
		if (balance > _tokenSwapThreshold) {
			// Swap on next relevant transaction
			return 0;
		}

		return _tokenSwapThreshold - balance;
	}

	function getMaxTransactionAmount() public pure returns (uint256) {
		return _maxTransactionAmount;
	}

	function getPancakeSwapRouterAddress() public view returns (address) {
		return _pancakeSwapRouterAddress;
	}

	function getPancakeSwapPairAddress() public view returns (address) {
		return _pancakeswapV2Pair;
	}

	function getTotalFeesDistributed() public view returns (uint256) {
		return _totalFeesDistributed;
	}

	function getTotalFeesPooled() public view returns (uint256) {
		return _totalFeesPooled;
	}

	function getTotalBNBLiquidityAddedFromFees() public view returns (uint256) {
		return _totalBNBLiquidityAddedFromFees;
	}

	// Needed to receive BNB after swap
	receive() external payable {}
}