// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;


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
        // On the first call to nonReentrant, _notEntered will be true
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }
}
interface IARC721A {
    /**
     * The caller must own the token or be an approved operator.
     */
    error ApprovalCallerNotOwnerNorApproved();

    /**
     * The token does not exist.
     */
    error ApprovalQueryForNonexistentToken();

    /**
     * The caller cannot approve to their own address.
     */
    error ApproveToCaller();

    /**
     * Cannot query the balance for the zero address.
     */
    error BalanceQueryForZeroAddress();

    /**
     * Cannot mint to the zero address.
     */
    error MintToZeroAddress();

    /**
     * The quantity of tokens minted must be more than zero.
     */
    error MintZeroQuantity();

    /**
     * The token does not exist.
     */
    error OwnerQueryForNonexistentToken();

    /**
     * The caller must own the token or be an approved operator.
     */
    error TransferCallerNotOwnerNorApproved();

    /**
     * The token must be owned by `from`.
     */
    error TransferFromIncorrectOwner();

    /**
     * Cannot safely transfer to a contract that does not implement the ERC721Receiver interface.
     */
    error TransferToNonERC721ReceiverImplementer();

    /**
     * Cannot transfer to the zero address.
     */
    error TransferToZeroAddress();

    /**
     * The token does not exist.
     */
    error URIQueryForNonexistentToken();

    /**
     * The `quantity` minted with ERC2309 exceeds the safety limit.
     */
    error MintERC2309QuantityExceedsLimit();

    /**
     * The `extraData` cannot be set on an unintialized ownership slot.
     */
    error OwnershipNotInitializedForExtraData();

    struct TokenOwnership {
        // The address of the owner.
        address addr;
        // Keeps track of the start time of ownership with minimal overhead for tokenomics.
        uint64 startTimestamp;
        // Whether the token has been burned.
        bool burned;
        // Arbitrary data similar to `startTimestamp` that can be set through `_extraData`.
        uint24 extraData;
    }

    /**
     * @dev Returns the total amount of tokens stored by the contract.
     *
     * Burned tokens are calculated here, use `_totalMinted()` if you want to count just minted tokens.
     */
    function totalSupply() external view returns (uint256);

    // ==============================
    //            IERC165
    // ==============================

    /**
     * @dev Returns true if this contract implements the interface defined by
     * `interfaceId`. See the corresponding
     * https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified[EIP section]
     * to learn more about how these ids are created.
     *
     * This function call must use less than 30 000 gas.
     */
    function supportsInterface(bytes4 interfaceId) external view returns (bool);

    // ==============================
    //            IERC721
    // ==============================

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
    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId,
        bytes calldata data
    ) external;

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

    // ==============================
    //        IERC721Metadata
    // ==============================

    /**
     * @dev Returns the token collection name.
     */
    function name() external view returns (string memory);

    /**
     * @dev Returns the token collection symbol.
     */
    function symbol() external view returns (string memory);

    /**
     * @dev Returns the Uniform Resource Identifier (URI) for `tokenId` token.
     */
    function tokenURI(uint256 tokenId) external view returns (string memory);

    // ==============================
    //            IERC2309
    // ==============================

    /**
     * @dev Emitted when tokens in `fromTokenId` to `toTokenId` (inclusive) is transferred from `from` to `to`,
     * as defined in the ERC2309 standard. See `_mintERC2309` for more details.
     */
    event ConsecutiveTransfer(uint256 indexed fromTokenId, uint256 toTokenId, address indexed from, address indexed to);
}

interface IARC20 {
    function transfer(address to, uint256 value) external returns (bool);

    function approve(address spender, uint256 value) external returns (bool);

    function transferFrom(address from, address to, uint256 value) external returns (bool);

    function totalSupply() external view returns (uint256);

    function balanceOf(address who) external view returns (uint256);

    function allowance(address owner, address spender) external view returns (uint256);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

library SafeMath {
    /**

    * @dev Multiplies two unsigned integers, reverts on overflow.

    */

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the

        // benefit is lost if 'b' is also tested.

        // See: https://github.com/OpenZeppelin/openzeppelin-solidity/pull/522

        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;

        require(c / a == b);

        return c;
    }

