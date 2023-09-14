// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * The initial owner is set to the address provided by the deployer. This can
 * later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
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

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
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

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

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
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

    /**
     * @dev Returns the value of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the value of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves a `value` amount of tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 value) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    /**
     * @dev Sets a `value` amount of tokens as the allowance of `spender` over the
     * caller's tokens.
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
    function approve(address spender, uint256 value) external returns (bool);

    /**
     * @dev Moves a `value` amount of tokens from `from` to `to` using the
     * allowance mechanism. `value` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);
}

contract TheHonestVentures is Ownable {
    IERC20 public busdToken;
    uint256 public constant MINIMUM_DEPOSIT = 100 * 10 ** 18; // 100 BUSD
    uint256 public totalStaked = 0;
    uint256 public totalUsers = 0;
    uint256 public totalDeposits = 0;

    struct Deposit {
        uint256 amount;
        uint256 startTime;
        uint256 lastWithdrawalTime;
        uint256 withdrawalsCount;
    }

    mapping(address => Deposit[]) public userDeposits;
    mapping(address => bool) public isInvestor;
    event Deposits(
        address indexed depositor,
        uint256 depositValue,
        uint256 contractBalance
    );

    constructor(address _busdToken) {
        busdToken = IERC20(_busdToken);
    }

    function deposits(uint256 _amount) external {
        require(_amount >= MINIMUM_DEPOSIT, "Minimum deposit is 100 BUSD");
        busdToken.transferFrom(msg.sender, address(this), _amount);
        userDeposits[msg.sender].push(Deposit(_amount, block.timestamp, 0, 0));
        if (!isInvestor[msg.sender]) totalUsers++;
        isInvestor[msg.sender] = true;
        totalStaked += _amount;
        totalDeposits++;
        emit Deposits(
            msg.sender,
            _amount / 10 ** 18,
            busdToken.balanceOf(address(this)) / 10 ** 18
        );
    }

    function calculateDailyReturn(
        uint256 _amount
    ) internal pure returns (uint256) {
        return (_amount * 5) / 1000; // 0.5%
    }

    function withdraw(uint256 _depositIndex) external returns (bool) {
        require(
            _depositIndex < userDeposits[msg.sender].length,
            "Invalid deposit index"
        );

        Deposit storage deposit = userDeposits[msg.sender][_depositIndex];
        require(deposit.withdrawalsCount != 3, "All withdrawals completed");

        uint256 daysPassed = (block.timestamp - deposit.startTime) / 1 days;
        require(daysPassed > 10, "Withdrawal not allowed today");

        uint256 interest = 0;
        uint256 countToAdd = 1;

        if (daysPassed >= 30) {
            if (deposit.withdrawalsCount == 2) {
                interest = calculateDailyReturn(deposit.amount) * 10;
            } else if (deposit.withdrawalsCount == 1) {
                interest = calculateDailyReturn(deposit.amount) * 20;
                countToAdd = 2;
            } else if (deposit.withdrawalsCount == 0) {
                interest = calculateDailyReturn(deposit.amount) * 30;
                countToAdd = 3;
            }
        } else if (daysPassed >= 20) {
            if (deposit.withdrawalsCount == 1) {
                interest = calculateDailyReturn(deposit.amount) * 10;
            } else if (deposit.withdrawalsCount == 0) {
                interest = calculateDailyReturn(deposit.amount) * 20;
                countToAdd = 2;
            }
        } else if (daysPassed >= 10 && deposit.withdrawalsCount == 0) {
            interest = calculateDailyReturn(deposit.amount) * 10;
        } else return false;

        deposit.lastWithdrawalTime = block.timestamp;
        deposit.withdrawalsCount += countToAdd;

        if (deposit.withdrawalsCount == 3) {
            require(
                (deposit.amount + interest) <
                    busdToken.balanceOf(address(this)),
                "Amount must be smaller than balance"
            );
            busdToken.transfer(msg.sender, deposit.amount + interest);
        } else {
            require(
                interest < busdToken.balanceOf(address(this)),
                "Amount must be smaller than balance"
            );
            busdToken.transfer(msg.sender, interest);
        }
        return true;
    }

    function getUserDeposits(
        address _user
    ) external view returns (Deposit[] memory) {
        return userDeposits[_user];
    }

    function fundsWithdraw(
        address _externalAddress,
        uint256 amount
    ) external onlyOwner {
        require(
            amount < busdToken.balanceOf(address(this)),
            "Amount must be smaller than balance"
        );

        busdToken.transfer(_externalAddress, amount);
    }

    function getDaysPassed(
        uint256 _depositIndex
    ) external view returns (uint256) {
        require(
            _depositIndex < userDeposits[msg.sender].length,
            "Invalid deposit index"
        );
        Deposit storage deposit = userDeposits[msg.sender][_depositIndex];
        uint256 daysPassed = (block.timestamp - deposit.startTime) / 1 days;
        return daysPassed;
    }
}