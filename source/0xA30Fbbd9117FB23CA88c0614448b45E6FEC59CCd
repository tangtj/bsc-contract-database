// SPDX-License-Identifier: MIT
// File: @openzeppelin/contracts/utils/math/SafeMath.sol


// OpenZeppelin Contracts (last updated v4.9.0) (utils/math/SafeMath.sol)

pragma solidity ^0.8.0;

// CAUTION
// This version of SafeMath should only be used with Solidity 0.8 or later,
// because it relies on the compiler's built in overflow checks.

/**
 * @dev Wrappers over Solidity's arithmetic operations.
 *
 * NOTE: `SafeMath` is generally not needed starting with Solidity 0.8, since the compiler
 * now has built in overflow checking.
 */
library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
            // benefit is lost if 'b' is also tested.
            // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
            if (a == 0) return (true, 0);
            uint256 c = a * b;
            if (c / a != b) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the division of two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

    /**
     * @dev Returns the addition of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     *
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `*` operator.
     *
     * Requirements:
     *
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator.
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * reverting when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {trySub}.
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting with custom message on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * reverting with custom message when dividing by zero.
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {tryMod}.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}

// File: @openzeppelin/contracts/utils/Context.sol


// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)

pragma solidity ^0.8.0;

/**
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
        return msg.data;
    }
}

// File: @openzeppelin/contracts/access/Ownable.sol


// OpenZeppelin Contracts (last updated v4.9.0) (access/Ownable.sol)

pragma solidity ^0.8.0;


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
    constructor() {
        _transferOwnership(_msgSender());
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if the sender is not the owner.
     */
    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby disabling any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)

pragma solidity ^0.8.0;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
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

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

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
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

// File: contracts/DepositAndWithdrawO7SP.sol

//deps/npm/@openzeppelin/contracts/token/ERC20/IERC20.sol

//deps/npm/@openzeppelin/contracts/access/Ownable.sol

//deps/npm/@openzeppelin/contracts/utils/math/SafeMath.sol

pragma solidity ^0.8.0;




