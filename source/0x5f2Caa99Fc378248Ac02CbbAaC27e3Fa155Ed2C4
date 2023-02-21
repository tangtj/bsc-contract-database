// File: contracts/InterFaces/IWhiteList.sol

pragma solidity ^0.5.9;

interface IWhiteList {
    function address_belongs(address _who) external view returns (address);

    function isWhiteListed(address _who) external view returns (bool);

    function isAllowedInAuction(address _which) external view returns (bool);

    function isAddressByPassed(address _which) external view returns (bool);

    function isExchangeAddress(address _which) external view returns (bool);

    function main_isTransferAllowed(
        address _msgSender,
        address _from,
        address _to
    ) external returns (bool);

    function etn_isTransferAllowed(
        address _msgSender,
        address _from,
        address _to
    ) external returns (bool);

    function stock_isTransferAllowed(
        address _msgSender,
        address _from,
        address _to
    ) external returns (bool);

    function addWalletBehalfExchange(address _mainWallet, address _subWallet)
        external
        returns (bool);

    function main_isReceiveAllowed(address user) external view returns (bool);

    function etn_isReceiveAllowed(address user) external view returns (bool);

    function stock_isReceiveAllowed(address user) external view returns (bool);
}

// File: contracts/Tokens/IBEP20.sol

pragma solidity ^0.5.9;

interface IBEP20 {
  /**
   * @dev Returns the amount of tokens in existence.
   */
  function totalSupply() external view returns (uint256);

  /**
   * @dev Returns the token decimals.
   */
  function decimals() external view returns (uint8);

  /**
   * @dev Returns the token symbol.
   */
  function symbol() external view returns (string memory);

  /**
  * @dev Returns the token name.
  */
  function name() external view returns (string memory);

  /**
   * @dev Returns the bep token owner.
   */
  function getOwner() external view returns (address);

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
  function allowance(address _owner, address spender) external view returns (uint256);

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

// File: contracts/common/SafeMath.sol

pragma solidity ^0.5.9;

contract SafeMath {
    function safeMul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        assert(c / a == b);
        return c;
    }

    function safeDiv(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a / b;
        return c;
    }

    function safeSub(uint256 a, uint256 b) internal pure returns (uint256) {
        assert(b <= a);
        return a - b;
    }

    function safeAdd(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        assert(c >= a);
        return c;
    }

    function safeExponent(uint256 a, uint256 b)
        internal
        pure
        returns (uint256)
    {
        uint256 result;
        assembly {
            result := exp(a, b)
        }
        return result;
    }

    // calculates a^(1/n) to dp decimal places
    // maxIts bounds the number of iterations performed
    function nthRoot(
        uint256 _a,
        uint256 _n,
        uint256 _dp,
        uint256 _maxIts
    ) internal pure returns (uint256) {
        assert(_n > 1);

        // The scale factor is a crude way to turn everything into integer calcs.
        // Actually do (a * (10 ^ ((dp + 1) * n))) ^ (1/n)
        // We calculate to one extra dp and round at the end
        uint256 one = 10**(1 + _dp);
        uint256 a0 = one**_n * _a;

        // Initial guess: 1.0
        uint256 xNew = one;
        uint256 x;

        uint256 iter = 0;
        while (xNew != x && iter < _maxIts) {
            x = xNew;
            uint256 t0 = x**(_n - 1);
            if (x * t0 > a0) {
                xNew = x - (x - a0 / t0) / _n;
            } else {
                xNew = x + (a0 / t0 - x) / _n;
            }
            ++iter;
        }

        // Round to nearest in the last dp.
        return (xNew + 5) / 10;
    }
}

// File: contracts/common/Constant.sol

pragma solidity ^0.5.9;

contract Constant {
    string constant ERR_CONTRACT_SELF_ADDRESS = "ERR_CONTRACT_SELF_ADDRESS";

    string constant ERR_ZERO_ADDRESS = "ERR_ZERO_ADDRESS";

    string constant ERR_NOT_OWN_ADDRESS = "ERR_NOT_OWN_ADDRESS";

    string constant ERR_VALUE_IS_ZERO = "ERR_VALUE_IS_ZERO";

    string constant ERR_SAME_ADDRESS = "ERR_SAME_ADDRESS";

    string constant ERR_AUTHORIZED_ADDRESS_ONLY = "ERR_AUTHORIZED_ADDRESS_ONLY";

    modifier notOwnAddress(address _which) {
        require(msg.sender != _which, ERR_NOT_OWN_ADDRESS);
        _;
    }

    // validates an address is not zero
    modifier notZeroAddress(address _which) {
        require(_which != address(0), ERR_ZERO_ADDRESS);
        _;
    }

    // verifies that the address is different than this contract address
    modifier notThisAddress(address _which) {
        require(_which != address(this), ERR_CONTRACT_SELF_ADDRESS);
        _;
    }

    modifier notZeroValue(uint256 _value) {
        require(_value > 0, ERR_VALUE_IS_ZERO);
        _;
    }
}

