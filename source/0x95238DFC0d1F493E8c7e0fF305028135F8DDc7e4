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

// File: @openzeppelin/contracts/security/Pausable.sol


// OpenZeppelin Contracts v4.4.1 (security/Pausable.sol)

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

// File: contracts/MiningV6/MiningReconcile.sol


//0x83F88dbC1e1CAbE185Eb12cF98400F47a83764C9


pragma solidity ^0.8.4;





contract MiningReconcile is Pausable, Ownable {

    mapping ( address => bool ) public allowedContracts;

    mapping(uint => address) public mining_contracts;
    mapping(uint => bool) public version_allowed;

    address public airdropCon;
    address public stakeCon;
    uint public miningFees;
    uint public maxCliamsBatch;

    mapping(uint=> mapping(uint => bool)) public minedByNft;
    mapping(address=> mapping(uint => bool)) public minedByWallet;
    mapping(uint=> uint) public minedByNftNbr;
    mapping(address=> uint) public minedByWalletNbr;
    
    constructor() Ownable()  {
        mining_contracts[1] = 0x7D5Dacc91772078EeC7d48cAF21d51D221fC5a28;
        mining_contracts[2] = 0x4Dd499133887AD0e901918e310B09c778aEC9b3d;
        mining_contracts[3] = 0x8dF8bC74251a2b345ec4756BF87a2Aa726927f78;
        mining_contracts[4] = 0x0e3d770F97Ee2dbEE301bfb68CF3A833696cb434;
        airdropCon = 0x0463AE91155a6Dc9140d3A63D9D18a998542d0fa;
        stakeCon = 0x922c94B201B22058f11041551C7A7B623DD94e8e;
        version_allowed[2] = true;
        version_allowed[3] = true;
        version_allowed[4] = true;
        miningFees = 530000000000000;
        maxCliamsBatch = 10;
    }

    struct shareData{
        address _owner;
        uint shares;
        uint32 day;
        uint32 month;
        uint32 year;
        uint _days;
    }

    struct submittedData{
        uint shares;
        uint _days;
        uint32 day;
        uint32 month;
        uint32 year;
    }
    event claimEvent(uint _version, address _owner, uint _tkn,uint _totalShares,uint _totalDays, uint _totalRatio);



    function getPendingClaimsNbr(address _user, uint _tkn, uint _version) public view returns (uint){
        if(_version >=4) return IMining(mining_contracts[_version]).getPendingClaimsNbr(_user);        
        return IMining(mining_contracts[_version]).getPendingClaimsNbr(_tkn);
        
    }
    function getPendingClaims(address _user, uint _tkn, uint _version) public view returns (IMining.submittedDataMining[] memory){
        if(_version >=4) return IMining(mining_contracts[_version]).getPendingClaims(_user);        
        return IMining(mining_contracts[_version]).getPendingClaims(_tkn);
    }
    function getDayShares (uint _day, uint _version) public view returns (uint){
        return IMining(mining_contracts[_version]).getDayShares(_day);
    }
    function claim(address _user_, uint _tkn_, uint _version_) public payable whenNotPaused returns (uint _totalShares,uint _totalDays, uint _totalRatio){
        require(msg.value >= miningFees, "Not enough funds; check fees!");
        require(version_allowed[_version_] == true, "Version not allowed");
        uint _version = _version_;
        address _user = _user_;
        uint _tkn = _tkn_;
        IMining.submittedDataMining[] memory array = getPendingClaims(_user, _tkn, _version);
        uint32 ratio_multiplier = 1000000;
        uint mined;
        uint claimsToProcess;
        if(_version >=4){
            mined = minedByWalletNbr[_user];
        }else{
            mined = minedByNftNbr[_tkn];
            _user = ownerOfToken(_tkn);
        } 
        require(array.length > 0, "Nothing to claim");
        require(array.length > mined, "Already claimed");
        claimsToProcess = mined + maxCliamsBatch;
        uint end = claimsToProcess > array.length ? array.length : claimsToProcess;
        uint totalRatio = 0;
        uint totalShares = 0;
        uint totalDays = 0;
        for (uint i = mined; i < end; i++) {
            totalRatio += (array[i].shares*ratio_multiplier)/getDayShares(array[i]._days, _version);
            totalShares += array[i].shares;
            totalDays ++;
            if(_version >=4){
                minedByWalletNbr[_user] ++;
            }else{
                minedByNftNbr[_tkn] ++;
            } 
        }
        IAirdrop(airdropCon).claimTokens(_user, uint32(totalRatio));
        emit claimEvent(_version, _user, _tkn, totalShares, totalDays, totalRatio);
        _totalShares = totalShares;
        _totalDays = totalDays;
        _totalRatio = totalRatio;
    }
    
    //ADMIN
    function getMinedByNft(uint _tkn, uint _day) public view returns (bool){return minedByNft[_tkn][_day];}
    function getMinedByWallet(address _user, uint _day) public view returns (bool){return minedByWallet[_user][_day];}
    function getMinedByNftNbr(uint _tkn) public view returns (uint){return minedByNftNbr[_tkn];}
    function getMinedByWalletNbr(address _user) public view returns (uint){return minedByWalletNbr[_user];}
    function ownerOfToken(uint _tkn) public view returns(address){
        (address _owner,,,,,) = IStake(stakeCon).ownerOf(_tkn);
        return _owner;
    }
    //ADMIN
    function setMaxBatch(uint _v) public onlyOwner{maxCliamsBatch = _v;}
    function setMiningFees(uint _v) public onlyOwner{miningFees = _v;}
    function setMiningContract(uint _version, address _addr) public onlyOwner{mining_contracts[_version] = _addr;}
    function setAirdropContract(address _addr) public onlyOwner{airdropCon = _addr;}
    function setStakeContract(address _addr) public onlyOwner{stakeCon = _addr;}
    //
    
    //TRANSFER
    // ALLOW
    function pause() public onlyOwner {_pause();}
    function unpause() public onlyOwner {_unpause();}
    modifier onlyAllowedContract (){
        require(allowedContracts[msg.sender] == true);
        _;
    }
    function allowContract ( address _addr ) onlyOwner public {allowedContracts[_addr ] = true;}    
    function disallowContract ( address _addr ) onlyOwner public {allowedContracts[_addr ] = false;}
    // ALLOW
}


interface IMining {
    struct shareDataMining {address _owner;uint shares;uint32 day;uint32 month;uint32 year;uint _days;}
    struct submittedDataMining {uint shares;uint _days;uint32 day;uint32 month;uint32 year;}
    //V4
    function getPendingClaimsNbr (address _owner) external view returns (uint);
    function getPendingClaims (address _owner) external view returns (submittedDataMining[] memory _array);
    function claim (string memory _tkn, address _owner) payable external returns ( bool );
    //
    function getPendingClaimsNbr (uint tokenId) external view returns (uint);
    function getPendingClaims (uint tokenId) external view returns (submittedDataMining[] memory _array);
    function claim (string memory _tkn, uint256 tokenId) payable external returns ( bool );
    //
    function getDayShares (uint _day) external view returns (uint);
}
interface IAirdrop {
    function claimTokens(address addr, uint32 ratio) external returns (bool);
}
interface IStake {
    function ownerOf (uint256 tokenId) external view returns (address _owner, address _int_wallet ,uint32 _day, uint32 _month, uint32 _year, uint32 _days);
}