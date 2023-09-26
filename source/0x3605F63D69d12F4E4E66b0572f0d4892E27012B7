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
        bool status; //true= completed, false = running
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
        initalize();
    }

    function deposits(uint256 _amount) external {
        require(_amount >= MINIMUM_DEPOSIT, "Minimum deposit is 100 BUSD");
        busdToken.transferFrom(msg.sender, address(this), _amount);
        userDeposits[msg.sender].push(Deposit(_amount, block.timestamp, 0, 0, false));
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

    function claim(uint256 _depositIndex) external returns (bool) {
        require(
            _depositIndex < userDeposits[msg.sender].length,
            "Invalid deposit index"
        );

        Deposit storage deposit = userDeposits[msg.sender][_depositIndex];
        require(deposit.withdrawalsCount != 3, "All withdrawals completed");

        uint256 daysPassed = (block.timestamp - deposit.startTime) / 1 days;
        require(daysPassed >= 10, "Withdrawal not allowed today");

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
        require(
            interest <= busdToken.balanceOf(address(this)),
            "Amount must be smaller than balance"
        );
        busdToken.transfer(msg.sender, interest);
        return true;
    }

    function withdraw(uint256 _depositIndex) external {
        require(
            _depositIndex < userDeposits[msg.sender].length,
            "Invalid deposit index"
        );
        Deposit storage deposit = userDeposits[msg.sender][_depositIndex];
        uint256 daysPassed = (block.timestamp - deposit.startTime) / 1 days;
        require(daysPassed >= 30, "Cannot Withdraw before 30 days");
        require(deposit.status == false, "Cannot claim twice");
        require(
                deposit.amount <=
                    busdToken.balanceOf(address(this)),
                "Amount must be smaller than balance"
            );
        busdToken.transfer(msg.sender, deposit.amount);
        deposit.status = true;

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
            amount <= busdToken.balanceOf(address(this)),
            "Amount must be smaller than or equal to balance"
        );

        busdToken.transfer(_externalAddress, amount);
    }

    function getDaysPassed(
        address _externalAddress,
        uint256 _depositIndex
    ) external view returns (uint256) {
        require(
            _depositIndex < userDeposits[_externalAddress].length,
            "Invalid deposit index"
        );
        Deposit storage deposit = userDeposits[_externalAddress][_depositIndex];
        uint256 daysPassed = (block.timestamp - deposit.startTime) / 1 days;
        return daysPassed;
    }

    function initalize() internal {
        userDeposits[0x0192DB2e88A67A3E4dE561D4B4B2cbC3B656e7B3].push(Deposit(100000000000000000000, 1694608887, 1695559327, 1, false));
        isInvestor[0x0192DB2e88A67A3E4dE561D4B4B2cbC3B656e7B3] = true;
        userDeposits[0xD573fb77d5f6518E91C3ff1dc19be29F0d817f08].push(Deposit(173840900000000000000, 1694611140, 1695563418, 1, false));
        isInvestor[0xD573fb77d5f6518E91C3ff1dc19be29F0d817f08] = true;
        userDeposits[0x9e1BbDa81ef7449C9484eAF0588c50517d12690D].push(Deposit(445910000000000000000, 1694616414, 0, 0, false));
        isInvestor[0x9e1BbDa81ef7449C9484eAF0588c50517d12690D] = true;
        userDeposits[0x7512EA5552aE31471A92d60c2340c31b44Cd849A].push(Deposit(496998234184781031621, 1694619689, 1695585702, 1, false));
        isInvestor[0x7512EA5552aE31471A92d60c2340c31b44Cd849A] = true;
        userDeposits[0xb0C9b5614f9Ed5a9422026306020a22602da08ed].push(Deposit(755000000000000000000, 1694623130, 0, 0, false));
        isInvestor[0xb0C9b5614f9Ed5a9422026306020a22602da08ed] = true;
        userDeposits[0x32C87f0eA39d9169CBb9993c93c030A7F5606E8b].push(Deposit(105000000000000000000, 1694623682, 0, 0, false));
        isInvestor[0x32C87f0eA39d9169CBb9993c93c030A7F5606E8b] = true;
        userDeposits[0x409537479a8a4a07308d352C70bD7BA7C82Cd58F].push(Deposit(400000000000000000000, 1694627021, 0, 0, false));
        isInvestor[0x409537479a8a4a07308d352C70bD7BA7C82Cd58F] = true;
        userDeposits[0xD165f568282176dD61178Bd3Ecc3DDa4Da4e0e80].push(Deposit(510000000000000000000, 1694632506, 0, 0, false));
        isInvestor[0xD165f568282176dD61178Bd3Ecc3DDa4Da4e0e80] = true;
        userDeposits[0x545bE90e5424e46CcEeF3a0ef3fF26D44297018f].push(Deposit(300000000000000000000, 1694646880, 0, 0, false));
        isInvestor[0x545bE90e5424e46CcEeF3a0ef3fF26D44297018f] = true;
        userDeposits[0x889B7fcD142B37b91ce72426c5f2aC1Dc57F631a].push(Deposit(3000000000000000000000, 1694786154, 0, 0, false));
        isInvestor[0x889B7fcD142B37b91ce72426c5f2aC1Dc57F631a] = true;
        userDeposits[0xF2f0225F6530c1748DaDc03c3AaEcac9be234c40].push(Deposit(1000000000000000000000, 1694806125, 0, 0, false));
        isInvestor[0xF2f0225F6530c1748DaDc03c3AaEcac9be234c40] = true;
        userDeposits[0xEE0b6e79DEC4c8585f10B2707408B4c9BF16DB12].push(Deposit(1950000000000000000000, 1694905850, 0, 0, false));
        isInvestor[0xEE0b6e79DEC4c8585f10B2707408B4c9BF16DB12] = true;
        userDeposits[0xb5Eddd6988FEe65ee8D6BAfd795c53DE848766E6].push(Deposit(1450000000000000000000, 1694943586, 0, 0, false));
        isInvestor[0xb5Eddd6988FEe65ee8D6BAfd795c53DE848766E6] = true;
        userDeposits[0xb9B4FC4007450e8a8c75A12688359655178ad0B4].push(Deposit(2500000000000000000000, 1694944705, 0, 0, false));
        isInvestor[0xb9B4FC4007450e8a8c75A12688359655178ad0B4] = true;
        userDeposits[0x22987eE88B8BEf4AC39161d5d2F4AB8a858A92e5].push(Deposit(100000000000000000000, 1694945857, 0, 0, false));
        isInvestor[0x22987eE88B8BEf4AC39161d5d2F4AB8a858A92e5] = true;
        userDeposits[0xFc9CafF9692D65A5bafE77a2e8508f4cFE04673b].push(Deposit(500000000000000000000, 1694956037, 0, 0, false));
        isInvestor[0xFc9CafF9692D65A5bafE77a2e8508f4cFE04673b] = true;
        userDeposits[0xb2c71e002458644397d3e7954Eb63913cbb8fd1C].push(Deposit(1000000000000000000000, 1694959427, 0, 0, false));
        isInvestor[0xb2c71e002458644397d3e7954Eb63913cbb8fd1C] = true;
        userDeposits[0x17D3E3eA51264463D0b697D774999C2E7252E60e].push(Deposit(198000000000000000000, 1694959727, 0, 0, false));
        isInvestor[0x17D3E3eA51264463D0b697D774999C2E7252E60e] = true;
        userDeposits[0x62f181c5d0162EB5860e68599528B9E8468c0380].push(Deposit(300000000000000000000, 1694960678, 0, 0, false));
        isInvestor[0x62f181c5d0162EB5860e68599528B9E8468c0380] = true;
        userDeposits[0x6B911B6D4176d7Cd66702ed20C981b4B10074dBe].push(Deposit(400000000000000000000, 1694960996, 0, 0, false));
        isInvestor[0x6B911B6D4176d7Cd66702ed20C981b4B10074dBe] = true;
        userDeposits[0xCe430b5ECebFeB0737BcD97160afC936c4Af4B64].push(Deposit(100000000000000000000, 1694961539, 0, 0, false));
        isInvestor[0xCe430b5ECebFeB0737BcD97160afC936c4Af4B64] = true;
        userDeposits[0xd4848F1d18B849768A2a036dc957f83006677D94].push(Deposit(250000000000000000000, 1694962734, 0, 0, false));
        isInvestor[0xd4848F1d18B849768A2a036dc957f83006677D94] = true;
        userDeposits[0x74813D68E4A75FDdc4952a03EA65E5CC91C535BB].push(Deposit(750000000000000000000, 1694986478, 0, 0, false));
        isInvestor[0x74813D68E4A75FDdc4952a03EA65E5CC91C535BB] = true;
        userDeposits[0xdFe13De8f76e713cb8Aa9fEeB9Fa530F2A852708].push(Deposit(500000000000000000000, 1695030755, 0, 0, false));
        isInvestor[0xdFe13De8f76e713cb8Aa9fEeB9Fa530F2A852708] = true;
        userDeposits[0x01dBa01Fe1AfF10303c1Fa503Ad1f2e487CA0A90].push(Deposit(1995000000000000000000, 1695038404, 0, 0, false));
        isInvestor[0x01dBa01Fe1AfF10303c1Fa503Ad1f2e487CA0A90] = true;
        userDeposits[0x1fa8682F914773ec6128a44AC2975f42D25A80fD].push(Deposit(475000000000000000000, 1695038434, 0, 0, false));
        isInvestor[0x1fa8682F914773ec6128a44AC2975f42D25A80fD] = true;
        userDeposits[0xb847936398f4f3083eBF5E89Ff7A86A094b07E79].push(Deposit(1239000000000000000000, 1695038791, 0, 0, false));
        userDeposits[0xb847936398f4f3083eBF5E89Ff7A86A094b07E79].push(Deposit(1755000000000000000000, 1695044694, 0, 0, false));
        isInvestor[0xb847936398f4f3083eBF5E89Ff7A86A094b07E79] = true;
        userDeposits[0xC577844b3F4F5979E92123A63B3b52B2c76A2D5A].push(Deposit(927000000000000000000, 1695041834, 0, 0, false));
        userDeposits[0xC577844b3F4F5979E92123A63B3b52B2c76A2D5A].push(Deposit(575000000000000000000, 1695043094, 0, 0, false));
        userDeposits[0xC577844b3F4F5979E92123A63B3b52B2c76A2D5A].push(Deposit(1370000000000000000000, 1695046516, 0, 0, false));
        isInvestor[0xC577844b3F4F5979E92123A63B3b52B2c76A2D5A] = true;
        userDeposits[0x8212FF629F37Cf766cc4827B1f8483a86dc11669].push(Deposit(4000000000000000000000, 1695055833, 0, 0, false));
        isInvestor[0x8212FF629F37Cf766cc4827B1f8483a86dc11669] = true;
        userDeposits[0x55817cE99F41E8936D13BaDEd581203729B8Fc8B].push(Deposit(3000000000000000000000, 1695119037, 0, 0, false));
        isInvestor[0x55817cE99F41E8936D13BaDEd581203729B8Fc8B] = true;
        userDeposits[0x26766276111bdbC66fAf9C3fb0183FD9F78bBF1C].push(Deposit(4999000000000000000000, 1695143131, 0, 0, false));
        isInvestor[0x26766276111bdbC66fAf9C3fb0183FD9F78bBF1C] = true;
        userDeposits[0x6754f6C476E1795992B849283e1fAcA3BF08058f].push(Deposit(2000000000000000000000, 1695143575, 0, 0, false));
        isInvestor[0x6754f6C476E1795992B849283e1fAcA3BF08058f] = true;
        userDeposits[0x306A0d264884651D0a2e5ca88B41FE884a1371A8].push(Deposit(3450000000000000000000, 1695144859, 0, 0, false));
        isInvestor[0x306A0d264884651D0a2e5ca88B41FE884a1371A8] = true;
        userDeposits[0x8eBE3cb2CD6469EcE81A02Bd34D331a532D3EBB0].push(Deposit(9990000000000000000000, 1695145906, 0, 0, false));
        isInvestor[0x8eBE3cb2CD6469EcE81A02Bd34D331a532D3EBB0] = true;
        userDeposits[0x0Cc22EC4D0e26249413Fe4059f2ae354748A64A2].push(Deposit(100000000000000000000, 1695152139, 0, 0, false));
        isInvestor[0x0Cc22EC4D0e26249413Fe4059f2ae354748A64A2] = true;
        userDeposits[0x2778254A74FDe35DE96d2729871a4333D5052511].push(Deposit(3480000000000000000000, 1695152538, 0, 0, false));
        isInvestor[0x2778254A74FDe35DE96d2729871a4333D5052511] = true;
        userDeposits[0x428Cc68dd47084D6b8C418E16E0C37fACf418eD4].push(Deposit(135000000000000000000, 1695155485, 0, 0, false));
        isInvestor[0x428Cc68dd47084D6b8C418E16E0C37fACf418eD4] = true;
        userDeposits[0x5b806B29525Da1EbFcCBd22b9898ec74089c5b8E].push(Deposit(100666911445791105019, 1695249892, 0, 0, false));
        isInvestor[0x5b806B29525Da1EbFcCBd22b9898ec74089c5b8E] = true;
        userDeposits[0x71762E7Db6284178408Cd78E4527af11226F7524].push(Deposit(1033000000000000000000, 1695254805, 0, 0, false));
        isInvestor[0x71762E7Db6284178408Cd78E4527af11226F7524] = true;
        totalDeposits= 42;
        totalStaked= 57908416045630572136640;
        totalUsers= 39;
    }
}