// File: contracts/common/Ownable.sol

pragma solidity ^0.5.9;


contract Ownable is Constant {
    address public primaryOwner = address(0);

    address public authorityAddress = address(0);

    address public systemAddress = address(0);

    address public newAuthorityAddress = address(0);

    event OwnershipTransferred(
        string ownerType,
        address indexed previousOwner,
        address indexed newOwner
    );
    event AuthorityAddressChnageCall(
        address indexed previousOwner,
        address indexed newOwner
    );

    /**
     * @dev The Ownable constructor sets the `primaryOwner` and `systemAddress` and '_multisigAddress'
     * account.
     */
    constructor(address _systemAddress, address _authorityAddress)
        public
        notZeroAddress(_systemAddress)
    {
        require(msg.sender != _systemAddress, ERR_SAME_ADDRESS);

        require(_systemAddress != _authorityAddress, ERR_SAME_ADDRESS);

        require(msg.sender != _authorityAddress, ERR_SAME_ADDRESS);

        primaryOwner = msg.sender;

        systemAddress = _systemAddress;

        authorityAddress = _authorityAddress;
    }

    modifier onlyOwner() {
        require(msg.sender == primaryOwner, ERR_AUTHORIZED_ADDRESS_ONLY);
        _;
    }

    modifier onlySystem() {
        require(msg.sender == systemAddress, ERR_AUTHORIZED_ADDRESS_ONLY);
        _;
    }

    modifier onlyOneOfOnwer() {
        require(
            msg.sender == primaryOwner || msg.sender == systemAddress,
            ERR_AUTHORIZED_ADDRESS_ONLY
        );
        _;
    }

    modifier onlyAuthorized() {
        require(msg.sender == authorityAddress, ERR_AUTHORIZED_ADDRESS_ONLY);
        _;
    }

   /**
     * @dev change primary ownership governance 
     */
    function changePrimaryOwner()
        public
        onlyOwner()
        returns (bool)
    {
        emit OwnershipTransferred("PRIMARY_OWNER", primaryOwner, authorityAddress);
        primaryOwner = authorityAddress;
        return true;
    }

    /**
     * @dev change system address
     * @param _which The address to which is new system address
     */
    function changeSystemAddress(address _which)
        public
        onlyAuthorized()
        notThisAddress(_which)
        notZeroAddress(_which)
        returns (bool)
    {
        require(
            _which != systemAddress &&
                _which != authorityAddress &&
                _which != primaryOwner,
            ERR_SAME_ADDRESS
        );
        emit OwnershipTransferred("SYSTEM_ADDRESS", systemAddress, _which);
        systemAddress = _which;
        return true;
    }

    /**
     * @dev change system address
     * @param _which The address to which is new Authority address
     */
    function changeAuthorityAddress(address _which)
        public
        onlyAuthorized()
        notZeroAddress(_which)
        returns (bool)
    {
        require(
            _which != systemAddress &&
                _which != authorityAddress &&
                _which != primaryOwner,
            ERR_SAME_ADDRESS
        );
        newAuthorityAddress = _which;
        return true;
    }

    function acceptAuthorityAddress() public returns (bool) {
        require(msg.sender == newAuthorityAddress, ERR_AUTHORIZED_ADDRESS_ONLY);
        emit OwnershipTransferred(
            "AUTHORITY_ADDRESS",
            authorityAddress,
            newAuthorityAddress
        );
        authorityAddress = newAuthorityAddress;
        newAuthorityAddress = address(0);
        return true;
    }
}

// File: contracts/Tokens/StandardToken.sol

pragma solidity ^0.5.9;





