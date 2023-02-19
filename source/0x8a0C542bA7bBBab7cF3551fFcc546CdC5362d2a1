// File: @openzeppelin/contracts/GSN/Context.sol

pragma solidity ^0.5.0;

/*
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with GSN meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
contract Context {
    // Empty internal constructor, to prevent people from mistakenly deploying
    // an instance of this contract, which should be used via inheritance.
    constructor () internal { }
    // solhint-disable-previous-line no-empty-blocks

    function _msgSender() internal view returns (address payable) {
        return msg.sender;
    }

    function _msgData() internal view returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
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
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor () internal {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
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
        return _msgSender() == _owner;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
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

// File: @openzeppelin/contracts/token/ERC20/IERC20.sol

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

// File: @openzeppelin/contracts/token/ERC20/ERC20.sol

pragma solidity ^0.5.0;




/**
 * @dev Implementation of the {IERC20} interface.
 *
 * This implementation is agnostic to the way tokens are created. This means
 * that a supply mechanism has to be added in a derived contract using {_mint}.
 * For a generic mechanism see {ERC20Mintable}.
 *
 * TIP: For a detailed writeup see our guide
 * https://forum.zeppelin.solutions/t/how-to-implement-erc20-supply-mechanisms/226[How
 * to implement supply mechanisms].
 *
 * We have followed general OpenZeppelin guidelines: functions revert instead
 * of returning `false` on failure. This behavior is nonetheless conventional
 * and does not conflict with the expectations of ERC20 applications.
 *
 * Additionally, an {Approval} event is emitted on calls to {transferFrom}.
 * This allows applications to reconstruct the allowance for all accounts just
 * by listening to said events. Other implementations of the EIP may not emit
 * these events, as it isn't required by the specification.
 *
 * Finally, the non-standard {decreaseAllowance} and {increaseAllowance}
 * functions have been added to mitigate the well-known issues around setting
 * allowances. See {IERC20-approve}.
 */
contract ERC20 is Context, IERC20 {
    using SafeMath for uint256;

    mapping (address => uint256) private _balances;

    mapping (address => mapping (address => uint256)) private _allowances;

    uint256 private _totalSupply;

    /**
     * @dev See {IERC20-totalSupply}.
     */
    function totalSupply() public view returns (uint256) {
        return _totalSupply;
    }

    /**
     * @dev See {IERC20-balanceOf}.
     */
    function balanceOf(address account) public view returns (uint256) {
        return _balances[account];
    }

    /**
     * @dev See {IERC20-transfer}.
     *
     * Requirements:
     *
     * - `recipient` cannot be the zero address.
     * - the caller must have a balance of at least `amount`.
     */
    function transfer(address recipient, uint256 amount) public returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    /**
     * @dev See {IERC20-allowance}.
     */
    function allowance(address owner, address spender) public view returns (uint256) {
        return _allowances[owner][spender];
    }

    /**
     * @dev See {IERC20-approve}.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function approve(address spender, uint256 amount) public returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    /**
     * @dev See {IERC20-transferFrom}.
     *
     * Emits an {Approval} event indicating the updated allowance. This is not
     * required by the EIP. See the note at the beginning of {ERC20};
     *
     * Requirements:
     * - `sender` and `recipient` cannot be the zero address.
     * - `sender` must have a balance of at least `amount`.
     * - the caller must have allowance for `sender`'s tokens of at least
     * `amount`.
     */
    function transferFrom(address sender, address recipient, uint256 amount) public returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    /**
     * @dev Atomically increases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IERC20-approve}.
     *
     * Emits an {Approval} event indicating the updated allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    /**
     * @dev Atomically decreases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IERC20-approve}.
     *
     * Emits an {Approval} event indicating the updated allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     * - `spender` must have allowance for the caller of at least
     * `subtractedValue`.
     */
    function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }

    /**
     * @dev Moves tokens `amount` from `sender` to `recipient`.
     *
     * This is internal function is equivalent to {transfer}, and can be used to
     * e.g. implement automatic token fees, slashing mechanisms, etc.
     *
     * Emits a {Transfer} event.
     *
     * Requirements:
     *
     * - `sender` cannot be the zero address.
     * - `recipient` cannot be the zero address.
     * - `sender` must have a balance of at least `amount`.
     */
    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        _balances[sender] = _balances[sender].sub(amount, "ERC20: transfer amount exceeds balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }

    /** @dev Creates `amount` tokens and assigns them to `account`, increasing
     * the total supply.
     *
     * Emits a {Transfer} event with `from` set to the zero address.
     *
     * Requirements
     *
     * - `to` cannot be the zero address.
     */
    function _mint(address account, uint256 amount) internal {
        require(account != address(0), "ERC20: mint to the zero address");

        _totalSupply = _totalSupply.add(amount);
        _balances[account] = _balances[account].add(amount);
        emit Transfer(address(0), account, amount);
    }

    /**
     * @dev Destroys `amount` tokens from `account`, reducing the
     * total supply.
     *
     * Emits a {Transfer} event with `to` set to the zero address.
     *
     * Requirements
     *
     * - `account` cannot be the zero address.
     * - `account` must have at least `amount` tokens.
     */
    function _burn(address account, uint256 amount) internal {
        require(account != address(0), "ERC20: burn from the zero address");

        _balances[account] = _balances[account].sub(amount, "ERC20: burn amount exceeds balance");
        _totalSupply = _totalSupply.sub(amount);
        emit Transfer(account, address(0), amount);
    }

    /**
     * @dev Sets `amount` as the allowance of `spender` over the `owner`s tokens.
     *
     * This is internal function is equivalent to `approve`, and can be used to
     * e.g. set automatic allowances for certain subsystems, etc.
     *
     * Emits an {Approval} event.
     *
     * Requirements:
     *
     * - `owner` cannot be the zero address.
     * - `spender` cannot be the zero address.
     */
    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    /**
     * @dev Destroys `amount` tokens from `account`.`amount` is then deducted
     * from the caller's allowance.
     *
     * See {_burn} and {_approve}.
     */
    function _burnFrom(address account, uint256 amount) internal {
        _burn(account, amount);
        _approve(account, _msgSender(), _allowances[account][_msgSender()].sub(amount, "ERC20: burn amount exceeds allowance"));
    }
}

// File: contracts/CZXPToken.sol

pragma solidity ^0.5.0;

/******************************************

Our Matra our Motto for coding The Cryptoz Universe.

      Fail EARLY and Fail LOUD !!!!!!!

1.    First - Check all pre-conditions
2.    Then  - make changes to contract state
3.    Final - interact with other contracts

Pull funds over pushing them
Platform limits like max of 1023 loop interations.

all Events start with Log

******************************************/



