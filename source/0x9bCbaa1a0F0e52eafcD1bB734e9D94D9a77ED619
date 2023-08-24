// SPDX-License-Identifier: MIT
pragma solidity ^0.6.12;
/**
* @dev Wrappers over Solidity's arithmetic operations with added overflow
* checks.
*
* Arithmetic operations in Solidity wrap on overflow. This can easily result
* in bugs, because programmers usually assume that an overflow raises an
* error, which is the standard behavior in high level programming languages.
* `SafeMath` restores this intuition by reverting the transaction when an
* operation overflows.
*
* Using this library instead of the unchecked operations eliminates an entire
* class of bugs, so it's recommended to use it always.
*/
library SafeMath {
        /**
        * @dev Returns the addition of two unsigned integers, reverting on
        * overflow.
        *
        * Counterpart to Solidity's `+` operator.
        *
        * Requirements:
        * - Addition cannot overflow.
        */
        function add(uint256 a, uint256 b) internal pure returns (uint256) {
                uint256 c = a + b;
                require(c >= a, "SafeMath: addition overflow");
                return c;
        }
        /**
        * @dev Returns the subtraction of two unsigned integers, reverting on
        * overflow (when the result is negative).
        *
        * Counterpart to Solidity's `-` operator.
        *
        * Requirements:
        * - Subtraction cannot overflow.
        */
        function sub(uint256 a, uint256 b) internal pure returns (uint256) {
                return sub(a, b, "SafeMath: subtraction overflow");
        }
        /**
        * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
        * overflow (when the result is negative).
        *
        * Counterpart to Solidity's `-` operator.
        *
        * Requirements:
        * - Subtraction cannot overflow.
        */
        function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
                require(b <= a, errorMessage);
                uint256 c = a - b;
                return c;
        }
        /**
        * @dev Returns the multiplication of two unsigned integers, reverting on
        * overflow.
        *
        * Counterpart to Solidity's `*` operator.
        *
        * Requirements:
        * - Multiplication cannot overflow.
        */
        function mul(uint256 a, uint256 b) internal pure returns (uint256) {
                // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
                // benefit is lost if 'b' is also tested.
                // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
                if (a == 0) {
                        return 0;
                }
                uint256 c = a * b;
                require(c / a == b, "SafeMath: multiplication overflow");
                return c;
        }
        /**
        * @dev Returns the integer division of two unsigned integers. Reverts on
        * division by zero. The result is rounded towards zero.
        *
        * Counterpart to Solidity's `/` operator. Note: this function uses a
        * `revert` opcode (which leaves remaining gas untouched) while Solidity
        * uses an invalid opcode to revert (consuming all remaining gas).
        *
        * Requirements:
        * - The divisor cannot be zero.
        */
        function div(uint256 a, uint256 b) internal pure returns (uint256) {
                return div(a, b, "SafeMath: division by zero");
        }
        /**
        * @dev Returns the integer division of two unsigned integers. Reverts with custom message on
        * division by zero. The result is rounded towards zero.
        *
        * Counterpart to Solidity's `/` operator. Note: this function uses a
        * `revert` opcode (which leaves remaining gas untouched) while Solidity
        * uses an invalid opcode to revert (consuming all remaining gas).
        *
        * Requirements:
        * - The divisor cannot be zero.
        */
        function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
                // Solidity only automatically asserts when dividing by 0
                require(b > 0, errorMessage);
                uint256 c = a / b;
                // assert(a == b * c + a % b); // There is no case in which this doesn't hold
                return c;
        }
        /**
        * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
        * Reverts when dividing by zero.
        *
        * Counterpart to Solidity's `%` operator. This function uses a `revert`
        * opcode (which leaves remaining gas untouched) while Solidity uses an
        * invalid opcode to revert (consuming all remaining gas).
        *
        * Requirements:
        * - The divisor cannot be zero.
        */
        function mod(uint256 a, uint256 b) internal pure returns (uint256) {
                return mod(a, b, "SafeMath: modulo by zero");
        }
        /**
        * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
        * Reverts with custom message when dividing by zero.
        *
        * Counterpart to Solidity's `%` operator. This function uses a `revert`
        * opcode (which leaves remaining gas untouched) while Solidity uses an
        * invalid opcode to revert (consuming all remaining gas).
        *
        * Requirements:
        * - The divisor cannot be zero.
        */
        function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
                require(b != 0, errorMessage);
                return a % b;
        }
}
/*
* @dev Provides information about the current execution context, including the
* sender of the transaction and its data. While these are generally available
* via msg.sender and msg.data, they should not be accessed in such a direct
* manner, since when dealing with GSN meta-transactions the account sending and
* paying for execution may not be the actual sender (as far as an application
* is concerned).
*
* This contract is only required for intermediate, library-like contracts.
*/
contract Context {
        // Empty internal constructor, to prevent people from mistakenly deploying
        // an instance of this contract, which should be used via inheritance.
        constructor () internal { }
        function _msgSender() internal view returns (address payable) {
                return msg.sender;
        }
        function _msgData() internal view returns (bytes memory) {
                this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
                return msg.data;
        }
}
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
contract Ownable is Context {
        address private _owner;
        event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
        /**
        * @dev Initializes the contract setting the deployer as the initial owner.
        */
        constructor () internal {
                address msgSender = _msgSender();
                _owner = msgSender;
                emit OwnershipTransferred(address(0), msgSender);
        }
        /**
        * @dev Returns the address of the current owner.
        */
        function owner() public view returns (address) {
                return _owner;
        }
        /**
        * @dev Throws if called by any account other than the owner.
        */
        modifier onlyOwner() {
                require(_owner == _msgSender(), "Ownable: caller is not the owner");
                _;
        }
        /**
        * @dev Leaves the contract without owner. It will not be possible to call
        * `onlyOwner` functions anymore. Can only be called by the current owner.
        *
        * NOTE: Renouncing ownership will leave the contract without an owner,
        * thereby removing any functionality that is only available to the owner.
        */
        function renounceOwnership() public onlyOwner {
                emit OwnershipTransferred(_owner, address(0));
                _owner = address(0);
        }
        /**
        * @dev Transfers ownership of the contract to a new account (`newOwner`).
        * Can only be called by the current owner.
        */
        function transferOwnership(address newOwner) public onlyOwner {
                _transferOwnership(newOwner);
        }
        /**
        * @dev Transfers ownership of the contract to a new account (`newOwner`).
        */
        function _transferOwnership(address newOwner) internal {
                require(newOwner != address(0), "Ownable: new owner is the zero address");
                emit OwnershipTransferred(_owner, newOwner);
                _owner = newOwner;
        }
}
interface IPancakeSwapRouter {
        function swapExactTokensForTokens(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline) external returns (uint[] memory amounts);
        function swapTokensForExactTokens(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline) external returns (uint[] memory amounts);
        function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline) external payable returns (uint[] memory amounts);
        function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline) external returns (uint[] memory amounts);
        function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline) external returns (uint[] memory amounts);
        function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline) external payable returns (uint[] memory amounts);
        // Get Data
        function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
        function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
        function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
        function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}