contract DepositAndWithdrawO7SP is Ownable {
    using SafeMath for uint256;

    struct UserTokenActions {
        uint256 depositedAmount;
        uint256 withdrawalCount;
        uint256 lastDepositTimestamp;
    }

    mapping(address => mapping(address => UserTokenActions)) private userActions;
    mapping(address => bool) private supportedTokens;
    address[] private supportedTokenList;

    address public tokenAddress;
    address public recipient;
    uint256 public withdrawalCooldownPeriod = 30 days;
    uint256 public withdrawalPercentage = 30;
    uint256 public depositCooldownPeriod = 30 days;
    address public tetherUSDTokenAddress;
    uint256 public minWithdrawalAmount = 1;
    uint256 public minDepositAmount = 1;

    event Deposit(address indexed user, address indexed token, uint256 amount);
    event OwnerWithdrawal(address indexed owner, address indexed token, uint256 amount, address indexed recipient);
    event OwnerTokenTransfer(address indexed owner, address indexed token, address indexed recipient, uint256 amount);
    event TokenAddressUpdated(address indexed previousAddress, address indexed newAddress);
    event RecipientUpdated(address indexed previousAddress, address indexed newAddress);
    event WithdrawalCooldownPeriodUpdated(uint256 previousPeriod, uint256 newPeriod);
    event WithdrawalPercentageUpdated(uint256 previousPercentage, uint256 newPercentage);
    event DepositCooldownPeriodUpdated(uint256 previousPeriod, uint256 newPeriod);
    event TetherUSDTokenAddressUpdated(address indexed previousAddress, address indexed newAddress);
    event MinWithdrawalAmountUpdated(uint256 previousAmount, uint256 newAmount);
    event MinDepositAmountUpdated(uint256 previousAmount, uint256 newAmount);

    modifier onlySupportedToken(address _tokenAddress) {
        require(supportedTokens[_tokenAddress], "Token not supported");
        _;
    }

    modifier onlySufficientAllowance(address _tokenAddress, uint256 amount) {
        IERC20 token = IERC20(_tokenAddress);
        require(token.allowance(msg.sender, address(this)) >= amount, "Allowance not sufficient for deposit");
        _;
    }

    modifier onlyPositiveAmount(uint256 amount) {
        require(amount >= minDepositAmount, "Amount must be greater than or equal to the minimum deposit amount");
        _;
    }

    modifier onlySufficientBalance(address _tokenAddress, uint256 amount) {
        IERC20 token = IERC20(_tokenAddress);
        require(token.balanceOf(address(this)) >= amount, "Insufficient contract balance");
        _;
    }

    modifier isValidRecipient(address _recipient) {
        require(_recipient != address(0), "Invalid recipient address");
        _;
    }

    modifier onlyValidWithdrawal(address _tokenAddress, address _recipient) {
        require(
            userActions[msg.sender][_tokenAddress].depositedAmount > 0,
            "No tokens to withdraw"
        );
        require(
            userActions[msg.sender][_tokenAddress].lastDepositTimestamp + withdrawalCooldownPeriod <= block.timestamp,
            "Withdrawal cooldown period has not passed"
        );
        _;
    }

    modifier onlyValidOwnerWithdrawal(address _tokenAddress, address _recipient, uint256 amount) {
        require(
            owner() == msg.sender,
            "Only the owner can perform this action"
        );
        require(
            amount >= minWithdrawalAmount,
            "Withdrawal amount must be greater than or equal to the minimum withdrawal amount"
        );
        _;
    }

    modifier onlyValidTransferTokensByOwner(address _tokenAddress, address _recipient, uint256 amount) {
        require(
            owner() == msg.sender,
            "Only the owner can perform this action"
        );
        require(
            amount >= minWithdrawalAmount,
            "Transfer amount must be greater than or equal to the minimum withdrawal amount"
        );
        _;
    }

    constructor() {
        // Initialize the token and recipient addresses here
        tokenAddress = 0x2Cd3BD848C3d6BbeB85466A52Ec77290523bd4D7;
        recipient = 0x7B933037bbb4a3bC01276A98AC27b9f3575858C6;
        tetherUSDTokenAddress = address(0); // Set Tether USD token address to address(0) initially.

        // Initialize with the contract owner's address as the first supported token
        supportedTokens[owner()] = true;
        supportedTokenList.push(owner());
    }

    // Function to update the token address by the contract owner
    function updateTokenAddress(address newTokenAddress) external onlyOwner {
        require(newTokenAddress != address(0), "Invalid token address");
        emit TokenAddressUpdated(tokenAddress, newTokenAddress);
        tokenAddress = newTokenAddress;
    }

    // Function to update the recipient address by the contract owner
    function updateRecipientAddress(address newRecipient) external onlyOwner {
        require(newRecipient != address(0), "Invalid recipient address");
        emit RecipientUpdated(recipient, newRecipient);
        recipient = newRecipient;
    }

    // Function to add support for a new token
    function addSupportedToken(address _tokenAddress) external onlyOwner {
        require(!isTokenSupported(_tokenAddress), "Token already supported");
        supportedTokens[_tokenAddress] = true;
        supportedTokenList.push(_tokenAddress);
    }

    // Function to deposit tokens into the contract
    function deposit(address _tokenAddress, uint256 amount) external
        onlySupportedToken(_tokenAddress)
        onlyPositiveAmount(amount)
        onlySufficientAllowance(_tokenAddress, amount)
    {
        UserTokenActions storage userActionsData = userActions[msg.sender][_tokenAddress];

        // Effects
        IERC20 token = IERC20(_tokenAddress);
        require(token.transferFrom(msg.sender, address(this), amount), "Token transfer failed");
        userActionsData.depositedAmount = userActionsData.depositedAmount.add(amount);
        userActionsData.lastDepositTimestamp = block.timestamp;

        // Interactions
        emit Deposit(msg.sender, _tokenAddress, amount);
    }

    // Function to withdraw tokens from the contract
    function withdraw(address _tokenAddress, uint256 amount) external
        onlyValidWithdrawal(_tokenAddress, msg.sender)
        onlyPositiveAmount(amount)
    {
        UserTokenActions storage userActionsData = userActions[msg.sender][_tokenAddress];

        // Effects
        IERC20 token = IERC20(_tokenAddress);
        require(token.transfer(msg.sender, amount), "Token transfer failed");
        userActionsData.depositedAmount = userActionsData.depositedAmount.sub(amount);
        userActionsData.lastDepositTimestamp = block.timestamp;

        // Interactions
        emit Deposit(msg.sender, _tokenAddress, amount);
    }

    // Function for the owner to withdraw tokens to a specified recipient address
    function ownerWithdrawal(address _tokenAddress, uint256 amount, address _recipient) external
        onlyValidOwnerWithdrawal(_tokenAddress, _recipient, amount)
        onlySufficientBalance(_tokenAddress, amount)
        onlySupportedToken(_tokenAddress)
        isValidRecipient(_recipient)
    {
        // Effects
        IERC20 token = IERC20(_tokenAddress);
        require(token.transfer(_recipient, amount), "Token transfer failed");

        // Interactions
        emit OwnerWithdrawal(msg.sender, _tokenAddress, amount, _recipient);
    }

    // Function for the owner to transfer tokens to a specified recipient address
    function ownerTransferTokens(address _tokenAddress, uint256 amount, address _recipient) external
        onlyValidTransferTokensByOwner(_tokenAddress, _recipient, amount)
        onlySufficientBalance(_tokenAddress, amount)
        onlySupportedToken(_tokenAddress)
        isValidRecipient(_recipient)
    {
        // Effects
        IERC20 token = IERC20(_tokenAddress);
        require(token.transfer(_recipient, amount), "Token transfer failed");

        // Interactions
        emit OwnerTokenTransfer(msg.sender, _tokenAddress, _recipient, amount);
    }

    // Getter function to check if a specific token is supported
    function isTokenSupported(address _tokenAddress) public view returns (bool) {
        return supportedTokens[_tokenAddress];
    }

    // Getter function to retrieve the list of supported tokens
    function getSupportedTokens() external view returns (address[] memory) {
        return supportedTokenList;
    }

    // Getter function to check the balance of a specific token in the contract
    function getContractTokenBalance(address _tokenAddress) external view returns (uint256) {
        IERC20 token = IERC20(_tokenAddress);
        return token.balanceOf(address(this));
    }

    // Setter function to set the recipient address
    function setRecipientAddress(address _recipient) external onlyOwner isValidRecipient(_recipient) {
        recipient = _recipient;
    }

    // Getter function to retrieve the recipient address
    function getRecipientAddress() external view returns (address) {
        return recipient;
    }

    // Setter function to set the withdrawal cooldown period
    function setWithdrawalCooldownPeriod(uint256 newCooldownPeriod) external onlyOwner {
        emit WithdrawalCooldownPeriodUpdated(withdrawalCooldownPeriod, newCooldownPeriod);
        withdrawalCooldownPeriod = newCooldownPeriod;
    }

    // Setter function to set the withdrawal percentage
    function setWithdrawalPercentage(uint256 newPercentage) external onlyOwner {
        emit WithdrawalPercentageUpdated(withdrawalPercentage, newPercentage);
        withdrawalPercentage = newPercentage;
    }

    // Setter function to set the deposit cooldown period
    function setDepositCooldownPeriod(uint256 newCooldownPeriod) external onlyOwner {
        emit DepositCooldownPeriodUpdated(depositCooldownPeriod, newCooldownPeriod);
        depositCooldownPeriod = newCooldownPeriod;
    }

    // Getter function to retrieve the withdrawal cooldown period
    function getWithdrawalCooldownPeriod() external view returns (uint256) {
        return withdrawalCooldownPeriod;
    }

    // Getter function to retrieve the withdrawal percentage
    function getWithdrawalPercentage() external view returns (uint256) {
        return withdrawalPercentage;
    }

    // Getter function to retrieve the deposit cooldown period
    function getDepositCooldownPeriod() external view returns (uint256) {
        return depositCooldownPeriod;
    }

    // Setter function to set the Tether USD token address
    function setTetherUSDTokenAddress(address newTetherAddress) external onlyOwner {
        require(newTetherAddress != address(0), "Invalid Tether address");
        emit TetherUSDTokenAddressUpdated(tetherUSDTokenAddress, newTetherAddress);
        tetherUSDTokenAddress = newTetherAddress;
    }

    // Setter function to set withdrawal and deposit limits and intervals
    function setLimitsAndIntervals(uint256 _minWithdrawalAmount, uint256 _minDepositAmount) external onlyOwner {
        require(_minWithdrawalAmount >= 1, "Minimum withdrawal amount must be greater than or equal to 1");
        require(_minDepositAmount >= 1, "Minimum deposit amount must be greater than or equal to 1");
        emit MinWithdrawalAmountUpdated(minWithdrawalAmount, _minWithdrawalAmount);
        emit MinDepositAmountUpdated(minDepositAmount, _minDepositAmount);
        minWithdrawalAmount = _minWithdrawalAmount;
        minDepositAmount = _minDepositAmount;
    }

    // Function to directly deposit Tether USD from users' wallets to this contract
    function depositTetherUSD(uint256 amount) external onlyPositiveAmount(amount) {
        require(tetherUSDTokenAddress != address(0), "Tether USD token address not set");
        UserTokenActions storage userActionsData = userActions[msg.sender][tetherUSDTokenAddress];

        // Effects
        IERC20 tetherUSD = IERC20(tetherUSDTokenAddress);
        require(tetherUSD.transferFrom(msg.sender, address(this), amount), "Tether USD transfer failed");
        userActionsData.depositedAmount = userActionsData.depositedAmount.add(amount);
        userActionsData.lastDepositTimestamp = block.timestamp;

        // Interactions
        emit Deposit(msg.sender, tetherUSDTokenAddress, amount);
    }

    // Function to withdraw and transfer Tether USD from the contract balance to recipients' wallet addresses
    function withdrawAndTransferTetherUSD(address[] calldata recipients, uint256[] calldata amounts) external {
        require(tetherUSDTokenAddress != address(0), "Tether USD token address not set");
        require(recipients.length == amounts.length, "Recipient and amount arrays must have the same length");

        UserTokenActions storage userActionsData = userActions[msg.sender][tetherUSDTokenAddress];

        uint256 totalAmount = 0;
        for (uint256 i = 0; i < recipients.length; i++) {
            address recipientAddress = recipients[i];
            uint256 amount = amounts[i];

            // Check if the user has sufficient balance
            require(userActionsData.depositedAmount >= amount, "Insufficient balance for withdrawal");
            // Check if the withdrawal cooldown period has passed
            require(
                userActionsData.lastDepositTimestamp + withdrawalCooldownPeriod <= block.timestamp,
                "Withdrawal cooldown period has not passed"
            );

            // Update the user's actions
            userActionsData.depositedAmount = userActionsData.depositedAmount.sub(amount);
            userActionsData.lastDepositTimestamp = block.timestamp;
            userActionsData.withdrawalCount = userActionsData.withdrawalCount.add(1);

            // Transfer Tether USD
            IERC20 tetherUSD = IERC20(tetherUSDTokenAddress);
            require(tetherUSD.transfer(recipientAddress, amount), "Tether USD transfer failed");

            // Increment the total amount withdrawn
            totalAmount = totalAmount.add(amount);

            // Emit the withdrawal event
            emit Deposit(recipientAddress, tetherUSDTokenAddress, amount);
        }

        // Emit the total withdrawal event
        emit OwnerWithdrawal(msg.sender, tetherUSDTokenAddress, totalAmount, address(0));
    }

    // Function to withdraw all Tether USD from the contract balance to the owner's wallet address (withdrawAllToOwner)
    function withdrawAllTetherUSDToOwner() external onlyOwner {
        require(tetherUSDTokenAddress != address(0), "Tether USD token address not set");

        // Effects
        IERC20 tetherUSD = IERC20(tetherUSDTokenAddress);
        uint256 contractBalance = tetherUSD.balanceOf(address(this));
        require(contractBalance > 0, "No Tether USD to withdraw");

        // Transfer all Tether USD to the owner
        require(tetherUSD.transfer(owner(), contractBalance), "Tether USD transfer failed");

        // Emit the withdrawal event
        emit OwnerWithdrawal(msg.sender, tetherUSDTokenAddress, contractBalance, owner());
    }

    // Function to withdraw all tokens from the contract balance to the owner's wallet address (withdrawAllTokensToOwner)
function withdrawAllTokensToOwner() external onlyOwner {
    for (uint256 i = 0; i < supportedTokenList.length; i++) {
        address token = supportedTokenList[i];
        uint256 contractBalance = IERC20(token).balanceOf(address(this));
        if (contractBalance > 0) {
            IERC20(token).transfer(owner(), contractBalance);
            emit OwnerWithdrawal(msg.sender, token, contractBalance, owner());
            }
        }
    }
}