    /**

    * @dev Integer division of two unsigned integers truncating the quotient, reverts on division by zero.

    */

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        // Solidity only automatically asserts when dividing by 0

        require(b > 0);

        uint256 c = a / b;

        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    /**

    * @dev Subtracts two unsigned integers, reverts on overflow (i.e. if subtrahend is greater than minuend).

    */

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a);

        uint256 c = a - b;

        return c;
    }

    /**

    * @dev Adds two unsigned integers, reverts on overflow.

    */

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;

        require(c >= a);

        return c;
    }

    /**

    * @dev Divides two unsigned integers and returns the remainder (unsigned integer modulo),

    * reverts when dividing by zero.

    */

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b != 0);

        return a % b;
    }
}

contract Ownable   {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    /**

     * @dev Initializes the contract setting the deployer as the initial owner.

     */

    constructor()  {
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
        require(_owner == msg.sender, "Ownable: caller is not the owner");

        _;
    }

    /**

     * @dev Transfers ownership of the contract to a new account (`newOwner`).

     * Can only be called by the current owner.

     */

    function transferOwnership(address newOwner) public onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );

        emit OwnershipTransferred(_owner, newOwner);

        _owner = newOwner;
    }
}


contract NFT_Staking is Ownable,ReentrancyGuard{
    using SafeMath for uint256;
    IARC20 public Token;
    // IARC721A public NFT;

    struct Stake{
        uint _tokens;
        uint[] _NFTs;
        uint _days;
        uint _stakeTime;
    }
    
    uint256 public minimumERC20Deposit = 100000e9;
    uint256 public time = 1 days;
    uint256 public minimumNFT = 25;
    uint256 public maximumNFT = 1000;
    uint256 public deductionPercentage=10000000000;  //10%

    //clubs information ////////////////////


    //elite club
    uint256 public ELITE_CLUB_NFTs_count=25;
    uint256 public elite30reward=1250000000;  //1.25 %
    uint256 public elite90reward=5250000000;   //5.25 %
    uint256 public elite180reward=13500000000;   //13.5%
    uint256 public elite360reward=33000000000;   //33 %

    function setelite_Counts_reward(uint256 _elitecount,uint256 _elite30reward,uint256 _elite90reward,uint256 _elite180reward,uint256 _elite360reward) external onlyOwner{
        ELITE_CLUB_NFTs_count=_elitecount;
        elite30reward=_elite30reward;
        elite90reward=_elite90reward;
        elite180reward=_elite180reward;
        elite360reward=_elite360reward;
    }


//top gun club
    uint256 public TOP_GUN_CLUB_NFTs_count=50;
    uint256 public topgun30reward=1500000000;    //1.5 %
    uint256 public topgun90reward=6000000000;     // 6 %
    uint256 public topgun180reward=15000000000;     //15 %
    uint256 public topgun360reward=36000000000;    //36 %

     function settopgun_Counts_reward(uint256 _topguncount,uint256 _topgun30reward,uint256 _topgun90reward,uint256 _topgun180reward,uint256 _topgun360reward) external onlyOwner{
        TOP_GUN_CLUB_NFTs_count=_topguncount;
        topgun30reward=_topgun30reward;
        topgun90reward=_topgun90reward;
        topgun180reward=_topgun180reward;
        topgun360reward=_topgun360reward;
    }


    //ADMIRALS CLUB

     uint256 public ADMIRALS_CLUB_NFTs_count=75;
    uint256 public ADMIRALS30reward=1750000000;  //1.75 %
    uint256 public ADMIRALS90reward=6750000000;   //6.75 %
    uint256 public ADMIRALS180reward=16500000000;  //16.5 %
    uint256 public ADMIRALS360reward=39000000000;   // 39 %

     function setADMIRALS_Counts_reward(uint256 _admiralscount,uint256 _ADMIRALS30reward,uint256 _ADMIRALS90reward,uint256 _ADMIRALS180reward,uint256 _ADMIRALS360reward) external onlyOwner{
        ADMIRALS_CLUB_NFTs_count=_admiralscount;
        ADMIRALS30reward=_ADMIRALS30reward;
        ADMIRALS90reward=_ADMIRALS90reward;
        ADMIRALS180reward=_ADMIRALS180reward;
        ADMIRALS360reward=_ADMIRALS360reward;
    }


   // CLUB 777

    uint256 public CLUB777_NFTs_count=101;
    uint256 public CLUB777_30reward=2000000000;  //2 %
    uint256 public CLUB777_90reward=7500000000; //7.5 %
    uint256 public CLUB777_180reward=18000000000; //18 %
    uint256 public CLUB777_360reward=42000000000; //42 %

     function setCLUB777_Counts_reward(uint256 _CLUB777_NFTs_count,uint256 _CLUB777_30reward,uint256 _CLUB777_90reward,uint256 _CLUB777_180reward,uint256 _CLUB777_360reward) external onlyOwner{
        CLUB777_NFTs_count=_CLUB777_NFTs_count;
        CLUB777_30reward=_CLUB777_30reward;
        CLUB777_90reward=_CLUB777_90reward;
        CLUB777_180reward=_CLUB777_180reward;
        CLUB777_360reward=_CLUB777_360reward;
    }

      // HIGH ROLLERS Club

    uint256 public HIGH_ROLLERS_NFTs_count=150;
    uint256 public HIGH_ROLLERS_30reward=2250000000;  //2.25 %
    uint256 public HIGH_ROLLERS_90reward=8250000000; //8.25 %
    uint256 public HIGH_ROLLERS_180reward=19500000000; //19.5 %
    uint256 public HIGH_ROLLERS_360reward=45000000000; //45 %

     function setHIGH_ROLLERS_Counts_reward(uint256 _HIGH_ROLLERS_NFTs_count,uint256 _HIGH_ROLLERS_30reward,uint256 _HIGH_ROLLERS_90reward,uint256 _HIGH_ROLLERS_180reward,uint256 _HIGH_ROLLERS_360reward) external onlyOwner{
        HIGH_ROLLERS_NFTs_count=_HIGH_ROLLERS_NFTs_count;
        HIGH_ROLLERS_30reward=_HIGH_ROLLERS_30reward;
        HIGH_ROLLERS_90reward=_HIGH_ROLLERS_90reward;
        HIGH_ROLLERS_180reward=_HIGH_ROLLERS_180reward;
        HIGH_ROLLERS_360reward=_HIGH_ROLLERS_360reward;
    }

    //META MASTER Club

     uint256 public META_MASTER_NFTs_count=200;
    uint256 public META_MASTER_30reward=2750000000; //2.75 %
    uint256 public META_MASTER_90reward=9750000000; // 9.75 %
    uint256 public META_MASTER_180reward=22500000000; //22.5 %
    uint256 public META_MASTER_360reward=51000000000; //51 %

     function setMETA_MASTER_Counts_reward(uint256 _META_MASTER_NFTs_count,uint256 _META_MASTER_30reward,uint256 _META_MASTER_90reward,uint256 _META_MASTER_180reward,uint256 _META_MASTER_360reward) external onlyOwner{
        META_MASTER_NFTs_count=_META_MASTER_NFTs_count;
        META_MASTER_30reward=_META_MASTER_30reward;
        META_MASTER_90reward=_META_MASTER_90reward;
        META_MASTER_180reward=_META_MASTER_180reward;
        META_MASTER_360reward=_META_MASTER_360reward;
    }


  //ARCHIE EXECUTIVE club

   uint256 public ARCHIE_EXECUTIVE_NFTs_count=500;
    uint256 public ARCHIE_EXECUTIVE_30reward=3000000000; //3 %
    uint256 public ARCHIE_EXECUTIVE_90reward=10500000000; // 10.5 %
    uint256 public ARCHIE_EXECUTIVE_180reward=24000000000; // 24 %
    uint256 public ARCHIE_EXECUTIVE_360reward=54000000000; //54 %

     function setARCHIE_EXECUTIVE_Counts_reward(uint256 _ARCHIE_EXECUTIVE_NFTs_count,uint256 _ARCHIE_EXECUTIVE_30reward,uint256 _ARCHIE_EXECUTIVE_90reward,uint256 _ARCHIE_EXECUTIVE_180reward,uint256 _ARCHIE_EXECUTIVE_360reward) external onlyOwner{
        ARCHIE_EXECUTIVE_NFTs_count=_ARCHIE_EXECUTIVE_NFTs_count;
        ARCHIE_EXECUTIVE_30reward=_ARCHIE_EXECUTIVE_30reward;
        ARCHIE_EXECUTIVE_90reward=_ARCHIE_EXECUTIVE_90reward;
        ARCHIE_EXECUTIVE_180reward=_ARCHIE_EXECUTIVE_180reward;
        ARCHIE_EXECUTIVE_360reward=_ARCHIE_EXECUTIVE_360reward;
    }




    mapping (address => Stake[]) public stakesOf;
    mapping(uint256 => uint256) public allocation;
    mapping(address => uint256) public commulativeDepositTokensOf;
    mapping(address => uint256) public commulativeWithdrawTokensOf;
    mapping(uint256 => bool) public isNFTStaked;
    mapping(address => bool) public isSpam;


    event Deposite(address indexed to,address indexed From, uint256 amount, uint256 day,uint256 time);

    constructor(IARC20 _tokenaddr)  {
        Token = _tokenaddr;

        _paused = false;
    }

   

    

    function farm(uint256 _amount, uint256 _lockableDays, uint256[] memory _tokenIDs) public whenNotPaused nonReentrant{    
        address caller = msg.sender; // to save gas fee
        require(isSpam[msg.sender]==false,"Account is spam!");
        require(_tokenIDs.length >= minimumNFT && _tokenIDs.length <= maximumNFT, "length is not valid");
        require(_amount >= minimumERC20Deposit, "Invalid amount");
        for (uint256 i; i < _tokenIDs.length; i++){
            require(isNFTStaked[_tokenIDs[i]] == false,"You already staked these nft ids!");
        }

        // take tokens from caller
        Token.transferFrom(caller, address(this), _amount);

        // update state data.
        stakesOf[caller].push(Stake({
            _tokens: _amount, 
            _NFTs: _tokenIDs, 
            _days: _lockableDays,
            _stakeTime: block.timestamp
        }));
        commulativeDepositTokensOf[caller] += _amount;

        // mark staked true for given NFT IDs
        for(uint256 i;i<_tokenIDs.length;i++){
            isNFTStaked[_tokenIDs[i]] = true;
        }

        emit Deposite(caller,address(this),_amount,_lockableDays,block.timestamp);
    }
    
    
    function harvest(uint256 _index) public whenNotPaused nonReentrant{
        uint currentTime = block.timestamp; // to save gas fee.
        address caller = msg.sender; // to save gas fee.

        Stake[] memory _userAllStakes = stakesOf[caller];
        Stake storage _userData = stakesOf[caller][_index];
        uint _tokensCount = _userData._tokens;
        
        require(isSpam[msg.sender]==false,"Account is spam!");
        require(_userData._tokens != 0, "Already Unstaked");
        require(_index <= _userAllStakes.length, "Invalid index number");
        uint256 reward;
        uint256 deductionamount;
        uint256 totalWithdraw;
        if(currentTime >= (_userData._days * time + _userData._stakeTime))
        {
            reward = calculateReward(caller, _index);
            totalWithdraw = _userData._tokens + reward; 
        }

        else{
        if(deductionPercentage>0)	
        {	
            deductionamount = (_tokensCount.mul(deductionPercentage).div(100)).div(1e9);
            totalWithdraw = _userData._tokens - deductionamount; 
         }
         else
         {
            reward = calculateReward(caller, _index);
            totalWithdraw = _userData._tokens + reward; 

         }
        }
        
        // mark staking false for give NFT IDs.
        unstakeData(_userData._NFTs);

        // send staked ERC20 tokens + reward tokens back to the staker.
         
        Token.transfer(caller, totalWithdraw);
        commulativeWithdrawTokensOf[caller] += totalWithdraw;

        // reset all the state data for given index.
        delete _userData._tokens;
        delete _userData._NFTs;
        delete _userData._days;
        delete _userData._stakeTime;
    }


    function calculateReward(address _owner, uint _index) public view returns (uint reward){
        Stake memory _userData = stakesOf[_owner][_index];

        uint _NFTsCount = _userData._NFTs.length;
        uint _daysCount = _userData._days;
        uint _tokensCount = _userData._tokens;

        // calculate reward based on the given conditions.
       
        if(_NFTsCount >= ARCHIE_EXECUTIVE_NFTs_count){
            if(_daysCount == 30){
              reward = (_tokensCount.mul(ARCHIE_EXECUTIVE_30reward).div(100)).div(1e9);
            }
            else if (_daysCount == 90){
               reward = (_tokensCount.mul(ARCHIE_EXECUTIVE_90reward).div(100)).div(1e9);
            }
            else if (_daysCount == 180){
               reward = (_tokensCount.mul(ARCHIE_EXECUTIVE_180reward).div(100)).div(1e9);
            }
            else if (_daysCount == 360){
                reward = (_tokensCount.mul(ARCHIE_EXECUTIVE_360reward).div(100)).div(1e9);
            }
        }
      
        else if(_NFTsCount >= META_MASTER_NFTs_count){
            if(_daysCount == 30){
                
                reward = (_tokensCount.mul(META_MASTER_30reward).div(100)).div(1e9);
            }
            else if (_daysCount == 90){
             
                reward = (_tokensCount.mul(META_MASTER_90reward).div(100)).div(1e9);
            }
            else if (_daysCount == 180){
               
                reward = (_tokensCount.mul(META_MASTER_180reward).div(100)).div(1e9);
            }
            else if (_daysCount == 360){
                reward = (_tokensCount.mul(META_MASTER_360reward).div(100)).div(1e9);
            }
        }
     
        else if(_NFTsCount >= HIGH_ROLLERS_NFTs_count){
            if(_daysCount == 30){
                reward = (_tokensCount.mul(HIGH_ROLLERS_30reward).div(100)).div(1e9);
            }
            else if (_daysCount == 90){
               
                reward = (_tokensCount.mul(HIGH_ROLLERS_90reward).div(100)).div(1e9);
            }
            else if (_daysCount == 180){
                reward = (_tokensCount.mul(HIGH_ROLLERS_180reward).div(100)).div(1e9);
            }
            else if (_daysCount == 360){
                
                reward = (_tokensCount.mul(HIGH_ROLLERS_360reward).div(100)).div(1e9);
            }
        }
        
        else if(_NFTsCount >= CLUB777_NFTs_count){
            if(_daysCount == 30){
               
                reward = (_tokensCount.mul(CLUB777_30reward).div(100)).div(1e9);
            }
            else if (_daysCount == 90){
                reward = (_tokensCount.mul(CLUB777_90reward).div(100)).div(1e9);
            }
            else if (_daysCount == 180){
             
                reward = (_tokensCount.mul(CLUB777_180reward).div(100)).div(1e9);
            }
            else if (_daysCount == 360){
              
                reward = (_tokensCount.mul(CLUB777_360reward).div(100)).div(1e9);
            }
        }
       
        else if(_NFTsCount >= ADMIRALS_CLUB_NFTs_count){
            if(_daysCount == 30){
              
                reward = (_tokensCount.mul(ADMIRALS30reward).div(100)).div(1e9);
            }
            else if (_daysCount == 90){
               
                reward = (_tokensCount.mul(ADMIRALS90reward).div(100)).div(1e9);
            }
            else if (_daysCount == 180){
               
                reward = (_tokensCount.mul(ADMIRALS180reward).div(100)).div(1e9);
            }
            else if (_daysCount == 360){
               
                reward = (_tokensCount.mul(ADMIRALS360reward).div(100)).div(1e9);
            }
        }
     
        else if(_NFTsCount >= TOP_GUN_CLUB_NFTs_count){
            if(_daysCount == 30){
           
                reward = (_tokensCount.mul(topgun30reward).div(100)).div(1e9);
            }
            else if (_daysCount == 90){
                
                reward = (_tokensCount.mul(topgun90reward).div(100)).div(1e9);
            }
            else if (_daysCount == 180){
            
                reward = (_tokensCount.mul(topgun180reward).div(100)).div(1e9);
            }
            else if (_daysCount == 360){
                reward = (_tokensCount.mul(topgun360reward).div(100)).div(1e9);
            }
        }
      
        else if(_NFTsCount >= ELITE_CLUB_NFTs_count){

            if(_daysCount == 30){
               
                reward = (_tokensCount.mul(elite30reward).div(100)).div(1e9);
            }
            else if (_daysCount == 90){
               
                reward = (_tokensCount.mul(elite90reward).div(100)).div(1e9);
            }
            else if (_daysCount == 180){
               
                reward = (_tokensCount.mul(elite180reward).div(100)).div(1e9);
            }
            else if (_daysCount == 360){
              
                reward = (_tokensCount.mul(elite360reward).div(100)).div(1e9);
            }
        }
      
        else {
             if(_daysCount == 30){
               
                reward = (_tokensCount.mul(elite30reward).div(100)).div(1e9);
            }
            else if (_daysCount == 90){
               
                reward = (_tokensCount.mul(elite90reward).div(100)).div(1e9);
            }
            else if (_daysCount == 180){
               
                reward = (_tokensCount.mul(elite180reward).div(100)).div(1e9);
            }
            else if (_daysCount == 360){
              
                reward = (_tokensCount.mul(elite360reward).div(100)).div(1e9);
            }
        }
    }

    // function to mark stake false for the given NFT IDs.
    // Note: This function can only be called within harvest function.
    function unstakeData(uint[] memory _tokenIDs) internal {
        for (uint i; i < _tokenIDs.length; i++) {
            isNFTStaked[_tokenIDs[i]] = false;
        }
    }

    // return all the staked information of given user in the form of array.
    function UserInformation(address _addr) public view returns(Stake[] memory _userData){
        return stakesOf[_addr];
    }

    // return all the desposited and withDrawn ERC20 tokens count for a specific user.
    function UserERC20Information(address _addr) public view returns(uint256, uint256){
        return (commulativeDepositTokensOf[_addr], commulativeWithdrawTokensOf[_addr]);
    }

    function emergencyWithdraw(IARC20 _token,uint256 _amount) external onlyOwner {
         _token.transfer(msg.sender, _amount);
    }
    function emergencyWithdrawARC(uint256 Amount) external onlyOwner {
        payable(msg.sender).transfer(Amount);
    }

    // function to change the time according to the seconds of one day.
    function changetime(uint256 _time) external onlyOwner{
        time = _time;
    }

    function changeMinimmumAmount(uint256 amount) external onlyOwner{
        minimumERC20Deposit = amount;
    }

    function setMinMaxNFT(uint256 _min,uint256 _max) external onlyOwner{
        maximumNFT = _max;
        minimumNFT = _min;
    }

    function addorRemoveSpam(address _Addr,bool _state) external onlyOwner{
        isSpam[_Addr]=_state;
    }
    

    event Paused(address account);

    /**
     * @dev Emitted when the pause is lifted by `account`.
     */
    event Unpaused(address account);

    bool private _paused;

    /**
     * @dev Initializes the contract in unpaused state.
     */
   

    /**
     * @dev Returns true if the contract is paused, and false otherwise.
     */
    function paused() public view virtual returns (bool) {
        return _paused;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is not paused.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    modifier whenNotPaused() {
        require(!paused(), "Pausable: paused");
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
        require(paused(), "Pausable: not paused");
        _;
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
        emit Paused(msg.sender);
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
        emit Unpaused(msg.sender);
    }

    function Pause() external onlyOwner{
        _paused=true;
    }
     function UnPause() external onlyOwner{
        _paused=false;
    }
    function changeToken(IARC20 addr) public onlyOwner{
        Token=addr;
        
    }
}