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

// File: contracts/TTDAOMinter_NEW.sol


pragma solidity ^0.8.9;




interface INFTMinter {
    function mint(address to, uint256[] calldata uids, address referrer) external;

    function safeMint(address to) external;

    function safeBatchMint(address to, uint256 quantity) external;

    function balanceOf(address owner) external view returns (uint256 balance);
}

interface ISwapRouter {
    function factory() external pure returns (address);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

interface ITTDAOLiquidity {
    function addInitLiquidity() external;

    function addLiquidity() external;
}

interface ITTDAOBurn {
    function burn() external;
}

interface IERC20Burn {
    function burn(uint256 amount) external;
}

interface IBank {
    function withdraw(address account, uint256 amount) external;
}

contract TTDAOMinter is Ownable {
    using SafeMath for uint256;

    IERC20  public usdtToken;

    address private constant _ttDaoNodeAddress = 0x4AEFB89b8c6493E94E5E9Ac9F223EE25589b4522;
    address private constant _usdtAddress = 0x55d398326f99059fF775485246999027B3197955;
    address private constant _swapAddress = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
    address private constant _officalMinterAddress = 0x7d6F5Ac2B06aa6C78b079A29fBC584f23C38950b;
    address private bankAdddress = 0x967ff7D5f3D428f9236F31197677a7C84FE2F5D1;

    mapping(address => address) private _refers;
    mapping(address => uint256) private _referCount;
    mapping(address => uint256) private _referNodeCostCount;

    mapping(uint256 => mapping(address => uint256)) private _nodeMints;
    mapping(uint256 => uint256) private _nodeAllMint;


    uint256 public  DAY_SEC = 86400;
    uint256 public  DAY_SEC_7 = 7 * 86400;
    uint256 public  DAY_SEC_360 = 360 * 86400;
    uint256 public  constant basePrice = 30 * 10 ** 18;
    uint256 public  constant basePower = 10;
    uint256 public  constant MAX = ~uint256(0);
    address private constant _marketFd = 0x977164848E49D1ecB5CF490e5ab1eF51B6f26D56;
    address private constant _marketQd = 0x53e9D9dc8f2CD7b6b84E6620C9c4020895d3542E;

    ITTDAOBurn private _burnContract;
    ITTDAOLiquidity private _liquidityContract;

    address private _ttDaoAddress = address(0);
    address private _burnPool = address(0);
    address private _liquidityPool = address(0);
    uint256 private _mintNodeEndTime = 1690288200;
    uint256 private _mintNodeReferCount = 50;
    uint256 private _mintedAmount = 0;
    mapping(address => uint256) private accountUsdtRewards;
    mapping(address => uint256) private accountReferRewards;

    struct MintData {
        uint256 nftMintCount;
        uint256 totalTTDAO;
        uint256 claimedTTDAO;
        uint256 totalNftPower;
        uint256 totalReferPower;
        uint256 startTime;
        uint256 miningNftPower;
        uint256 deduction;
        uint256 lastReferClaimTime;
        uint256 totalReferTTDAO;
    }

    mapping(address => MintData) public minters;

    event MintPay(address sender, uint256 payAmount, uint256 nftCount, address referrer);
    event MintNodePay(address sender, uint256 payAmount, uint256 nftCount);
    event ClaimReferReward(address sender, uint256 amount, uint256 power);
    event ClaimNftReward(address sender, uint256 power, uint256 amount, uint256 burnAmount);

    constructor() {
        usdtToken = IERC20(_usdtAddress);
        IERC20(_usdtAddress).approve(_officalMinterAddress, MAX);
    }

    function mint(uint256[] calldata uids, address referrer) public {
        require(uids.length > 0, "need uids");
        require(msg.sender != referrer, "error referrer");
        require(block.timestamp >= _mintNodeEndTime, "can not mint now");
        require(block.timestamp < _mintNodeEndTime + DAY_SEC_360, "end");
        // pay & mint
        uint256 price = getPrice();
        uint256 payAmount = price.mul(uids.length);
        usdtToken.transferFrom(msg.sender, address(this), payAmount);
        INFTMinter(_officalMinterAddress).mint(msg.sender, uids, referrer);

        // refer
        if (_refers[msg.sender] == address(0)) {
            // need did
            MintData storage user = minters[referrer];
            if (user.nftMintCount > 0) {
                _refers[msg.sender] = referrer;
                _referCount[referrer] = _referCount[referrer].add(uids.length);
            }
        } else {
            address olderRefer = _refers[msg.sender];
            _referCount[olderRefer] = _referCount[olderRefer].add(uids.length);
        }
        _referCount[msg.sender] = _referCount[msg.sender].add(uids.length);

        // calc power
        updateNftPower(msg.sender, uids.length);
        address Lv1 = _refers[msg.sender];
        address Lv2 = _refers[Lv1];
        address Lv3 = _refers[Lv2];
        updateReferPower(Lv1, uids.length, 10);
        updateReferPower(Lv2, uids.length, 7);
        updateReferPower(Lv3, uids.length, 3);

        // transfer
        uint256 payAmount1 = payAmount.mul(40).div(100) - 12 * 10 ** 18 * uids.length;
        usdtToken.transfer(_marketFd, payAmount1);
        uint256 payAmount2 = payAmount.mul(10).div(100);
        usdtToken.transfer(_marketQd, payAmount2);
        usdtToken.transfer(_burnPool, payAmount2);
        uint256 payAmount3 = payAmount.mul(40).div(100);
        usdtToken.transfer(_liquidityPool, payAmount3);

        _burnContract.burn();
        _liquidityContract.addLiquidity();

        if (Lv1 != address(0) && INFTMinter(_ttDaoNodeAddress).balanceOf(Lv1) > 0) {
            uint256 dayIndex = block.timestamp / 86400;
            _nodeMints[dayIndex][Lv1] = _nodeMints[dayIndex][Lv1].add(uids.length);
            _nodeAllMint[dayIndex] = _nodeAllMint[dayIndex].add(uids.length);
        }

        emit MintPay(msg.sender, payAmount, uids.length, referrer);
    }

    function updateNftPower(address account, uint256 nftCount) internal {
        MintData storage user = minters[account];

        uint256 addPower = getPower() * nftCount;
        user.totalNftPower = user.totalNftPower.add(addPower);
        user.nftMintCount = user.nftMintCount.add(nftCount);
        user.totalTTDAO = user.totalTTDAO.add(addPower * 360 * 1e18);

        // sale statistic
        _mintedAmount = _mintedAmount.add(nftCount);

        // mining
        if (user.startTime > 0 && block.timestamp < user.startTime + DAY_SEC_7) {
            uint256 lost = (block.timestamp - user.startTime) / DAY_SEC * addPower;
            user.deduction = user.deduction.add(lost);
            user.miningNftPower = user.miningNftPower.add(addPower);
        }
    }

    function updateReferPower(address account, uint256 nftCount, uint256 ratio) internal {
        if (account == address(0)) {
            return;
        }

        // need did
        MintData storage user = minters[account];
        if (user.nftMintCount == 0) {
            return;
        }

        uint256 addPower = getPower() * nftCount;
        addPower = addPower.mul(ratio).div(100);
        user.totalReferPower = user.totalReferPower.add(addPower);
        user.totalReferTTDAO = user.totalReferTTDAO.add(addPower * 360 * 1e18);

        if (user.lastReferClaimTime == 0) {
            user.lastReferClaimTime = block.timestamp;
        }
    }

    function getPrice() public view returns (uint256) {
        return basePrice.add(_mintedAmount.div(10000).mul(10 ** 19));
    }

    function getPower() public view returns (uint256) {
        uint256 subPower = _mintedAmount.div(20000);
        if (subPower >= 5) {
            return 5;
        } else {
            return basePower.sub(subPower);
        }
    }

    function getMintedAmount() public view returns (uint256) {
        return _mintedAmount;
    }

    function getReferDetail(address account) public view returns (uint256, uint256, address, uint256) {
        uint256 mintNodeCount = (_referCount[account] - _referNodeCostCount[account]) / _mintNodeReferCount;
        return (
        _referCount[account], // refer count
        _referNodeCostCount[account], // swap node cost count
        _refers[account], // referal
        mintNodeCount
        );
    }

    function getAccountMintData(address account) public view returns (MintData memory) {
        return minters[account];
    }

    function getMintData(address account) public view returns (
        uint256, //  nftMintCount;
        uint256, //  totalNftPower;
        uint256, //  totalReferPower;
        uint256, //  startTime;
        uint256, //  miningNftPower,
        uint256, //  lastReferClaimTime,
        uint256  //  refererCount,
    ) {
        MintData storage user = minters[account];
        uint256 cnt = _referCount[account];
        return (user.nftMintCount,
        user.totalNftPower,
        user.totalReferPower,
        user.startTime,
        user.miningNftPower,
        user.lastReferClaimTime,
        cnt);
    }

    function startMine() public {
        require(block.timestamp >= _mintNodeEndTime, "can not mine now");
        MintData storage user = minters[msg.sender];
        require(user.totalNftPower > 0, "no power");
        require(user.startTime == 0, "start again");

        user.startTime = block.timestamp;
        user.miningNftPower = user.totalNftPower;
        user.deduction = 0;
    }

    function claimNftReward() public {
        MintData storage user = minters[msg.sender];
        require(user.miningNftPower > 0, "no power");
        require(user.startTime > 0 && block.timestamp > user.startTime + DAY_SEC_7, "unripe");

        // claim USDT
        uint256 mintBalance = (user.miningNftPower * 7 - user.deduction) * 10 ** 18;
        if (user.claimedTTDAO.add(mintBalance) > user.totalTTDAO) {
            mintBalance = user.totalTTDAO - user.claimedTTDAO;
        }

        address[] memory path = new address[](2);
        path[0] = _ttDaoAddress;
        path[1] = _usdtAddress;
        uint256 usdtBeforeBalance = usdtToken.balanceOf(msg.sender);

        IBank(bankAdddress).withdraw(address(this), mintBalance);

        ISwapRouter(_swapAddress).swapExactTokensForTokensSupportingFeeOnTransferTokens(
            mintBalance,
            0,
            path,
            msg.sender,
            block.timestamp
        );
        uint256 usdtAfterBalance = usdtToken.balanceOf(msg.sender);

        // burn
        uint256 burnBalance = user.deduction * 10 ** 18;
        burnBalance = burnBalance + (block.timestamp - user.startTime - DAY_SEC_7) * user.miningNftPower * 10 ** 18 / DAY_SEC;

        if (mintBalance + user.claimedTTDAO + burnBalance > user.totalTTDAO) {
            burnBalance = user.totalTTDAO - mintBalance - user.claimedTTDAO;
        }

        if (burnBalance > 0) {
            IERC20Burn(bankAdddress).burn(burnBalance);
        }

        emit ClaimNftReward(msg.sender, user.totalNftPower, mintBalance, burnBalance);

        // restart
        user.claimedTTDAO = user.claimedTTDAO.add(mintBalance).add(burnBalance);
        user.startTime = block.timestamp;
        user.miningNftPower = user.totalNftPower;
        user.deduction = 0;

        accountUsdtRewards[msg.sender] += (usdtAfterBalance.sub(usdtBeforeBalance));
    }

    function claimReferReward() public {

        MintData storage user = minters[msg.sender];
        require(user.nftMintCount > 0, "no did");
        require(user.totalReferPower > 0, "no power");
        require(user.lastReferClaimTime > 0 && block.timestamp > user.lastReferClaimTime + DAY_SEC, "unripe");

        uint256 balance = (block.timestamp - user.lastReferClaimTime) * user.totalReferPower * 10 ** 18 / DAY_SEC;
        if (accountReferRewards[msg.sender] + balance > user.totalReferTTDAO) {
            balance = user.totalReferTTDAO - accountReferRewards[msg.sender];
        }
        IBank(bankAdddress).withdraw(msg.sender, balance);

        user.lastReferClaimTime = block.timestamp;
        accountReferRewards[msg.sender] += balance;
        emit ClaimReferReward(msg.sender, balance, user.totalReferPower);
    }

    function getRewardAmount(address account) public view returns (uint256 nftRewardAmount, uint256 referReward) {
        MintData memory user = minters[account];
        if (user.miningNftPower == 0 || user.startTime == 0) {
            nftRewardAmount = 0;
        } else {
            uint256 day = (block.timestamp - user.startTime) / DAY_SEC;
            if (day > 7) {
                day = 7;
            }

            if (day == 0) {
                nftRewardAmount = 0;
            } else {
                nftRewardAmount = (day * user.miningNftPower - user.deduction) * 10 ** 18;
                if (nftRewardAmount + user.claimedTTDAO > user.totalTTDAO) {
                    nftRewardAmount = user.totalTTDAO - user.claimedTTDAO;
                }
            }
        }

        if (user.nftMintCount == 0 || user.totalReferPower == 0 || user.lastReferClaimTime == 0 || block.timestamp <= user.lastReferClaimTime + DAY_SEC) {
            referReward = 0;
        } else {
            referReward = (block.timestamp - user.lastReferClaimTime) * user.totalReferPower * 10 ** 18 / DAY_SEC;
            if (accountReferRewards[account] + referReward > user.totalReferTTDAO) {
                referReward = user.totalReferTTDAO - accountReferRewards[account];
            }
        }

    }

    function getAccountUsdtReward(address account) public view returns (uint256) {
        return accountUsdtRewards[account];
    }

    function getAccountReferRewards(address account) public view returns (uint256) {
        return accountReferRewards[account];
    }

    function swapNode() public {
        MintData storage user = minters[msg.sender];
        require(user.nftMintCount > 0, "no did");

        uint256 mintNodeCount = (_referCount[msg.sender] - _referNodeCostCount[msg.sender]) / _mintNodeReferCount;
        if (mintNodeCount > 0) {
            INFTMinter(_ttDaoNodeAddress).safeBatchMint(msg.sender, mintNodeCount);
            _referNodeCostCount[msg.sender] = _referNodeCostCount[msg.sender].add(mintNodeCount * _mintNodeReferCount);
        }
    }

    function setAddress(address ttDaoAddr, address liquidityAddr, address burnAddr) public onlyOwner {
        _ttDaoAddress = ttDaoAddr;
        _burnPool = burnAddr;
        _liquidityPool = liquidityAddr;

        IERC20(_ttDaoAddress).approve(_swapAddress, MAX);
        _burnContract = ITTDAOBurn(_burnPool);
        _liquidityContract = ITTDAOLiquidity(_liquidityPool);
    }

    function update(address[] memory accounts) public onlyOwner {
        for (uint256 i = 0; i < accounts.length; ++i) {
            _updateAddress(accounts[i]);
        }
    }

    function setIntitor(address account, address invitor) public onlyOwner {
        _refers[account] = invitor;
    }

    function setIntiteAmount(address account, uint256 amount) public onlyOwner {
        _referCount[account] = amount;
    }

    function updateReferLv(address [] memory accounts) public onlyOwner {
        for (uint256 i = 0; i < accounts.length; ++i) {
            MintData storage user = minters[accounts[i]];
            address Lv1 = _refers[accounts[i]];
            address Lv2 = _refers[Lv1];
            address Lv3 = _refers[Lv2];
            updateReferPower(Lv1, user.nftMintCount, 10);
            updateReferPower(Lv2, user.nftMintCount, 7);
            updateReferPower(Lv3, user.nftMintCount, 3);
        }
    }

    function setAccountReferPower(address account, uint256 addPower) public onlyOwner {
        MintData storage user = minters[account];
        user.totalReferPower = addPower;
        user.totalReferTTDAO = addPower * 360 * 1e18;
    }

    function _updateAddress(address account) internal {
        uint256 nftMintCount;
        uint256 totalNftPower;
        uint256 totalReferPower;
        uint256 startTime;
        uint256 miningNftPower;
        uint256 lastReferClaimTime;
        TTDAOMinter dao = TTDAOMinter(0xfb1e3Dfd965EB70DdAb273C3279e23f3B1ECb359);
        (nftMintCount, totalNftPower, totalReferPower, startTime, miningNftPower, lastReferClaimTime,) = dao.getMintData(account);
        MintData storage user = minters[account];
        user.totalNftPower = totalNftPower;
        user.nftMintCount = nftMintCount;
        user.totalTTDAO = user.totalTTDAO.add(totalNftPower * 360 * 1e18);

        uint256 refererCount;
        address refer;
        uint256 referNodeCostCount;
        (refererCount, referNodeCostCount, refer,) = dao.getReferDetail(account);
        _refers[account] = refer;
        _referCount[account] = refererCount;
        _referNodeCostCount[account] = referNodeCostCount;

        _mintedAmount = dao.getMintedAmount();
    }

    function setMintNodeEndTime(uint256 t) public onlyOwner {
        _mintNodeEndTime = t;
    }

    function getMintNodeEndTime() public view returns (uint256) {
        return _mintNodeEndTime;
    }

    function setMintNodeReferCount(uint256 c) public onlyOwner {
        _mintNodeReferCount = c;
    }

    function getNodeMint(uint256 dayIndex, address account) public view returns (uint256, uint256) {
        return (_nodeMints[dayIndex][account], _nodeAllMint[dayIndex]);
    }
}