abstract contract IAlpacaFairLaunch {
	// Info of each user that stakes Staking tokens.
  	mapping(uint256 => mapping(address => UserInfo)) public userInfo;
  	// Info of each user.
  	struct UserInfo {
    	uint256 amount; // How many Staking tokens the user has provided.
    	uint256 rewardDebt; // Reward debt. See explanation below.
    	uint256 bonusDebt; // Last block that user exec something to the pool.
    	address fundedBy; // Funded by who?
	    //
	    // We do some fancy math here. Basically, any point in time, the amount of ALPACAs
	    // entitled to a user but is pending to be distributed is:
	    //
	    //   pending reward = (user.amount * pool.accAlpacaPerShare) - user.rewardDebt
	    //
	    // Whenever a user deposits or withdraws Staking tokens to a pool. Here's what happens:
	    //   1. The pool's `accAlpacaPerShare` (and `lastRewardBlock`) gets updated.
	    //   2. User receives the pending reward sent to his/her address.
	    //   3. User's `amount` gets updated.
	    //   4. User's `rewardDebt` gets updated.
  	}
  	function pendingAlpaca(uint256 _pid, address _user) external view returns (uint256) {}
	// Deposit Staking tokens to FairLaunchToken for ALPACA allocation.
  	function deposit(address _for, uint256 _pid, uint256 _amount) external {}
  	// Withdraw Staking tokens from FairLaunchToken.
  	function withdraw(address _for, uint256 _pid, uint256 _amount) external {}
  	function withdrawAll(address _for, uint256 _pid) external {}
  	// Harvest ALPACAs earn from the pool.
  	function harvest(uint256 _pid) external {}
}
interface IBEP20 {
        /**
        * @dev Returns the amount of tokens in existence.
        */
        function totalSupply() external view returns (uint256);
        /**
        * @dev Returns the token decimals.
        */
        function decimals() external view returns (uint8);
        /**
        * @dev Returns the token symbol.
        */
        function symbol() external view returns (string memory);
        /**
        * @dev Returns the token name.
        */
        function name() external view returns (string memory);
        /**
        * @dev Returns the bep token owner.
        */
        function getOwner() external view returns (address);
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
        function allowance(address _owner, address spender) external view returns (uint256);
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
interface IAlpacaVault {
	/**
   	* @dev Returns the amount of tokens in existence.
   	*/
        function totalSupply() external view returns (uint256);
        function totalToken() external view returns(uint256);
        function balanceOf(address owner) external view returns (uint);
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
        function transfer(address recipient, uint256 amount) external returns (bool);
        function approve(address spender, uint256 amount) external returns (bool); 
        function deposit(uint256 amountToken) external payable;
        function withdraw(uint256 share) external;
}
interface IDevDefiTimelock {
	function isQueuedTransaction(address _contractRequest, string memory _fn) external view returns(bool);

