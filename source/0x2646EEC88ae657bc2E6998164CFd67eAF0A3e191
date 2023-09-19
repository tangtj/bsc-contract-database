// SPDX-License-Identifier: MIT
// File: Context.sol


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
// File: Ownable.sol


// OpenZeppelin Contracts v4.4.1 (access/Ownable.sol)

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
// File: IERC20.sol


// OpenZeppelin Contracts (last updated v4.6.0) (token/ERC20/IERC20.sol)

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
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}
// File: IERC721Receiver.sol



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
     * The selector can be obtained in Solidity with `IERC721.onERC721Received.selector`.
     */
    function onERC721Received(
        address operator,
        address from,
        uint256 tokenId,
        bytes calldata data
    ) external returns (bytes4);
}

// File: IERC165.sol


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
// File: IERC721.sol


// OpenZeppelin Contracts v4.4.1 (token/ERC721/IERC721.sol)

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
     * @dev Safely transfers `tokenId` token from `from` to `to`, checking first that contract recipients
     * are aware of the ERC721 protocol to prevent tokens from being forever locked.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `tokenId` token must exist and be owned by `from`.
     * - If the caller is not `from`, it must be have been allowed to move this token by either {approve} or {setApprovalForAll}.
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon a safe transfer.
     *
     * Emits a {Transfer} event.
     */
    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId
    ) external;

    /**
     * @dev Transfers `tokenId` token from `from` to `to`.
     *
     * WARNING: Usage of this method is discouraged, use {safeTransferFrom} whenever possible.
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
    function transferFrom(
        address from,
        address to,
        uint256 tokenId
    ) external;

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
     * @dev Returns the account approved for `tokenId` token.
     *
     * Requirements:
     *
     * - `tokenId` must exist.
     */
    function getApproved(uint256 tokenId) external view returns (address operator);

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
    function setApprovalForAll(address operator, bool _approved) external;

    /**
     * @dev Returns if the `operator` is allowed to manage all of the assets of `owner`.
     *
     * See {setApprovalForAll}
     */
    function isApprovedForAll(address owner, address operator) external view returns (bool);

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
    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId,
        bytes calldata data
    ) external;
}

    pragma solidity ^0.8.4;

    
    contract FrogChainGame is Ownable, IERC721Receiver{

    IERC20 public token;
    IERC721 public nft;

    uint256 public decimalNumber = 9;
    uint256 public countOfOverallStakers;

    uint256 public gameStartTime;
    uint256 public gameDuration = 604800;
    uint256 public stakingEligibilityTime = 259200;
    uint256 public additionalTime = 0;
    uint256 public gameIndex = 0;
    uint256 public originalGameIndex = 0;
    uint256 public winnerPrizeRate = 90;
 
    // Contract Addresses
    address _nft_Contract = 0x65DCB33FaaDC14151b0B7FBffD2863C5229Cc29E;
    address _token_Contract = 0xDCD103Bc6D14829C39Afc9c10c9c373CE385D2C5;

    // Mapping 
    mapping(address => mapping(uint256 => uint256)) public tokenStakedTime;
    mapping(address => mapping(uint256 => uint256)) public tokenStakedDuration;
    mapping(uint256 => address) public stakedTokenOwner;
    mapping(address => uint256[]) public stakedTokens;
    mapping(address => uint256) public countofMyStakedTokens;
    mapping(address => uint256) public totalRewardReleased;
    mapping(uint256 => address) public stakers;  
    mapping(uint256 => address) public nftOwner;
    mapping(uint256 => uint256) public winner;  

    mapping(uint256 => uint256) public AquaFrogsCount;
    mapping(uint256 => uint256) public FireFrogsCount;

    mapping(address => uint256) public individualStakingStartTime; 
    mapping(address => mapping(uint256 => bool)) public isAquaFrog;  
    mapping(address => mapping(uint256 => bool)) public isFireFrog;
    mapping(address => mapping(uint256 => uint256)) public individualStakingCount; 
    mapping(address => uint256) public myGameIndex; 
    mapping(address => mapping(uint256 => uint256)) public individualStakingIndex;
    mapping(address => bool) public alreadyStakedNFT; 
    mapping(uint256 => bool) public gameEnded; 

    mapping(address => mapping(uint256 => uint256)) public rewardsClaimable1_map; 
    mapping(address => mapping(uint256 => uint256)) public rewardsClaimable2_map; 
    mapping(address => mapping(uint256 => uint256)) public rewardsClaimable_map; 
    mapping(address => mapping(uint256 => bool)) public rewardsTaken; 


    constructor(){
    nft = IERC721(_nft_Contract);
    token = IERC20(_token_Contract);
    }

    function stakeNFT(uint256 _tokenID) public timeConditions {

        require(nft.ownerOf(_tokenID) == msg.sender, "Not the owner");
        require(!alreadyStakedNFT[msg.sender], "1 NFT is already staked");
        alreadyStakedNFT[msg.sender] = true;        
        nftOwner[_tokenID] = msg.sender;
        stakedTokens[msg.sender].push(_tokenID);
        countofMyStakedTokens[msg.sender]++;

        uint256 length = stakedTokens[msg.sender].length;

        if(stakedTokens[msg.sender].length != countofMyStakedTokens[msg.sender]){
            stakedTokens[msg.sender][countofMyStakedTokens[msg.sender]-1] = stakedTokens[msg.sender][length-1];
            delete stakedTokens[msg.sender][length-1];
        }
    
        stakedTokenOwner[_tokenID] = msg.sender;
        tokenStakedTime[msg.sender][_tokenID] = block.timestamp;
        nft.safeTransferFrom(msg.sender,address(this),_tokenID,"0x00");

        stakers[countOfOverallStakers] = msg.sender;    
        countOfOverallStakers++;
    }

    function stakeFrog_AquaFrogs(uint256 amount) public timeConditions {
        require(alreadyStakedNFT[msg.sender], "No NFT staked");

        if(individualStakingIndex[msg.sender][gameIndex] > 0){
        require(block.timestamp < individualStakingStartTime[msg.sender] + stakingEligibilityTime, "Staking eligibility is over");

        }else {
            individualStakingStartTime[msg.sender] = block.timestamp;
        }

        require(!isFireFrog[msg.sender][gameIndex], "You are already a member in Fire Frogs");
        myGameIndex[msg.sender] = gameIndex;
        isAquaFrog[msg.sender][gameIndex] = true;
        token.transferFrom(msg.sender, address(this), amount * 10 ** decimalNumber);    
        AquaFrogsCount[gameIndex] = AquaFrogsCount[gameIndex] + amount;
        individualStakingCount[msg.sender][gameIndex] = individualStakingCount[msg.sender][gameIndex] + amount;
        individualStakingIndex[msg.sender][gameIndex]++;
    }

    function stakeFrog_FireFrogs(uint256 amount) public timeConditions {
        require(alreadyStakedNFT[msg.sender], "No NFT staked");

        if(individualStakingIndex[msg.sender][gameIndex] > 0){
        require(block.timestamp < individualStakingStartTime[msg.sender] + stakingEligibilityTime, "Staking eligibility is over");

        }else {
            individualStakingStartTime[msg.sender] = block.timestamp;
        }

        require(block.timestamp < individualStakingStartTime[msg.sender] + stakingEligibilityTime, "Staking eligibility is over");
        require(!isAquaFrog[msg.sender][gameIndex], "You are already a member in Aqua Frogs");
        myGameIndex[msg.sender] = gameIndex;
        isFireFrog[msg.sender][gameIndex] = true;
        token.transferFrom(msg.sender, address(this), amount * 10 ** decimalNumber);    
        FireFrogsCount[gameIndex] = FireFrogsCount[gameIndex] + amount;
        individualStakingCount[msg.sender][gameIndex] = individualStakingCount[msg.sender][gameIndex] + amount;
        individualStakingIndex[msg.sender][gameIndex]++;
    }

    function setGameStartTime(uint256 _gameStartTime) public onlyOwner{
        
        gameStartTime = _gameStartTime;

        if(gameIndex > 0){
        originalGameIndex++;
        }


    }

    function setAdditionalTime(uint256 _additionalTime) public onlyOwner{
        
        additionalTime = _additionalTime;

    }

    function setGameStart() public onlyOwner{

        setGameStartTime(block.timestamp); 

    }

    function setGameEnd(uint256 _additionalTime) public onlyOwner{  

        uint256 gameEndingTime = gameStartTime + gameDuration + additionalTime;
        require(block.timestamp > gameEndingTime, "Game still running");  

        if(AquaFrogsCount[gameIndex] > FireFrogsCount[gameIndex]){
            winner[gameIndex] = 1;       
            gameEnded[gameIndex] = true;   
            gameIndex++;
        }

        if(FireFrogsCount[gameIndex] > AquaFrogsCount[gameIndex]){
            winner[gameIndex] = 2;   
            gameEnded[gameIndex] = true; 
            gameIndex++;
        }

        if(FireFrogsCount[gameIndex] == AquaFrogsCount[gameIndex]){
            winner[gameIndex] = 3;
            setAdditionalTime(_additionalTime);
        }
                            
    }

    function claimWinningRewards() public {
        
        uint256 gameEndingTime = gameStartTime + gameDuration + additionalTime;

        require(block.timestamp > gameEndingTime, "Game still running");
        require(myGameIndex[msg.sender] == gameIndex - 1, "The current game is not the game you are reffering to");
        require(!rewardsTaken[msg.sender][gameIndex - 1], "Already claimed for this round");

            if(isAquaFrog[msg.sender][gameIndex - 1]){
                require(winner[gameIndex - 1] == 1);

                uint256 rewardsClaimable1 = individualStakingCount[msg.sender][gameIndex - 1] * 10 ** decimalNumber;
                rewardsClaimable1_map[msg.sender][gameIndex - 1] = rewardsClaimable1;

                uint256 rewardsClaimable2 = ((individualStakingCount[msg.sender][gameIndex - 1] * 10 ** decimalNumber / AquaFrogsCount[gameIndex - 1]) * (FireFrogsCount[gameIndex - 1] * winnerPrizeRate))/100 ;
                rewardsClaimable2_map[msg.sender][gameIndex - 1] = rewardsClaimable2;

                uint256 rewardsClaimable = rewardsClaimable1 + rewardsClaimable2;    
                rewardsClaimable_map[msg.sender][gameIndex - 1] = rewardsClaimable;

                token.transfer(msg.sender, rewardsClaimable);

            } else if(isFireFrog[msg.sender][gameIndex - 1]){
                require(winner[gameIndex - 1] == 2);

                uint256 rewardsClaimable1 = individualStakingCount[msg.sender][gameIndex - 1] * 10 ** decimalNumber;
                rewardsClaimable1_map[msg.sender][gameIndex - 1] = rewardsClaimable1;

                uint256 rewardsClaimable2 = ((individualStakingCount[msg.sender][gameIndex - 1] * 10 ** decimalNumber / FireFrogsCount[gameIndex - 1]) * (AquaFrogsCount[gameIndex - 1] * winnerPrizeRate))/100;
                rewardsClaimable2_map[msg.sender][gameIndex - 1] = rewardsClaimable2;

                uint256 rewardsClaimable = rewardsClaimable1 + rewardsClaimable2;    
                rewardsClaimable_map[msg.sender][gameIndex - 1] = rewardsClaimable;

                token.transfer(msg.sender, rewardsClaimable); 
            }

            rewardsTaken[msg.sender][gameIndex - 1] = true; 

        
    }

    modifier timeConditions() {

        uint256 gameEndingTime = gameStartTime + gameDuration + additionalTime;
        require(block.timestamp > gameStartTime, "Game not yet started");
        require(block.timestamp < gameEndingTime, "Game already finished");
        _;

    }


    function batchStakeNFT(uint256[] memory _tokenIDs) public {
        
        for(uint256 x = 0; x <  _tokenIDs.length ; x++){
            stakeNFT(_tokenIDs[x]);

        }

    }
        
    function unstakeNFT(uint256 _tokenID) public {

        uint256 gameEndingTime = gameStartTime + gameDuration + additionalTime;

        require(gameEndingTime < block.timestamp, "Game is still running. Unstake after game ends");
        alreadyStakedNFT[msg.sender] = false;   
        isAquaFrog[msg.sender][gameIndex] = false;
        isFireFrog[msg.sender][gameIndex] = false;     

        require(nftOwner[_tokenID] == msg.sender, "You are not the staker");
        nft.safeTransferFrom(address(this), msg.sender, _tokenID,"0x00");

       // delete tokenStakedTime[msg.sender][_tokenID];
        delete stakedTokenOwner[_tokenID]; 
        delete nftOwner[_tokenID];

        for(uint256 i = 0; i < countofMyStakedTokens[msg.sender]; i++){
            if(stakedTokens[msg.sender][i] == _tokenID){    
            countofMyStakedTokens[msg.sender] = countofMyStakedTokens[msg.sender] - 1;


                for(uint256 x = i; x < countofMyStakedTokens[msg.sender]; x++){                   
                stakedTokens[msg.sender][x] = stakedTokens[msg.sender][x+1];
                }

                delete stakedTokens[msg.sender][countofMyStakedTokens[msg.sender]];

                           
            }
        }
    } 

    function batchUnstakeNFT(uint256[] memory _tokenIDs) public{

        for(uint256 x = 0; x <  _tokenIDs.length ; x++){
            unstakeNFT(_tokenIDs[x]);

        }
    }

    function onERC721Received(address, address, uint256, bytes memory) public virtual override returns (bytes4){
    return this.onERC721Received.selector;
    }

    function setNFTContract(address _nftContract) public onlyOwner{
    nft = IERC721(_nftContract);

    }
  
    function setTokenContract(address _tokenContract) public onlyOwner{
    token = IERC20(_tokenContract);

    }
    
    function setDecimalNumber(uint256 _decimalNumber) public onlyOwner{
    decimalNumber = _decimalNumber;

    }

    function setGameDuration(uint256 _gameDuration) public onlyOwner{
    gameDuration = _gameDuration;

    }

    function setGameIndex(uint256 _gameIndex) public onlyOwner{
    gameIndex = _gameIndex;

    }
        
    function setWinnerPrizeRate(uint256 _winnerPrizeRate) public onlyOwner{
    winnerPrizeRate = _winnerPrizeRate;

    }

    function setStakingEligibilityTime(uint256 _stakingEligibilityTime) public onlyOwner{
    stakingEligibilityTime = _stakingEligibilityTime;

    }
    
    function tokenWithdrawal() public onlyOwner{
    token.transfer(msg.sender,token.balanceOf(address(this)));

    }    
  
    function withdrawal() public onlyOwner {

    (bool main, ) = payable(owner()).call{value: address(this).balance}("");
    require(main);

    }
}