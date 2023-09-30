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

// File: @openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol


// OpenZeppelin Contracts v4.4.1 (token/ERC20/extensions/IERC20Metadata.sol)

pragma solidity ^0.8.0;


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

// File: contracts/new_presale_pepe.sol


pragma solidity ^0.8.9;




contract PepeLoveINUPresale is Ownable {
    IERC20 public token;
    address public sellerAddress;
    IERC20 public usdtToken;
    IERC20Metadata public usdtTokenAddress;

    uint256 public presaleTokenAmount;
    bool public presaleActive = true;
    bool public capsActive = true;
    uint256 public softCap = 200 ether;
    uint256 public hardCap = 500 ether;
    uint256 public maxTxAmount = 16666666666;
    uint256 public maxWalletAmount = 1666666666600;
    uint256 public totalSold = 0;
    uint256 public totalFund = 0 ether;
    uint256 public totalFundUsdt = 0 ether;

    //stages info
    struct Stage {
        uint256 id;
        uint256 bonus;
        uint256 price;
        uint256 usdtPrice;
        uint256 start;
        uint256 end;
    }
    mapping(uint256 => Stage) public stages;
    uint256 public maxStage = 4;
    uint256 currentStageId = 0;

    // constructor
    constructor(
        address _token,
        address _seller,
        address _usdt
    ) {
        token = IERC20(_token);
        sellerAddress = _seller;
        usdtToken = IERC20(_usdt);
        usdtTokenAddress = IERC20Metadata(_usdt);
    }

    //token buy with usdt
    function buyTokenWithUsdt(uint256 _amount) public payable {
        require(presaleActive, "Presale is not active!");
        if (capsActive) {
            require(address(this).balance <= hardCap, "Hardcap limit exceeds!");
        }
        require(_amount >= 0, "Please enter minimum token!");
        uint256 _id = getCurrentStageIdActive();
        require(_id > 0, "Stage info not available!");
        uint256 _bonus = stages[_id].bonus;
        uint256 _usdtPrice = stages[_id].usdtPrice;
        uint256 _tokenPrice = _usdtPrice * _amount;
        uint256 _start = stages[_id].start;
        uint256 _end = stages[_id].end;
        require(_start <= block.timestamp, "Presale comming soon!");
        require(_end >= block.timestamp, "Presale end!");
        require(
            usdtToken.balanceOf(msg.sender) >= _tokenPrice,
            "Not enough USDT!"
        );
        uint256 _weiAmount = _amount * 1 ether;
        uint256 _bonusAmount = (_amount * _bonus) / 100;
        _bonusAmount *= 1 ether;
        uint256 _totalAmount = _weiAmount + _bonusAmount;
        require(
            (totalSold + _totalAmount) <= presaleTokenAmount,
            "Presale token amount exceeds!"
        );
        require(
            _totalAmount <= (maxTxAmount * 1 ether),
            "Maximum transaction limit excceds!"
        );
        require(
            (_totalAmount + token.balanceOf(msg.sender)) <=
                (maxWalletAmount * 1 ether),
            "Maximum wallet token limit excceds!"
        );
        //usdt transfer to this contract
        bool successUsdtToken = usdtToken.transferFrom(
            msg.sender,
            address(this),
            _tokenPrice
        );
        require(successUsdtToken, "Failed to transfer USDT payment!");
        //purchase token transfer
        bool success = token.transferFrom(
            sellerAddress,
            msg.sender,
            _totalAmount
        );
        require(success, "Failed to transfer token!");
        totalSold += _totalAmount;
        totalFundUsdt += _tokenPrice;
    }

    // token buy
    function buyToken(uint256 _amount) public payable {
        require(presaleActive, "Presale is not active!");
        if (capsActive) {
            require(address(this).balance <= hardCap, "Hardcap limit exceeds!");
        }
        require(_amount >= 0, "Please enter minimum token!");
        uint256 _id = getCurrentStageIdActive();
        require(_id > 0, "Stage info not available!");
        uint256 _bonus = stages[_id].bonus;
        uint256 _price = stages[_id].price;
        uint256 _start = stages[_id].start;
        uint256 _end = stages[_id].end;
        require(_start <= block.timestamp, "Presale comming soon!");
        require(_end >= block.timestamp, "Presale end!");
        require(msg.value >= (_amount * _price), "Not enough payment!");
        uint256 _weiAmount = _amount * 1 ether;
        uint256 _bonusAmount = (_amount * _bonus) / 100;
        _bonusAmount *= 1 ether;
        uint256 _totalAmount = _weiAmount + _bonusAmount;
        require(
            (totalSold + _totalAmount) <= presaleTokenAmount,
            "Presale token amount exceeds!"
        );
        require(
            _totalAmount <= (maxTxAmount * 1 ether),
            "Maximum transaction limit excceds!"
        );
        require(
            (_totalAmount + token.balanceOf(msg.sender)) <=
                (maxWalletAmount * 1 ether),
            "Maximum wallet token limit excceds!"
        );
        //purchase token transfer
        bool success = token.transferFrom(
            sellerAddress,
            msg.sender,
            _totalAmount
        );
        require(success, "Failed to transfer token!");
        totalSold += _totalAmount;
        totalFund += msg.value;
    }

    // update token address
    function setToken(address _token) public onlyOwner {
        require(_token != address(0), "Token is zero address!");
        token = IERC20(_token);
    }

    // update seller address
    function setSellerAddress(address _seller) public onlyOwner {
        sellerAddress = _seller;
    }

    // update usdt token
    function setUsdtToken(address _token) public onlyOwner {
        require(_token != address(0), "Token is zero address!");
        usdtToken = IERC20(_token);
        usdtTokenAddress = IERC20Metadata(_token);
    }

    // update presale token amount
    function setPresaleTokenAmount() public onlyOwner {
        presaleTokenAmount = token.allowance(sellerAddress, address(this));
    }

    // update presale active
    function flipPresaleActive() public onlyOwner {
        presaleActive = !presaleActive;
    }

    // update caps active
    function flipCapsActive() public onlyOwner {
        capsActive = !capsActive;
    }

    // update soft cap
    function setSoftCap(uint256 _softCap) public onlyOwner {
        softCap = _softCap;
    }

    // update hard cap
    function setHardCap(uint256 _hardCap) public onlyOwner {
        hardCap = _hardCap;
    }

    // update maxTxAmount
    function setMaxTxAmount(uint256 _amount) public onlyOwner {
        maxTxAmount = _amount;
    }

    // update maxWalletAmount
    function setMaxWalletAmount(uint256 _amount) public onlyOwner {
        maxWalletAmount = _amount;
    }

    // update maximum stage
    function setMaxStage(uint256 _maxStage) public onlyOwner {
        maxStage = _maxStage;
    }

    // adding stage info
    function addStage(
        uint256 _bonus,
        uint256 _price,
        uint256 _usdtPrice,
        uint256 _start,
        uint256 _end
    ) public onlyOwner {
        uint256 _id = currentStageId + 1;
        require(_id <= maxStage, "Maximum stage excceds!");
        require(_bonus <= 100, "Bonus should be between 0 and 100");
        require(_start > 0 && _end > 0, "Invalid date!");
        require(_start < _end, "End date smaller than start!");
        currentStageId += 1;
        stages[_id].id = _id;
        stages[_id].bonus = _bonus;
        stages[_id].price = _price;
        stages[_id].usdtPrice = _usdtPrice;
        stages[_id].start = _start;
        stages[_id].end = _end;
    }

    // update stage info
    function setStage(
        uint256 _id,
        uint256 _bonus,
        uint256 _price,
        uint256 _usdtPrice,
        uint256 _start,
        uint256 _end
    ) public onlyOwner {
        require(stages[_id].id == _id, "ID doesn't exist!");
        require(_bonus <= 100, "Bonus should be between 0 and 100");
        require(_start > 0 && _end > 0, "Invalid date!");
        require(_start < _end, "End date smaller than start!");
        stages[_id].bonus = _bonus;
        stages[_id].price = _price;
        stages[_id].usdtPrice = _usdtPrice;
        stages[_id].start = _start;
        stages[_id].end = _end;
    }

    // get current stage id active
    function getCurrentStageIdActive() public view returns (uint256) {
        uint256 _id = 0;
        if (currentStageId == 0) {
            _id = 0;
        } else {
            for (uint256 i = 1; i <= currentStageId; i++) {
                if (
                    (block.timestamp >= stages[i].start) &&
                    (block.timestamp <= stages[i].end)
                ) {
                    _id = i;
                }
            }
            if (_id == 0) {
                _id = currentStageId;
            }
        }
        return _id;
    }

    // withdraw funds
    function withdrawFunds() public onlyOwner {
        if (capsActive) {
            require(softCap <= address(this).balance, "Smaller than Softcap!");
            require(hardCap == address(this).balance, "Not equal to Hardcap!");
        }
        require(
            payable(msg.sender).send(address(this).balance),
            "Failed withdraw!"
        );
        totalFund = address(this).balance;
    }

    // withdraw usdt
    function withdrawUsdt() public onlyOwner {
        if (capsActive) {
            require(softCap <= address(this).balance, "Smaller than Softcap!");
            require(hardCap == address(this).balance, "Not equal to Hardcap!");
        }
        bool success = usdtToken.transfer(
            msg.sender,
            usdtToken.balanceOf(address(this))
        );
        require(success, "Failed withdraw USDT!");
        totalFundUsdt = usdtToken.balanceOf(address(this));
    }
}