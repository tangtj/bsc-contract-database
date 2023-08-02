// SPDX-License-Identifier: MIT
// File: @openzeppelin/contracts/token/ERC721/IERC721Receiver.sol

// OpenZeppelin Contracts (last updated v4.6.0) (token/ERC721/IERC721Receiver.sol)

pragma solidity ^0.8.0;

/**
 * @title ERC721 token receiver interface
 * @dev Interface for any contract that wants to support safeTransfers
 * from ERC721 asset contracts.
 */
interface IERC721Receiver {
    /**
     * @dev Whenever an {IERC721} `tokenId` token is transferred to this contract via {IERC721-safeTransferFrom}
     * by `operator` from `from`, this function is called.
     *
     * It must return its Solidity selector to confirm the token transfer.
     * If any other value is returned or the interface is not implemented by the recipient, the transfer will be reverted.
     *
     * The selector can be obtained in Solidity with `IERC721Receiver.onERC721Received.selector`.
     */
    function onERC721Received(
        address operator,
        address from,
        uint256 tokenId,
        bytes calldata data
    ) external returns (bytes4);
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

// File: busd-stake-final.sol
pragma solidity 0.8.19;

interface NFT {
    function mint() external payable;

    function armaPercentage() external view returns (uint256);

    function ausdPercentage() external view returns (uint256);

    function walletOfOwner(address _owner)
        external
        view
        returns (uint256[] memory);

