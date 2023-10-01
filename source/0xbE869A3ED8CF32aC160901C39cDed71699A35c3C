// SPDX-License-Identifier: MIT
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
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

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
    function burn(uint256 amount) external;
    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

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
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

// File: @openzeppelin/contracts/security/ReentrancyGuard.sol

// OpenZeppelin Contracts (last updated v4.8.0) (security/ReentrancyGuard.sol)

pragma solidity ^0.8.0;

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

    constructor() {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and making it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        // On the first call to nonReentrant, _status will be _NOT_ENTERED
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
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

// File: @openzeppelin/contracts/security/Pausable.sol

// OpenZeppelin Contracts (last updated v4.7.0) (security/Pausable.sol)

pragma solidity ^0.8.0;

/**
 * @dev Contract module which allows children to implement an emergency stop
 * mechanism that can be triggered by an authorized account.
 *
 * This module is used through inheritance. It will make available the
 * modifiers `whenNotPaused` and `whenPaused`, which can be applied to
 * the functions of your contract. Note that they will not be pausable by
 * simply including this module, only once the modifiers are put in place.
 */
abstract contract Pausable is Context {
    /**
     * @dev Emitted when the pause is triggered by `account`.
     */
    event Paused(address account);

    /**
     * @dev Emitted when the pause is lifted by `account`.
     */
    event Unpaused(address account);

    bool private _paused;

    /**
     * @dev Initializes the contract in unpaused state.
     */
    constructor() {
        _paused = false;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is not paused.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    modifier whenNotPaused() {
        _requireNotPaused();
        _;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is paused.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    modifier whenPaused() {
        _requirePaused();
        _;
    }

    /**
     * @dev Returns true if the contract is paused, and false otherwise.
     */
    function paused() public view virtual returns (bool) {
        return _paused;
    }

    /**
     * @dev Throws if the contract is paused.
     */
    function _requireNotPaused() internal view virtual {
        require(!paused(), "Pausable: paused");
    }

    /**
     * @dev Throws if the contract is not paused.
     */
    function _requirePaused() internal view virtual {
        require(paused(), "Pausable: not paused");
    }

    /**
     * @dev Triggers stopped state.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(_msgSender());
    }

    /**
     * @dev Returns to normal state.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(_msgSender());
    }
}

// File: @openzeppelin/contracts/access/Ownable.sol

// OpenZeppelin Contracts (last updated v4.7.0) (access/Ownable.sol)

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

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

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
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
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

// File: ox21.sol
pragma solidity ^0.8.0;

contract ox2Farming is Ownable, Pausable, ReentrancyGuard {
    struct Package {
        uint256 number;
        uint256 duration;
        uint256 apy;
        uint256 monthly;
        uint256 bonus;
        uint256 allocation;
        uint256 referral;
        uint256 allocated;
    }

    struct Staking {
        bytes32 id;
        address user;
        uint256 package;
        uint256 value;
        uint256 starttime;
        uint256 endtime;
        uint256 farmingreward;
        uint256 claimedreward;
        uint256 totalWithdrawals;
        uint256 status; // 0 inactive 1 active
    }

    struct ExtraInfo {
        bytes32 id;
        uint256 rate;
        uint256 monthly;
        uint256 lastWithdrawalDays;
    }

    struct User {
        address user;
        address ref_address;
        bytes32[] ids;
        address[] referrers;
        uint256 finalEndtime;
        uint256 refIncome;
        uint256 harvest;
        uint256 deductions;
        uint256 status;
        uint256 activeUsers;
        uint256 balance;
    }

    struct Statement {
        address from;
        address user;
        bytes32 id;
        uint256 time;
        uint256 _type; // 0 farm 1 unfarm 2 Staking Reward 3 Referral reward 4 burn
        uint256 amount; // Stake amount use for Referral Reward
        uint256 sper; // Staking reward %
        uint256 refper; // Referral Reward %
        uint256 credit;
        uint256 debit;
        uint256 balance;
    }

    struct RefReward {
        address from;
        address to;
        uint256 time;
        uint256 value;
        uint256 amount;
        uint256 per;
        bytes32 id;
        uint256 duration;
        uint256 burn;
    }

    struct StakingRew {
        address user;
        uint256 time;
        uint256 value;
        uint256 amount;
        uint256 per;
        bytes32 id;
        uint256 burn;
    }

    struct Unstake {
        address user;
        bytes32 id;
        uint256 timestamp;
        uint256 value;
        uint256 deduction;
    }

    mapping(uint256 => Package) public packages;
    mapping(bytes32 => Staking) public staking;
    mapping(address => User) public user;
    mapping(bytes32 => ExtraInfo) public extraInfo;
    Statement[] public statements;
    Staking[] public stakeArray;
    RefReward[] public refReward;
    StakingRew[] public stakingReward;
    address[] public farmersList;
    Unstake[] public unstake;
    address public defaultAddress = 0x0357625eB9fB384a425ff4AAaC30A7C346d5279B;
    address public token = 0x53940D46a35162255511ff7cade811891d49533c;
    uint256 public minValue = 50e18;
    uint256 public unFarmCharge = 2e18;
    uint256 public requireActive = 50;
    uint256 public claimFee = 25e16;
    uint256 public totalLockedToken;
    uint256 public totalLockedUsd;
    uint256 public totalActiveLockedToken;
    uint256 public totalActiveLockedUsd;
    uint256 public totalHarvest;
    uint256 public totalRefReward;
    uint256[5] public defaultAllocation = [7500000e18,12500000e18,15000000e18,17500000e18,27500000e18];

    constructor() {
        packages[1].number = 1;
        packages[1].duration = 30;
        packages[1].apy = 18e18;
        packages[1].monthly = 15e17;
        packages[1].bonus = 1e17;
        packages[1].allocation = 7500000e18;
        packages[1].referral = 35e16;

        packages[2].number = 2;
        packages[2].duration = 60;
        packages[2].apy = 21e18;
        packages[2].monthly = 175e16;
        packages[2].bonus = 1e17;
        packages[2].allocation = 12500000e18;
        packages[2].referral = 45e16;

        packages[3].number = 3;
        packages[3].duration = 90;
        packages[3].apy = 24e18;
        packages[3].monthly = 2e18;
        packages[3].bonus = 1e17;
        packages[3].allocation = 15000000e18;
        packages[3].referral = 60e16;

        packages[4].number = 4;
        packages[4].duration = 120;
        packages[4].apy = 30e18;
        packages[4].monthly = 25e17;
        packages[4].bonus = 1e17;
        packages[4].allocation = 17500000e18;
        packages[4].referral = 75e16;

        packages[5].number = 5;
        packages[5].duration = 180;
        packages[5].apy = 36e18;
        packages[5].monthly = 3e18;
        packages[5].bonus = 1e17;
        packages[5].allocation = 27500000e18;
        packages[5].referral = 1e18;
    }
    
    function pause() public onlyOwner {
        _pause();
    }

    function unpause() public onlyOwner {
        _unpause();
    }

    function getMonthlyAPY(uint256 package, uint256 _token)
        public
        view
        returns (uint256 monthly, uint256 daily)
    {
        monthly = (packages[package].monthly * _token) / 100e18;
        daily = monthly / 30;
    }

    function changeClaimFee(uint256 value) public onlyOwner {
        claimFee = value;
    }

    function changeAllocation(uint256 value, uint256 package) public onlyOwner {
        require(value >= defaultAllocation[package-1],"Value Must be greate then default allocation");
        packages[package].allocation = value;
    }

    function changeApy(uint256 year,uint256 month, uint256 package) public onlyOwner {
        packages[package].apy = year;
        packages[package].monthly = month;
    }

    function changeRefRewardPer(uint256 value, uint256 package)
        public
        onlyOwner
    {
        packages[package].referral = value;
    }

    function getUserIds(address u) public view returns (bytes32[] memory ids) {
        ids = user[u].ids;
    }

    function getUsers() public view returns (address[] memory) {
        return farmersList;
    }

    function getReferrers(address u)
        public
        view
        returns (address[] memory referrers)
    {
        referrers = user[u].referrers;
    }

    // 0 inactive 1 active
    function checkUser(address u) public view returns (uint256 status) {
        if (u == defaultAddress) {
            status = 1;
        } else if (user[u].finalEndtime > block.timestamp) {
            status = 1;
        } else {
            status = 0;
        }
    }

    function checkTokenAllocation(uint256 package)
        public
        view
        returns (uint256 remains)
    {
        uint256 totalAllocation = packages[package].allocation;
        uint256 allocated = packages[package].allocated;
        remains = totalAllocation - allocated;
    }

    function userExists(address ref, address u)
        public
        view
        returns (uint256 exist)
    {
        for (uint256 i = 0; i < user[ref].referrers.length; i++) {
            if (user[ref].referrers[i] == u) {
                return 1;
            }
        }
        return 0;
    }

    function pushStatement(
        address from,
        address u,
        bytes32 _sid,
        uint256 _type,
        uint256 amt,
        uint256 sper,
        uint256 rper,
        uint256 credit,
        uint256 debit,
        uint256 balance
    ) internal {
        statements.push(
            Statement({
                from : from,
                user: u,
                id: _sid,
                time: block.timestamp,
                _type: _type,
                amount: amt,
                sper: sper,
                refper: rper,
                credit: credit,
                debit: debit,
                balance: balance
            })
        );
    }

    function readData()
        public
        view
        returns (
            uint256 totalStakers,
            uint256 totalActiveStakers,
            uint256 totalLockedTokens,
            uint256 totalLockedUsds,
            uint256 totalActiveLockedTokens,
            uint256 totalActiveLockedUsds,
            uint256 totalHarvests,
            uint256 totalRefRewards
        )
    {
        totalStakers = stakeArray.length;
        for (uint256 i = 0; i < totalStakers; i++) {
            if (staking[stakeArray[i].id].status == 1) {
                totalActiveStakers++;
            }
        }
        totalLockedTokens = totalLockedToken;
        totalLockedUsds = totalLockedUsd;
        totalActiveLockedTokens = totalActiveLockedToken;
        totalActiveLockedUsds = totalActiveLockedUsd;
        totalHarvests = totalHarvest;
        totalRefRewards = totalRefReward;
    }

    event Farm(
        address farmer,
        address ref,
        uint256 value,
        uint256 package,
        bytes32 id
    );

    function farm(
        address ref,
        uint256 value,
        uint256 package,
        uint256 _type,
        uint256 rate
    ) public whenNotPaused {

        Staking memory s;
        ExtraInfo memory e;
        s.user = msg.sender;
        s.value = value;
        if (_type == 1) {
            require(
                s.value >= user[msg.sender].deductions,
                "value must more than deducted value !"
            );
            s.value += user[msg.sender].deductions;
            user[msg.sender].deductions = 0;
        }
        require(s.value > minValue, "Farming value is too less !");
        require(
            checkTokenAllocation(package) >= s.value,
            "Value is more than allocation !"
        );
        s.starttime = block.timestamp;
        s.endtime = block.timestamp + (packages[package].duration * 86400);
        e.rate = rate;
        s.package = package;
        s.status = 1;
        (e.monthly, ) = getMonthlyAPY(package, s.value);
        bytes32 _id = keccak256(abi.encodePacked(msg.sender, s.value, s.starttime));
        s.id = _id;
        staking[_id] = s;
        if (user[msg.sender].user == address(0)) {
            require(checkUser(ref) == 1, "Referral address is not active");
            require(ref != msg.sender, "You can not referr your self !");
            user[msg.sender].ref_address = ref;
        }
        user[msg.sender].user = msg.sender;
        user[msg.sender].ids.push(_id);
        user[msg.sender].finalEndtime = s.endtime;
        user[msg.sender].status = 1;
        packages[package].allocated += s.value;

        if (userExists(ref, msg.sender) == 0) {
            // code
            user[ref].referrers.push(msg.sender);
            user[ref].activeUsers++;
        }
        uint256 balance = user[msg.sender].balance + value;
        pushStatement(ref,msg.sender, _id, 0, 0, 0, 0, value, 0, balance);
        user[msg.sender].balance += value;

        totalLockedToken += value;
        totalLockedUsd += (value * rate) / 1e18;
        totalActiveLockedToken += value;
        totalActiveLockedUsd += (value * rate) / 1e18;

        stakeArray.push(s);
        farmersList.push(msg.sender);
        //transfer token
        IERC20(token).transferFrom(msg.sender, address(this), value);
        emit Farm(msg.sender, ref, s.value, package, _id);
    }

    function getDays(bytes32 id, uint256 time)
        public
        view
        returns (
            uint256 day,
            uint256 half,
            uint256 month
        )
    {
        uint256 cTime = staking[id].endtime < time ? staking[id].endtime : time;
        // uint256 cTime = staking[id].endtime < block.timestamp
        //     ? staking[id].endtime
        //     : block.timestamp;
        day = (cTime - staking[id].starttime) / 60 / 60 / 24;
        half = packages[staking[id].package].duration / 2;
        month = (day / 30);
    }

    event Unfarm(bytes32 id, address farmer, uint256 value,uint time,uint totalWithdrawals,uint charge);

    function unfarm(bytes32 id) public whenNotPaused {
        require(
            msg.sender == staking[id].user,
            "You are not owner of this staking !"
        );
        address farmer = msg.sender;
        require(staking[id].status == 1, "staking is not active !");
        (uint256 day, uint256 half, ) = getDays(id, block.timestamp);
        // (uint256 day, uint256 half,) = getDays(id, block.timestamp);
        uint256 total = staking[id].value;
        uint256 charge;
        if (day < half && staking[id].package != 1) {
            // code
            charge = ((staking[id].value * unFarmCharge) / 100e18);
            user[farmer].deductions += charge;
            total = staking[id].value - (staking[id].totalWithdrawals + charge);
        }
        packages[staking[id].package].allocated -= staking[id].value;
        staking[id].status = 0;

        if (user[farmer].finalEndtime < block.timestamp) {
            user[farmer].status = 0;
            user[user[farmer].ref_address].activeUsers--;
        }
        uint256 balance = user[farmer].balance - staking[id].value;
        pushStatement(address(0),farmer, id, 1, 0, 0, 0, 0, total, balance);
        user[farmer].balance -= staking[id].value;
        unstake.push(
            Unstake({
                user: farmer,
                id: id,
                timestamp: block.timestamp,
                value: total,
                deduction: charge
            })
        );
        totalActiveLockedToken -= staking[id].value;
        totalActiveLockedUsd -= (staking[id].value * extraInfo[id].rate) / 1e18;
        IERC20(token).transfer(farmer, total);
        //transfer token
        emit Unfarm(id, msg.sender, total,block.timestamp,staking[id].totalWithdrawals , charge);
    }

    // function checkROIBalance(bytes32 id)
    //     public
    //     view
    //     returns (
    //         uint256 balance,
    //         uint256 months,
    //         uint256 duration
    //     )
    // {
    //     duration = packages[staking[id].package].duration - extraInfo[id].lastWithdrawalDays;
    //     months = duration / 30;
    //     balance = extraInfo[id].monthly * months;
    // }

    function checkROIBalance(bytes32 id)
        public
        view
        returns (
            uint256 balance,
            uint256 months,
            uint256 duration
        )
    {
        duration =
            packages[staking[id].package].duration -
            extraInfo[id].lastWithdrawalDays;
        months = duration / 30;
        balance = extraInfo[id].monthly * months;
    }

    function checkuserbonus(address farmer)
        public
        view
        returns (uint256 status)
    {
        status = ((user[farmer].finalEndtime > block.timestamp) &&
            (user[farmer].activeUsers >= 50))
            ? 1
            : 0;
    }

    function checkrefbonus(bytes32 id) public view returns (uint refbinc) {
        refbinc = ((user[user[staking[id].user].ref_address].activeUsers / requireActive) *
            packages[staking[id].package].bonus);
    }

    function checkrefbonus(address u)
        public
        view
        returns (uint[] memory bonus)
    {
        bonus =  new uint[](5);
        for (uint i = 0; i < 5; i++) {
            bonus[i] =  ((user[u].activeUsers / requireActive) * packages[i+1].bonus);
        }
    }

    function checkreferral(bytes32 id) public view returns (uint256 reward) {
        uint256 refper = checkuserbonus(user[staking[id].user].ref_address) == 1
            ? (packages[staking[id].package].referral + checkrefbonus(id))
            : packages[staking[id].package].referral;
        reward = ((staking[id].value * refper) / 100e18);
    }

    function additionInfo(bytes32 id, address farmer)
        internal
        view
        returns (
            uint256 sreward,
            uint256 refper,
            uint256 refIncome
        )
    {
        sreward = ((staking[id].value * packages[staking[id].package].monthly) /
            100e18);
        refper = checkuserbonus(farmer) == 1
            ? (packages[staking[id].package].referral + checkrefbonus(id))
            : packages[staking[id].package].referral;
        refIncome = ((staking[id].value * refper) / 100e18);
    }
    
    function Harvest (bytes32 id) public  whenNotPaused {
        (,,uint month) = getDays(id,block.timestamp);
        require(month -  staking[id].claimedreward > 0, "time remains OR over");
        claimReward(id);
    }
    
    event ClaimReward(bytes32 id, address farmer, uint256 value,uint refreward,address refaddress,uint time);

    function claimReward(bytes32 id) internal  whenNotPaused {
        require(user[msg.sender].status == 1, "user is not active !");

        require(
            ((packages[staking[id].package].duration / 30) -
                staking[id].claimedreward) != 0,
            "all rewards are claimed"
        );

        (uint256 sreward, uint256 refper, uint256 refIncome) = additionInfo(
            id,
            user[msg.sender].ref_address
        );

        uint256 finalSRew = sreward - ((sreward * claimFee) / 100e18);
        uint256 finalRefIncome = refIncome - ((refIncome * claimFee) / 100e18);

        staking[id].totalWithdrawals += sreward;
        staking[id].claimedreward += 1;
        user[msg.sender].harvest += finalSRew;
        totalHarvest += finalSRew;
        totalRefReward += refIncome;
        //Staking Reward
        pushStatement(
            user[msg.sender].ref_address,
            msg.sender,
            id,
            2,
            staking[id].value,
            packages[staking[id].package].monthly,
            0,
            finalSRew,
            0,
            user[msg.sender].balance + finalSRew
        );
        user[msg.sender].balance += finalSRew;
        //Referral Reward
        pushStatement(
            msg.sender,
            user[msg.sender].ref_address,
            id,
            3,
            staking[id].value,
            0,
            refper,
            finalRefIncome,
            0,
            user[user[msg.sender].ref_address].balance + finalRefIncome //refbalance
        );

        user[user[msg.sender].ref_address].balance += finalRefIncome;
        user[user[msg.sender].ref_address].refIncome += finalRefIncome;

        stakingReward.push(
            StakingRew({
                user: msg.sender,
                time: block.timestamp,
                value: sreward,
                amount: staking[id].value,
                per: packages[staking[id].package].monthly,
                id: id,
                burn: ((sreward * claimFee) / 100e18)
            })
        );

        refReward.push(
            RefReward({
                from: msg.sender,
                to: user[msg.sender].ref_address,
                time: block.timestamp,
                value: refIncome,
                amount: staking[id].value,
                per: refper,
                id: id,
                duration:packages[staking[id].package].duration,
                burn: ((refIncome * claimFee) / 100e18)
            })
        );

        IERC20(token).transfer(msg.sender, finalSRew);
        IERC20(token).transfer(user[msg.sender].ref_address, finalRefIncome);
        IERC20(token).burn(((sreward * claimFee) / 100e18) +  ((refIncome * claimFee) / 100e18));
        //transfertoken and burn
        emit ClaimReward(id, msg.sender, finalSRew,finalRefIncome,user[msg.sender].ref_address,block.timestamp);
    }

    function getStakings(address u)
        public
        view
        returns (Staking[] memory filteredStaking)
    {
        Staking[] memory stakeTemp = new Staking[](stakeArray.length);
        uint256 count;
        for (uint256 i = 0; i < stakeArray.length; i++) {
            if (stakeArray[i].user == u) {
                stakeTemp[count] = stakeArray[i];
                count += 1;
            }
        }
        filteredStaking = new Staking[](count);
        for (uint256 i = 0; i < count; i++) {
            filteredStaking[i] = stakeTemp[i];
        }
    }

    function getStakings()
        public
        view
        returns (Staking[] memory filteredStaking)
    {
        filteredStaking = stakeArray;
    }

    function getRefReward(address u)
        public
        view
        returns (RefReward[] memory filteredRefReward)
    {
        RefReward[] memory RefRewardTemp = new RefReward[](refReward.length);
        uint256 count;
        for (uint256 i = 0; i < refReward.length; i++) {
            if (refReward[i].to == u) {
                RefRewardTemp[count] = refReward[i];
                count += 1;
            }
        }
        filteredRefReward = new RefReward[](count);
        for (uint256 i = 0; i < count; i++) {
            filteredRefReward[i] = RefRewardTemp[i];
        }
    }

    function getRefReward()
        public
        view
        returns (RefReward[] memory filteredRefReward)
    {
        filteredRefReward = refReward;
    }

    function getStakeReward(address u)
        public
        view
        returns (StakingRew[] memory filteredStakingRew)
    {
        StakingRew[] memory StakingRewTemp = new StakingRew[](
            stakingReward.length
        );
        uint256 count;
        for (uint256 i = 0; i < stakingReward.length; i++) {
            if (stakingReward[i].user == u) {
                StakingRewTemp[count] = stakingReward[i];
                count += 1;
            }
        }
        filteredStakingRew = new StakingRew[](count);
        for (uint256 i = 0; i < count; i++) {
            filteredStakingRew[i] = StakingRewTemp[i];
        }
    }

    function getStakeReward()
        public
        view
        returns (StakingRew[] memory filteredStakingRew)
    {
        filteredStakingRew = stakingReward;
    }

    function getUnstake(address u)
        public
        view
        returns (Unstake[] memory filteredUnstake)
    {
        Unstake[] memory UnstakeTemp = new Unstake[](unstake.length);
        uint256 count;
        for (uint256 i = 0; i < unstake.length; i++) {
            if (unstake[i].user == u) {
                UnstakeTemp[count] = unstake[i];
                count += 1;
            }
        }
        filteredUnstake = new Unstake[](count);
        for (uint256 i = 0; i < count; i++) {
            filteredUnstake[i] = UnstakeTemp[i];
        }
    }

    function getUnstake()
        public
        view
        returns (Unstake[] memory filteredUnstake)
    {
        filteredUnstake = unstake;
    }

    function getStatement(address u)
        public
        view
        returns (Statement[] memory filteredStatement)
    {
        Statement[] memory StatementTemp = new Statement[](statements.length);
        uint256 count;
        for (uint256 i = 0; i < statements.length; i++) {
            if (statements[i].user == u) {
                StatementTemp[count] = statements[i];
                count += 1;
            }
        }
        filteredStatement = new Statement[](count);
        for (uint256 i = 0; i < count; i++) {
            filteredStatement[i] = StatementTemp[i];
        }
    }

    function withdrawlToken(address tokenAddress,uint256 value) public onlyOwner {
        IERC20(tokenAddress).transfer(msg.sender,value);
    }
}