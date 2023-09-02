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

// File: @openzeppelin/contracts/security/ReentrancyGuard.sol


// OpenZeppelin Contracts (last updated v4.9.0) (security/ReentrancyGuard.sol)

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

    /**
     * @dev Returns true if the reentrancy guard is currently set to "entered", which indicates there is a
     * `nonReentrant` function in the call stack.
     */
    function _reentrancyGuardEntered() internal view returns (bool) {
        return _status == _ENTERED;
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

// File: contracts/launcpad/sales.sol


pragma solidity ^0.8.0;





interface ILaunchpad {
    function getStakeData(address user) external view returns (uint);
}

contract Sales is Ownable, ReentrancyGuard {
    ILaunchpad private launchpadContract;
    IERC20 private purchaseTokenContract;
    IERC20 private saleTokenContract;
    IERC20 private stakeTokenContract;

    uint private decimal;
    uint private price;
    uint private startTime;
    uint private endTime;
    uint private refRate;

    address private launchpadAddress;

    bool private status = false;

    constructor(
        address _launchpadContractAddress,
        address _purhchaseTokenContractAddress,
        address _saleTokenContractAddress,
        address _stakeTokenContract,
        uint _decimal,
        uint _price,
        uint _startTime,
        uint _endTime,
        uint _refRate
    ) {
        launchpadContract = ILaunchpad(_launchpadContractAddress);
        launchpadAddress = _launchpadContractAddress;
        stakeTokenContract = IERC20(_stakeTokenContract);
        purchaseTokenContract = IERC20(_purhchaseTokenContractAddress);
        saleTokenContract = IERC20(_saleTokenContractAddress);
        decimal = _decimal;
        price = _price;
        startTime = _startTime;
        endTime = _endTime;
        refRate = _refRate;
        status = true;
    }

    function buyTokens(
        uint256 tokenAmount,
        address _receiver,
        address _refer
    ) external nonReentrant returns (bool) {
        require(
            block.timestamp >= startTime && block.timestamp <= endTime,
            "Sale not active"
        );  
        require(
           status,
            "Sale not active"
        );

        uint256 maxAllocation = launchpadContract.getStakeData(_receiver);
        require(
            tokenAmount <= maxAllocation * (10 ** decimal),
            "amount is bigger than allocation"
        );
        require(
            purchaseTokenContract.transferFrom(
                msg.sender,
                address(this),
                tokenAmount
            ),
            "Token transfer failed"
        );
        uint refRewards = tokenAmount / 100 * refRate ;
        uint256 totalToken = (tokenAmount * price) / 100;
        purchaseTokenContract.transfer(_refer, refRewards);
        require(
            saleTokenContract.transfer(_receiver, totalToken),
            "Transfer failed"
        );
        return true;
    }

    function getPrice() public view returns (uint) {
        return price;
    }

    function getUserStakeData() public view returns (uint) {
        return launchpadContract.getStakeData(msg.sender);
    }

    function getLaunchContract() public view returns (address) {
        return address(launchpadContract);
    }

    function getstakeTokenContract() public view returns (address) {
        return address(stakeTokenContract);
    }

    function getpurchaseTokenContract() public view returns (address) {
        return address(purchaseTokenContract);
    }

    function getSaleTokenContract() public view returns (address) {
        return address(saleTokenContract);
    }

    function setPrice(uint _newPrice) external onlyOwner {
        price = _newPrice;
    }

    function endSale(address _receiverAddress) external onlyLaunchpad returns (bool) {
        status = false;
        uint balance = purchaseTokenContract.balanceOf(address(this));
        endTime = block.timestamp;
        purchaseTokenContract.transferFrom(
            address(this),
            _receiverAddress,
            balance
        );
        return true;
    }

    modifier onlyLaunchpad() {
        require(msg.sender == launchpadAddress);
        _;
    }
}
// File: contracts/launcpad/launchpad.sol


pragma solidity ^0.8.0;




interface ISales {
    function buyTokens(
        uint256 tokenAmount,
        address _receiver,
        address _refer
    ) external returns (bool);

    function endSale(
        address _receiverAddress
    ) external returns (bool);
    
}

contract Launchpad is ReentrancyGuard {
    address private owner;
    IERC20 private stakeToken;
    constructor(address _stakeTokenAddress, uint _decimal) {
        stakeToken = IERC20(_stakeTokenAddress);
        stakeDecimal = _decimal;
        owner = msg.sender;
    }

    uint stakeDecimal;

    struct Stake {
        uint stakeTypeId;
        uint startedTime;
        uint amount;
        uint lockTime;
        uint allocation;
        uint apr;
        string stakeTypeName;
        bool isActive;
    }

    struct StakeType {
        uint id;
        string typeName;
        uint lockTime;
        uint apr;
        uint allocation;
        uint minimumStakeAmount;
        uint tvl;
        bool isActive;
    }
 
    struct SalesClone {
        uint id;
        string image;
        string description;
        uint decimal;
        address cloneAddress;
        address purchaseToken;
        address saleToken;
        uint price;
        uint startTime;
        uint endTime;
        uint refRate;
    }

    mapping(address => Stake) private stakes;
    mapping(uint => StakeType) private idToStakeType;

    mapping(uint => SalesClone) private idToSalesClone;

    uint private stakeTypeCount;
    uint private uniqueStakeCount;
    uint private salesCloneCount;

    function createStakeType(
        string calldata _typeName,
        uint _lockTime,
        uint _apr,
        uint _minimumStakeAmount,
        uint _allocation
    ) external onlyOwner {
        stakeTypeCount++;
        idToStakeType[stakeTypeCount] = StakeType({
            id: stakeTypeCount,
            typeName: _typeName,
            lockTime: _lockTime,
            apr: _apr,
            allocation: _allocation,
            minimumStakeAmount: _minimumStakeAmount * 10 ** stakeDecimal,
            tvl: 0,
            isActive: true
        });
    }

    function createTokenSale(
        string calldata _image,
        string calldata _description,
        address _purchaseTokenAddress,
        address _saleToken,
        uint _decimal,
        uint _price,
        uint _startTime,
        uint _endTime,
        uint _refRate
    ) external onlyOwner returns (address) {
        salesCloneCount++;
        Sales saleClone = new Sales(
            address(this),
            _purchaseTokenAddress,
            _saleToken,
            address(stakeToken),
            _decimal,
            _price,
            _startTime,
            _endTime,
            _refRate
        );

        saleClone.transferOwnership(msg.sender);

        idToSalesClone[salesCloneCount] = SalesClone({
            id: salesCloneCount,
            image: _image,
            description: _description,
            decimal: _decimal,
            cloneAddress: address(saleClone),
            purchaseToken: _purchaseTokenAddress,
            saleToken: _saleToken,
            price: _price,
            startTime: _startTime,
            endTime: _endTime,
            refRate: _refRate
        });

        return address(saleClone);
    }

    function closeStakeType(uint _id) external onlyOwner {
        StakeType storage stakeType = idToStakeType[_id];
        require(stakeType.isActive == true);
        stakeType.isActive = false;
    }

    function stake(uint _stakeTypeId, uint _amount) external {
        StakeType memory stakeType = idToStakeType[_stakeTypeId];
        require(stakeType.isActive == true);
        require(
            _amount >= stakeType.minimumStakeAmount,
            "Staking amount is below the minimum"
        );
        require(
            stakeToken.transferFrom(msg.sender, address(this), _amount),
            "Transfer failed"
        );
        Stake storage userStake = stakes[msg.sender];

        if (userStake.isActive && userStake.stakeTypeId == _stakeTypeId) {
            userStake.startedTime = block.timestamp;
            userStake.amount += _amount;
            idToStakeType[_stakeTypeId].tvl += _amount;
        } else if (userStake.isActive == false) {
            stakes[msg.sender] = Stake({
                stakeTypeId: _stakeTypeId,
                startedTime: block.timestamp,
                amount: _amount,
                lockTime: stakeType.lockTime,
                apr: stakeType.apr,
                allocation: stakeType.allocation,
                stakeTypeName: stakeType.typeName,
                isActive: true
            });
            idToStakeType[_stakeTypeId].tvl += _amount;
        } else {
            revert(
                "you cannot stake another type. claim your previous stake firstly"
            );
        }
    }

    function withdrawTokens() external nonReentrant {
        Stake memory userStake = stakes[msg.sender];
        require(userStake.startedTime + userStake.lockTime <= block.timestamp);
        delete stakes[msg.sender];

        uint userStakedValue = userStake.amount;
        uint apr = userStake.apr;
        uint reward = userStakedValue * apr / 100;
        uint amountToSend = userStakedValue + reward;
        idToStakeType[userStake.stakeTypeId].tvl -= amountToSend;

        stakeToken.transfer(msg.sender, amountToSend);
    }

    //will be removed after audit
    function emergencyWithdraw() external onlyOwner {
        uint amount = stakeToken.balanceOf(address(this));
        stakeToken.transfer(msg.sender, amount);
    }

    //will be removed after audit
    //address ile yapalım
    function emergencyWithdrawERC20(address _tokenAddress) external onlyOwner {
        IERC20 token = IERC20(_tokenAddress);
        uint amount = token.balanceOf(address(this));
        token.transfer(msg.sender, amount);
    }

    function buyTokens(uint _saleId, uint _amount, address _refer) external nonReentrant{
        SalesClone memory salesCloneDetails = idToSalesClone[_saleId];
        require(
            IERC20(salesCloneDetails.purchaseToken).transferFrom(
                msg.sender,
                address(this),
                _amount
            )
        );

        IERC20(salesCloneDetails.purchaseToken).approve(
            salesCloneDetails.cloneAddress,
            _amount
        );

        require(
            ISales(salesCloneDetails.cloneAddress).buyTokens(
                _amount,
                msg.sender,
                _refer
            )
        );
    }

    function endSale(uint _saleId, address _receiver) external onlyOwner{
        idToSalesClone[_saleId].endTime = block.timestamp;
        require(
            ISales(idToSalesClone[_saleId].cloneAddress).endSale(_receiver),"TX is not executed"
        );
    }

    function getUpcomingSales() public view returns (SalesClone[] memory) {
        uint currentIndex = 0;
        uint upcomingSaleCount = 0;
        //aktif type sayısını hesapla
        for (uint i = 1; i <= salesCloneCount; i++) {
            if (idToSalesClone[i].startTime < block.timestamp) {
                upcomingSaleCount++;
            }
        }

        SalesClone[] memory upcomingSales = new SalesClone[](upcomingSaleCount);
        for (uint i = 1; i <= salesCloneCount; i++) {
            if (idToSalesClone[i].startTime < block.timestamp) {
                upcomingSales[currentIndex] = idToSalesClone[i];
                currentIndex++;
            }
        }

        return upcomingSales;
    }
    function getActiveSales() public view returns (SalesClone[] memory) {
        uint currentIndex = 0;
        uint activeSaleCount = 0;
        //aktif type sayısını hesapla
        for (uint i = 1; i <= salesCloneCount; i++) {
            if ((idToSalesClone[i].endTime > block.timestamp) && (idToSalesClone[i].startTime < block.timestamp)) {
                activeSaleCount++;
            }
        }

        SalesClone[] memory activeSales = new SalesClone[](activeSaleCount);
        for (uint i = 1; i <= salesCloneCount; i++) {
            if ((idToSalesClone[i].endTime > block.timestamp) && (idToSalesClone[i].startTime < block.timestamp)) {
                activeSales[currentIndex] = idToSalesClone[i];
                currentIndex++;
            }
        }

        return activeSales;
    }

    function getFundedSales() public view returns (SalesClone[] memory) {
        uint currentIndex = 0;
        uint fundedSaleCount = 0;
        //aktif type sayısını hesapla
        for (uint i = 1; i <= salesCloneCount; i++) {
            if (idToSalesClone[i].endTime > block.timestamp) {
                fundedSaleCount++;
            }
        }

        SalesClone[] memory fundedSales = new SalesClone[](fundedSaleCount);
        for (uint i = 1; i <= salesCloneCount; i++) {
            if (idToSalesClone[i].endTime > block.timestamp) {
                fundedSales[currentIndex] = idToSalesClone[i];
                currentIndex++;
            }
        }

        return fundedSales;
    }

    function getActiveStakeTypes() public view returns (StakeType[] memory) {
        uint currentIndex = 0;
        uint activeTypeCount = 0;
        //aktif type sayısını hesapla
        for (uint i = 1; i <= stakeTypeCount; i++) {
            if (idToStakeType[i].isActive == true) {
                activeTypeCount++;
            }
        }

        StakeType[] memory activeStakeTypes = new StakeType[](activeTypeCount);
        for (uint i = 1; i <= stakeTypeCount; i++) {
            if (idToStakeType[i].isActive == true) {
                activeStakeTypes[currentIndex] = idToStakeType[i];
                currentIndex++;
            }
        }

        return activeStakeTypes;
    }

    function getSale(uint _saleId) public view returns (SalesClone memory) {
        return idToSalesClone[_saleId];
    }

    function getStakeTokenAddress() public view returns (address) {
        return address(stakeToken);
    }

    function getStakeData(address user) external view returns (uint) {
        return (stakes[user].allocation);
    }

    function getUserAllStakeData(address _user) external view returns(Stake memory) {
        return stakes[_user];
    }

    function getStakeType(
        uint _stakeTypeId
    ) public view returns (StakeType memory) {
        return idToStakeType[_stakeTypeId];
    }

    function transferOwnership(address _newOwner) external onlyOwner {
        owner = _newOwner;
    }

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }
}