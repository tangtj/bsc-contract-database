/***
*     ██████╗ ██████╗  ██████╗ ██████╗ ███████╗      ██████╗  ██████╗██╗  ██╗
*    ██╔════╝██╔═══██╗██╔════╝██╔═══██╗██╔════╝      ██╔══██╗██╔════╝╚██╗██╔╝
*    ██║     ██║   ██║██║     ██║   ██║███████╗█████╗██████╔╝██║      ╚███╔╝ 
*    ██║     ██║   ██║██║     ██║   ██║╚════██║╚════╝██╔══██╗██║      ██╔██╗ 
*    ╚██████╗╚██████╔╝╚██████╗╚██████╔╝███████║      ██████╔╝╚██████╗██╔╝ ██╗
*     ╚═════╝ ╚═════╝  ╚═════╝ ╚═════╝ ╚══════╝      ╚═════╝  ╚═════╝╚═╝  ╚═╝
*                                                                            
* https://cocos.finance                                
* MIT License
* ===========
*
* Copyright (c) 2020 cocos-bcx
*
* Permission is hereby granted, free of charge, to any person obtaining a copy
* of this software and associated documentation files (the "Software"), to deal
* in the Software without restriction, including without limitation the rights
* to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
* copies of the Software, and to permit persons to whom the Software is
* furnished to do so, subject to the following conditions:
*
* The above copyright notice and this permission notice shall be included in all
* copies or substantial portions of the Software.
*
* THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
* IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
* FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
* AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
* LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
* OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
*/// File: contracts/interface/IERC20.sol

pragma solidity ^0.5.0;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP. Does not include
 * the optional functions; to access them see {ERC20Detailed}.
 */
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
    function mint(address account, uint amount) external;
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
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

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
}

// File: @openzeppelin/contracts/utils/Address.sol

pragma solidity ^0.5.5;

/**
 * @dev Collection of functions related to the address type
 */
library Address {
    /**
     * @dev Returns true if `account` is a contract.
     *
     * [IMPORTANT]
     * ====
     * It is unsafe to assume that an address for which this function returns
     * false is an externally-owned account (EOA) and not a contract.
     *
     * Among others, `isContract` will return false for the following 
     * types of addresses:
     *
     *  - an externally-owned account
     *  - a contract in construction
     *  - an address where a contract will be created
     *  - an address where a contract lived, but was destroyed
     * ====
     */
    function isContract(address account) internal view returns (bool) {
        // According to EIP-1052, 0x0 is the value returned for not-yet created accounts
        // and 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470 is returned
        // for accounts without code, i.e. `keccak256('')`
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        // solhint-disable-next-line no-inline-assembly
        assembly { codehash := extcodehash(account) }
        return (codehash != accountHash && codehash != 0x0);
    }

    /**
     * @dev Converts an `address` into `address payable`. Note that this is
     * simply a type cast: the actual underlying value is not changed.
     *
     * _Available since v2.4.0._
     */
    function toPayable(address account) internal pure returns (address payable) {
        return address(uint160(account));
    }

    /**
     * @dev Replacement for Solidity's `transfer`: sends `amount` wei to
     * `recipient`, forwarding all available gas and reverting on errors.
     *
     * https://eips.ethereum.org/EIPS/eip-1884[EIP1884] increases the gas cost
     * of certain opcodes, possibly making contracts go over the 2300 gas limit
     * imposed by `transfer`, making them unable to receive funds via
     * `transfer`. {sendValue} removes this limitation.
     *
     * https://diligence.consensys.net/posts/2019/09/stop-using-soliditys-transfer-now/[Learn more].
     *
     * IMPORTANT: because control is transferred to `recipient`, care must be
     * taken to not create reentrancy vulnerabilities. Consider using
     * {ReentrancyGuard} or the
     * https://solidity.readthedocs.io/en/v0.5.11/security-considerations.html#use-the-checks-effects-interactions-pattern[checks-effects-interactions pattern].
     *
     * _Available since v2.4.0._
     */
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        // solhint-disable-next-line avoid-call-value
        (bool success, ) = recipient.call.value(amount)("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }
}

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
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     * - Subtraction cannot overflow.
     *
     * _Available since v2.4.0._
     */
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
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
        return div(a, b, "SafeMath: division by zero");
    }

    /**
     * @dev Returns the integer division of two unsigned integers. Reverts with custom message on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     *
     * _Available since v2.4.0._
     */
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        // Solidity only automatically asserts when dividing by 0
        require(b > 0, errorMessage);
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
        return mod(a, b, "SafeMath: modulo by zero");
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Reverts with custom message when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     *
     * _Available since v2.4.0._
     */
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

