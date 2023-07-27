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

// File: @openzeppelin/contracts/utils/introspection/IERC165.sol


// OpenZeppelin Contracts v4.4.1 (utils/introspection/IERC165.sol)

pragma solidity ^0.8.0;

/**
 * @dev Interface of the ERC165 standard, as defined in the
 * https://eips.ethereum.org/EIPS/eip-165[EIP].
 *
 * Implementers can declare support of contract interfaces, which can then be
 * queried by others ({ERC165Checker}).
 *
 * For an implementation, see {ERC165}.
 */
interface IERC165 {
    /**
     * @dev Returns true if this contract implements the interface defined by
     * `interfaceId`. See the corresponding
     * https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified[EIP section]
     * to learn more about how these ids are created.
     *
     * This function call must use less than 30 000 gas.
     */
    function supportsInterface(bytes4 interfaceId) external view returns (bool);
}

// File: @openzeppelin/contracts/token/ERC721/IERC721.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC721/IERC721.sol)

pragma solidity ^0.8.0;


/**
 * @dev Required interface of an ERC721 compliant contract.
 */
interface IERC721 is IERC165 {
    /**
     * @dev Emitted when `tokenId` token is transferred from `from` to `to`.
     */
    event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);

    /**
     * @dev Emitted when `owner` enables `approved` to manage the `tokenId` token.
     */
    event Approval(address indexed owner, address indexed approved, uint256 indexed tokenId);

    /**
     * @dev Emitted when `owner` enables or disables (`approved`) `operator` to manage all of its assets.
     */
    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);

    /**
     * @dev Returns the number of tokens in ``owner``'s account.
     */
    function balanceOf(address owner) external view returns (uint256 balance);

    /**
     * @dev Returns the owner of the `tokenId` token.
     *
     * Requirements:
     *
     * - `tokenId` must exist.
     */
    function ownerOf(uint256 tokenId) external view returns (address owner);

    /**
     * @dev Safely transfers `tokenId` token from `from` to `to`.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `tokenId` token must exist and be owned by `from`.
     * - If the caller is not `from`, it must be approved to move this token by either {approve} or {setApprovalForAll}.
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon a safe transfer.
     *
     * Emits a {Transfer} event.
     */
    function safeTransferFrom(address from, address to, uint256 tokenId, bytes calldata data) external;

    /**
     * @dev Safely transfers `tokenId` token from `from` to `to`, checking first that contract recipients
     * are aware of the ERC721 protocol to prevent tokens from being forever locked.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `tokenId` token must exist and be owned by `from`.
     * - If the caller is not `from`, it must have been allowed to move this token by either {approve} or {setApprovalForAll}.
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon a safe transfer.
     *
     * Emits a {Transfer} event.
     */
    function safeTransferFrom(address from, address to, uint256 tokenId) external;

    /**
     * @dev Transfers `tokenId` token from `from` to `to`.
     *
     * WARNING: Note that the caller is responsible to confirm that the recipient is capable of receiving ERC721
     * or else they may be permanently lost. Usage of {safeTransferFrom} prevents loss, though the caller must
     * understand this adds an external call which potentially creates a reentrancy vulnerability.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `tokenId` token must be owned by `from`.
     * - If the caller is not `from`, it must be approved to move this token by either {approve} or {setApprovalForAll}.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 tokenId) external;

    /**
     * @dev Gives permission to `to` to transfer `tokenId` token to another account.
     * The approval is cleared when the token is transferred.
     *
     * Only a single account can be approved at a time, so approving the zero address clears previous approvals.
     *
     * Requirements:
     *
     * - The caller must own the token or be an approved operator.
     * - `tokenId` must exist.
     *
     * Emits an {Approval} event.
     */
    function approve(address to, uint256 tokenId) external;

    /**
     * @dev Approve or remove `operator` as an operator for the caller.
     * Operators can call {transferFrom} or {safeTransferFrom} for any token owned by the caller.
     *
     * Requirements:
     *
     * - The `operator` cannot be the caller.
     *
     * Emits an {ApprovalForAll} event.
     */
    function setApprovalForAll(address operator, bool approved) external;

    /**
     * @dev Returns the account approved for `tokenId` token.
     *
     * Requirements:
     *
     * - `tokenId` must exist.
     */
    function getApproved(uint256 tokenId) external view returns (address operator);

    /**
     * @dev Returns if the `operator` is allowed to manage all of the assets of `owner`.
     *
     * See {setApprovalForAll}
     */
    function isApprovedForAll(address owner, address operator) external view returns (bool);
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