contract CzxpToken is Ownable,ERC20 {

//1. Condition
//2. Effects
//3.Interaction
    
    /*
    * We accept payment for the token generation event only
    * Rules:
    *   Founders-pre:        6000000
    *   TGE - max   :     1000000000
    *   start is    :    Mar  9,2021
    *   end is      :        No date
    *   min         :  0.0000001 BNB
    *   max         :          1 BNB
    *   max wallets :            100
    */



// Advisor+team wallets
    address[] advisors = [
         0x60D7a79367B35573B27E4F020794AEF299E9fd49, //owner
         0x591D4Fe5f1e147eac7b144C43e4f2A0d83a4DF84, //M
         0x4b01629a1bAf82D8aB62726776b1332539E51cEc  //R
    ];

// Public variables of the token
    string public name          = "Cryptoz eXPerience";
    string public symbol        = "CZXP";
    uint8  public decimals      = 0;
    uint8  public walletCounter = 0;
    
//Storage
    //supporters
    mapping (address => uint256) public contributions;    // BNB contributed per address

//TGE restrictions
    //Token Generation event
    bool public isTGE = false;
    bool public TGEcomplete = false;
    uint256 maxCap = 1000000000000000000; //in wei
    
//Our external cryptozContract
    address private cryptozContract;
    
    //MODIFIERS
    modifier checkTGE(){
        require(isTGE);
        _;
    }
    
    modifier onlyAuthorizedContract(){
        require(msg.sender == cryptozContract,"Caller not authorized for this action");
        _;
    }
    
    // ALL FUNCTIONS
    function() external {
        revert();
    }

    
    function setCryptozContract(address _cryptozContract) public onlyOwner{
        require(_cryptozContract != address(0x0));
        cryptozContract = _cryptozContract;
    }
    
    function getCrypozContractAddress() public view returns(address){
        return cryptozContract;
    }

    /*
    *   Make sure we accept payment only during TGE
    */
    function buy() payable checkTGE external {
        require(walletCounter < 101, "A maximum of 100 unique wallets has been reached");
        require(msg.sender != address(0));
        
        //We dont offer less than 1 czxp
        require(msg.value >= 0.0000001 ether,"Contribution must be greater than 0.0000001");
        
        //Check contributor is under the cap
        require(contributions[msg.sender] <= maxCap,"Contribution is over the maximum of 1 BNB");
        
        //if contribution+balance is >= cap
        uint allowedContribution = 0;
        if((msg.value+contributions[msg.sender]) >= maxCap){
            allowedContribution = maxCap - contributions[msg.sender];
        }else{
            allowedContribution = msg.value;
        }
        
        //sanity ??
        require(allowedContribution > 0);
        
        //track total wallets
        if(contributions[msg.sender] == 0) {
            walletCounter++;
        }
        
        //Finally increment the contributor
        contributions[msg.sender] = contributions[msg.sender].add(allowedContribution);
        
        //all good ?  give tokens
        _mint(msg.sender, convertWeiToCZXP(allowedContribution));
    }
    
    /**
     *  One time event to give token allocations to our advisors ONCE when TGE is activated
     */
     
    function mintAdvisorTokens() internal checkTGE {
        
        
        //100K to each advisor, update to maxed out
        for(uint8 i=0; i < advisors.length; i++){
            contributions[advisors[i]] = maxCap;
            _mint(advisors[i], 20000000);
        }
    }
    
    /**
     *  Turn on TGE and then lock it forever when turned off
     */
     
    function flipTGE() public onlyOwner returns(bool) {
        require(!TGEcomplete);
        
        if(!isTGE){
            isTGE = true;
            mintAdvisorTokens();
        }else{
            isTGE = false;
            TGEcomplete = true;
        }
        return true;
    }
    
   
   /**
    * Our base conversion rate for the TGE
    */
    function convertWeiToCZXP(uint weiToConvert) pure internal returns (uint) {
        // 1 czxp = 100000000000 wei;
        return weiToConvert/100000000000;
    }
    
    /**
     *  Called in from our ERC721 contract actions
     *
     */
     function awardCzxp(address _to, uint256 _amount) public onlyAuthorizedContract {
         require(_to != address(0));
         require(_amount > 0);
         
         _mint(_to, _amount);
     }
     
    /**
     *  Called in from our ERC721 contract actions
     *
     */
     function burnCzxp(address _wallet, uint256 _amount) public onlyAuthorizedContract {
         require(_wallet != address(0));
         require(_amount > 0);
         
         _burn(_wallet, _amount);
     }
    
    /**
    * @dev Returns the amount contributed so far by a sepecific user.
    * @param _beneficiary Address of contributor
    * @return User contribution so far
    */
    function getUserContribution(address _beneficiary) public view returns (uint256) {
        return contributions[_beneficiary];
    }
    
       
   /**
    * Withdraw balance to wallet
    */
   function withdraw() public onlyOwner returns(bool) {
      msg.sender.transfer(address(this).balance);
      return true;
   }

}

// File: contracts/CryptozUniverse.sol

pragma solidity ^0.5.0;

/******************************************

Our Matra our Motto for coding The Cryptoz Universe.

      Fail EARLY and Fail LOUD !!!!!!!

1.    First - Check all pre-conditions
2.    Then  - make changes to contract state
3.    Final - interact with other contracts

Pull funds over pushing them
Platform limits like max of 1023 loop interations.

all Events start with Log

******************************************/




