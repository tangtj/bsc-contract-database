pragma solidity ^0.5.17;

/*
       ______                            __             
      / ____/____ _ ____   ____ _ _____ / /_ ___   _____
     / / __ / __ `// __ \ / __ `// ___// __// _ \ / ___/
    / /_/ // /_/ // / / // /_/ /(__  )/ /_ /  __// /    
    \____/ \__,_//_/ /_/ \__, //____/ \__/ \___//_/     
        ______ _        /____/                          
       / ____/(_)____   ____ _ ____   _____ ___         
      / /_   / // __ \ / __ `// __ \ / ___// _ \        
     / __/  / // / / // /_/ // / / // /__ /  __/        
    /_/    /_//_/ /_/ \__,_//_/ /_/ \___/ \___/         
    ==================================================
    | V1.0.0 | Gangster Finance Token | MIT License |
    =================================================
    
    This contract is for the "OGX Token" component of Gangster Finance.
    
=======================================================================================
    
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, 
INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A 
PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT 
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF 
CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE 
OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
    
=======================================================================================
*/

//////////////////////////////////////////////////////////////////////////
// SafeMath - prevents a whole class of underflow/overflow math issues  //
//////////////////////////////////////////////////////////////////////////

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns(uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function safeSub(uint a, uint b) internal pure returns(uint) {
        if (b > a) {return 0;} else {return a - b;}
    }

    function sub(uint256 a, uint256 b) internal pure returns(uint256) {return sub(a, b, "SafeMath: subtraction overflow");}
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns(uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;
        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns(uint256) {
        if (a == 0) {return 0;}
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns(uint256) {return div(a, b, "SafeMath: division by zero");}
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns(uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns(uint256) {return mod(a, b, "SafeMath: modulo by zero");}
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns(uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }

    function max(uint256 a, uint256 b) internal pure returns(uint256) {return a >= b ? a : b;}
    function min(uint256 a, uint256 b) internal pure returns(uint256) {return a < b ? a : b;}
}


//////////////////////////////////////////////////////////////////////////
// Address - helps with address manipulations (toPayable, isContract)   //
//////////////////////////////////////////////////////////////////////////

library Address {
    function isContract(address account) internal view returns(bool) {
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        assembly {codehash := extcodehash(account)}
        return (codehash != accountHash && codehash != 0x0);
    }

    function toPayable(address account) internal pure returns(address payable) {
        return address(uint160(account));
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");
        (bool success, ) = recipient.call.value(amount)("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }
}


//////////////////////////////////////////////////////////////////////////
// SafeERC20 - Extensions upon ERC20, assisting more advanced tokens    //
//////////////////////////////////////////////////////////////////////////

library SafeERC20 {
    using SafeMath for uint256;
    using Address for address;

    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
        callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    function safeApprove(IERC20 token, address spender, uint256 value) internal {
        require((value == 0) || (token.allowance(address(this), spender) == 0), "SafeERC20: approve from non-zero to non-zero allowance");
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

    function callOptionalReturn(IERC20 token, bytes memory data) private {
        require(address(token).isContract(), "SafeERC20: call to non-contract");
        (bool success, bytes memory returndata) = address(token).call(data);
        require(success, "SafeERC20: low-level call failed");

        if (returndata.length > 0) {
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}


//////////////////////////////////////////////////////////////////////////
// IERC20 - Interface for ERC20 to enable functions for other contracts //
//////////////////////////////////////////////////////////////////////////

interface IERC20 {
    function totalSupply() external view returns(uint256);
    function balanceOf(address account) external view returns(uint256);
    function transfer(address recipient, uint256 amount) external returns(bool);
    function allowance(address owner, address spender) external view returns(uint256);
    function approve(address spender, uint256 amount) external returns(bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns(bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}


/////////////////////////////////////////////////////////////////////////
// CONTEXT - Helps to clarify context of transactions.                 //
/////////////////////////////////////////////////////////////////////////

contract Context {
    constructor() internal {}

    function _msgSender() internal view returns(address payable) {return msg.sender;}
    function _msgData() internal view returns(bytes memory) {
        this;
        return msg.data;
    }
}


/////////////////////////////////////////////////////////////////////////
// ERC20 & ERC20Detailed - the meat and guts of basically every token. //
/////////////////////////////////////////////////////////////////////////

contract ERC20 is Context, IERC20 {
    using SafeMath for uint256;

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;

    function totalSupply() public view returns(uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view returns(uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public returns(bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view returns(uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public returns(bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public returns(bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public returns(bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public returns(bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        _balances[sender] = _balances[sender].sub(amount, "ERC20: transfer amount exceeds balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }

    function _mint(address account, uint256 amount) internal {
        require(account != address(0), "ERC20: mint to the zero address");
        _totalSupply = _totalSupply.add(amount);
        _balances[account] = _balances[account].add(amount);
        emit Transfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal {
        require(account != address(0), "ERC20: burn from the zero address");
        _balances[account] = _balances[account].sub(amount, "ERC20: burn amount exceeds balance");
        _totalSupply = _totalSupply.sub(amount);
        emit Transfer(account, address(0), amount);
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _burnFrom(address account, uint256 amount) internal {
        _burn(account, amount);
        _approve(account, _msgSender(), _allowances[account][_msgSender()].sub(amount, "ERC20: burn amount exceeds allowance"));
    }
}

contract ERC20Detailed is IERC20 {
    string private _name;
    string private _symbol;
    uint8 private _decimals;

    constructor(string memory name, string memory symbol, uint8 decimals) public {
        _name = name;
        _symbol = symbol;
        _decimals = decimals;
    }

    function name() public view returns(string memory) {
        return _name;
    }

    function symbol() public view returns(string memory) {
        return _symbol;
    }

    function decimals() public view returns(uint8) {
        return _decimals;
    }
}


/////////////////////////////////////////////////////////////////////////
// OGToken - The Crown-jewel of this composition - the OGX Token!!!    //
/////////////////////////////////////////////////////////////////////////

contract OGToken is ERC20, ERC20Detailed {
    using Address for address;
    using SafeMath for uint256;
    
    ////////////////////////////////////////////////
    // CONFIGURABLES & VARIABLES
    ////////////////////////////////////////////////

    // Record the deployer's address as governance on contract creation.
    address payable public governance;
    
    uint256 public totalBNBDonations;

    // Record total numbers of tokens minted and burned.
    uint256 public totalTokensMinted;
    uint256 public totalTokensBurned;


    ////////////////////////////////////////////////
    // DATA MAPPING
    ////////////////////////////////////////////////

    // Mint and Burn counts for every address
    mapping(address => uint256) _burnedOf;
    mapping(address => uint256) _mintedOf;

    // Keep track of addresses which can call minter functions
    mapping(address => bool) _isWhitelisted;


    ////////////////////////////////////////////////
    // MODIFIERS & SCOPES
    ////////////////////////////////////////////////

    // Restricts a function to Governance only
    modifier onlyGovernance() {
        require(msg.sender == governance, 'ONLY_GOVERNANCE');
        _;
    }

    // Restricts a function to whitelisted addresses only
    modifier onlyWhitelisted() {
        address _caller = msg.sender;
        require(_isWhitelisted[_caller] == true, 'ONLY_WHITELISTED');
        _;
    }


    ////////////////////////////////////////////////
    // EVENTS
    ////////////////////////////////////////////////

    event Whitelist(address indexed user, bool status);

    event TokensMinted(address indexed _minter, address _mintedTo, uint256 _tokens, uint256 _timestamp);
    event TokensBurned(address indexed _burner, address _burnedFrom, uint256 _tokens, uint256 _timestamp);

    event StatsMigrated(address indexed _migrator, address indexed _recipient, uint256 _timestamp);
    
    event onEjectGovernance(address indexed _governance, uint256 _timestamp);
    event onUpdateWhitelist(address indexed _contractAddress, bool _wasWhitelisted, bool _isWhitelisted, bool _isContract, uint256 _timestamp);
    
    event onCollectDonations(address indexed _recipient, uint256 _amount, uint256 _timestamp);


    ////////////////////////////////////////////////
    // CONSTRUCTOR & FALLBACK
    ////////////////////////////////////////////////

    constructor() public ERC20Detailed("Gangster Finance", "OGX", 18) {
        governance = msg.sender;
    }

    // This fallback will collect all received BNB as a donation.
    // Donations can be collected by the current governance address.
    
    function() external payable {
        totalBNBDonations += msg.value;
    }


    ////////////////////////////////////////////////
    // PUBLIC READ FUNCTIONS
    ////////////////////////////////////////////////

    // How many tokens has an address minted? find out with this function...
    function mintedOf(address _addr) public view returns(uint256 _amount) {
        return _mintedOf[_addr];
    }

    // How many tokens has an address burned? find out with this function...
    function burnedOf(address _addr) public view returns(uint256 _amount) {
        return _burnedOf[_addr];
    }


    ////////////////////////////////////////////////
    // PUBLIC TOKEN FUNCTIONS
    ////////////////////////////////////////////////

    // Transfer account stats to another address.
    // For whatever reason, you can move your numbers to a new address.
    // This might be useful in the future - for now, it's just nice.
    function transferStatsTo(address _addr) external returns(bool _success) {
        address _currentAccount = msg.sender;

        // First, we note down the stats being migrated...
        uint256 _minted = _mintedOf[_currentAccount];
        uint256 _burned = _burnedOf[_currentAccount];

        // Then, the contract makes your current account empty...
        _setAccount(_currentAccount, 0, 0);

        // THEN, the contract adds your stats to the recipient totals.
        _addToAccount(_addr, _minted, _burned);

        // Tell the network, successful function!
        emit StatsMigrated(_currentAccount, _addr, block.timestamp);
        return true;
    }
    
    // Mint Tokens - can only be called by whitelisted contracts.
    // EG: OG Vault would call this function, to mint OGX rewards on deposit and compound.
    function mintTokens(address _receiver, uint256 _amount) onlyWhitelisted external {
        
        // Mint the tokens...
        _mint(_receiver, _amount);
        
        // Then add the stats to the minter's records...
        _addToAccount(_receiver, _amount, 0);
    }
    
    // Burn Tokens - can be called by any address.
    // Burning tokens reduces the number which the market cap has to be divided between.
    // This is one methodwhich might help to increase the price and value of remaining tokens.
    function burnTokens(address _receiver, uint256 _amount) external {
        
        // Burn the tokens...
        _burn(_receiver, _amount);
        
        // Then add the stats to the burner's records...
        _addToAccount(_receiver, 0, _amount);
    }


    ////////////////////////////////////////////////
    // GOVERNANCE FUNCTIONS
    ////////////////////////////////////////////////

    // Governor can add addresses to the whitelist
    function addToWhitelist(address _addr) onlyGovernance() external returns(bool _success) {
        
        // Only contract addresses can be added to the whitelist
        require(_isContract(_addr), "CANNOT_ADD_EOA_ADDRESSES_TO_WHITELIST");
        
        // Whitelist the address...
        _isWhitelisted[_addr] = true;

        // Tell the network, successful function!
        emit onUpdateWhitelist(_addr, false, true, _isContract(_addr), block.timestamp);
        return true;
    }

    // Governor can remove addresses from the whitelist
    function removeFromWhitelist(address _addr) onlyGovernance() external returns(bool _success) {
        
        // An address must be on the whitelist, before it can be removed.
        require(_isWhitelisted[_addr], 'ONLY_MINTERS');

        // Remove the address from the whitelist...
        _isWhitelisted[_addr] = false;

        // Tell the network, successful function!
        emit onUpdateWhitelist(_addr, true, false, _isContract(_addr), block.timestamp);
        return true;
    }
    
    // Governor can eject themselves
    function eject() onlyGovernance() public returns (bool _success) {
        
        // Set the governance address to address(0)
        governance = address(0);
        
        // Tell the network, successful function!
        emit onEjectGovernance(msg.sender, block.timestamp);
        return true;
    }
    
    // Governor can withdraw and receive any BNB donations.
    function collectDonations() onlyGovernance() public returns (bool _success) {
        
        // Transfer BNB in this contract to Governance...
        governance.transfer(address(this).balance);
        
        // Tell the network, successful function!
        emit onCollectDonations(msg.sender, address(this).balance, block.timestamp);
        return true;
    }


    ////////////////////////////////////////////////
    // INTERNAL AND PRIVATE FUNCTIONS
    ////////////////////////////////////////////////

    // Helper to identify if an address is a contract.
    // Not perfect, but fine for now.
    function _isContract(address _addr) internal view returns(bool) {
        
        // Empty uint32, for size return.
        uint32 size;
        
        // Measure the size of whatever's at address(_addr).
        assembly {
            size := extcodesize(_addr)
        }
        
        // Return a bool, of whether the size is more than zero or not.
        return (size > 0);
    }

    // Internal function to set the stats of an address.
    // WARNING: This replaces the values currently stored!
    function _setAccount(address _addr, uint256 _stat1, uint256 _stat2) internal {
        _mintedOf[_addr] = _stat1;
        _burnedOf[_addr] = _stat2;
    }

    // Internal function to update the stats of an address.
    // NOTICE: This adds to the values already stored!
    function _addToAccount(address _addr, uint256 _stat1, uint256 _stat2) internal {
        _mintedOf[_addr] += _stat1;
        _burnedOf[_addr] += _stat2;
        
        totalTokensMinted += _stat1;
        totalTokensBurned += _stat2;
    }
}