// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

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

    function burn(uint256 amount) external;

    function burnFrom(address account, uint256 amount) external;

    function feeDistribution(uint _amount, address _addr) external;

    function emission(uint256 amount) external;

    /**
     * @dev Returns the decimals places of the token.
     */
    function decimals() external view returns (uint8);

    /**
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

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

    function mint(address to, uint256 amount) external;
}

interface IERC20Metadata is IERC20 {
    /**
     * @dev Returns the name of the token.
     */
    function name() external view returns (string memory);

    /**
     * @dev Returns the symbol of the token.
     */
    function symbol() external view returns (string memory);

}

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

contract CryptopolySale is Ownable {
    constructor(IERC20 _USDT, IERC20 _BUSD, IERC20 _USDC, IERC20 _CRP) {
        USDT = _USDT;
        BUSD = _BUSD;
        USDC = _USDC;
        CRP = _CRP;
    }

    IERC20 public CRP;
    IERC20 public USDT;
    IERC20 public BUSD;
    IERC20 public USDC;

    uint public price;
    uint public rateNativeToken;
    uint public currentStage;
    uint public sold;
    uint public min = 16666666666666666;

    mapping(address => uint) public soldAddress;

    struct Stage {
        uint sold;
        uint price;
        uint endTime;
    }
    mapping (uint => Stage) public stages;

    // Currency 1 - USDT
    // Currency 2 - BUSD
    // Currency 3 - USDC
    function buy(uint _amount, uint _currency) public {
        require(_amount >= min);
        require(_currency == 1 || _currency == 2 || _currency == 3);

        IERC20 _stable;
        _currency == 1 ? _stable = USDT : _currency == 2 ? _stable = BUSD : _stable = USDC;

        uint _amountStable = _amount / (1 * 10 ** CRP.decimals()) * stages[currentStage].price;

        require(sold + _amountStable <= stages[5].sold);

        _stable.transferFrom(msg.sender, owner(), _amountStable);
        CRP.mint(msg.sender, _amount);

        sold += _amountStable;
        soldAddress[msg.sender] += _amount;

        checkStage();
    }

    function checkStage() private {
        if(block.timestamp > stages[currentStage].endTime ) {
            currentStage++;
        } else if (sold > stages[currentStage].sold) {
            currentStage++;
        }
    }

    //Admin Functions
    function setMinOrder(uint _min) public onlyOwner {
        min = _min;
    }

    function startSale() public onlyOwner {
        //Stage #1
        stages[1].sold = 200000000000000000000000;
        stages[1].price = 600000000000000;
        stages[1].endTime = block.timestamp + 1814400;

        //Stage #2
        stages[2].sold = 300000000000000000000000;
        stages[2].price = 700000000000000;
        stages[2].endTime = block.timestamp + 3628800;

        //Stage #3
        stages[3].sold = 400000000000000000000000;
        stages[3].price = 800000000000000;
        stages[3].endTime = block.timestamp + 5443200;

        //Stage #4
        stages[4].sold = 500000000000000000000000;
        stages[4].price = 900000000000000;
        stages[4].endTime = block.timestamp + 7257600;

        //Stage #5
        stages[5].sold = 600000000000000000000000;
        stages[5].price = 1000000000000000;
        stages[5].endTime = block.timestamp + 9072000;

        currentStage = 1;
        sold = 0;
    }
}