contract CryptozUniverse is Ownable {
    using SafeMath for uint256;

//Definitons
    struct cardType{
        uint32 cardTypeId;
        string name;
        string set;
        //uint8 assetType; //1=human,2=animal,3=plant, 4=thing
        //uint8 notStoreOrBonus; //0 =inStore, 1 = pack, 2= bonus
        //uint8 rarity; //1 = diamond,6 = common
        //uint16 totalAvailable;
        uint256 weiCost;
        uint256 buyCzxp;
        uint256 transferCzxp;
        uint256 sacrificeCzxp;
        uint256 unlockCzxp;
        uint8 cardLevel;
    }

    struct Card{
        uint32 cardTypeId;
        uint256 editionNumber;
        uint256 transferCount;
    }

    string baseTokenURI = 'https://cryptoz.cards/data/'; //append ID on
//storage

    //Containers for the card types
    mapping(uint256 => uint8) cardAssetType;
    mapping(uint256 => uint8) storeBoosterBonus;
    mapping(uint256 => uint8) public rarity; // lookup cardTypeId return rarity
    mapping(uint256 => uint16) totalAvailable;

    //All the cards in the Cryptoz Universe
    Card[] internal Cards;

    mapping(uint32 => cardType) public allCardTypes;        // All card type definitions
    uint256[] public cardTypesIds; //Just an array of all the id

    //track editionNumberTotal, always increments, starts at 1
    mapping(uint256 => uint256) public cardTypeToEdition;

// Event Logs
    event CZXPGained(address indexed beneficiary, uint256 czxpAmount);

//Other variables we need
    address payable internal CzxpContractAddress_;


    /**
     * Not much going on here so exit.
     */
    function() external{
        revert();
    }

    /**
     *
     */
     function getTotalTypes() public view returns(uint){
         return cardTypesIds.length;
     }

    /**
     *  Internal function called from Cryptoz once all the checks are done
     */
    function awardCzxp(address _beneficiary, uint czxpReward) internal {
        //Give the czxp tokens
        CzxpToken(CzxpContractAddress_).awardCzxp(_beneficiary, czxpReward);
    }

    /**
     * Random as it gets for now. Return a val between 1-10000
     */
    uint nonce = 0;
    function selectRandom(uint256 range) internal returns (uint) {
        uint randomnumber = uint(keccak256(abi.encodePacked(now, msg.sender, nonce))) % range;
        nonce++;
        return randomnumber;
    }
    
    function getProbs(uint wager) internal returns (uint16,uint16,uint16,uint16)  {
        
        uint8[4] memory group = [1,2,3,4];
        uint8[4] memory Fac = [1,2,6,24];
        uint[4] memory DistGroup;
        uint16[4] memory FinalProbs;

        for(uint i=0; i<4; i++){
            DistGroup[i] = (((getLambda(wager)**group[i]) / getLambda(wager)) / Fac[i]) * 100;
        }
        
        uint DistTotal = DistGroup[0] + DistGroup[1] + DistGroup[2] + DistGroup[3];
        
        for(uint j=0; j<4; j++){
            FinalProbs[j] = uint16(DistGroup[j] * 10000 / DistTotal);
        }
        return(FinalProbs[0],FinalProbs[1],FinalProbs[2],FinalProbs[3]);
    }
    
    function getLambda(uint wager) internal returns (uint) {
        uint Dist   = log2(wager / 280000);
        return Dist / 4;
    }
    
    function log2(uint x) internal returns (uint y){
       assembly {
            let arg := x
            x := sub(x,1)
            x := or(x, div(x, 0x02))
            x := or(x, div(x, 0x04))
            x := or(x, div(x, 0x10))
            x := or(x, div(x, 0x100))
            x := or(x, div(x, 0x10000))
            x := or(x, div(x, 0x100000000))
            x := or(x, div(x, 0x10000000000000000))
            x := or(x, div(x, 0x100000000000000000000000000000000))
            x := add(x, 1)
            let m := mload(0x40)
            mstore(m,           0xf8f9cbfae6cc78fbefe7cdc3a1793dfcf4f0e8bbd8cec470b6a28a7a5a3e1efd)
            mstore(add(m,0x20), 0xf5ecf1b3e9debc68e1d9cfabc5997135bfb7a7a3938b7b606b5b4b3f2f1f0ffe)
            mstore(add(m,0x40), 0xf6e4ed9ff2d6b458eadcdf97bd91692de2d4da8fd2d0ac50c6ae9a8272523616)
            mstore(add(m,0x60), 0xc8c0b887b0a8a4489c948c7f847c6125746c645c544c444038302820181008ff)
            mstore(add(m,0x80), 0xf7cae577eec2a03cf3bad76fb589591debb2dd67e0aa9834bea6925f6a4a2e0e)
            mstore(add(m,0xa0), 0xe39ed557db96902cd38ed14fad815115c786af479b7e83247363534337271707)
            mstore(add(m,0xc0), 0xc976c13bb96e881cb166a933a55e490d9d56952b8d4e801485467d2362422606)
            mstore(add(m,0xe0), 0x753a6d1b65325d0c552a4d1345224105391a310b29122104190a110309020100)
            mstore(0x40, add(m, 0x100))
            let magic := 0x818283848586878898a8b8c8d8e8f929395969799a9b9d9e9faaeb6bedeeff
            let shift := 0x100000000000000000000000000000000000000000000000000000000000000
            let a := div(mul(x, magic), shift)
            y := div(mload(add(m,sub(255,a))), shift)
            y := add(y, mul(256, gt(arg, 0x8000000000000000000000000000000000000000000000000000000000000000)))
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

// File: @openzeppelin/contracts/token/ERC721/IERC721Enumerable.sol

pragma solidity ^0.5.0;


/**
 * @title ERC-721 Non-Fungible Token Standard, optional enumeration extension
 * @dev See https://eips.ethereum.org/EIPS/eip-721
 */
contract IERC721Enumerable is IERC721 {
    function totalSupply() public view returns (uint256);
    function tokenOfOwnerByIndex(address owner, uint256 index) public view returns (uint256 tokenId);

    function tokenByIndex(uint256 index) public view returns (uint256);
}

// File: @openzeppelin/contracts/token/ERC721/IERC721Receiver.sol

pragma solidity ^0.5.0;

/**
 * @title ERC721 token receiver interface
 * @dev Interface for any contract that wants to support safeTransfers
 * from ERC721 asset contracts.
 */
contract IERC721Receiver {
    /**
     * @notice Handle the receipt of an NFT
     * @dev The ERC721 smart contract calls this function on the recipient
     * after a {IERC721-safeTransferFrom}. This function MUST return the function selector,
     * otherwise the caller will revert the transaction. The selector to be
     * returned can be obtained as `this.onERC721Received.selector`. This
     * function MAY throw to revert and reject the transfer.
     * Note: the ERC721 contract address is always the message sender.
     * @param operator The address which called `safeTransferFrom` function
     * @param from The address which previously owned the token
     * @param tokenId The NFT identifier which is being transferred
     * @param data Additional data with no specified format
     * @return bytes4 `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`
     */
    function onERC721Received(address operator, address from, uint256 tokenId, bytes memory data)
    public returns (bytes4);
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

// File: @openzeppelin/contracts/drafts/Counters.sol

pragma solidity ^0.5.0;


/**
 * @title Counters
 * @author Matt Condon (@shrugs)
 * @dev Provides counters that can only be incremented or decremented by one. This can be used e.g. to track the number
 * of elements in a mapping, issuing ERC721 ids, or counting request ids.
 *
 * Include with `using Counters for Counters.Counter;`
 * Since it is not possible to overflow a 256 bit integer with increments of one, `increment` can skip the {SafeMath}
 * overflow check, thereby saving gas. This does assume however correct usage, in that the underlying `_value` is never
 * directly accessed.
 */
library Counters {
    using SafeMath for uint256;

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
        // The {SafeMath} overflow check can be skipped here, see the comment at the top
        counter._value += 1;
    }

    function decrement(Counter storage counter) internal {
        counter._value = counter._value.sub(1);
    }
}

// File: @openzeppelin/contracts/introspection/ERC165.sol

pragma solidity ^0.5.0;


/**
 * @dev Implementation of the {IERC165} interface.
 *
 * Contracts may inherit from this and call {_registerInterface} to declare
 * their support of an interface.
 */
contract ERC165 is IERC165 {
    /*
     * bytes4(keccak256('supportsInterface(bytes4)')) == 0x01ffc9a7
     */
    bytes4 private constant _INTERFACE_ID_ERC165 = 0x01ffc9a7;

    /**
     * @dev Mapping of interface ids to whether or not it's supported.
     */
    mapping(bytes4 => bool) private _supportedInterfaces;

    constructor () internal {
        // Derived contracts need only register support for their own interfaces,
        // we register support for ERC165 itself here
        _registerInterface(_INTERFACE_ID_ERC165);
    }

    /**
     * @dev See {IERC165-supportsInterface}.
     *
     * Time complexity O(1), guaranteed to always use less than 30 000 gas.
     */
    function supportsInterface(bytes4 interfaceId) external view returns (bool) {
        return _supportedInterfaces[interfaceId];
    }

    /**
     * @dev Registers the contract as an implementer of the interface defined by
     * `interfaceId`. Support of the actual ERC165 interface is automatic and
     * registering its interface id is not required.
     *
     * See {IERC165-supportsInterface}.
     *
     * Requirements:
     *
     * - `interfaceId` cannot be the ERC165 invalid interface (`0xffffffff`).
     */
    function _registerInterface(bytes4 interfaceId) internal {
        require(interfaceId != 0xffffffff, "ERC165: invalid interface id");
        _supportedInterfaces[interfaceId] = true;
    }
}

// File: @openzeppelin/contracts/token/ERC721/ERC721.sol

pragma solidity ^0.5.0;








/**
 * @title ERC721 Non-Fungible Token Standard basic implementation
 * @dev see https://eips.ethereum.org/EIPS/eip-721
 */
contract ERC721 is Context, ERC165, IERC721 {
    using SafeMath for uint256;
    using Address for address;
    using Counters for Counters.Counter;

    // Equals to `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`
    // which can be also obtained as `IERC721Receiver(0).onERC721Received.selector`
    bytes4 private constant _ERC721_RECEIVED = 0x150b7a02;

    // Mapping from token ID to owner
    mapping (uint256 => address) private _tokenOwner;

    // Mapping from token ID to approved address
    mapping (uint256 => address) private _tokenApprovals;

    // Mapping from owner to number of owned token
    mapping (address => Counters.Counter) private _ownedTokensCount;

    // Mapping from owner to operator approvals
    mapping (address => mapping (address => bool)) private _operatorApprovals;

    /*
     *     bytes4(keccak256('balanceOf(address)')) == 0x70a08231
     *     bytes4(keccak256('ownerOf(uint256)')) == 0x6352211e
     *     bytes4(keccak256('approve(address,uint256)')) == 0x095ea7b3
     *     bytes4(keccak256('getApproved(uint256)')) == 0x081812fc
     *     bytes4(keccak256('setApprovalForAll(address,bool)')) == 0xa22cb465
     *     bytes4(keccak256('isApprovedForAll(address,address)')) == 0xe985e9c5
     *     bytes4(keccak256('transferFrom(address,address,uint256)')) == 0x23b872dd
     *     bytes4(keccak256('safeTransferFrom(address,address,uint256)')) == 0x42842e0e
     *     bytes4(keccak256('safeTransferFrom(address,address,uint256,bytes)')) == 0xb88d4fde
     *
     *     => 0x70a08231 ^ 0x6352211e ^ 0x095ea7b3 ^ 0x081812fc ^
     *        0xa22cb465 ^ 0xe985e9c ^ 0x23b872dd ^ 0x42842e0e ^ 0xb88d4fde == 0x80ac58cd
     */
    bytes4 private constant _INTERFACE_ID_ERC721 = 0x80ac58cd;

    constructor () public {
        // register the supported interfaces to conform to ERC721 via ERC165
        _registerInterface(_INTERFACE_ID_ERC721);
    }

    /**
     * @dev Gets the balance of the specified address.
     * @param owner address to query the balance of
     * @return uint256 representing the amount owned by the passed address
     */
    function balanceOf(address owner) public view returns (uint256) {
        require(owner != address(0), "ERC721: balance query for the zero address");

        return _ownedTokensCount[owner].current();
    }

    /**
     * @dev Gets the owner of the specified token ID.
     * @param tokenId uint256 ID of the token to query the owner of
     * @return address currently marked as the owner of the given token ID
     */
    function ownerOf(uint256 tokenId) public view returns (address) {
        address owner = _tokenOwner[tokenId];
        require(owner != address(0), "ERC721: owner query for nonexistent token");

        return owner;
    }

    /**
     * @dev Approves another address to transfer the given token ID
     * The zero address indicates there is no approved address.
     * There can only be one approved address per token at a given time.
     * Can only be called by the token owner or an approved operator.
     * @param to address to be approved for the given token ID
     * @param tokenId uint256 ID of the token to be approved
     */
    function approve(address to, uint256 tokenId) public {
        address owner = ownerOf(tokenId);
        require(to != owner, "ERC721: approval to current owner");

        require(_msgSender() == owner || isApprovedForAll(owner, _msgSender()),
            "ERC721: approve caller is not owner nor approved for all"
        );

        _tokenApprovals[tokenId] = to;
        emit Approval(owner, to, tokenId);
    }

    /**
     * @dev Gets the approved address for a token ID, or zero if no address set
     * Reverts if the token ID does not exist.
     * @param tokenId uint256 ID of the token to query the approval of
     * @return address currently approved for the given token ID
     */
    function getApproved(uint256 tokenId) public view returns (address) {
        require(_exists(tokenId), "ERC721: approved query for nonexistent token");

        return _tokenApprovals[tokenId];
    }

    /**
     * @dev Sets or unsets the approval of a given operator
     * An operator is allowed to transfer all tokens of the sender on their behalf.
     * @param to operator address to set the approval
     * @param approved representing the status of the approval to be set
     */
    function setApprovalForAll(address to, bool approved) public {
        require(to != _msgSender(), "ERC721: approve to caller");

        _operatorApprovals[_msgSender()][to] = approved;
        emit ApprovalForAll(_msgSender(), to, approved);
    }

    /**
     * @dev Tells whether an operator is approved by a given owner.
     * @param owner owner address which you want to query the approval of
     * @param operator operator address which you want to query the approval of
     * @return bool whether the given operator is approved by the given owner
     */
    function isApprovedForAll(address owner, address operator) public view returns (bool) {
        return _operatorApprovals[owner][operator];
    }

    /**
     * @dev Transfers the ownership of a given token ID to another address.
     * Usage of this method is discouraged, use {safeTransferFrom} whenever possible.
     * Requires the msg.sender to be the owner, approved, or operator.
     * @param from current owner of the token
     * @param to address to receive the ownership of the given token ID
     * @param tokenId uint256 ID of the token to be transferred
     */
    function transferFrom(address from, address to, uint256 tokenId) public {
        //solhint-disable-next-line max-line-length
        require(_isApprovedOrOwner(_msgSender(), tokenId), "ERC721: transfer caller is not owner nor approved");

        _transferFrom(from, to, tokenId);
    }

    /**
     * @dev Safely transfers the ownership of a given token ID to another address
     * If the target address is a contract, it must implement {IERC721Receiver-onERC721Received},
     * which is called upon a safe transfer, and return the magic value
     * `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`; otherwise,
     * the transfer is reverted.
     * Requires the msg.sender to be the owner, approved, or operator
     * @param from current owner of the token
     * @param to address to receive the ownership of the given token ID
     * @param tokenId uint256 ID of the token to be transferred
     */
    function safeTransferFrom(address from, address to, uint256 tokenId) public {
        safeTransferFrom(from, to, tokenId, "");
    }

    /**
     * @dev Safely transfers the ownership of a given token ID to another address
     * If the target address is a contract, it must implement {IERC721Receiver-onERC721Received},
     * which is called upon a safe transfer, and return the magic value
     * `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`; otherwise,
     * the transfer is reverted.
     * Requires the _msgSender() to be the owner, approved, or operator
     * @param from current owner of the token
     * @param to address to receive the ownership of the given token ID
     * @param tokenId uint256 ID of the token to be transferred
     * @param _data bytes data to send along with a safe transfer check
     */
    function safeTransferFrom(address from, address to, uint256 tokenId, bytes memory _data) public {
        require(_isApprovedOrOwner(_msgSender(), tokenId), "ERC721: transfer caller is not owner nor approved");
        _safeTransferFrom(from, to, tokenId, _data);
    }

    /**
     * @dev Safely transfers the ownership of a given token ID to another address
     * If the target address is a contract, it must implement `onERC721Received`,
     * which is called upon a safe transfer, and return the magic value
     * `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`; otherwise,
     * the transfer is reverted.
     * Requires the msg.sender to be the owner, approved, or operator
     * @param from current owner of the token
     * @param to address to receive the ownership of the given token ID
     * @param tokenId uint256 ID of the token to be transferred
     * @param _data bytes data to send along with a safe transfer check
     */
    function _safeTransferFrom(address from, address to, uint256 tokenId, bytes memory _data) internal {
        _transferFrom(from, to, tokenId);
        require(_checkOnERC721Received(from, to, tokenId, _data), "ERC721: transfer to non ERC721Receiver implementer");
    }

    /**
     * @dev Returns whether the specified token exists.
     * @param tokenId uint256 ID of the token to query the existence of
     * @return bool whether the token exists
     */
    function _exists(uint256 tokenId) internal view returns (bool) {
        address owner = _tokenOwner[tokenId];
        return owner != address(0);
    }

    /**
     * @dev Returns whether the given spender can transfer a given token ID.
     * @param spender address of the spender to query
     * @param tokenId uint256 ID of the token to be transferred
     * @return bool whether the msg.sender is approved for the given token ID,
     * is an operator of the owner, or is the owner of the token
     */
    function _isApprovedOrOwner(address spender, uint256 tokenId) internal view returns (bool) {
        require(_exists(tokenId), "ERC721: operator query for nonexistent token");
        address owner = ownerOf(tokenId);
        return (spender == owner || getApproved(tokenId) == spender || isApprovedForAll(owner, spender));
    }

    /**
     * @dev Internal function to safely mint a new token.
     * Reverts if the given token ID already exists.
     * If the target address is a contract, it must implement `onERC721Received`,
     * which is called upon a safe transfer, and return the magic value
     * `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`; otherwise,
     * the transfer is reverted.
     * @param to The address that will own the minted token
     * @param tokenId uint256 ID of the token to be minted
     */
    function _safeMint(address to, uint256 tokenId) internal {
        _safeMint(to, tokenId, "");
    }

    /**
     * @dev Internal function to safely mint a new token.
     * Reverts if the given token ID already exists.
     * If the target address is a contract, it must implement `onERC721Received`,
     * which is called upon a safe transfer, and return the magic value
     * `bytes4(keccak256("onERC721Received(address,address,uint256,bytes)"))`; otherwise,
     * the transfer is reverted.
     * @param to The address that will own the minted token
     * @param tokenId uint256 ID of the token to be minted
     * @param _data bytes data to send along with a safe transfer check
     */
    function _safeMint(address to, uint256 tokenId, bytes memory _data) internal {
        _mint(to, tokenId);
        require(_checkOnERC721Received(address(0), to, tokenId, _data), "ERC721: transfer to non ERC721Receiver implementer");
    }

    /**
     * @dev Internal function to mint a new token.
     * Reverts if the given token ID already exists.
     * @param to The address that will own the minted token
     * @param tokenId uint256 ID of the token to be minted
     */
    function _mint(address to, uint256 tokenId) internal {
        require(to != address(0), "ERC721: mint to the zero address");
        require(!_exists(tokenId), "ERC721: token already minted");

        _tokenOwner[tokenId] = to;
        _ownedTokensCount[to].increment();

        emit Transfer(address(0), to, tokenId);
    }

    /**
     * @dev Internal function to burn a specific token.
     * Reverts if the token does not exist.
     * Deprecated, use {_burn} instead.
     * @param owner owner of the token to burn
     * @param tokenId uint256 ID of the token being burned
     */
    function _burn(address owner, uint256 tokenId) internal {
        require(ownerOf(tokenId) == owner, "ERC721: burn of token that is not own");

        _clearApproval(tokenId);

        _ownedTokensCount[owner].decrement();
        _tokenOwner[tokenId] = address(0);

        emit Transfer(owner, address(0), tokenId);
    }

    /**
     * @dev Internal function to burn a specific token.
     * Reverts if the token does not exist.
     * @param tokenId uint256 ID of the token being burned
     */
    function _burn(uint256 tokenId) internal {
        _burn(ownerOf(tokenId), tokenId);
    }

    /**
     * @dev Internal function to transfer ownership of a given token ID to another address.
     * As opposed to {transferFrom}, this imposes no restrictions on msg.sender.
     * @param from current owner of the token
     * @param to address to receive the ownership of the given token ID
     * @param tokenId uint256 ID of the token to be transferred
     */
    function _transferFrom(address from, address to, uint256 tokenId) internal {
        require(ownerOf(tokenId) == from, "ERC721: transfer of token that is not own");
        require(to != address(0), "ERC721: transfer to the zero address");

        _clearApproval(tokenId);

        _ownedTokensCount[from].decrement();
        _ownedTokensCount[to].increment();

        _tokenOwner[tokenId] = to;

        emit Transfer(from, to, tokenId);
    }

    /**
     * @dev Internal function to invoke {IERC721Receiver-onERC721Received} on a target address.
     * The call is not executed if the target address is not a contract.
     *
     * This is an internal detail of the `ERC721` contract and its use is deprecated.
     * @param from address representing the previous owner of the given token ID
     * @param to target address that will receive the tokens
     * @param tokenId uint256 ID of the token to be transferred
     * @param _data bytes optional data to send along with the call
     * @return bool whether the call correctly returned the expected magic value
     */
    function _checkOnERC721Received(address from, address to, uint256 tokenId, bytes memory _data)
        internal returns (bool)
    {
        if (!to.isContract()) {
            return true;
        }
        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = to.call(abi.encodeWithSelector(
            IERC721Receiver(to).onERC721Received.selector,
            _msgSender(),
            from,
            tokenId,
            _data
        ));
        if (!success) {
            if (returndata.length > 0) {
                // solhint-disable-next-line no-inline-assembly
                assembly {
                    let returndata_size := mload(returndata)
                    revert(add(32, returndata), returndata_size)
                }
            } else {
                revert("ERC721: transfer to non ERC721Receiver implementer");
            }
        } else {
            bytes4 retval = abi.decode(returndata, (bytes4));
            return (retval == _ERC721_RECEIVED);
        }
    }

    /**
     * @dev Private function to clear current approval of a given token ID.
     * @param tokenId uint256 ID of the token to be transferred
     */
    function _clearApproval(uint256 tokenId) private {
        if (_tokenApprovals[tokenId] != address(0)) {
            _tokenApprovals[tokenId] = address(0);
        }
    }
}

// File: @openzeppelin/contracts/token/ERC721/ERC721Enumerable.sol

pragma solidity ^0.5.0;





/**
 * @title ERC-721 Non-Fungible Token with optional enumeration extension logic
 * @dev See https://eips.ethereum.org/EIPS/eip-721
 */
contract ERC721Enumerable is Context, ERC165, ERC721, IERC721Enumerable {
    // Mapping from owner to list of owned token IDs
    mapping(address => uint256[]) private _ownedTokens;

    // Mapping from token ID to index of the owner tokens list
    mapping(uint256 => uint256) private _ownedTokensIndex;

    // Array with all token ids, used for enumeration
    uint256[] private _allTokens;

    // Mapping from token id to position in the allTokens array
    mapping(uint256 => uint256) private _allTokensIndex;

    /*
     *     bytes4(keccak256('totalSupply()')) == 0x18160ddd
     *     bytes4(keccak256('tokenOfOwnerByIndex(address,uint256)')) == 0x2f745c59
     *     bytes4(keccak256('tokenByIndex(uint256)')) == 0x4f6ccce7
     *
     *     => 0x18160ddd ^ 0x2f745c59 ^ 0x4f6ccce7 == 0x780e9d63
     */
    bytes4 private constant _INTERFACE_ID_ERC721_ENUMERABLE = 0x780e9d63;

    /**
     * @dev Constructor function.
     */
    constructor () public {
        // register the supported interface to conform to ERC721Enumerable via ERC165
        _registerInterface(_INTERFACE_ID_ERC721_ENUMERABLE);
    }

    /**
     * @dev Gets the token ID at a given index of the tokens list of the requested owner.
     * @param owner address owning the tokens list to be accessed
     * @param index uint256 representing the index to be accessed of the requested tokens list
     * @return uint256 token ID at the given index of the tokens list owned by the requested address
     */
    function tokenOfOwnerByIndex(address owner, uint256 index) public view returns (uint256) {
        require(index < balanceOf(owner), "ERC721Enumerable: owner index out of bounds");
        return _ownedTokens[owner][index];
    }

    /**
     * @dev Gets the total amount of tokens stored by the contract.
     * @return uint256 representing the total amount of tokens
     */
    function totalSupply() public view returns (uint256) {
        return _allTokens.length;
    }

    /**
     * @dev Gets the token ID at a given index of all the tokens in this contract
     * Reverts if the index is greater or equal to the total number of tokens.
     * @param index uint256 representing the index to be accessed of the tokens list
     * @return uint256 token ID at the given index of the tokens list
     */
    function tokenByIndex(uint256 index) public view returns (uint256) {
        require(index < totalSupply(), "ERC721Enumerable: global index out of bounds");
        return _allTokens[index];
    }

    /**
     * @dev Internal function to transfer ownership of a given token ID to another address.
     * As opposed to transferFrom, this imposes no restrictions on msg.sender.
     * @param from current owner of the token
     * @param to address to receive the ownership of the given token ID
     * @param tokenId uint256 ID of the token to be transferred
     */
    function _transferFrom(address from, address to, uint256 tokenId) internal {
        super._transferFrom(from, to, tokenId);

        _removeTokenFromOwnerEnumeration(from, tokenId);

        _addTokenToOwnerEnumeration(to, tokenId);
    }

    /**
     * @dev Internal function to mint a new token.
     * Reverts if the given token ID already exists.
     * @param to address the beneficiary that will own the minted token
     * @param tokenId uint256 ID of the token to be minted
     */
    function _mint(address to, uint256 tokenId) internal {
        super._mint(to, tokenId);

        _addTokenToOwnerEnumeration(to, tokenId);

        _addTokenToAllTokensEnumeration(tokenId);
    }

    /**
     * @dev Internal function to burn a specific token.
     * Reverts if the token does not exist.
     * Deprecated, use {ERC721-_burn} instead.
     * @param owner owner of the token to burn
     * @param tokenId uint256 ID of the token being burned
     */
    function _burn(address owner, uint256 tokenId) internal {
        super._burn(owner, tokenId);

        _removeTokenFromOwnerEnumeration(owner, tokenId);
        // Since tokenId will be deleted, we can clear its slot in _ownedTokensIndex to trigger a gas refund
        _ownedTokensIndex[tokenId] = 0;

        _removeTokenFromAllTokensEnumeration(tokenId);
    }

    /**
     * @dev Gets the list of token IDs of the requested owner.
     * @param owner address owning the tokens
     * @return uint256[] List of token IDs owned by the requested address
     */
    function _tokensOfOwner(address owner) internal view returns (uint256[] storage) {
        return _ownedTokens[owner];
    }

    /**
     * @dev Private function to add a token to this extension's ownership-tracking data structures.
     * @param to address representing the new owner of the given token ID
     * @param tokenId uint256 ID of the token to be added to the tokens list of the given address
     */
    function _addTokenToOwnerEnumeration(address to, uint256 tokenId) private {
        _ownedTokensIndex[tokenId] = _ownedTokens[to].length;
        _ownedTokens[to].push(tokenId);
    }

    /**
     * @dev Private function to add a token to this extension's token tracking data structures.
     * @param tokenId uint256 ID of the token to be added to the tokens list
     */
    function _addTokenToAllTokensEnumeration(uint256 tokenId) private {
        _allTokensIndex[tokenId] = _allTokens.length;
        _allTokens.push(tokenId);
    }

    /**
     * @dev Private function to remove a token from this extension's ownership-tracking data structures. Note that
     * while the token is not assigned a new owner, the `_ownedTokensIndex` mapping is _not_ updated: this allows for
     * gas optimizations e.g. when performing a transfer operation (avoiding double writes).
     * This has O(1) time complexity, but alters the order of the _ownedTokens array.
     * @param from address representing the previous owner of the given token ID
     * @param tokenId uint256 ID of the token to be removed from the tokens list of the given address
     */
    function _removeTokenFromOwnerEnumeration(address from, uint256 tokenId) private {
        // To prevent a gap in from's tokens array, we store the last token in the index of the token to delete, and
        // then delete the last slot (swap and pop).

        uint256 lastTokenIndex = _ownedTokens[from].length.sub(1);
        uint256 tokenIndex = _ownedTokensIndex[tokenId];

        // When the token to delete is the last token, the swap operation is unnecessary
        if (tokenIndex != lastTokenIndex) {
            uint256 lastTokenId = _ownedTokens[from][lastTokenIndex];

            _ownedTokens[from][tokenIndex] = lastTokenId; // Move the last token to the slot of the to-delete token
            _ownedTokensIndex[lastTokenId] = tokenIndex; // Update the moved token's index
        }

        // This also deletes the contents at the last position of the array
        _ownedTokens[from].length--;

        // Note that _ownedTokensIndex[tokenId] hasn't been cleared: it still points to the old slot (now occupied by
        // lastTokenId, or just over the end of the array if the token was the last one).
    }

    /**
     * @dev Private function to remove a token from this extension's token tracking data structures.
     * This has O(1) time complexity, but alters the order of the _allTokens array.
     * @param tokenId uint256 ID of the token to be removed from the tokens list
     */
    function _removeTokenFromAllTokensEnumeration(uint256 tokenId) private {
        // To prevent a gap in the tokens array, we store the last token in the index of the token to delete, and
        // then delete the last slot (swap and pop).

        uint256 lastTokenIndex = _allTokens.length.sub(1);
        uint256 tokenIndex = _allTokensIndex[tokenId];

        // When the token to delete is the last token, the swap operation is unnecessary. However, since this occurs so
        // rarely (when the last minted token is burnt) that we still do the swap here to avoid the gas cost of adding
        // an 'if' statement (like in _removeTokenFromOwnerEnumeration)
        uint256 lastTokenId = _allTokens[lastTokenIndex];

        _allTokens[tokenIndex] = lastTokenId; // Move the last token to the slot of the to-delete token
        _allTokensIndex[lastTokenId] = tokenIndex; // Update the moved token's index

        // This also deletes the contents at the last position of the array
        _allTokens.length--;
        _allTokensIndex[tokenId] = 0;
    }
}

// File: contracts/Strings.sol

pragma solidity ^0.5.0;

library Strings {
  // via https://github.com/oraclize/ethereum-api/blob/master/oraclizeAPI_0.5.sol
  function strConcat(string memory _a, string memory _b, string memory _c, string memory _d, string memory _e) internal pure returns (string memory) {
      bytes memory _ba = bytes(_a);
      bytes memory _bb = bytes(_b);
      bytes memory _bc = bytes(_c);
      bytes memory _bd = bytes(_d);
      bytes memory _be = bytes(_e);
      string memory abcde = new string(_ba.length + _bb.length + _bc.length + _bd.length + _be.length);
      bytes memory babcde = bytes(abcde);
      uint k = 0;
      for (uint i = 0; i < _ba.length; i++) babcde[k++] = _ba[i];
      for (uint i = 0; i < _bb.length; i++) babcde[k++] = _bb[i];
      for (uint i = 0; i < _bc.length; i++) babcde[k++] = _bc[i];
      for (uint i = 0; i < _bd.length; i++) babcde[k++] = _bd[i];
      for (uint i = 0; i < _be.length; i++) babcde[k++] = _be[i];
      return string(babcde);
    }

    function strConcat(string memory _a, string memory _b, string memory _c, string memory _d) internal pure returns (string memory) {
        return strConcat(_a, _b, _c, _d, "");
    }

    function strConcat(string memory _a, string memory _b, string memory _c) internal pure returns (string memory) {
        return strConcat(_a, _b, _c, "", "");
    }

    function strConcat(string memory _a, string memory _b) internal pure returns (string memory) {
        return strConcat(_a, _b, "", "", "");
    }

    function uint2str(uint _i) internal pure returns (string memory _uintAsString) {
        if (_i == 0) {
            return "0";
        }
        uint j = _i;
        uint len;
        while (j != 0) {
            len++;
            j /= 10;
        }
        bytes memory bstr = new bytes(len);
        uint k = len - 1;
        while (_i != 0) {
            bstr[k--] = byte(uint8(48 + _i % 10));
            _i /= 10;
        }
        return string(bstr);
    }
}

// File: contracts/CryptozCard.sol

pragma solidity ^0.5.0;




/*
interface ERC165 {
 /// @notice Query if a contract implements an interface
 /// @param interfaceID The interface identifier, as
 ///  specified in ERC-165
 /// @dev Interface identification is specified in
 ///  ERC-165. This function uses less than 30,000 gas.
 /// @return `true` if the contract implements `interfaceID`
 ///  and `interfaceID` is not 0xffffffff, `false` otherwise
 function supportsInterface(bytes4 interfaceID) external view returns (bool);
}
*/

/**
 * @title Full ERC721 Token
 * This implementation includes all the required and some optional functionality of the ERC721 standard
 * Moreover, it includes approve all functionality using operator terminology
 * @dev see https://github.com/ethereum/EIPs/blob/master/EIPS/eip-721.md
 */
contract CryptozCard is CryptozUniverse, ERC721Enumerable {
    
  // Token name
  string internal name_ = "Cryptoz Cards";

  // Token symbol
  string internal symbol_ = "Cryptoz";

  // Mapping from owner to list of owned token IDs
  mapping(address => uint256[]) internal ownedTokens;

  // Mapping from token ID to index of the owner tokens list
  mapping(uint256 => uint256) internal ownedTokensIndex;

  // Array with all token ids, used for enumeration
  uint256[] internal allTokens;

  // Mapping from token id to position in the allTokens array
  mapping(uint256 => uint256) internal allTokensIndex;

  // Optional mapping for token URIs
  mapping(uint256 => string) internal tokenURIs;
  
  //Track all tokens by rarity
  uint[7] public tokensByRarity = [0,0,0,0,0,0,0]; //[rarity] = total

  /*CRYPTOZ Overrides*/

  //Event Logs
  event LogCardCreated(address indexed owner, uint indexed cardTokenId, uint editionNumber, uint indexed cardTypeId, uint8 rarity, uint CzxpGained);
  event SacrificeCardEvent(address indexed owner, uint indexed cardTokenId, uint indexed cardTypeId, uint CzxpGained);

  // Track ownership of Card Type purchases from store to enforce 1 type per person once
  mapping (address => mapping(uint => bool)) public cardTypesOwned; //    [address][CardTypeId] = bool

  /**
   * @dev Gets the token name
   * @return string representing the token name
   */
  function name() external view returns (string memory _name) {
    return name_;
  }

  /**
   * @dev Gets the token symbol
   * @return string representing the token symbol
   */
  function symbol() external view returns (string memory _symbol) {
    return symbol_;
  }

  /**
   * @dev Returns an URI for a given token ID
   * @dev Throws if the token ID does not exist. May return an empty string.
   * @param _tokenId uint256 ID of the token to query
   */
  function tokenURI(uint256 _tokenId) external view returns (string memory) {
    return Strings.strConcat(
        baseTokenURI,
        Strings.uint2str(_tokenId)
    );
  }

  /**
   * @dev Gets the token ID at a given index of the tokens list of the requested owner
   * @param _owner address owning the tokens list to be accessed
   * @param _index uint256 representing the index to be accessed of the requested tokens list
   * @return uint256 token ID at the given index of the tokens list owned by the requested address
   */
  function tokenOfOwnerByIndex(
    address _owner,
    uint256 _index
  )
    public
    view
    returns (uint256)
  {
    require(_index < balanceOf(_owner));
    return ownedTokens[_owner][_index];
  }

  /**
   * @dev Gets the total amount of tokens stored by the contract
   * @return uint256 representing the total amount of tokens
   */
  function totalSupply() public view returns (uint256) {
    return allTokens.length;
  }

  /**
   * @dev Gets the token ID at a given index of all the tokens in this contract
   * @dev Reverts if the index is greater or equal to the total number of tokens
   * @param _index uint256 representing the index to be accessed of the tokens list
   * @return uint256 token ID at the given index of the tokens list
   */
  function tokenByIndex(uint256 _index) public view returns (uint256) {
    require(_index < totalSupply());
    return allTokens[_index];
  }

  /**
   * @dev Internal function to set the token URI for a given token
   * @dev Reverts if the token ID does not exist
   * @param _tokenId uint256 ID of the token to set its URI
   * @param _uri string URI to assign
   */
  function _setTokenURI(uint256 _tokenId, string memory _uri) internal {
    require(_exists(_tokenId));
    tokenURIs[_tokenId] = _uri;
  }

  /**
   * @dev Internal function to mint a new token
   * @dev Reverts if the given token ID already exists
   * @param _to address the beneficiary that will own the minted token
   * @param _tokenId uint256 ID of the token to be minted by the msg.sender
   */
  function _mint(address _to, uint256 _tokenId) internal {
    super._mint(_to, _tokenId);

    allTokensIndex[_tokenId] = allTokens.length;
    allTokens.push(_tokenId);
  }

  /**
   * @dev Internal function to burn a specific token
   * @dev Reverts if the token does not exist
   * @param _owner owner of the token to burn
   * @param _tokenId uint256 ID of the token being burned by the msg.sender
   */
  function _burn(address _owner, uint256 _tokenId) internal {
    
    super._burn(_owner, _tokenId);

    // Clear metadata (if any)
    if (bytes(tokenURIs[_tokenId]).length != 0) {
      delete tokenURIs[_tokenId];
    }

    // Reorg all tokens array
    uint256 tokenIndex = allTokensIndex[_tokenId];
    uint256 lastTokenIndex = allTokens.length.sub(1);
    uint256 lastToken = allTokens[lastTokenIndex];

    allTokens[tokenIndex] = lastToken;
    allTokens[lastTokenIndex] = 0;

    allTokens.length--;
    allTokensIndex[_tokenId] = 0;
    allTokensIndex[lastToken] = tokenIndex;

    //track rarity totals
    tokensByRarity[rarity[Cards[_tokenId].cardTypeId]] = tokensByRarity[rarity[Cards[_tokenId].cardTypeId]] - 1;

    // Gain czxp for sacrificing card
    uint256 sacrificeCzxp = allCardTypes[Cards[_tokenId].cardTypeId].sacrificeCzxp;
    awardCzxp(msg.sender,sacrificeCzxp);
    
     //remove from universe
    delete Cards[_tokenId];
    
    emit SacrificeCardEvent(msg.sender, _tokenId, Cards[_tokenId].cardTypeId, sacrificeCzxp);
  }

    /*CRYPTOZ Overrides */

    function _transferFrom(address from, address to, uint256 tokenId) internal {
        super._transferFrom(from, to, tokenId);
    }
    
    

    /**
     *  Public method to allow file transfer, we call our internal function
     */
    function transferFrom(address from, address to, uint256 tokenId) public {
        require(_isApprovedOrOwner(msg.sender, tokenId));
        
        //Lets track the card transfers
        Cards[tokenId].transferCount += 1;
        
        _transferFrom(from, to, tokenId);
    }

    /**
    *   Create the Card for the owner
    */
    function _createCard(uint32 _cardTypeId, address _owner, uint czxpGained) internal returns (uint){
        //make sure we have a definition for this card
        require(allCardTypes[_cardTypeId].cardTypeId == _cardTypeId);

        //Increment edition number
        cardTypeToEdition[_cardTypeId] = cardTypeToEdition[_cardTypeId].add(1);

        //Create the card!
        Card memory _tempCard = Card({
            cardTypeId:_cardTypeId,
            editionNumber:cardTypeToEdition[_cardTypeId],
            transferCount:0
        });

        //add to the universe and get the ID, -1 for array offset
        uint256 _newCardId = Cards.push(_tempCard) - 1;

        //Give the ERC721 Card
        _mint(_owner,_newCardId);

        //Set the Metadata for this new token
        _setTokenURI(_newCardId, baseTokenURI);
        
        //track rarity
        uint8 _temp_rarity = rarity[_cardTypeId];
        tokensByRarity[_temp_rarity] = tokensByRarity[_temp_rarity] + 1;
        
        //Emit card created owner,tokenId,Edition of Token,Card type,czxp gained
        emit LogCardCreated(_owner, _newCardId, cardTypeToEdition[_cardTypeId], _cardTypeId, rarity[_cardTypeId], czxpGained);
        
        return _newCardId;
    }
    

    //Get a list of all the CardIDs( tokenIDs ) this user owns
    function tokensOfOwner(address _owner) external view returns(uint256[] memory ownerTokens) {
        return _tokensOfOwner(_owner);
    }
    
    //Get total tokens by rarity. diamond,platinum,epic,rare,uncommon,common
    function getTokensByRarity() external view returns(uint,uint,uint,uint,uint,uint) {
        return(tokensByRarity[1],tokensByRarity[2],tokensByRarity[3],tokensByRarity[4],tokensByRarity[5],tokensByRarity[6]);
    }
    
    //Get the info about the card
    function getOwnedCard(uint256 _tokenId) public view returns(uint32, uint, uint) {
        require(_exists(_tokenId));
        return (Cards[_tokenId].cardTypeId,Cards[_tokenId].editionNumber,Cards[_tokenId].transferCount);
    }
}

// File: contracts/Cryptoz.sol

pragma solidity ^0.5.0;

/******************************************

Our Matra our Motto for coding The Cryptoz Universe.buy

      Fail EARLY and Fail LOUD !!!!!!!

1.    First - Check all pre-conditions
2.    Then  - make changes to contract state
3.    Final - interact with other contracts

Pull funds over pushing them
Platform limits like max of 1023 loop interations.

all Events start with Log

******************************************/


contract Cryptoz is CryptozCard {

//Constants
    uint constant public weiCostOfCard = 2000000000000000; //booster in wei = 0.002 ETH
    uint constant public czxpGainedBoosterPurchase = 120;
    uint constant public maxCardTypes = 5000; //allowed types in the Universe
    uint constant public maxBoostersReward = 50000; // Promotional boosters for airdrop rewards

//Event Logs
    event LogCardTypeLoaded(uint32 indexed cardTypeId, string cardName);
    event LogCardPurchased(uint32 indexed cardTypeId, uint editionNumber, address indexed buyer);
    event LogPackOpened(address indexed buyer, uint packsOpened);
    event LogSponsorLinked(address sponsor, address affiliate);
    event LogSponsorReward(address sponsor, address affiliate, uint CzxpReward);
    event LogDailyReward(address player, uint newBonusBalance);
    event LogRewardBoosters(address winner, uint boostersAwarded);

//storage

    //Track the total amount of Booster rewards
    uint public totalBoostersRewarded = 0;
    
    //Tracking affiliate sponsors
    mapping(address => address) public sponsors; // 1 sponsor can have many affiliates, but only 1 sponsor, returns sponsor
    
    //Track the timestamps for users to get their daily pull
    mapping(address => uint) private timeToCardsPull; //address to timestamp
    
    //Tracking booster pack count ownership. These are NOT tokens, Play can mint a random card
    mapping (address => uint256) public boosterPacksOwned;
    
    //where we hold inventory of the  booster packs
    mapping(uint8 => uint32[]) private allBoosterCardIds; // [rarityId][cardId]
    
    //check if cardType exists
    modifier isValidCard(uint32 _cardTypeId) {
//TIGHTEN   //Make sure cardType exists and we dont have bad data coming in
        require(allCardTypes[_cardTypeId].cardTypeId == _cardTypeId);
        require(storeBoosterBonus[_cardTypeId] < 1); //you cant get cards marked for booster pack or bonus
        _;
    }
    
    function() external {
        revert();
    }

    //Set our cxzp contract in the constructor
    function initialize(address payable _czxpContractAddress) onlyOwner public {
        CzxpContractAddress_ = _czxpContractAddress;
    }

    function loadNewCardType(
            uint8 _cardTypeId,
            string calldata _name,
            string calldata _set,
            uint8 _assetType,
            uint8 _notStoreOrBonus,
            uint8 _rarity,
            uint16 _totalAvailable,
            uint256 _weiCost,
            uint256 _buyCzxp
        ) external onlyOwner returns(bool){
            
        //what requires should we add ?
        
        //a max. of 5000 types in the universe ever
        require(cardTypesIds.length <= maxCardTypes);
        
        //not allowed to update existing cardTypeIds
        require(allCardTypes[_cardTypeId].cardTypeId == 0);
        
        allCardTypes[_cardTypeId].cardTypeId    = _cardTypeId;
        allCardTypes[_cardTypeId].name          = _name;
        allCardTypes[_cardTypeId].set           = _set;
        cardAssetType[_cardTypeId]              = _assetType;
        storeBoosterBonus[_cardTypeId]          = _notStoreOrBonus;
        rarity[_cardTypeId]                     = _rarity;
        totalAvailable[_cardTypeId]             = _totalAvailable;
        allCardTypes[_cardTypeId].weiCost       = _weiCost;
        allCardTypes[_cardTypeId].buyCzxp       = _buyCzxp;
    
        //If she be booster worthy
        if(_notStoreOrBonus == 1){ // 0=store,1=booster,2=bonus
            allBoosterCardIds[_rarity].push(_cardTypeId);
        }
        
        //Track the cardTypeIds count
        cardTypesIds.push(_cardTypeId);
        return true;
    }

    function addTocardType(
                        uint32  _cardTypeId,
                        uint256 _transferCzxp,
                        uint256 _sacrificeCzxp,
                        uint256 _unlockCzxp,
                        uint8 _cardLevel
                      ) external onlyOwner returns(bool){
                          
        //not allowed to update existing cardTypeIds
//TODO check for NULL in case we push 0 for a property
        require(allCardTypes[_cardTypeId].transferCzxp == 0);
        require(allCardTypes[_cardTypeId].sacrificeCzxp == 0);
        require(allCardTypes[_cardTypeId].unlockCzxp == 0);
        require(allCardTypes[_cardTypeId].cardLevel == 0);
                          
        require(allCardTypes[_cardTypeId].cardTypeId == _cardTypeId);
        
        
        allCardTypes[_cardTypeId].transferCzxp  = _transferCzxp;
        allCardTypes[_cardTypeId].sacrificeCzxp = _sacrificeCzxp;
        allCardTypes[_cardTypeId].unlockCzxp    = _unlockCzxp;
        allCardTypes[_cardTypeId].cardLevel     = _cardLevel;
        
        //initialize editionNumber
        cardTypeToEdition[_cardTypeId] = 0;
        
        //Card type is defined, now emit a Log of it
        emit LogCardTypeLoaded(_cardTypeId,allCardTypes[_cardTypeId].name);
        return true;
    }
    
    
/**
    /**
     *  Public interface to purchase a card, probablly the most insecure entry point !
     */
    function buyCard(uint32 _cardTypeId) external payable isValidCard(_cardTypeId) returns(bool) {
        
        // dont even bother if no ETH sent
        require(msg.value > 0, "Pay up!");
        
        //check for valid cardType
        require(allCardTypes[_cardTypeId].cardTypeId == _cardTypeId, "Cannot buy cards that are not defined");
        
        cardType memory _tempCard = allCardTypes[_cardTypeId];
        
        //check if store only
        require(storeBoosterBonus[_cardTypeId] == 0, "Can only buy cards from Store");
        
        //CHECKS-EFFECT, Can't buy cards you own
        require(cardTypesOwned[msg.sender][_cardTypeId] == false, "Only 1 Card Type purchase per wallet");
         
        //Check if this card amount we sell is more than edition+1 for this card type
        require((totalAvailable[_cardTypeId] >= cardTypeToEdition[_cardTypeId] + 1 ), "Maximum edition reached for this Card Type");
        
        //check if they have paid enough for it
        require(msg.value >= _tempCard.weiCost, "Not enough Ether sent to purchase this card");
        
        //check if we have enough czxp to unlock this card
        uint czxp = CzxpToken(CzxpContractAddress_).balanceOf(msg.sender);
        require(czxp >= _tempCard.unlockCzxp, "Wallet does not have enough czxp to unlock this Card Type");
        
    //Clear ? to buy the card
    
        //stop re-entrancy. Track the type of card puchased for this owner, so they cant buy again
        cardTypesOwned[msg.sender][_cardTypeId] = true;
        
        //Let the universe award our friend
        super.awardCzxp(msg.sender, allCardTypes[_cardTypeId].buyCzxp);
        
        //and award their sponsor
        rewardAffiliate(allCardTypes[_cardTypeId].buyCzxp);
    
        //Mint the real token and pass czxp gained for logging
        super._createCard(_cardTypeId, msg.sender,allCardTypes[_cardTypeId].buyCzxp);
        
        
        return true;
    }
    
    //every 8 hours, the address can get 2 free booster cards
    function getBonusBoosters() external {
        //this only runs if time to get new cards
        require(now > getTimeToDailyBonus(msg.sender), "Can't claim before time to claim next bonus");
        
        //Stop re-entrancy, update the lastpull value
        timeToCardsPull[msg.sender] = now + 8 hours;
        
        // add the boosters and emit event
        boosterPacksOwned[msg.sender] = boosterPacksOwned[msg.sender].add(2);
        emit LogDailyReward(msg.sender, boosterPacksOwned[msg.sender]);
    }
    
    /**
     *  Public interface to get card at no cost
     */
    function getFreeCard(uint32 _cardTypeId) external isValidCard(_cardTypeId) {
        //check if store only
        require(storeBoosterBonus[_cardTypeId] == 0);
        
        // ensure there is enough of a supply left
        require(totalAvailable[_cardTypeId] > (cardTypeToEdition[_cardTypeId]+1));
        
        //Can't get cards From shop that you own
        require(cardTypesOwned[msg.sender][_cardTypeId] == false);
        
        //check if we have enough czxp to unlock this card
        uint czxp = CzxpToken(CzxpContractAddress_).balanceOf(msg.sender);
        require(czxp >= allCardTypes[_cardTypeId].unlockCzxp);
        
        //Only cards that are Free
        require(allCardTypes[_cardTypeId].weiCost == 0);

//ALL CLEAR ???????? claim a new card
    
        //Stop re-entrancy, Track the type of card puchased for this owner, so they cant buy again
        cardTypesOwned[msg.sender][_cardTypeId] = true;
        
        //Let the universe award our friend
        super.awardCzxp(msg.sender, allCardTypes[_cardTypeId].buyCzxp);
        
        //reward the sponsor
        rewardAffiliate(allCardTypes[_cardTypeId].buyCzxp);
        
        //now mint the card
        super._createCard(_cardTypeId, msg.sender, allCardTypes[_cardTypeId].buyCzxp);
    
    }
    
    /**
     *  Any user can sacrifice any card they own. Please don't burn down the universe :'(
     */
     function sacrifice(uint256 _tokenId) external {
         //Call our internal burn function does all our owner checks
         _burn(msg.sender, _tokenId);
     }
    
//Booster Pack functions

    /**
     *  Public interface to award booster credits from Airdrops - restricted to Owner
    */
    function awardBoosterCredits(address _winner, uint _amount) external onlyOwner returns(bool) {
        
        //Check max. for Universe
        require((totalBoostersRewarded + _amount) <= maxBoostersReward, "Reached max. number of Booster credit rewards");
        
        //All good
        //stop re-entrance, increase universe total
        totalBoostersRewarded.add(_amount);
        
        //All good increase the number owned for this winner
        boosterPacksOwned[_winner] = boosterPacksOwned[_winner].add(_amount);
        
        //Log the event
        emit LogRewardBoosters(_winner, _amount);

        return true;
    }

 
    /**
     *  Public interface to buy booster card(s)
    */
    function buyBoosterCard(uint _amount) payable external returns(bool) {
        
        // is there enough wei sent 1 pack = 0.002 ETH ?
        require(msg.value >= weiCostOfCard.mul(_amount));
        
        //All good increase the number owned
        boosterPacksOwned[msg.sender] = boosterPacksOwned[msg.sender].add(_amount);

        //Award czxp for booster
        super.awardCzxp(msg.sender, uint(czxpGainedBoosterPurchase.mul(_amount)));
        
        rewardAffiliate(uint(czxpGainedBoosterPurchase.mul(_amount)));

        return true;
    }
    
    /**
     *  Public interface to buy booster pack(s) and open them - NO burn option
     */
    function buyBoosterCardAndOpen() payable external {
        // is there enough wei sent 1 pack = 0.005 ETH ?
        require(msg.value >= weiCostOfCard);
        
        //All good increase the number owned
        boosterPacksOwned[msg.sender] = boosterPacksOwned[msg.sender].add(1);

        //Award czxp per pack
        super.awardCzxp(msg.sender, czxpGainedBoosterPurchase);
        
        rewardAffiliate(czxpGainedBoosterPurchase);
        
        //Now open the pack
        openBoosterCard(0);
    }
    
    /**
     *  Public interface to open already purchased booster pack(s) - User can wager an amount
     */
    function openBoosterCard(uint czxpWager) public returns(bool) {
        //Ensure user owns unopened packs
        require(boosterPacksOwned[msg.sender] > 0);
        
        //czxpWager check czxpWager
        require(czxpWager >= 0);
        
        //STOP re-entrancy , decrement number of packs
        boosterPacksOwned[msg.sender] = boosterPacksOwned[msg.sender].sub(1);
        
        //Pull the card
        uint8 rarity = getRarity(czxpWager);
        pullCard(rarity);
        
        //Send a log event
        emit LogPackOpened(msg.sender, rarity);
        return true;
    }
   
    /**
     * Public interface to allow a player to forever link their 1 sponsor
     */
    function linkMySponsor(address mySponsor) external {
        //check that mySponsor arg is not 0x0
        require(mySponsor != address(0));
        
        //ensure the sponsor isn't Linked
        require(sponsors[msg.sender] == address(0));
        
        //Check they are not linking to themselves
        require(msg.sender != mySponsor);
        
        //All clear?  stop re-entrancy, set the association
        sponsors[msg.sender] = mySponsor;
        
        //Mint the Platinum Sponsor Card
        pullCard(2);
    }

//Private

    /**
     * We always pay our affiliates 20% of the czxp
     */
     function rewardAffiliate(uint totalCZXP) private  {
         //first check if the caller has a sponsor
         if(sponsors[msg.sender] != address(0)){
             uint reward = totalCZXP / 20;
             if(reward == 0){
                 reward = 1;
             }
             super.awardCzxp(sponsors[msg.sender], reward);
             emit LogSponsorReward(sponsors[msg.sender], msg.sender, reward);
         }
     }

    /**
     *
     */
     function pullCard(uint8 rarity) private {
         
         //Get a random number for the card to pull
         uint rand = selectRandom(allBoosterCardIds[rarity].length);
         
         //hit up the cardTypes
         uint32 pulledId = allBoosterCardIds[rarity][rand];
         
        //Give the player this cardType
        super._createCard(pulledId, msg.sender, 100);
     }
    
    /**
     *  Private function to diceroll the rarity pull Only from booster packs
     */
    function getRarity(uint czxpWager) private returns(uint8){
        
        //Check if below upper limit
        require(czxpWager <= 1649267441667000);
        
        //FIRST ensure, player can back their wager
        uint _playerCZXPBalance = CzxpToken(CzxpContractAddress_).balanceOf(msg.sender);
        require(_playerCZXPBalance >= czxpWager);
        
        //Check effects - Take their czxp
        if(czxpWager > 0){
            CzxpToken(CzxpContractAddress_).burnCzxp(msg.sender, czxpWager);
        }
        
        //ALL CLEAR - grab a random number
        uint rand = selectRandom(10000);

        //get our probabilty distribution
        uint16[7] memory probs = burnAndBoost(czxpWager);
        
        //Get the rarity for this pull from the probs
        // 3=epic, 4=rare, 5=uncommon, 6=commmon
        for(uint8 i=2; i < probs.length; i++){
            if(rand <= probs[i]){
                return i;
            }
        }
//REMOVE THIS !!!!
        return 6;
    }
    
    
    /**
     * Private Booster distribution function
     */
     function burnAndBoost(uint czxpWager) private returns(uint16[7] memory) {
        
        //buyBoosterCardAndOpen will pass a zero, set a default
        if(czxpWager == 0){
            return [0,0,0,1,50,2700,10000];
        }
         
        (uint16 c, uint16 u, uint16 r, uint16 e) = super.getProbs(czxpWager);
        
        //default distribution is 1,50,2700,10000
         uint16[7] memory probs = [0,0,0,e,r,u,c];
         
        return probs;
     }
     
     /**
      * Return the timestamp of the next daily bonus for a player
      */
      function getTimeToDailyBonus(address _player) public view returns(uint timeStamp){
          
          //check if address exists
          if(timeToCardsPull[_player] == 0){
              return now - 2 seconds;
          }else{
              return timeToCardsPull[_player];
          }
      }


//Thanks! for supporting the Cryptoz universe <3 <3 <3
    /**
    * Withdraw balance to wallet ONLY for the owner to call this
    */
   function withdraw() external onlyOwner {
      msg.sender.transfer(address(this).balance);
   }
}