// File: contracts/library/SafeERC20.sol

pragma solidity ^0.5.0;





/**
 * @title SafeERC20
 * @dev Wrappers around ERC20 operations that throw on failure (when the token
 * contract returns false). Tokens that return no value (and instead revert or
 * throw on failure) are also supported, non-reverting calls are assumed to be
 * successful.
 * To use this library you can add a `using SafeERC20 for ERC20;` statement to your contract,
 * which allows you to call the safe operations as `token.safeTransfer(...)`, etc.
 */
library SafeERC20 {
    using SafeMath for uint256;
    using Address for address;

    bytes4 private constant SELECTOR = bytes4(keccak256(bytes('transfer(address,uint256)')));

    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        (bool success, bytes memory data) = address(token).call(abi.encodeWithSelector(SELECTOR, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'SafeERC20: TRANSFER_FAILED');
    }
    // function safeTransfer(IERC20 token, address to, uint256 value) internal {
    //     callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    // }

    function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
        callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    function safeApprove(IERC20 token, address spender, uint256 value) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        // solhint-disable-next-line max-line-length
        require((value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    function safeIncreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 newAllowance = token.allowance(address(this), spender).add(value);
        callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function safeDecreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 newAllowance = token.allowance(address(this), spender).sub(value, "SafeERC20: decreased allowance below zero");
        callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     */
    function callOptionalReturn(IERC20 token, bytes memory data) private {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves.

        // A Solidity high level call has three parts:
        //  1. The target address is checked to verify it contains contract code
        //  2. The call itself is made, and success asserted
        //  3. The return value is decoded, which in turn checks the size of the returned data.
        // solhint-disable-next-line max-line-length
        require(address(token).isContract(), "SafeERC20: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = address(token).call(data);
        require(success, "SafeERC20: low-level call failed");

        if (returndata.length > 0) { // Return data is optional
            // solhint-disable-next-line max-line-length
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}

// File: @openzeppelin/contracts/introspection/IERC165.sol

pragma solidity ^0.5.0;

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

pragma solidity ^0.5.0;


/**
 * @dev Required interface of an ERC721 compliant contract.
 */
contract IERC721 is IERC165 {
    event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);
    event Approval(address indexed owner, address indexed approved, uint256 indexed tokenId);
    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);

    /**
     * @dev Returns the number of NFTs in `owner`'s account.
     */
    function balanceOf(address owner) public view returns (uint256 balance);

    /**
     * @dev Returns the owner of the NFT specified by `tokenId`.
     */
    function ownerOf(uint256 tokenId) public view returns (address owner);

    /**
     * @dev Transfers a specific NFT (`tokenId`) from one account (`from`) to
     * another (`to`).
     *
     *
     *
     * Requirements:
     * - `from`, `to` cannot be zero.
     * - `tokenId` must be owned by `from`.
     * - If the caller is not `from`, it must be have been allowed to move this
     * NFT by either {approve} or {setApprovalForAll}.
     */
    function safeTransferFrom(address from, address to, uint256 tokenId) public;
    /**
     * @dev Transfers a specific NFT (`tokenId`) from one account (`from`) to
     * another (`to`).
     *
     * Requirements:
     * - If the caller is not `from`, it must be approved to move this NFT by
     * either {approve} or {setApprovalForAll}.
     */
    function transferFrom(address from, address to, uint256 tokenId) public;
    function approve(address to, uint256 tokenId) public;
    function getApproved(uint256 tokenId) public view returns (address operator);

    function setApprovalForAll(address operator, bool _approved) public;
    function isApprovedForAll(address owner, address operator) public view returns (bool);


    function safeTransferFrom(address from, address to, uint256 tokenId, bytes memory data) public;
}

// File: contracts/interface/ICocosNFT.sol

pragma solidity ^0.5.0;



contract ICocosNFT is IERC721 {

    struct CocosNFT {
        uint256 id;
        uint256 grade;
        uint256 quality;
        uint256 amount;
        uint256 resBaseId;
        uint256 tLevel;
        uint256 ruleId;
        uint256 nftType;
        address author;
        address erc20;
        uint256 createdTime;
        uint256 blockNum;
    }
    
    function mint(address to, uint256 tokenId) external returns (bool) ;
    function burn(uint256 tokenId) external;
}

// File: contracts/interface/ICocosNFTFactory.sol

pragma solidity ^0.5.0;

pragma experimental ABIEncoderV2;


interface ICocosNFTFactory {


    function getCocosNFT(uint256 tokenId)
        external view
        returns (
            uint256 grade,
            uint256 quality,
            uint256 amount,
            uint256 resBaseId,
            uint256 tLevel,
            uint256 ruleId,
            uint256 nftType,
            address author,
            address erc20,
            uint256 createdTime,
            uint256 blockNum
        );

    struct MintData{
        uint256 amount;
        uint256 resBaseId;
        uint256 nftType;
        uint256 ruleId;
        uint256 tLevel;
    }

    struct MintExtraData {
        uint256 cocosNFT_id;
        uint256 grade;
        uint256 quality;
        address author;
    }
    function getCocosNFTStruct(uint256 tokenId)
        external view
        returns (ICocosNFT.CocosNFT memory cocosNFT);

    function burn(uint256 tokenId) external returns ( bool );
    
    function isRulerProxyContract(address proxy) external view returns ( bool );
    
    function gmMint(MintData calldata mintData, MintExtraData calldata extraData) external;
}

// File: contracts/interface/ICocosNFTRuleProxy.sol

pragma solidity ^0.5.0;




interface ICocosNFTRuleProxy  {

    struct Cost721Asset{
        uint256 costErc721Id1;
        uint256 costErc721Id2;
        uint256 costErc721Id3;

        address costErc721Origin;
    }

    struct MintParams{
        address user;
        uint256 amount;
        uint256 ruleId;
    }

    function cost( MintParams calldata params, Cost721Asset calldata costSet1, Cost721Asset calldata costSet2 ) external returns (
        uint256 mintAmount,
        address mintErc20
    ) ;

    function destroy( address owner, ICocosNFT.CocosNFT calldata cocosNFT ) external ;

    function generate( address user,uint256 ruleId ) external view returns (ICocosNFT.CocosNFT memory cocosNFT );

}

// File: contracts/library/Governance.sol

pragma solidity ^0.5.0;

contract Governance {

    address public _governance;

    constructor() public {
        _governance = tx.origin;
    }

    event GovernanceTransferred(address indexed previousOwner, address indexed newOwner);

    modifier onlyGovernance {
        require(msg.sender == _governance, "not governance");
        _;
    }

    function setGovernance(address governance)  public  onlyGovernance
    {
        require(governance != address(0), "new governance the zero address");
        emit GovernanceTransferred(_governance, governance);
        _governance = governance;
    }


}

// File: contracts/nft/CocosNFTFactory.sol

pragma solidity ^0.5.5;










contract CocosNFTFactory is Governance, ICocosNFTFactory {
    using SafeERC20 for IERC20;
    using Address for address;
    using SafeMath for uint256;

    event CocosNFTAdded(
        uint256 indexed id,
        uint256 grade,
        uint256 quality,
        uint256 amount,
        uint256 resBaseId,
        uint256 tLevel,
        uint256 ruleId,
        uint256 nftType,
        address author,
        address erc20,
        uint256 createdTime,
        uint256 blockNum
    );

    event CocosNFTBurn(
        uint256 indexed id,
        uint256 amount,
        address erc20
    );

    event NFTReceived(address operator, address from, uint256 tokenId, bytes data);

    // for minters
    mapping(address => bool) public _minters;

    mapping(uint256 => ICocosNFT.CocosNFT) public _cocosNFTs;

    mapping(uint256 => ICocosNFTRuleProxy) public _ruleProxys;

    mapping(address => bool) public _ruleProxyFlags;

    uint256 public _maxCocosNFTId = 1000000;
    uint256 public _cocosNFTId = _maxCocosNFTId;


    ICocosNFT public _cocosNFT = ICocosNFT(0x0);

    bool public _isUserStart = false;

    constructor(address cocosNFT) public {
        _cocosNFT = ICocosNFT(cocosNFT);
    }

    function setUserStart(bool start) public onlyGovernance {
        _isUserStart = start;
    }

    function addMinter(address minter) public onlyGovernance {
        _minters[minter] = true;
    }

    function removeMinter(address minter) public onlyGovernance {
        _minters[minter] = false;
    }


    // only function for creating additional rewards from dust
    function seize(IERC20 asset, address teamWallet) public onlyGovernance {
        uint256 balance = asset.balanceOf(address(this));
        asset.safeTransfer(teamWallet, balance);
    }
    

    /**
     * @dev add cocosNFT mint strategy address
     * can't remove
     */
    function addCocosNFTRuleProxy(uint256 nftType, address ruleProxy)  public  
    onlyGovernance{
        require(_ruleProxys[nftType] == ICocosNFTRuleProxy(0x0), "must null");

        _ruleProxys[nftType] = ICocosNFTRuleProxy(ruleProxy);

        _ruleProxyFlags[ruleProxy] = true;
    }

    function isRulerProxyContract(address proxy) external view returns ( bool ){
        return _ruleProxyFlags[proxy];
    }

    /*
     * @dev set cocosNFT contract address
     */
    function setCocosNFTContract(address cocosNFT)  public  
        onlyGovernance{
        _cocosNFT = ICocosNFT(cocosNFT);
    }

    function setCurrentCocosNFTId(uint256 id)  public  
        onlyGovernance{
        _cocosNFTId = id;
    }

    uint private unlocked = 1;
    modifier lock() {
        require(unlocked == 1, 'CocosNFTFactoryV2: LOCKED');
        unlocked = 0;
        _;
        unlocked = 1;
    }

    function getCocosNFT(uint256 tokenId)
        external view
        returns (
            uint256 grade,
            uint256 quality,
            uint256 amount,
            uint256 resBaseId,
            uint256 tLevel,
            uint256 ruleId,
            uint256 nftType,
            address author,
            address erc20,
            uint256 createdTime,
            uint256 blockNum
        )
    {
        ICocosNFT.CocosNFT storage cocosNFT = _cocosNFTs[tokenId];
        require(cocosNFT.id > 0, "cocosNFT not exist");
        grade = cocosNFT.grade;
        quality = cocosNFT.quality;
        amount = cocosNFT.amount;
        resBaseId = cocosNFT.resBaseId;
        tLevel = cocosNFT.tLevel;
        ruleId = cocosNFT.ruleId;
        nftType = cocosNFT.nftType;
        author = cocosNFT.author;
        erc20 = cocosNFT.erc20;
        createdTime = cocosNFT.createdTime;
        blockNum = cocosNFT.blockNum;
    }

    function getCocosNFTStruct(uint256 tokenId)
        external view
        returns (ICocosNFT.CocosNFT memory cocosNFT){
            require(_cocosNFTs[tokenId].id > 0, "cocosNFT not exist");
            cocosNFT = _cocosNFTs[tokenId];
        }


    function setBaseResId(uint256 tokenId, uint256 resBaseId) external onlyGovernance {
        require( _cocosNFTs[tokenId].id > 0, "cocosNFT not exist");
        _cocosNFTs[tokenId].resBaseId = resBaseId;
    }

    function mint(
        MintData memory mintData, 
        ICocosNFTRuleProxy.Cost721Asset memory costSet1, 
        ICocosNFTRuleProxy.Cost721Asset memory costSet2 
        ) public lock 
    {
        address origin = msg.sender;
        if(_minters[msg.sender] == false){
            require(!origin.isContract(), "call to non-contract");
        }

        require(_isUserStart ||  _minters[msg.sender]  , "can't mint" );

        require( _ruleProxys[mintData.nftType] != ICocosNFTRuleProxy(0x0), " invalid mint nftType !" );

        uint256 mintAmount;
        address mintErc20;

        ICocosNFTRuleProxy.MintParams memory params;
        params.user = msg.sender;
        params.amount = mintData.amount;
        params.ruleId = mintData.ruleId;

        (mintAmount,mintErc20) = _ruleProxys[mintData.nftType].cost( params,costSet1,costSet2 );

        ICocosNFT.CocosNFT memory cocosNFT = _ruleProxys[mintData.nftType].generate( msg.sender, mintData.ruleId );

        uint256 cocosNFTId = cocosNFT.id;
        if(cocosNFTId  == 0){
            _cocosNFTId++;
            cocosNFTId = _cocosNFTId;
        }
        cocosNFT.id = cocosNFTId;
        cocosNFT.blockNum = cocosNFT.blockNum > 0 ? cocosNFT.blockNum:block.number;
        cocosNFT.createdTime =  cocosNFT.createdTime > 0?cocosNFT.createdTime:block.timestamp ;

        cocosNFT.amount = cocosNFT.amount>0?cocosNFT.amount:mintAmount;
        cocosNFT.resBaseId = cocosNFT.resBaseId>0?cocosNFT.resBaseId:mintData.resBaseId;
        cocosNFT.tLevel = cocosNFT.tLevel>0?cocosNFT.tLevel:mintData.tLevel;
        cocosNFT.erc20 = cocosNFT.erc20==address(0x0)?mintErc20:cocosNFT.erc20;

        cocosNFT.ruleId = mintData.ruleId;
        cocosNFT.nftType = mintData.nftType;
        cocosNFT.author = cocosNFT.author==address(0x0)?msg.sender:cocosNFT.author;

        _cocosNFTs[cocosNFTId] = cocosNFT;

        _cocosNFT.mint(msg.sender, cocosNFTId);

        emit CocosNFTAdded(
            cocosNFT.id,
            cocosNFT.grade,
            cocosNFT.quality,
            cocosNFT.amount,
            cocosNFT.resBaseId,
            cocosNFT.tLevel,
            cocosNFT.ruleId,
            cocosNFT.nftType,
            cocosNFT.author,
            cocosNFT.erc20,
            cocosNFT.createdTime,
            cocosNFT.blockNum
        );

    } 


    function gmMint(MintData memory mintData, MintExtraData memory extraData) public {
        require(_minters[msg.sender]  , "can't mint");

        ICocosNFTRuleProxy.Cost721Asset memory costSet1;
        ICocosNFTRuleProxy.Cost721Asset memory costSet2;

        ICocosNFTRuleProxy.MintParams memory params;
        params.user = msg.sender;
        params.amount = mintData.amount;
        params.ruleId = mintData.ruleId;

        uint256 mintAmount;
        address mintErc20;

        (mintAmount,mintErc20) = _ruleProxys[mintData.nftType].cost( params,costSet1,costSet2 );
      
        uint256 cocosNFTId = extraData.cocosNFT_id;
        if(extraData.cocosNFT_id == 0){
            _cocosNFTId++ ;
            cocosNFTId = _cocosNFTId;
        }else{
            if(cocosNFTId > _cocosNFTId){
                _cocosNFTId =  cocosNFTId;
            }
        }

        ICocosNFT.CocosNFT memory cocosNFT;
        cocosNFT.id = cocosNFTId;
        cocosNFT.blockNum = block.number;
        cocosNFT.createdTime =  block.timestamp ;
        cocosNFT.grade = extraData.grade;
        cocosNFT.quality = extraData.quality;
        cocosNFT.amount = mintAmount;
        cocosNFT.resBaseId = mintData.resBaseId;
        cocosNFT.tLevel = mintData.tLevel;
        cocosNFT.ruleId = mintData.ruleId;
        cocosNFT.nftType = mintData.nftType;
        cocosNFT.author = extraData.author;
        cocosNFT.erc20 = mintErc20;

        _cocosNFTs[cocosNFTId] = cocosNFT;

        _cocosNFT.mint(extraData.author, cocosNFTId);

        emit CocosNFTAdded(
            cocosNFT.id,
            cocosNFT.grade,
            cocosNFT.quality,
            cocosNFT.amount,
            cocosNFT.resBaseId,
            cocosNFT.tLevel,
            cocosNFT.ruleId,
            cocosNFT.nftType,
            cocosNFT.author,
            cocosNFT.erc20,
            cocosNFT.createdTime,
            cocosNFT.blockNum
        );
    } 


    function burn(uint256 tokenId) external returns ( bool ) {
      
        ICocosNFT.CocosNFT memory cocosNFT = _cocosNFTs[tokenId];
        require(cocosNFT.id > 0, "not exist");

        _cocosNFT.safeTransferFrom(msg.sender, address(this), tokenId);
        _cocosNFT.burn(tokenId);

        emit CocosNFTBurn(cocosNFT.id, cocosNFT.amount, cocosNFT.erc20);

        _ruleProxys[cocosNFT.nftType].destroy( msg.sender, cocosNFT );

        _cocosNFTs[tokenId].id = 0;

        return true;
    }

    
    function onERC721Received(address operator, address from, uint256 tokenId, bytes memory data) public returns (bytes4) {
        //only receive the _nft staff
        if(address(this) != operator) {
            //invalid from nft
            return 0;
        }
        //success
        emit NFTReceived(operator, from, tokenId, data);
        return bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"));
    }
}