contract StandardToken is IBEP20, SafeMath, Ownable {
    
    uint256 public totalSupply;
    
    string public name;

    string public symbol;
 
    uint256 public constant decimals = 18;

    mapping(address => uint256) balances;

    mapping(address => mapping(address => uint256)) allowed;
    
    constructor(string memory _name,
                string memory _symbol,
                address _systemAddress,
                address _authorityAddress) public Ownable(_systemAddress,_authorityAddress)  {
                    
        require(bytes(_name).length > 0 && bytes(_symbol).length > 0);
        name = _name;
        symbol = _symbol;
    }

    event Mint(address indexed _to, uint256 value);
    event Burn(address indexed from, uint256 value);
    event TransferFrom(
        address indexed spender,
        address indexed _from,
        address indexed _to
    );
    
    
    /**
     * @dev transfer token for a specified address
     * @param _from The address from token transfer.
     * @param _to The address to transfer to.
     * @param _value The amount to be transferred.
     */
    function _transfer(address _from, address _to, uint256 _value)
        internal
        returns (bool)
    {
        uint256 senderBalance = balances[_from];
        require(senderBalance >= _value, "ERR_NOT_ENOUGH_BALANCE");
        senderBalance = safeSub(senderBalance, _value);
        balances[_from] = senderBalance;
        balances[_to] = safeAdd(balances[_to], _value);
        emit Transfer(_from, _to, _value);
        return true;
    }
    
     /**
     * @dev Transfer tokens from one address to another
     * @param _from address The address which you want to send tokens from
     * @param _to address The address which you want to transfer tokens
     * @param _value uint256 the amount of tokens to be transferred
     */
    function _transferFrom(address _from, address _to, uint256 _value)
        internal
        notThisAddress(_to)
        notZeroAddress(_to)
        returns (bool)
    {
        require(allowed[_from][msg.sender] >= _value, "ERR_NOT_ENOUGH_BALANCE");
        require(_transfer(_from, _to, _value));
        allowed[_from][msg.sender] = safeSub(
            allowed[_from][msg.sender],
            _value
        );
        emit TransferFrom(msg.sender, _from, _to);
        return true;
    }
    
    function _approve(address owner, address spender, uint256 amount) internal notZeroAddress(spender) {
        
        allowed[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
    
    function _burn(address _from, uint256 _value) internal returns (bool) {
        uint256 senderBalance = balances[_from];
        require(senderBalance >= _value, "ERR_NOT_ENOUGH_BALANCE");
        senderBalance = safeSub(senderBalance, _value);
        balances[_from] = senderBalance;
        totalSupply = safeSub(totalSupply, _value);
        emit Burn(_from, _value);
        emit Transfer(_from, address(0), _value);
        return true;
    }
    
    function _mint(address _to, uint256 _value) internal returns (bool) {
        balances[_to] = safeAdd(balances[_to], _value);
        totalSupply = safeAdd(totalSupply, _value);
        emit Mint(_to, _value);
        emit Transfer(address(0), _to, _value);
        return true;
    }
    
    
    
    /**
     * @dev Gets the balance of the specified address.
     * @param _who The address to query the the balance of.
     * @return balance An uint256 representing the amount owned by the passed address.
     */
    function balanceOf(address _who) public view returns (uint256 balance) {
        return balances[_who];
    }
    
 
    /**
     * @dev `msg.sender` approves `spender` to spend `value` tokens
     * @param _spender The address of the account able to transfer the tokens
     * @param _value The amount of wei to be approved for transfer
     * @return ok Whether the approval was successful or not
     */
    function approve(address _spender, uint256 _value)
        external
        notZeroAddress(_spender)
        returns (bool ok)
    {
        _approve(msg.sender, _spender, _value);
        return true;
    }
    
     /**
     * @dev `msg.sender` approves `spender` to increase spend `value` tokens
     * @param _spender The address of the account able to transfer the tokens
     * @param _value The amount of wei to be approved for transfer
     * @return Whether the approval was successful or not
     */
    function increaseAllowance(address _spender, uint256 _value) external  returns (bool) {
        uint256 currentAllowed = allowed[msg.sender][_spender];
        _approve(msg.sender, _spender, safeAdd(currentAllowed,_value));
        return true;
        
    }
    
     /**
     * @dev `msg.sender` approves `spender` to decrease spend `value` tokens
     * @param _spender The address of the account able to transfer the tokens
     * @param _value The amount of wei to be approved for transfer
     * @return Whether the approval was successful or not
     */
    function decreaseAllowance(address _spender, uint256 _value) external  returns (bool) {
        uint256 currentAllowed = allowed[msg.sender][_spender];
        require(currentAllowed >= _value,"ERR_ALLOWENCE");
        _approve(msg.sender, _spender, safeSub(currentAllowed,_value));
        return true;
    }


    
    /**
     * @dev to check allowed token for transferFrom
     * @param _owner The address of the account owning tokens
     * @param _spender The address of the account able to transfer the tokens
     * @return Amount of remaining tokens allowed to spent
     */
    function allowance(address _owner, address _spender)
        external
        view
        returns (uint256)
    {
        return allowed[_owner][_spender];
    }

   

    /**
     * @dev burn token from this address
     * @param _value uint256 the amount of tokens to be burned
     */
    function burn(uint256 _value) external returns (bool) {
        return _burn(msg.sender, _value);
    }
    
    /**
    * @dev Returns the bep token owner.
    */
    function getOwner() external view returns (address) {
        return systemAddress;
    }

}

// File: contracts/InterFaces/IAuctionRegistery.sol

pragma solidity ^0.5.9;

contract AuctionRegisteryContracts {
    bytes32 internal constant MAIN_TOKEN = "MAIN_TOKEN";
    bytes32 internal constant ETN_TOKEN = "ETN_TOKEN";
    bytes32 internal constant STOCK_TOKEN = "STOCK_TOKEN";
    bytes32 internal constant WHITE_LIST = "WHITE_LIST";
    bytes32 internal constant AUCTION = "AUCTION";
    bytes32 internal constant AUCTION_PROTECTION = "AUCTION_PROTECTION";
    bytes32 internal constant LIQUIDITY = "LIQUIDITY";
    bytes32 internal constant CURRENCY = "CURRENCY";
    bytes32 internal constant VAULT = "VAULT";
    bytes32 internal constant CONTRIBUTION_TRIGGER = "CONTRIBUTION_TRIGGER";
    bytes32 internal constant COMPANY_FUND_WALLET = "COMPANY_FUND_WALLET";
    bytes32 internal constant SMART_SWAP = "SMART_SWAP";
    bytes32 internal constant SMART_SWAP_P2P = "SMART_SWAP_P2P";
    bytes32 internal constant ESCROW = "ESCROW";
}

interface IAuctionRegistery {
    function getAddressOf(bytes32 _contractName)
        external
        view
        returns (address payable);
}

// File: contracts/Tokens/TokenUtils.sol

pragma solidity ^0.5.9;






/**@dev keeps track of registry contract at which all the addresses of the wholes system's contracts are stored */
contract AuctionRegistery is AuctionRegisteryContracts, Ownable {
    
    IAuctionRegistery public contractsRegistry;
    
    address public whiteListAddress;
    address public smartSwapAddress;
    address public currencyPricesAddress;
    address public vaultAddress;
    address public auctionAddress;
    
    /**@dev sets the initial registry address */
    constructor(address _registeryAddress)
        public
        notZeroAddress(_registeryAddress)
    {
        contractsRegistry = IAuctionRegistery(_registeryAddress);
         _updateAddresses();
    }

    /**@dev updates the address of the registry, called only by the system */
    function updateRegistery(address _address)
        external
        onlyAuthorized()
        notZeroAddress(_address)
        returns (bool)
    {
        contractsRegistry = IAuctionRegistery(_address);
        _updateAddresses();
        return true;
    }

    /**@dev returns address of the asked contract got from registry contract at the registryAddress
    @param _contractName name of the contract  */
    function getAddressOf(bytes32 _contractName)
        internal
        view
        returns (address)
    {
        return contractsRegistry.getAddressOf(_contractName);
    }
    
    /**@dev updates all the address from the registry contract
    this decision was made to save gas that occurs from calling an external view function */
    function _updateAddresses() internal {
        whiteListAddress = getAddressOf(WHITE_LIST);
        smartSwapAddress = getAddressOf(SMART_SWAP);
        currencyPricesAddress = getAddressOf(CURRENCY);
        vaultAddress = getAddressOf(VAULT);
        auctionAddress = getAddressOf(AUCTION);
    }
    
    function updateAddresses() external returns (bool) {
        _updateAddresses();
    }
}


/**@dev Also is a standard IBEP20 token*/
contract TokenUtils is StandardToken, AuctionRegistery {
    
    /**
     *@dev contructs standard IBEP20 token and auction registry
     *@param _name name of the token
     *@param _symbol symbol of the token
     *@param _systemAddress address that acts as an admin of the system
     *@param _authorityAddress address that can change the systemAddress
     *@param _registeryAddress address of the registry contract the keeps track of all the contract Addresses
     **/
    constructor(
        string memory _name,
        string memory _symbol,
        address _systemAddress,
        address _authorityAddress,
        address _registeryAddress
    )
        public
        StandardToken(_name, _symbol, _systemAddress, _authorityAddress)
        AuctionRegistery(_registeryAddress)
    {
       _updateAddresses();
    }

    
}

// File: contracts/Tokens/MainToken.sol

pragma solidity ^0.5.9;





contract TokenMinter is TokenUtils {
    modifier onlyAuthorizedAddress() {
        require(msg.sender == auctionAddress, ERR_AUTHORIZED_ADDRESS_ONLY);
        _;
    }

    function mintTokens(uint256 _amount)
        external
        onlyAuthorizedAddress()
        returns (bool)
    {
        return _mint(msg.sender, _amount);
    }
}


contract MainToken is TokenMinter {
    mapping(address => uint256) public lockedToken;
    mapping(address => uint256) public lastLock;

    /**
     *@dev constructs contract and premints tokens
     *@param _name name of the token
     *@param _symbol symbol of the token
     *@param _systemAddress address that acts as an admin of the system
     *@param _authorityAddress address that can change the systemAddress
     *@param _registeryAddress address of the registry contract the keeps track of all the contract Addresses
     *@param _which array of address to mint tokens to
     *@param _amount array of corresponding amount getting minted
     **/
    constructor(
        string memory _name,
        string memory _symbol,
        address _systemAddress,
        address _authorityAddress,
        address _registeryAddress,
        address[] memory _which,
        uint256[] memory _amount
    )
        public
        TokenUtils(
            _name,
            _symbol,
            _systemAddress,
            _authorityAddress,
            _registeryAddress
        )
    {
        require(_which.length == _amount.length, "ERR_NOT_SAME_LENGTH");

        for (uint256 tempX = 0; tempX < _which.length; tempX++) {
            require(
                IWhiteList(whiteListAddress).isWhiteListed(_which[tempX]),
                "ERR_TRANSFER_CHECK_WHITELIST"
            );
            _mint(_which[tempX], _amount[tempX]);
        }
    }

    function checkBeforeTransfer(address _from, address _to)
        internal
        returns (bool)
    {
        require(
            IWhiteList(whiteListAddress).main_isTransferAllowed(
                msg.sender,
                _from,
                _to
            ),
            "ERR_NOT_HAVE_PERMISSION_TO_TRANSFER"
        );

        return true;
    }

    function transfer(address _to, uint256 _value) external  returns (bool ok) {
        uint256 senderBalance = safeSub(
            balances[msg.sender],
            lockedToken[msg.sender]
        );
        require(senderBalance >= _value, "ERR_NOT_ENOUGH_BALANCE");
        require(checkBeforeTransfer(msg.sender, _to));
        return _transfer(msg.sender, _to, _value);
    }

    function transferFrom(
        address _from,
        address _to,
        uint256 _value
    ) external  returns (bool) {
        
        uint256 senderBalance = safeSub(balances[_from], lockedToken[_from]);
        require(senderBalance >= _value, "ERR_NOT_ENOUGH_BALANCE");
        
        require(checkBeforeTransfer(_from, _to));
        return _transferFrom(_from, _to, _value);
    }

    // we need lock time
    // becuse we can check if user invest after new auction start
    // if user invest before token distrubution we dont change anything
    // ex -> user invest  at 11:35 and token distrubution happened at 11:40
    // if in between user invest we dont unlock user token we keep as it as
    // to unlock token set _amount = 0
    function lockToken(
        address _which,
        uint256 _amount,
        uint256 _locktime
    ) external returns (bool) {
        require(msg.sender == auctionAddress, ERR_AUTHORIZED_ADDRESS_ONLY);
        if (_locktime > lastLock[_which]) {
            lockedToken[_which] = _amount;
            lastLock[_which] = _locktime;
        }
        return true;
    }

    // user can unlock their token after 1 day of locking
    // user dont need to call this function as auction set 0 after token distrubution
    // It is failsafe function for user that their token not locked all the time if AUCTION distrubution dont happened
    function unlockToken() external returns (bool) {
        require(
            safeAdd(lastLock[msg.sender], 86400) > now,
            "ERR_TOKEN_UNLCOK_AFTER_DAY"
        );
        lockedToken[msg.sender] = 0;
        return true;
    }

    function() external payable {
        revert("ERR_CAN'T_FORCE_ETH");
    }
}