    function transferFrom(address _from, address _to, uint256 _tokenId) external payable;
    function safeTransferFrom(address _from, address _to, uint256 _tokenId) external payable;
}

interface Staking {
    function buyNft(address nftId, uint256 _tokenId) external;
}

interface pancake {
    function getAmountsOut(uint256 amountIn, address[] memory path)
        external
        view
        returns (uint256[] memory amounts);
}

contract BuyNftAusd is IERC721Receiver, ReentrancyGuard, Ownable {
    event Depositbusd(address user, uint256 ausd, uint256 busd, uint256 trntype, uint256 timestamp);
    event Depositausd(address user, uint256 ausd, uint256 arma, uint256 trntype, uint256 timestamp);
    event Mintausd(address user, uint256 tokenId, uint256 trntype, uint256 timestamp, address nft);
    event Mintbusd(address user, uint256 tokenId, uint256 trntype, uint256 timestamp, address nft);
    address public routerAddress = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
    IERC20 public BUSD = IERC20(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56);
    IERC20 public AUSD = IERC20(0x7e48A0eb32CDcE54794aFe505Bf0AF9A7b83Ef1E);
    IERC20 public ARMA = IERC20(0xf1F0FA0287C47804636fFeF14e2C241f2587903e);
    Staking stake = Staking(0xE5e5306B8F47A6b1397c6B7d48a3c538E4a9B6bd);
    address busd_token = 0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56;
    address arma_token = 0xf1F0FA0287C47804636fFeF14e2C241f2587903e;
    uint256 public ausdPrice = 1e18;
    uint256 public busdPer = 70e18;
    uint256 public ausdPer = 30e18;
    uint256 public armaPer = 70e18;

    struct Txns {
        address user;
        address nft;
        uint256 tokenId;
        uint256 timestamp;
        uint256 trntype; // 0 busd-ausd 1 ausd-arma
        uint256 mint; // 0 mint, 1 buy
    }
    
    struct Deposits {
        address user;
        uint256 ausd;
        uint256 busd;
        uint256 arma;
        uint256 trntype;
        uint timestamp;
    }
    
    Deposits[] public deposits;
    Txns[] public txns;

    function onERC721Received(
        address,
        address,
        uint256,
        bytes memory
    ) public virtual override returns (bytes4) {
        return this.onERC721Received.selector;
    }

    function changePercentage(uint256 ausd, uint256 busd, uint256 arma) public onlyOwner {
        require((ausd + busd) == 100e18, "busd total should be 100!");
        require((ausd + arma) == 100e18, "ausd total should be 100!");
        busdPer = busd;
        ausdPer = ausd;
        armaPer = arma;
    }
    
    function changeTokens(address ausd,address busd,address arma) public onlyOwner {
        BUSD = IERC20(busd);
        ARMA = IERC20(arma);
        AUSD = IERC20(ausd);
        busd_token = busd;
        arma_token = arma;
    }
    
    function changeStakeContract(address sContract) public onlyOwner {
        stake = Staking(sContract);
    }

    function checkValuebusd(uint256 value)
        public
        view
        returns (uint256 ausd, uint256 busd)
    {
        ausd = (value * ausdPer) / (100e18);
        busd = (value * busdPer) / (100e18);
    }

    function checkValueausd(uint256 value)
        public
        view
        returns (uint256 ausd, uint256 arma)
    {
        ausd = (value * ausdPer) / (100e18);
        arma = (value * armaPer) / (100e18);
    }

    function depositebusd(uint256 value) public{
        address sender = msg.sender;
        uint256 ausd = (value * ausdPer) / (100e18);
        uint256 busd = (value * busdPer) / (100e18);
        BUSD.transferFrom(sender, address(this), busd);
        AUSD.transferFrom(sender, address(this), ausd);
        deposits.push(Deposits({
            user: msg.sender,
            ausd : ausd,
            busd : busd,
            arma : 0,
            trntype : 0,
            timestamp : block.timestamp
        }));
        emit Depositbusd(sender, ausd, busd, 0 , block.timestamp);
    }

    function countfinal(uint256 cost,uint256 finalurate,uint256 finalarate) public view returns(uint256, uint256) {
        uint256 armafinal = ((cost * armaPer)/100 / finalarate);
        uint256 ausdfinal = ((cost * ausdPer)/100 / finalurate);
        return (armafinal, ausdfinal);
    }

    function getTokenPrice(
        uint256 amount,
        address _token1,
        address _token2
    ) private view returns (uint256) {
        address[] memory path = new address[](2);
        path[0] = _token1;
        path[1] = _token2;
        uint256[] memory price = pancake(routerAddress).getAmountsOut(
            amount,
            path
        );
        return (price[0] * 1e18) / (price[1]);
    }
    
    function depositausd(uint256 value) public {
        address sender = msg.sender;
        uint256 getPrice = getTokenPrice(1e18, busd_token, arma_token);
        uint256 cost = value;
        (uint256 totalAarma,uint256 totalAusd) = countfinal(cost, ausdPrice, getPrice);
        ARMA.transferFrom(sender, address(this), totalAarma);
        AUSD.transferFrom(sender, address(this), totalAusd);
        deposits.push(Deposits({
            user: msg.sender,
            ausd : totalAusd,
            busd : 0,
            trntype : 1,
            arma : totalAarma,
            timestamp : block.timestamp
        }));
        emit Depositausd(sender, totalAusd, totalAarma, 1, block.timestamp);
    }

    function mintnftausd(address _to,address nftAddress)
        public
        onlyOwner
    {
        NFT nft = NFT(nftAddress);
        ARMA.approve(nftAddress, ARMA.totalSupply());
        nft.mint();
        uint256 tokenId = nft.walletOfOwner(address(this))[0];
        nft.transferFrom(address(this), _to, tokenId);
        txns.push(
            Txns({
                user: _to,
                nft: nftAddress,
                tokenId: tokenId,
                trntype : 1,
                timestamp: block.timestamp,
                mint: 0
            })
        );
        emit Mintausd(_to, tokenId, block.timestamp, 1, nftAddress);
    }

    function mintnftbusd(address _to,address nftAddress)
        public
        onlyOwner
    {
        NFT nft = NFT(nftAddress);
        ARMA.approve(nftAddress, ARMA.totalSupply());
        nft.mint();
        uint256 tokenId = nft.walletOfOwner(address(this))[0];
        nft.transferFrom(address(this), _to, tokenId);
        txns.push(
            Txns({
                user: _to,
                nft: nftAddress,
                tokenId: tokenId,
                trntype : 0,
                timestamp: block.timestamp,
                mint: 0
            })
        );
        emit Mintausd(_to, tokenId, block.timestamp, 0, nftAddress);
    }

    function getTxns(address user) public view returns (Txns[] memory) {
        Txns[] memory data = new Txns[](txns.length);
        for (uint256 i = 0; i < txns.length; i++) {
            Txns storage txs = txns[i];
            if (txs.user == user) {
                data[i] = txs;
            }
        }
        return data;
    }
    function getDeposits(address user) public view returns (Deposits[] memory) {
        Deposits[] memory data = new Deposits[](deposits.length);
        for (uint256 i = 0; i < deposits.length; i++) {
            Deposits storage txs = deposits[i];
            if (txs.user == user) {
                data[i] = txs;
            }
        }
        return data;
    }
    function withdrawToken(address token, uint256 value) public onlyOwner {
        IERC20 paytoken = IERC20(token);
        paytoken.transfer(msg.sender, value);
    }
    
    function withdrawNft(address token, uint256 tokenId) public onlyOwner {
        NFT nft = NFT(token);
        nft.transferFrom(address(this), msg.sender, tokenId);
    }
}