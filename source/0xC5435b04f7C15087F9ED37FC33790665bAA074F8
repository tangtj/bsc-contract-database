/**
 *Submitted for verification at BscScan.com on 2021-02-11
*/

//@QuokkaToken

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



pragma solidity ^0.5.0;


interface IBEP20 {
    
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


    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

  
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);


    event Transfer(address indexed from, address indexed to, uint256 value);

   
    event Approval(address indexed owner, address indexed spender, uint256 value);
}



pragma solidity ^0.5.0;


library SafeMath {
    
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

   
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

 
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
       
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

   
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        // Solidity only automatically asserts when dividing by 0
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

   
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

   
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}



pragma solidity ^0.5.0;


contract BEP20 is Context, IBEP20 {
    using SafeMath for uint256;

    mapping (address => uint256) private _balances;

    mapping (address => mapping (address => uint256)) private _allowances;

    uint256 private _totalSupply;
    constructor (uint256 totalSupply) public {
        _mint(_msgSender(),totalSupply);
    }
 
    function totalSupply() public view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view returns (uint256) {
        return _balances[account];
    }

   
    function transfer(address recipient, uint256 amount) public returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

  
    function allowance(address owner, address spender) public view returns (uint256) {
        return _allowances[owner][spender];
    }

    
    function approve(address spender, uint256 amount) public returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

   
     
    function transferFrom(address sender, address recipient, uint256 amount) public returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    
    function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }

    
    function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }

    
    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(recipient != address(0), "BEP20: transfer to the zero address");

        _balances[sender] = _balances[sender].sub(amount, "BEP20: transfer amount exceeds balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
    }

   
    function _mint(address account, uint256 amount) internal {
        require(account != address(0), "BEP20: mint to the zero address");

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



pragma solidity ^0.5.0;


contract BEP20Burnable is Context, BEP20 {
    
    function burn(uint256 amount) public {
        _burn(_msgSender(), amount);
    }

    
    function burnFrom(address account, uint256 amount) public {
        _burnFrom(account, amount);
    }
}



pragma solidity ^0.5.0;

/**
 * @title Roles
 * @dev Library for managing addresses assigned to a Role.
 */
library Roles {
    struct Role {
        mapping (address => bool) bearer;
    }

    /**
     * @dev Give an account access to this role.
     */
    function add(Role storage role, address account) internal {
        require(!has(role, account), "Roles: account already has role");
        role.bearer[account] = true;
    }

    /**
     * @dev Remove an account's access to this role.
     */
    function remove(Role storage role, address account) internal {
        require(has(role, account), "Roles: account does not have role");
        role.bearer[account] = false;
    }

    /**
     * @dev Check if an account has this role.
     * @return bool
     */
    function has(Role storage role, address account) internal view returns (bool) {
        require(account != address(0), "Roles: account is the zero address");
        return role.bearer[account];
    }
}

// File: @openzeppelin/contracts/token/ERC20/ERC20Detailed.sol

pragma solidity ^0.5.0;

/**
 * @dev Optional functions from the ERC20 standard.
 */
contract BEP20Detailed is IBEP20 {
    string private _name;
    string private _symbol;
    uint8 private _decimals;

    /**
     * @dev Sets the values for `name`, `symbol`, and `decimals`. All three of
     * these values are immutable: they can only be set once during
     * construction.
     */
    constructor (string memory name, string memory symbol, uint8 decimals) public {
        _name = name;
        _symbol = symbol;
        _decimals = decimals;
    }

    /**
     * @dev Returns the name of the token.
     */
    function name() public view returns (string memory) {
        return _name;
    }

    /**
     * @dev Returns the symbol of the token, usually a shorter version of the
     * name.
     */
    function symbol() public view returns (string memory) {
        return _symbol;
    }

    /**
     * @dev Returns the number of decimals used to get its user representation.
     * For example, if `decimals` equals `2`, a balance of `505` tokens should
     * be displayed to a user as `5,05` (`505 / 10 ** 2`).
     *
     * Tokens usually opt for a value of 18, imitating the relationship between
     * Ether and Wei.
     *
     * NOTE: This information is only used for _display_ purposes: it in
     * no way affects any of the arithmetic of the contract, including
     * {IERC20-balanceOf} and {IERC20-transfer}.
     */
    function decimals() public view returns (uint8) {
        return _decimals;
    }
}

// File: @openzeppelin/contracts/access/roles/WhitelistAdminRole.sol

pragma solidity ^0.5.0;

/**
 * @title WhitelistAdminRole
 * @dev WhitelistAdmins are responsible for assigning and removing Whitelisted accounts.
 */
contract WhitelistAdminRole is Context {
    using Roles for Roles.Role;

    event WhitelistAdminAdded(address indexed account);
    event WhitelistAdminRemoved(address indexed account);

    Roles.Role private _whitelistAdmins;

    constructor () internal {
        _addWhitelistAdmin(_msgSender());
    }

    modifier onlyWhitelistAdmin() {
        require(isWhitelistAdmin(_msgSender()), "WhitelistAdminRole: caller does not have the WhitelistAdmin role");
        _;
    }

    function isWhitelistAdmin(address account) public view returns (bool) {
        return _whitelistAdmins.has(account);
    }

    function addWhitelistAdmin(address account) public onlyWhitelistAdmin {
        _addWhitelistAdmin(account);
    }

    function renounceWhitelistAdmin() public {
        _removeWhitelistAdmin(_msgSender());
    }

    function _addWhitelistAdmin(address account) internal {
        _whitelistAdmins.add(account);
        emit WhitelistAdminAdded(account);
    }

    function _removeWhitelistAdmin(address account) internal {
        _whitelistAdmins.remove(account);
        emit WhitelistAdminRemoved(account);
    }
}

// File: contracts/ERC20/ERC20TransferLiquidityLock.sol

pragma solidity ^0.5.17;

contract BEP20TransferLiquidityLock is BEP20 {
    using SafeMath for uint256;


    event Rebalance(uint256 tokenBurnt);
    event SupplyRenaSwap(uint256 tokenAmount);
    event RewardLiquidityProviders(uint256 liquidityRewards);
    
    address public uniswapV2Router;
    address public uniswapV2Pair;
    address public RenaSwap;
    address payable public treasury;
    address public bounce = 0x73282A63F0e3D7e9604575420F777361ecA3C86A;
    mapping(address => bool) feelessAddr;
    mapping(address => bool) unlocked;
    
    // the amount of tokens to lock for liquidity during every transfer, i.e. 100 = 1%, 50 = 2%, 40 = 2.5%
    uint256 public liquidityLockDivisor;
    uint256 public callerRewardDivisor;
    uint256 public rebalanceDivisor;
    
    uint256 public minRebalanceAmount;
    uint256 public lastRebalance;
    uint256 public rebalanceInterval;
    
    uint256 public lpUnlocked;
    bool public locked;
    
    Balancer balancer;
    
    constructor() public {
        lastRebalance = block.timestamp;
        liquidityLockDivisor = 100;
        callerRewardDivisor = 25;
        rebalanceDivisor = 50;
        rebalanceInterval = 1 hours;
        lpUnlocked = block.timestamp + 5 days;
        minRebalanceAmount = 100 ether;
        treasury = msg.sender;
        balancer = new Balancer(treasury);
        feelessAddr[address(this)] = true;
        feelessAddr[address(balancer)] = true;
        feelessAddr[bounce] = true;
        locked = true;
        unlocked[msg.sender] = true;
        unlocked[bounce] = true;
        unlocked[address(balancer)] = true;
    }
    
    //sav3 transfer function
    function _transfer(address from, address to, uint256 amount) internal {
        // calculate liquidity lock amount
        // dont transfer burn from this contract
        // or can never lock full lockable amount
        if(locked && unlocked[from] != true && unlocked[to] != true)
            revert("Locked until end of presale");
            
        if (liquidityLockDivisor != 0 && feelessAddr[from] == false && feelessAddr[to] == false) {
            uint256 liquidityLockAmount = amount.div(liquidityLockDivisor);
            super._transfer(from, address(this), liquidityLockAmount);
            super._transfer(from, to, amount.sub(liquidityLockAmount));
        }
        else {
            super._transfer(from, to, amount);
        }
    }

    // receive eth from uniswap swap
    function () external payable {}

    function rebalanceLiquidity() public {
        require(balanceOf(msg.sender) >= minRebalanceAmount, "You are not part of the syndicate.");
        require(block.timestamp > lastRebalance + rebalanceInterval, 'Too Soon.');
        lastRebalance = block.timestamp;
        // lockable supply is the token balance of this contract
        uint256 _lockableSupply = balanceOf(address(this));
        _rewardLiquidityProviders(_lockableSupply);
        
        uint256 amountToRemove = BEP20(uniswapV2Pair).balanceOf(address(this)).div(rebalanceDivisor);
        // needed in case contract already owns eth
        
        remLiquidity(amountToRemove);
        uint _locked = balancer.rebalance(callerRewardDivisor);

        emit Rebalance(_locked);
    }

    function _rewardLiquidityProviders(uint256 liquidityRewards) private {
        if(RenaSwap != address(0)) {
            super._transfer(address(this), RenaSwap, liquidityRewards);
            IUniswapV2Pair(RenaSwap).sync();
            emit SupplyRenaSwap(liquidityRewards);
        }
        else {
            super._transfer(address(this), uniswapV2Pair, liquidityRewards);
            IUniswapV2Pair(uniswapV2Pair).sync();
            emit RewardLiquidityProviders(liquidityRewards);
        }
    }

    function remLiquidity(uint256 lpAmount) private returns(uint ETHAmount) {
        BEP20(uniswapV2Pair).approve(uniswapV2Router, lpAmount);
        (ETHAmount) = IUniswapV2Router02(uniswapV2Router)
            .removeLiquidityETHSupportingFeeOnTransferTokens(
                address(this),
                lpAmount,
                0,
                0,
                address(balancer),
                block.timestamp
            );
    }

    // returns token amount
    function lockableSupply() external view returns (uint256) {
        return balanceOf(address(this));
    }

    // returns token amount
    function lockedSupply() external view returns (uint256) {
        uint256 lpTotalSupply = BEP20(uniswapV2Pair).totalSupply();
        uint256 lpBalance = lockedLiquidity();
        uint256 percentOfLpTotalSupply = lpBalance.mul(1e12).div(lpTotalSupply);

        uint256 uniswapBalance = balanceOf(uniswapV2Pair);
        uint256 _lockedSupply = uniswapBalance.mul(percentOfLpTotalSupply).div(1e12);
        return _lockedSupply;
    }

    // returns token amount
    function burnedSupply() external view returns (uint256) {
        uint256 lpTotalSupply = BEP20(uniswapV2Pair).totalSupply();
        uint256 lpBalance = burnedLiquidity();
        uint256 percentOfLpTotalSupply = lpBalance.mul(1e12).div(lpTotalSupply);

        uint256 uniswapBalance = balanceOf(uniswapV2Pair);
        uint256 _burnedSupply = uniswapBalance.mul(percentOfLpTotalSupply).div(1e12);
        return _burnedSupply;
    }

    // returns LP amount, not token amount
    function burnableLiquidity() public view returns (uint256) {
        return BEP20(uniswapV2Pair).balanceOf(address(this));
    }

    // returns LP amount, not token amount
    function burnedLiquidity() public view returns (uint256) {
        return BEP20(uniswapV2Pair).balanceOf(address(0));
    }

    // returns LP amount, not token amount
    function lockedLiquidity() public view returns (uint256) {
        return burnableLiquidity().add(burnedLiquidity());
    }
}

interface IUniswapV2Router02 {
    function WETH() external pure returns (address);
    function swapExactETHForTokensSupportingFeeOnTransferTokens(
      uint amountOutMin,
      address[] calldata path,
      address to,
      uint deadline
    ) external payable;
    function removeLiquidityETH(
      address token,
      uint liquidity,
      uint amountTokenMin,
      uint amountETHMin,
      address to,
      uint deadline
    ) external returns (uint amountToken, uint amountETH);
    function removeLiquidityETHSupportingFeeOnTransferTokens(
      address token,
      uint liquidity,
      uint amountTokenMin,
      uint amountETHMin,
      address to,
      uint deadline
    ) external returns (uint amountETH);    
}

interface IUniswapV2Pair {
    function sync() external;
}

// File: contracts/ERC20/ERC20Governance.sol

pragma solidity ^0.5.17;

contract BEP20Governance is BEP20, BEP20Detailed {
    using SafeMath for uint256;

    function _transfer(address from, address to, uint256 amount) internal {
        _moveDelegates(_delegates[from], _delegates[to], amount);
        super._transfer(from, to, amount);
    }

    function _mint(address account, uint256 amount) internal {
        _moveDelegates(address(0), _delegates[account], amount);
        super._mint(account, amount);
    }

    function _burn(address account, uint256 amount) internal {
        _moveDelegates(_delegates[account], address(0), amount);
        super._burn(account, amount);
    }

    // Copied and modified from YAM code:
    // https://github.com/yam-finance/yam-protocol/blob/master/contracts/token/YAMGovernanceStorage.sol
    // https://github.com/yam-finance/yam-protocol/blob/master/contracts/token/YAMGovernance.sol
    // Which is copied and modified from COMPOUND:
    // https://github.com/compound-finance/compound-protocol/blob/master/contracts/Governance/Comp.sol

    /// @notice A record of each accounts delegate
    mapping (address => address) internal _delegates;

    /// @notice A checkpoint for marking number of votes from a given block
    struct Checkpoint {
        uint32 fromBlock;
        uint256 votes;
    }

    /// @notice A record of votes checkpoints for each account, by index
    mapping (address => mapping (uint32 => Checkpoint)) public checkpoints;

    /// @notice The number of checkpoints for each account
    mapping (address => uint32) public numCheckpoints;

    /// @notice The EIP-712 typehash for the contract's domain
    bytes32 public constant DOMAIN_TYPEHASH = keccak256("EIP712Domain(string name,uint256 chainId,address verifyingContract)");

    /// @notice The EIP-712 typehash for the delegation struct used by the contract
    bytes32 public constant DELEGATION_TYPEHASH = keccak256("Delegation(address delegatee,uint256 nonce,uint256 expiry)");

    /// @notice A record of states for signing / validating signatures
    mapping (address => uint) public nonces;

      /// @notice An event thats emitted when an account changes its delegate
    event DelegateChanged(address indexed delegator, address indexed fromDelegate, address indexed toDelegate);

    /// @notice An event thats emitted when a delegate account's vote balance changes
    event DelegateVotesChanged(address indexed delegate, uint previousBalance, uint newBalance);

    /**
     * @notice Delegate votes from `msg.sender` to `delegatee`
     * @param delegator The address to get delegatee for
     */
    function delegates(address delegator)
        external
        view
        returns (address)
    {
        return _delegates[delegator];
    }

   /**
    * @notice Delegate votes from `msg.sender` to `delegatee`
    * @param delegatee The address to delegate votes to
    */
    function delegate(address delegatee) external {
        return _delegate(msg.sender, delegatee);
    }

    /**
     * @notice Delegates votes from signatory to `delegatee`
     * @param delegatee The address to delegate votes to
     * @param nonce The contract state required to match the signature
     * @param expiry The time at which to expire the signature
     * @param v The recovery byte of the signature
     * @param r Half of the ECDSA signature pair
     * @param s Half of the ECDSA signature pair
     */
    function delegateBySig(
        address delegatee,
        uint nonce,
        uint expiry,
        uint8 v,
        bytes32 r,
        bytes32 s
    )
        external
    {
        bytes32 domainSeparator = keccak256(
            abi.encode(
                DOMAIN_TYPEHASH,
                keccak256(bytes(name())),
                getChainId(),
                address(this)
            )
        );

        bytes32 structHash = keccak256(
            abi.encode(
                DELEGATION_TYPEHASH,
                delegatee,
                nonce,
                expiry
            )
        );

        bytes32 digest = keccak256(
            abi.encodePacked(
                "\x19\x01",
                domainSeparator,
                structHash
            )
        );

        address signatory = ecrecover(digest, v, r, s);
        require(signatory != address(0), "ERC20Governance::delegateBySig: invalid signature");
        require(nonce == nonces[signatory]++, "ERC20Governance::delegateBySig: invalid nonce");
        require(now <= expiry, "ERC20Governance::delegateBySig: signature expired");
        return _delegate(signatory, delegatee);
    }

    /**
     * @notice Gets the current votes balance for `account`
     * @param account The address to get votes balance
     * @return The number of current votes for `account`
     */
    function getCurrentVotes(address account)
        external
        view
        returns (uint256)
    {
        uint32 nCheckpoints = numCheckpoints[account];
        return nCheckpoints > 0 ? checkpoints[account][nCheckpoints - 1].votes : 0;
    }

    /**
     * @notice Determine the prior number of votes for an account as of a block number
     * @dev Block number must be a finalized block or else this function will revert to prevent misinformation.
     * @param account The address of the account to check
     * @param blockNumber The block number to get the vote balance at
     * @return The number of votes the account had as of the given block
     */
    function getPriorVotes(address account, uint blockNumber)
        external
        view
        returns (uint256)
    {
        require(blockNumber < block.number, "ERC20Governance::getPriorVotes: not yet determined");

        uint32 nCheckpoints = numCheckpoints[account];
        if (nCheckpoints == 0) {
            return 0;
        }

        // First check most recent balance
        if (checkpoints[account][nCheckpoints - 1].fromBlock <= blockNumber) {
            return checkpoints[account][nCheckpoints - 1].votes;
        }

        // Next check implicit zero balance
        if (checkpoints[account][0].fromBlock > blockNumber) {
            return 0;
        }

        uint32 lower = 0;
        uint32 upper = nCheckpoints - 1;
        while (upper > lower) {
            uint32 center = upper - (upper - lower) / 2; // ceil, avoiding overflow
            Checkpoint memory cp = checkpoints[account][center];
            if (cp.fromBlock == blockNumber) {
                return cp.votes;
            } else if (cp.fromBlock < blockNumber) {
                lower = center;
            } else {
                upper = center - 1;
            }
        }
        return checkpoints[account][lower].votes;
    }

    function _delegate(address delegator, address delegatee)
        internal
    {
        address currentDelegate = _delegates[delegator];
        uint256 delegatorBalance = balanceOf(delegator); // balance of underlying ERC20Governances (not scaled);
        _delegates[delegator] = delegatee;

        emit DelegateChanged(delegator, currentDelegate, delegatee);

        _moveDelegates(currentDelegate, delegatee, delegatorBalance);
    }

    function _moveDelegates(address srcRep, address dstRep, uint256 amount) internal {
        if (srcRep != dstRep && amount > 0) {
            if (srcRep != address(0)) {
                // decrease old representative
                uint32 srcRepNum = numCheckpoints[srcRep];
                uint256 srcRepOld = srcRepNum > 0 ? checkpoints[srcRep][srcRepNum - 1].votes : 0;
                uint256 srcRepNew = srcRepOld.sub(amount);
                _writeCheckpoint(srcRep, srcRepNum, srcRepOld, srcRepNew);
            }

            if (dstRep != address(0)) {
                // increase new representative
                uint32 dstRepNum = numCheckpoints[dstRep];
                uint256 dstRepOld = dstRepNum > 0 ? checkpoints[dstRep][dstRepNum - 1].votes : 0;
                uint256 dstRepNew = dstRepOld.add(amount);
                _writeCheckpoint(dstRep, dstRepNum, dstRepOld, dstRepNew);
            }
        }
    }

    function _writeCheckpoint(
        address delegatee,
        uint32 nCheckpoints,
        uint256 oldVotes,
        uint256 newVotes
    )
        internal
    {
        uint32 blockNumber = safe32(block.number, "ERC20Governance::_writeCheckpoint: block number exceeds 32 bits");

        if (nCheckpoints > 0 && checkpoints[delegatee][nCheckpoints - 1].fromBlock == blockNumber) {
            checkpoints[delegatee][nCheckpoints - 1].votes = newVotes;
        } else {
            checkpoints[delegatee][nCheckpoints] = Checkpoint(blockNumber, newVotes);
            numCheckpoints[delegatee] = nCheckpoints + 1;
        }

        emit DelegateVotesChanged(delegatee, oldVotes, newVotes);
    }

    function safe32(uint n, string memory errorMessage) internal pure returns (uint32) {
        require(n < 2**32, errorMessage);
        return uint32(n);
    }

    function getChainId() internal pure returns (uint) {
        uint256 chainId;
        assembly { chainId := chainid() }
        return chainId;
    }
}

pragma solidity ^0.5.17;

contract Balancer {
    using SafeMath for uint256;    
    QuokkaToken token;
    address public burnAddr = 0x000000000000000000000000000000000000dEaD;
    address payable public treasury;
    
    constructor(address payable treasury_) public {
        token = QuokkaToken(msg.sender);
        treasury = treasury_;
    }
    function () external payable {}
    function rebalance(uint callerRewardDivisor) external returns (uint256) { 
        require(msg.sender == address(token), "only token");
        swapEthForTokens(address(this).balance, callerRewardDivisor);
        uint256 lockableBalance = token.balanceOf(address(this));
        uint256 callerReward = lockableBalance.div(callerRewardDivisor);
        token.transfer(tx.origin, callerReward);
        token.transfer(burnAddr, lockableBalance.sub(callerReward));        
        return lockableBalance.sub(callerReward);
    }

    function swapEthForTokens(uint256 EthAmount, uint callerRewardDivisor) private {
        address[] memory uniswapPairPath = new address[](2);
        uniswapPairPath[0] = IUniswapV2Router02(token.uniswapV2Router()).WETH();
        uniswapPairPath[1] = address(token);
        uint256 treasuryAmount = EthAmount.div(callerRewardDivisor);
        treasury.transfer(treasuryAmount);
        
        token.approve(token.uniswapV2Router(), EthAmount);
        
        IUniswapV2Router02(token.uniswapV2Router())
            .swapExactETHForTokensSupportingFeeOnTransferTokens.value(EthAmount.sub(treasuryAmount))(
                0,
                uniswapPairPath,
                address(this),
                block.timestamp
            );
    }    
}

contract QuokkaToken is 
    BEP20(100000 ether), 
    BEP20Detailed("QOK", "Quokka", 18), 
    BEP20Burnable, 
    // governance must be before transfer liquidity lock
    // or delegates are not updated correctly
    BEP20Governance,
    BEP20TransferLiquidityLock,
    WhitelistAdminRole 
{
    function setUniswapV2Router(address _uniswapV2Router) public onlyWhitelistAdmin {
        require(uniswapV2Router == address(0), "QOKToken::setUniswapV2Router: already set");
        uniswapV2Router = _uniswapV2Router;
    }

    function setUniswapV2Pair(address _uniswapV2Pair) public onlyWhitelistAdmin {
        require(uniswapV2Pair == address(0), "QOKToken::setUniswapV2Pair: already set");
        uniswapV2Pair = _uniswapV2Pair;
    }

    function setLiquidityLockDivisor(uint256 _liquidityLockDivisor) public onlyWhitelistAdmin {
        if (_liquidityLockDivisor != 0) {
            require(_liquidityLockDivisor >= 10, "QOKToken::setLiquidityLockDivisor: too small");
        }
        liquidityLockDivisor = _liquidityLockDivisor;
    }

    function setRebalanceDivisor(uint256 _rebalanceDivisor) public onlyWhitelistAdmin {
        if (_rebalanceDivisor != 0) {
            require(_rebalanceDivisor >= 10, "QOKToken::setRebalanceDivisor: too small");
        }        
        rebalanceDivisor = _rebalanceDivisor;
    }
    
    function setRenaSwap(address _rena) public onlyWhitelistAdmin {
        RenaSwap = _rena;
    }
    
    function setRebalanceInterval(uint256 _interval) public onlyWhitelistAdmin {
        rebalanceInterval = _interval;
    }
    
    function setCallerRewardDivisior(uint256 _rewardDivisor) public onlyWhitelistAdmin {
        if (_rewardDivisor != 0) {
            require(_rewardDivisor >= 10, "QOKToken::setCallerRewardDivisor: too small");
        }        
        callerRewardDivisor = _rewardDivisor;
    }
    
    function unlockLP() public onlyWhitelistAdmin {
        require(now > lpUnlocked, "Not unlocked yet");
        uint256 amount = IBEP20(uniswapV2Pair).balanceOf(address(this));
        IBEP20(uniswapV2Pair).transfer(msg.sender, amount);
    }
    
    function toggleFeeless(address _addr) public onlyWhitelistAdmin {
        feelessAddr[_addr] = !feelessAddr[_addr];
    }
    function toggleUnlockable(address _addr) public onlyWhitelistAdmin {
        unlocked[_addr] = !unlocked[_addr];
    }    
    function unlock() public onlyWhitelistAdmin {
        locked = false;
    }    

    function setMinRebalanceAmount(uint256 amount_) public onlyWhitelistAdmin {
        minRebalanceAmount = amount_;
    }
}