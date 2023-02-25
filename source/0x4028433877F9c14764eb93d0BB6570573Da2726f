// SPDX-License-Identifier: MIT

// File: @openzeppelin/contracts/math/SafeMath.sol

pragma solidity ^0.5.0;

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
        require(b <= a, "SafeMath: subtraction overflow");
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
        // Solidity only automatically asserts when dividing by 0
        require(b > 0, "SafeMath: division by zero");
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
        require(b != 0, "SafeMath: modulo by zero");
        return a % b;
    }
}

// File: @openzeppelin/contracts/ownership/Ownable.sol

pragma solidity ^0.5.0;

/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be aplied to your functions to restrict their use to
 * the owner.
 */
contract Ownable {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor () internal {
        _owner = msg.sender;
        emit OwnershipTransferred(address(0), _owner);
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
        require(isOwner(), "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Returns true if the caller is the current owner.
     */
    function isOwner() public view returns (bool) {
        return msg.sender == _owner;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * > Note: Renouncing ownership will leave the contract without an owner,
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

// File: internal/CYCToken.sol

pragma solidity >=0.5.0 <0.8.0;



contract CYCToken is Ownable {
    using SafeMath for uint256;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    address private _mintInvoker;
    address private _rebaseInvoker;
    uint256 private _perShareAmount;
    uint256 private _totalShares;

    mapping(address => uint256) private _shares;
    mapping(address => mapping(address => uint256)) private _allowedShares;

    modifier onlyRebaseInvoker() {
        require(
            msg.sender == _rebaseInvoker,
            "Rebase: caller is not the rebase invoker"
        );
        _;
    }

    modifier onlyMintInvoker() {
        require(msg.sender == _mintInvoker,
            "Mint: caller is not the mint invoker"
        );
        _;
    }

    constructor() public {
        _perShareAmount = 10**9;
        _totalShares = 0;
        _name = "CYCoin";
        _symbol = "CYC";
        _decimals = 18;

        _shares[msg.sender] = _totalShares;
        emit Transfer(address(0), msg.sender, _totalShares.mul(_perShareAmount));
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint8) {
        return _decimals;
    }

    function totalSupply() external view returns (uint256) {
        return _totalShares.mul(_perShareAmount);
    }

    function balanceOf(address account) external view returns (uint256) {
        return _shares[account].mul(_perShareAmount);
    }

    function transfer(address recipient, uint256 amount)
        external
        returns (bool)
    {
        uint256 share = amount.div(_perShareAmount);
        _shares[msg.sender] = _shares[msg.sender].sub(share);
        _shares[recipient] = _shares[recipient].add(share);
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender)
        external
        view
        returns (uint256)
    {
        return _allowedShares[owner][spender].mul(_perShareAmount);
    }

    function approve(address spender, uint256 amount) external returns (bool) {
        _allowedShares[msg.sender][spender] = amount.div(_perShareAmount);
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool) {
        uint256 share = amount.div(_perShareAmount);
        _allowedShares[sender][msg.sender] = _allowedShares[sender][msg.sender]
            .sub(share);
        _shares[sender] = _shares[sender].sub(share);
        _shares[recipient] = _shares[recipient].add(share);
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function mintInvoker() public view returns (address) {
        return _mintInvoker;
    }

    function rebaseInvoker() public view returns (address) {
        return _rebaseInvoker;
    }

    function perShareAmount() public view returns (uint256) {
        return _perShareAmount;
    }

    function totalShares() public view returns (uint256) {
        return _totalShares;
    }

    function changeRebaseInvoker(address newInvoker) public onlyOwner {
        require(
            newInvoker != address(0),
            "Rebase: new invoker is the zero address"
        );
        emit RebaseInvokerChanged(_rebaseInvoker, newInvoker);
        _rebaseInvoker = newInvoker;
    }

    function rebase(
        uint256 epoch,
        uint256 numerator,
        uint256 denominator
    ) external onlyRebaseInvoker returns (uint256) {
        uint256 newPerShareAmount = _perShareAmount.mul(numerator).div(
            denominator
        );
        emit Rebase(epoch, _perShareAmount, newPerShareAmount);
        _perShareAmount = newPerShareAmount;
        return _perShareAmount;
    }

    function changeMintInvoker(address newInvoker) public onlyOwner {
        require(
            newInvoker != address(0),
            "Mint: new invoker is the zero address"
        );
        emit MintInvokerChanged(_mintInvoker, newInvoker);
        _mintInvoker = newInvoker;
    }

    function mint(address to, uint256 share) external onlyMintInvoker {
        require(to != address(0), "mint to the zero address");
        _totalShares = _totalShares.add(share);
        _shares[to] = _shares[to].add(share);
        emit Transfer(address(0), to, share.mul(_perShareAmount));
    }

    event Transfer(address indexed from, address indexed to, uint256 amount);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 amount
    );

    event Rebase(
        uint256 indexed epoch,
        uint256 oldPerShareAmount,
        uint256 newPerShareAmount
    );

    event RebaseInvokerChanged(
        address indexed previousOwner,
        address indexed newOwner
    );

    event MintInvokerChanged(
        address indexed previousOwner,
        address indexed newOwner
    );
}