// File: @openzeppelin/contracts/utils/Counters.sol


// OpenZeppelin Contracts v4.4.1 (utils/Counters.sol)

pragma solidity ^0.8.0;

/**
 * @title Counters
 * @author Matt Condon (@shrugs)
 * @dev Provides counters that can only be incremented, decremented or reset. This can be used e.g. to track the number
 * of elements in a mapping, issuing ERC721 ids, or counting request ids.
 *
 * Include with `using Counters for Counters.Counter;`
 */
library Counters {
    struct Counter {
        // This variable should never be directly accessed by users of the library: interactions must be restricted to
        // the library's function. As of Solidity v0.5.2, this cannot be enforced, though there is a proposal to add
        // this feature: see https://github.com/ethereum/solidity/issues/4637
        uint256 _value; // default: 0
    }

    function current(Counter storage counter) internal view returns (uint256) {
        return counter._value;
    }

    function increment(Counter storage counter) internal {
        unchecked {
            counter._value += 1;
        }
    }

    function decrement(Counter storage counter) internal {
        uint256 value = counter._value;
        require(value > 0, "Counter: decrement overflow");
        unchecked {
            counter._value = value - 1;
        }
    }

    function reset(Counter storage counter) internal {
        counter._value = 0;
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

// File: @openzeppelin/contracts/token/ERC721/extensions/IERC721Enumerable.sol


// OpenZeppelin Contracts (last updated v4.5.0) (token/ERC721/extensions/IERC721Enumerable.sol)

pragma solidity ^0.8.0;


/**
 * @title ERC-721 Non-Fungible Token Standard, optional enumeration extension
 * @dev See https://eips.ethereum.org/EIPS/eip-721
 */
interface IERC721Enumerable is IERC721 {
    /**
     * @dev Returns the total amount of tokens stored by the contract.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns a token ID owned by `owner` at a given `index` of its token list.
     * Use along with {balanceOf} to enumerate all of ``owner``'s tokens.
     */
    function tokenOfOwnerByIndex(address owner, uint256 index) external view returns (uint256);

    /**
     * @dev Returns a token ID at a given `index` of all the tokens stored by the contract.
     * Use along with {totalSupply} to enumerate all tokens.
     */
    function tokenByIndex(uint256 index) external view returns (uint256);
}

// File: @uniswap/v3-core/contracts/libraries/BitMath.sol


pragma solidity >=0.5.0;

/// @title BitMath
/// @dev This library provides functionality for computing bit properties of an unsigned integer
library BitMath {
    /// @notice Returns the index of the most significant bit of the number,
    ///     where the least significant bit is at index 0 and the most significant bit is at index 255
    /// @dev The function satisfies the property:
    ///     x >= 2**mostSignificantBit(x) and x < 2**(mostSignificantBit(x)+1)
    /// @param x the value for which to compute the most significant bit, must be greater than 0
    /// @return r the index of the most significant bit
    function mostSignificantBit(uint256 x) internal pure returns (uint8 r) {
        require(x > 0);

        if (x >= 0x100000000000000000000000000000000) {
            x >>= 128;
            r += 128;
        }
        if (x >= 0x10000000000000000) {
            x >>= 64;
            r += 64;
        }
        if (x >= 0x100000000) {
            x >>= 32;
            r += 32;
        }
        if (x >= 0x10000) {
            x >>= 16;
            r += 16;
        }
        if (x >= 0x100) {
            x >>= 8;
            r += 8;
        }
        if (x >= 0x10) {
            x >>= 4;
            r += 4;
        }
        if (x >= 0x4) {
            x >>= 2;
            r += 2;
        }
        if (x >= 0x2) r += 1;
    }

    /// @notice Returns the index of the least significant bit of the number,
    ///     where the least significant bit is at index 0 and the most significant bit is at index 255
    /// @dev The function satisfies the property:
    ///     (x & 2**leastSignificantBit(x)) != 0 and (x & (2**(leastSignificantBit(x)) - 1)) == 0)
    /// @param x the value for which to compute the least significant bit, must be greater than 0
    /// @return r the index of the least significant bit
    function leastSignificantBit(uint256 x) internal pure returns (uint8 r) {
        require(x > 0);

        r = 255;
        if (x & type(uint128).max > 0) {
            r -= 128;
        } else {
            x >>= 128;
        }
        if (x & type(uint64).max > 0) {
            r -= 64;
        } else {
            x >>= 64;
        }
        if (x & type(uint32).max > 0) {
            r -= 32;
        } else {
            x >>= 32;
        }
        if (x & type(uint16).max > 0) {
            r -= 16;
        } else {
            x >>= 16;
        }
        if (x & type(uint8).max > 0) {
            r -= 8;
        } else {
            x >>= 8;
        }
        if (x & 0xf > 0) {
            r -= 4;
        } else {
            x >>= 4;
        }
        if (x & 0x3 > 0) {
            r -= 2;
        } else {
            x >>= 2;
        }
        if (x & 0x1 > 0) r -= 1;
    }
}

// File: ElephanNFTRarity.sol

/*
    SPDX-License-Identifier: MIT
    A Bankteller Production
    Elephant Money
    Copyright 2023
*/

pragma solidity ^0.8.9;





struct ElephantNFTTraits {
    uint rare;
    uint hue; 
    uint sat; 
    uint lum;
}

struct ElephantNFTRarityScore {
    uint tokenId;
    uint score;
}


contract ElephantNFTTraitTracker {
    using Counters for Counters.Counter;
    enum Trait { Rare, Hue, Sat, Lum }

    mapping (uint => uint8) private tokenTracker;

    mapping (uint => ElephantNFTTraits) private tokenTraits;

    mapping (Trait => mapping(uint => uint )) private  traitCount;

    IERC721Enumerable public nft;

    Counters.Counter private  counter;

    event TokenAddition(uint tokenId, uint rare, uint hue, uint sat, uint lum );
    event TraitCountUpdate(Trait trait, uint value,  uint count);
    event ProcessedSupply(uint estimatedSupply);

    constructor(address _nft) {

        nft = IERC721Enumerable(_nft);
        counter.increment(); //start at 1
    }

    

    function isAdded(uint tokenId) public view returns (bool added) {
        added = tokenTracker[tokenId] == 1;
    }

    function add(uint batchSize) external {  

        require(batchSize > 0, "batchSize must be non-zerp");
        
        uint totalSupply = nft.totalSupply();

        uint256 tokenId = counter.current();
        uint256 current = 0;

        while (current < batchSize && tokenId <= totalSupply) {
            add();
            tokenId = counter.current();
            current++;
        }

    }

    function getInfo(uint tokenId)  public view returns ( uint rare, uint hue, uint sat, uint lum, uint score) {
        
        uint totalSupply = nft.totalSupply();
        
        require(tokenId <= totalSupply, "token not in set");
        
        (rare, hue, sat, lum) = attributes(tokenId, address(nft)); 

        //compute rarity 
        score += (traitCount[Trait.Rare][rare] > 0) ?  1e18 / traitCount[Trait.Rare][rare] / totalSupply : 0;
        score += (traitCount[Trait.Hue][hue] > 0) ?  1e18 / traitCount[Trait.Hue][hue] / totalSupply : 0; 
        score += (traitCount[Trait.Sat][sat] > 0) ?  1e18 / traitCount[Trait.Sat][sat] / totalSupply : 0; 
        score += (traitCount[Trait.Lum][lum] > 0) ?  1e18 / traitCount[Trait.Lum][lum] / totalSupply : 0; 

    }

    function getScores(uint start, uint end) external view returns (ElephantNFTRarityScore[] memory scores){

        uint totalSupply = nft.totalSupply();

        end = (end == 0) ?  totalSupply : end;  //scan from start total supply when the end is not specified
        
        require(end > start && start > 0 && end <= totalSupply, "range out of bounds");

        uint length = end - start + 1;

        scores = new ElephantNFTRarityScore[](length);

        uint score;

        for (uint i = 0; i < length; i++ ) {
            (, , , , score) = getInfo(start);
            scores[i] = ElephantNFTRarityScore(start, score);
            start++;
        } 

    }

    function add()  private {
        uint tokenId = counter.current();

        if (tokenTracker[tokenId] == 0 ) {

            ( uint rare, uint hue, uint sat, uint lum) = attributes(tokenId, address(nft));

            tokenTracker[tokenId] = 1;

            //count occurance
            traitCount[Trait.Rare][rare] += 1; 
            traitCount[Trait.Hue][hue] += 1;
            traitCount[Trait.Sat][sat] += 1;
            traitCount[Trait.Lum][lum] += 1;

            //next
            counter.increment();

            emit TokenAddition(tokenId, rare, hue, sat, lum);
            emit TraitCountUpdate(Trait.Rare, rare, traitCount[Trait.Rare][rare]);
            emit TraitCountUpdate(Trait.Hue, hue, traitCount[Trait.Hue][hue]);
            emit TraitCountUpdate(Trait.Sat, sat, traitCount[Trait.Sat][sat]);
            emit TraitCountUpdate(Trait.Lum, lum, traitCount[Trait.Lum][lum]);
            emit ProcessedSupply(tokenId);          
        }
    }

    function isRare(uint256 tokenId, address collection) private pure returns (bool) {
        bytes32 h = keccak256(abi.encodePacked(tokenId, collection));
        return uint256(h) < type(uint256).max / (1 + BitMath.mostSignificantBit(tokenId) * 2);
    }

    function attributes(uint tokenId, address collection) private pure returns ( uint rare, uint hue, uint sat, uint lum) {

        //we don't want traverse the hue spectrum linearly
        //we hash to pick a random spot on the rainbow
        uint h = uint256(keccak256(abi.encodePacked(tokenId, collection)));

        hue = (h % 360) + 1;

        sat = 50 + (h % 50);

        lum = h % 30;

        rare = 0;


        if (isRare(tokenId, collection)) {

            hue = 0;
            sat = 0;
            rare = 1;

        } else {

            lum = 60;

        }
    }
}