	function doneTransaction(string memory _fn) external;
}
interface IDevDefiPool {
        function getTotalSupply() external view returns(uint256); 
}
interface IWBNBToken {
        function deposit() external payable;
        function withdraw(uint wad) external;
}
contract StrategyBNBFarmAlpaca is Ownable {
        using SafeMath for uint256;
        IAlpacaVault public IBBNB; // 
        IBEP20 public WBNB; // 
        IBEP20 public ALPACA; // 
        uint256 public pidOfAlpacaVault;
        uint256 public PERIOD_DAY = 1 days;
        uint256 public totalSupply = 0; 
        uint256 public timeOfSupply = 0;
        bool public isDisableDepositToAlpaca = false;
        IDevDefiTimelock public DevDefiTimelockContract;
        IPancakeSwapRouter public PancakeSwapRouterContract;
        IAlpacaFairLaunch public AlpacaFairLaunchContract;
        address public devDefiPool;
        modifier onlyOwnerOrDevDefiPool()
        {
                require(msg.sender == owner() || msg.sender == devDefiPool, 'INVALID_PERMISSION');
                _;
        }
        modifier isQueued(string memory _fn) 
        {
                require(DevDefiTimelockContract.isQueuedTransaction(address(this), _fn) == true, 'INVALID_PERMISTION');
                _;
                DevDefiTimelockContract.doneTransaction(_fn);
        }
        event onWithdrawAll(uint256 _amountVToken);
        event onClaimOtherToken(address _tokenAddr, uint256 _amountToken);
        event onReleaseFundToDevDefiPool(address _devDefiPool, uint256 _amountToken);
        event onProcessFee(address _to, uint256 _amount);
        event onWithdraw(address _devDefiPool, uint256 _amountToken);
        event onHarvest(uint256 _amountXVS, uint256 _amountToken);

        constructor(
                IAlpacaVault _ibBNB,
                IBEP20 _wbnb,
                IBEP20 _alpaca,
                IDevDefiTimelock _devdefiTimelockContract,
                IPancakeSwapRouter _pancakeSwapRouter,
                IAlpacaFairLaunch _alpacaFairLaunch,

                uint256 _pidOfAlpacaVault
        ) public {
                IBBNB = _ibBNB;
                WBNB = _wbnb;
                ALPACA = _alpaca;
                DevDefiTimelockContract = _devdefiTimelockContract;
                PancakeSwapRouterContract = _pancakeSwapRouter;
                AlpacaFairLaunchContract = _alpacaFairLaunch;
                pidOfAlpacaVault = _pidOfAlpacaVault; 
        }
        receive() external payable {}
        /**
        * Action::setDevDefiPool
        */
        function setDevDefiPool(address _addr) external onlyOwner isQueued('setDevDefiPool') 
        {
                require(_addr != address(0), "[INVALID]::ADDRESS");
                devDefiPool = _addr;
        }
        /**
        * Action::setPancakeSwapRouterContract
        */
        function setPancakeSwapRouterContract(IPancakeSwapRouter _addr) external onlyOwner isQueued('setPancakeSwapRouterContract') 
        {
                require(address(_addr) != address(0), "[INVALID]::ADDRESS");
                PancakeSwapRouterContract = _addr;
        }
        /**
        * Action::setAlpacaFairLaunchContract
        */
        function setAlpacaFairLaunchContract(IAlpacaFairLaunch _addr) external onlyOwner isQueued('setAlpacaFairLaunchContract') 
        {
                require(address(_addr) != address(0), "[INVALID]::ADDRESS");
                AlpacaFairLaunchContract = _addr;
        }
        /**
        * Action::setPidOfAlpacaVault
        */
        function setPidOfAlpacaVault(uint256 _pid) external onlyOwner isQueued('setPidOfAlpacaVault') 
        {
                pidOfAlpacaVault = _pid;
        }
        function connectToPancakeAndAlpaca() external onlyOwner {
                ALPACA.approve(address(PancakeSwapRouterContract), uint256(-1));
                IBBNB.approve(address(AlpacaFairLaunchContract), uint256(-1));
        }
        function claimOtherToken(address tokenAddress, uint tokenAmount) external onlyOwner 
        {
                require(tokenAddress != address(0), 'INVALID: ADDRESS');
                require(tokenAddress != address(IBBNB), 'INVALID: cannot recover IBBNB');
                require(tokenAddress != address(WBNB), 'INVALID: cannot recover WBNB');
                require(tokenAddress != address(ALPACA), 'INVALID: cannot recover ALPACA');
                IBEP20(tokenAddress).transfer(owner(), tokenAmount);
                emit onClaimOtherToken(tokenAddress, tokenAmount);
        }
        function disableToAlpaca() external onlyOwner 
        {
                isDisableDepositToAlpaca = true;
        }
        function enableToAlpaca() external onlyOwner 
        {
                isDisableDepositToAlpaca = false;
        }
        function harvest() public onlyOwnerOrDevDefiPool returns(uint256) 
        {
                _withdrawFromAlpaca();
                if (isDisableDepositToAlpaca == false) {
                        _depositToAlpaca();
                }
        }
        function releaseFundToDevDefiPool() public onlyOwnerOrDevDefiPool 
        {
                require(devDefiPool != address(0), "INVALID_DEV_DEFI_ADDRESS");
                _withdrawFromAlpaca();
                _convertBNBToWBNB();
                uint256 _availableBal = WBNB.balanceOf(address(this));
                if (_availableBal > 0) {
                        WBNB.transfer(devDefiPool, _availableBal);
                        _updateTotalSupply();
                        emit onReleaseFundToDevDefiPool(devDefiPool, _availableBal);
                }   
        }
        function withdraw(uint256 _amountToken) public onlyOwnerOrDevDefiPool returns(uint256) {
                require(_amountToken > 0, 'INVALID_AMOUNT_TOKEN');
                _withdrawFromAlpaca();
                _convertBNBToWBNB();
                uint256 _availableBal = WBNB.balanceOf(address(this));
                require(_availableBal >= _amountToken, 'INVALID_BALANCE');
                WBNB.transfer(devDefiPool, _amountToken);
                _depositToAlpaca();
                emit onWithdraw(devDefiPool, _amountToken);
                return _amountToken;
        }
        /*
        * PRIVATE ACTION
        */
        function getContractBalance() public view returns(uint256) {
                uint256 _totalIbBNBValue  = getTotalBNBValueOfIbBNB();
                return availableBal().add(_totalIbBNBValue);
        }
        function _convertBNBToWBNB() private {
                uint256 _bnbBal = address(this).balance;
                if (_bnbBal > 0) {
                        IWBNBToken(address(WBNB)).deposit{value : _bnbBal}();
                }
        }
        function _convertWBNBToBNB() private {
                uint256 _wbnbBal = WBNB.balanceOf(address(this));
                if (_wbnbBal > 0) {
                        IWBNBToken(address(WBNB)).withdraw(_wbnbBal);
                }
        }
        function _convertBNBToIbBNB() private 
        {
                uint256 _bnbBal = address(this).balance;
                // Convert BNB to ibBNB
                if (_bnbBal > 0) {
                        IBBNB.deposit{ value: _bnbBal }(_bnbBal);
                }
        }
        function _convertIbBNBToBNB() private 
        {
                uint256 _ibBNBBal = IBBNB.balanceOf(address(this));
                // Convert ibBNB to BNB
                if (_ibBNBBal > 0) {
                        IBBNB.withdraw(_ibBNBBal);
                }
        }
        function _convertAlpacaToBNB() private returns(uint256)
        {
                uint256 _alpacaBal = ALPACA.balanceOf(address(this));
                if (_alpacaBal > 0) {
                        address[] memory path = new address[](2);
                        path[0] = address(ALPACA);
                        path[1] = address(WBNB);
                        PancakeSwapRouterContract.swapExactTokensForETH(_alpacaBal, 0, path, address(this), block.timestamp);
                }
        }
        function _depositToAlpaca() private 
        {
                _convertWBNBToBNB();
                _convertBNBToIbBNB();
                uint256 _ibBNBBal = IBBNB.balanceOf(address(this));
                if (_ibBNBBal > 0) {
                        AlpacaFairLaunchContract.deposit(address(this), pidOfAlpacaVault, _ibBNBBal);
                } 
                _updateTotalSupply();
        }
        function _withdrawFromAlpaca() private 
        {
                // get ibBNB on Alpaca
                (uint256 _ibBNBBalOnAlpaca, , ,) = AlpacaFairLaunchContract.userInfo(pidOfAlpacaVault, address(this));
                if (_ibBNBBalOnAlpaca > 0) {
                        AlpacaFairLaunchContract.withdrawAll(address(this), pidOfAlpacaVault);
                }
                _convertIbBNBToBNB();
                _convertAlpacaToBNB();
        }
        function _updateTotalSupply() private {
                totalSupply = getTotalBNBValueOfIbBNB();
                timeOfSupply = block.timestamp;
        }
        function availableBal() public view returns(uint256) 
        {
                return WBNB.balanceOf(address(this)).add(address(this).balance);
        }
        function getTotalBNBValueOfIbBNB() public view returns(uint256) 
        {
                uint256 _ibBNBBal = getTotalIbBNB();
                if (_ibBNBBal <= 0) {
                        return 0;
                }
                // Convert ibBNB to BNB
                return _ibBNBBal.mul(IBBNB.totalToken()).div(IBBNB.totalSupply());
        }
        function getTotalBNBValueOfAlpaca() public view returns(uint256) 
        {
                address[] memory path = new address[](2);
                path[0] = address(ALPACA);
                path[1] = address(WBNB);
                // Get ALPACA balance
                uint256 _alpacaBal = ALPACA.balanceOf(address(this));
                _alpacaBal = _alpacaBal.add(getPendingAlpaca());
                if (_alpacaBal <= 0) {
                        return 0;
                }
                // Convert ALPACA to BNB
                uint256 _totalBNBValueOfAlpaca;
                try PancakeSwapRouterContract.getAmountsOut(_alpacaBal, path) returns(uint[] memory amounts) {
                        _totalBNBValueOfAlpaca = amounts[1];
                } catch {
                        _totalBNBValueOfAlpaca = 0;   
                }
                return _totalBNBValueOfAlpaca;
        }
        function getTotalIbBNB() public view returns(uint256) 
        {
                // get ibBNB on Alpaca
                (uint256 _ibBNBBal, , ,) = AlpacaFairLaunchContract.userInfo(pidOfAlpacaVault, address(this));
                return IBBNB.balanceOf(address(this)).add(_ibBNBBal);
        }
        function getPendingAlpaca() public view returns(uint256) 
        {
                return AlpacaFairLaunchContract.pendingAlpaca(pidOfAlpacaVault, address(this));
        }
        function getSupplyApy(uint256 _rateOfControllerFee) public view returns(uint256) {
                return _getSupplyApy(_rateOfControllerFee).add(_getAlpacaApy(_rateOfControllerFee));
        } 
        function _getSupplyApy(uint256 _rateOfControllerFee) public view returns(uint256) 
        {
                if (totalSupply <= 0) {
                        return 0;
                }
                if (timeOfSupply <=0) { 
                        return 0;
                }
                uint256 _totalBNBBal = getTotalBNBValueOfIbBNB();
                if (_totalBNBBal <= totalSupply) {
                        return 0;
                }
                uint256 _reward = _totalBNBBal.sub(totalSupply);
                // sub fee
                _reward = _reward.sub(_reward.mul(_rateOfControllerFee).div(10000));
                uint256 _totalSupply = IDevDefiPool(devDefiPool).getTotalSupply();
                return _reward.mul(PERIOD_DAY).mul(365).mul(1e12).div(block.timestamp.sub(timeOfSupply)).div(_totalSupply);
        }
        function _getAlpacaApy(uint256 _rateOfControllerFee) public view returns(uint256) 
        {
                if (totalSupply <= 0) {
                        return 0;
                }
                if (timeOfSupply <=0) { 
                        return 0;
                }
                uint256 _reward = getTotalBNBValueOfAlpaca();
                // sub fee
                _reward = _reward.sub(_reward.mul(_rateOfControllerFee).div(10000));
                uint256 _totalSupply = IDevDefiPool(devDefiPool).getTotalSupply();
                return _reward.mul(PERIOD_DAY).mul(365).mul(1e12).div(block.timestamp.sub(timeOfSupply)).div(_totalSupply);
        }
}