// File: contracts/TTDAOAirDropNode_NEW.sol


pragma solidity ^0.8.9;





interface ITTDAONode {
    function totalSupply() external view returns (uint256);

    function tokenOfOwnerByIndex(address owner, uint256 index) external view returns (uint256);

    function balanceOf(address owner) external view returns (uint256 balance);

    function ownerOf(uint256 tokenId) external view returns (address owner);
}

interface ITTDAONodeMint {
    function getNodeMint(uint256 t, address account) external view returns (uint256, uint256);
}

interface IBank {
    function withdraw(address account, uint256 amount) external;

    function burn(uint256 amount) external;
}

contract TTDAOAirDropNode is Ownable {
    using SafeMath for uint256;

    address public  _ttDaoAddress = 0xdD7d5DD1C91abE4376065266a1956e47c7567D5b;
    address public  _ttDaoNodeAddress = 0x4AEFB89b8c6493E94E5E9Ac9F223EE25589b4522;
    uint256 public  constant DAY_SEC = 86400;
    uint256 public  constant DAY_AIRDROP1 = 3000 ether;
    uint256 public  constant DAY_AIRDROP2 = 7000 ether;
    uint256 public  startTime = 0;

    mapping(uint256 => uint256) public    airdropDayNodeCount;
    mapping(uint256 => uint256) public    airdropLastClaimTime;
    mapping(address => uint256) public    airdropTotal;
    mapping(address => uint256) public    airdropMintClaimTime;
    mapping(uint256 => bool)    public    hasBurn;

    address private bankAdddress = 0x848a3AD48Fd544d63397fB521c7365FF4E4c2592;
    ITTDAONodeMint private minter = ITTDAONodeMint(0x4860d3371D4D3Cd9057Dc4c5bC7143913b64F8F4);

    constructor() {
        startTime = 1690243200 / DAY_SEC;
    }

    function _claimByToken(uint256 tokenId, uint256 dayIndex) internal view returns (uint256) {
        uint256 amount = 0;
        for (uint256 i = airdropLastClaimTime[tokenId]; i < dayIndex; ++i) {
            if (tokenId <= airdropDayNodeCount[i]) {
                amount = amount.add(DAY_AIRDROP1.div(airdropDayNodeCount[i]));
            }
        }
        return amount;
    }

    function _claimMintReward(address account, uint256 dayIndex) internal view returns (uint256) {

        uint256 nodeMintCount;
        uint256 mintAll;
        uint256 amount = 0;

        if (airdropMintClaimTime[account] == 0) {
            return 0;
        }

        for (uint256 i = airdropMintClaimTime[account]; i < dayIndex; ++i) {
            (nodeMintCount, mintAll) = minter.getNodeMint(i, account);
            if (mintAll == 0) {
                continue;
            }
            uint256 totalSupply = airdropDayNodeCount[i];
            if (mintAll < totalSupply) {
                mintAll = totalSupply;
            }
            amount = amount.add(nodeMintCount * DAY_AIRDROP2 / mintAll);
        }
        return amount;
    }

    function claimByToken(uint256 tokenId) public {
        uint256 dayIndex = block.timestamp / DAY_SEC;
        if (dayIndex.sub(startTime) > 360) {
            dayIndex = startTime + 360;
        }
        // update all data
        update();

        ITTDAONode node = ITTDAONode(_ttDaoNodeAddress);
        require(node.ownerOf(tokenId) == msg.sender, "not yours");
        uint256 claimAmount = _claimByToken(tokenId, dayIndex);
        airdropLastClaimTime[tokenId] = dayIndex;

        require(claimAmount > 0, "no claim token");
        IBank(bankAdddress).withdraw(msg.sender, claimAmount);
        airdropTotal[msg.sender] = airdropTotal[msg.sender].add(claimAmount);
    }

    function claim() public {
        uint256 dayIndex = block.timestamp / DAY_SEC;
        if (dayIndex.sub(startTime) > 360) {
            dayIndex = startTime + 360;
        }

        // update all data
        update();

        // claim
        uint256 claimAmount = 0;
        ITTDAONode node = ITTDAONode(_ttDaoNodeAddress);

        // fix
        uint256 ownerTokenCount = node.balanceOf(msg.sender);
        for (uint256 i = 0; i < ownerTokenCount; i++) {
            uint256 tokenId = node.tokenOfOwnerByIndex(msg.sender, i);
            claimAmount = claimAmount.add(_claimByToken(tokenId, dayIndex));
            airdropLastClaimTime[tokenId] = dayIndex;
        }

        // mint
        claimAmount = claimAmount.add(_claimMintReward(msg.sender, dayIndex));
        airdropMintClaimTime[msg.sender] = dayIndex;

        require(claimAmount > 0, "no claim token");

        IBank(bankAdddress).withdraw(msg.sender, claimAmount);
        airdropTotal[msg.sender] = airdropTotal[msg.sender].add(claimAmount);
    }

    function update() public {
        uint256 dayIndex = block.timestamp / DAY_SEC;
        if (dayIndex.sub(startTime) > 360) {
            dayIndex = startTime + 360;
        }
        uint256 totalSupply = ITTDAONode(_ttDaoNodeAddress).totalSupply();

        for (uint256 i = dayIndex; i >= startTime; --i) {
            if (airdropDayNodeCount[i] == 0) {
                airdropDayNodeCount[i] = totalSupply;
            } else {
                break;
            }
        }

        for (uint256 tokenId = totalSupply; tokenId >= 1; --tokenId) {
            if (airdropLastClaimTime[tokenId] == 0) {
                airdropLastClaimTime[tokenId] = dayIndex - 1;
                address account = ITTDAONode(_ttDaoNodeAddress).ownerOf(tokenId);
                if (airdropMintClaimTime[account] == 0) {
                    airdropMintClaimTime[account] = dayIndex - 1;
                }
            } else {
                break;
            }
        }

        uint256 lastDay = dayIndex - 1;
        if (!hasBurn[lastDay] && lastDay >= startTime) {
            uint256 lastTotalSupply = airdropDayNodeCount[lastDay];

            (, uint256 mintAll) = minter.getNodeMint(lastDay, address(0));
            uint256 burnAmount;
            if (mintAll == 0 || lastTotalSupply == 0) {
                burnAmount = DAY_AIRDROP2;
            } else {
                if (mintAll < lastTotalSupply) {
                    burnAmount = DAY_AIRDROP2 - DAY_AIRDROP2 * mintAll / lastTotalSupply;
                }
            }

            if (burnAmount > 0) {
                IBank(bankAdddress).burn(burnAmount);
            }
            hasBurn[lastDay] = true;

        }
    }

    function setAddress(address ttDaoAddr) public onlyOwner {
        _ttDaoAddress = ttDaoAddr;
    }

    function getAmount(address account) public view returns (uint256 availableAmount, uint256 claimedAmount) {
        uint256 dayIndex = block.timestamp / DAY_SEC;
        if (dayIndex.sub(startTime) > 360) {
            dayIndex = startTime + 360;
        }

        ITTDAONode node = ITTDAONode(_ttDaoNodeAddress);
        uint256 ownerTokenCount = node.balanceOf(account);
        for (uint256 i; i < ownerTokenCount; i++) {
            uint256 tokenId = node.tokenOfOwnerByIndex(account, i);
            availableAmount = availableAmount.add(_claimByToken(tokenId, dayIndex));
        }

        uint256 nodeMintCount;
        uint256 mintAll;

        if (airdropMintClaimTime[account] > 0) {
            for (uint256 i = airdropMintClaimTime[account]; i < dayIndex; ++i) {
                (nodeMintCount, mintAll) = minter.getNodeMint(i, account);
                if (mintAll == 0) {
                    continue;
                }
                uint256 totalSupply = airdropDayNodeCount[i];
                if (mintAll < totalSupply) {
                    mintAll = totalSupply;
                }
                availableAmount = availableAmount.add(nodeMintCount * DAY_AIRDROP2 / mintAll);
            }
        }

        claimedAmount = airdropTotal[account];
    }

}