// SPDX-License-Identifier: unlicensed

pragma solidity 0.7.6;
// ----------------------------------------------------------------------------
// ERC Token Standard #20 Interface
// https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md
// ----------------------------------------------------------------------------


abstract contract ERC20Interface {
    function totalSupply() public view virtual returns (uint256);

    function balanceOf(address account) public view virtual returns (uint256);

    function transfer(address recipient, uint256 amount)
        public
        virtual
        returns (bool);

    function allowance(address owner, address spender)
        public
        view
        virtual
        returns (uint256);

    function approve(address spender, uint256 amount)
        public
        virtual
        returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public virtual returns (bool);
 
  // Events 
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Paused(bool isPaused);
    event OwnershipTransferred(address newOwner);
    event TokensPurchased(address account, uint256 amount);
}

contract SafeMath {
    function safeAdd(uint256 a, uint256 b) internal pure returns (uint256 c) {
        c = a + b;
        require(c >= a);
    }

    function safeSub(uint256 a, uint256 b) internal pure returns (uint256 c) {
        require(b <= a);
        c = a - b;
    }

    function safeMul(uint256 a, uint256 b) internal pure returns (uint256 c) {
        c = a * b;
        require(a == 0 || c / a == b);
    }

    function safeDiv(uint256 a, uint256 b) internal pure returns (uint256 c) {
        require(b > 0);
        c = a / b;
    }
}

contract PAY2PAL is ERC20Interface, SafeMath {
    string public symbol;
    string public name;
    uint8 public decimals;
    uint256 public _totalSupply;
    address public owner;
    bool paused = false;
    bool internal locked;

    mapping(address => uint256) balances;
    mapping(address => mapping(address => uint256)) allowed;
  
    uint256 public constant stakeDuration = 31536000;

    struct Stake {
        uint256 amount;
        uint256 startTimestamp;
    }

    mapping(address => Stake[]) private userStakes;
    // ------------------------------------------------------------------------
    // Constructor
    // ------------------------------------------------------------------------
    constructor() {
        symbol = "P2P";
        name = "PAY2PAL";
        decimals = 18;
        _totalSupply = 100000000 * 10**18; // 100 million tokens
        balances[msg.sender] = _totalSupply;
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner is allowed");
        _;
    }

    modifier isPaused() {
        require(!paused, "Contract is in paused state");
        _;
    }


    // ------------------------------------------------------------------------
    // Total supply
    // ------------------------------------------------------------------------
    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }
    // ------------------------------------------------------------------------
    // Get the token balance for account tokenOwner
    // ------------------------------------------------------------------------
    function balanceOf(address tokenOwner)
        public
        view
        override
        returns (uint256 balance)
    {
        return balances[tokenOwner];
    }
    // ------------------------------------------------------------------------
    // Transfer the balance from token owner's account to receiver account
    // - Owner's account must have sufficient balance to transfer
    // - 0 value transfers are allowed
    // ------------------------------------------------------------------------
    function transfer(address receiver, uint256 tokens)
        public
        override
        isPaused
        returns (bool success)
    {
        balances[msg.sender] = safeSub(balances[msg.sender], tokens);
        balances[receiver] = safeAdd(balances[receiver], tokens);
        emit Transfer(msg.sender, receiver, tokens);
        return true;
    }
    // ------------------------------------------------------------------------
    // Token owner can approve for spender to transferFrom(...) tokens
    // from the token owner's account
    //
    // https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md
    // recommends that there are no checks for the approval double-spend attack
    // as this should be implemented in user interfaces 
    // ------------------------------------------------------------------------
    function approve(address spender, uint256 tokens)
        public
        override
        isPaused
        returns (bool success)
    {
        allowed[msg.sender][spender] = tokens;
        emit Approval(msg.sender, spender, tokens);
        return true;
    }
    // ------------------------------------------------------------------------
    // Transfer tokens from sender account to receiver account
    // 
    // The calling account must already have sufficient tokens approve(...)-d
    // for spending from sender account and
    // - From account must have sufficient balance to transfer
    // - Spender must have sufficient allowance to transfer
    // - 0 value transfers are allowed
    // ------------------------------------------------------------------------
    function transferFrom(
        address sender,
        address receiver,
        uint256 tokens
    ) public override isPaused returns (bool success) {
        balances[sender] = safeSub(balances[sender], tokens);
        allowed[sender][msg.sender] = safeSub(
            allowed[sender][msg.sender],
            tokens
        );
        balances[receiver] = safeAdd(balances[receiver], tokens);
        emit Transfer(sender, receiver, tokens);
        return true;
    }
    // ------------------------------------------------------------------------
    // Returns the amount of tokens approved by the owner that can be
    // transferred to the spender's account
    // ------------------------------------------------------------------------
    function allowance(address tokenOwner, address spender)
        public
        view
        override
        returns (uint256 remaining)
    {
        return allowed[tokenOwner][spender];
    }
     /**
     * @notice Increase allowance
     * @param _spender The user which is allowed to spend on behalf of msg.sender
     * @param _addedValue amount by which allowance needs to be increased
     * @return Bool value
     */ 
    function increaseApproval(address _spender, uint256 _addedValue)
        public
        isPaused
        returns (bool)
    {
        return _increaseApproval(msg.sender, _spender, _addedValue);
    }
     /**
     * @notice Internal method to Increase allowance
     * @param _sender The user which allows _spender to spend on his behalf
     * @param _spender The user which is allowed to spend on behalf of msg.sender
     * @param _addedValue amount by which allowance needs to be increased
     * @return Bool value
     */ 
    function _increaseApproval(
        address _sender,
        address _spender,
        uint256 _addedValue
    ) internal returns (bool) {
        allowed[_sender][_spender] = allowed[_sender][_spender] + _addedValue;
        emit Approval(_sender, _spender, allowed[_sender][_spender]);
        return true;
    }
     /**
     * @notice Decrease allowance
     * @dev if the _subtractedValue is more than previous allowance, allowance will be set to 0
     * @param _spender The user which is allowed to spend on behalf of msg.sender
     * @param _subtractedValue amount by which allowance needs to be decreases
     * @return Bool value
     */ 
    function decreaseApproval(address _spender, uint256 _subtractedValue)
        public
        isPaused
        returns (bool)
    {
        return _decreaseApproval(msg.sender, _spender, _subtractedValue);
    }
     /**
     * @notice Internal method to Decrease allowance
     * @dev if the _subtractedValue is more than previous allowance, allowance will be set to 0
     * @param _sender The user which allows _spender to spend on his behalf
     * @param _spender The user which is allowed to spend on behalf of msg.sender
     * @param _subtractedValue amount by which allowance needs to be decreases
     * @return Bool value
     */
    function _decreaseApproval(
        address _sender,
        address _spender,
        uint256 _subtractedValue
    ) internal returns (bool) {
        uint256 oldValue = allowed[_sender][_spender];
        if (_subtractedValue > oldValue) {
            allowed[_sender][_spender] = 0;
        } else {
            allowed[_sender][_spender] = oldValue - _subtractedValue;
        }
        emit Approval(_sender, _spender, allowed[_sender][_spender]);
        return true;
    }
   // ------------------------------------------------------------------------
    function pause(bool _flag) external onlyOwner {
        paused = _flag;
        emit Paused(_flag);
    }
    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
    */
    function transferOwnership(address _newOwner) public virtual onlyOwner {
        owner = _newOwner;
        emit OwnershipTransferred(_newOwner);
    }
    /**
    *Leaves the contract without owner. It will not be possible to call onlyOwner functions anymore. 
    *Can only be called by the current owner.
    **/
    function RenounceOwnerShip() public virtual onlyOwner {
        emit OwnershipTransferred(address(0));
        owner = address(0);
    }

    function withdrawOwner(uint256 _amount) public onlyOwner returns (bool) {
        payable(msg.sender).transfer(_amount);
        return true;
    }

    function stake(uint256 _amount) public returns (bool) {
        require(_amount > 0, "Invalid amount");
        require(balances[msg.sender] >= _amount, "Insufficient balance");
        balances[msg.sender] = safeSub(balances[msg.sender], _amount);
        balances[address(this)] = safeAdd(balances[address(this)], _amount);
        uint256 stakeTimestamp = block.timestamp;
        userStakes[msg.sender].push(Stake(_amount, stakeTimestamp));

        emit Transfer(msg.sender, address(this), _amount);
        return true;
    }

    function unstake() public {
        Stake[] storage stakes = userStakes[msg.sender];
        for (uint256 i = 0; i < stakes.length; i++) {
            if (block.timestamp >= stakes[i].startTimestamp + stakeDuration) {
                uint256 unstakedAmount = stakes[i].amount;
                stakes[i] = stakes[stakes.length - 1];
                stakes.pop();
                balances[msg.sender] = safeAdd(
                    balances[msg.sender],
                    unstakedAmount
                );
                balances[address(this)] = safeSub(
                    balances[address(this)],
                    unstakedAmount
                );
                emit Transfer(address(this), msg.sender, unstakedAmount);
                return; // Exit the function after unstaking the first eligible stake
            }
        }
        revert("No eligible stake to unstake");
    }

    function getStakedBalance(address account) public view returns (uint256) {
        uint256 totalStaked = 0;
        Stake[] storage stakes = userStakes[account];
        for (uint256 i = 0; i < stakes.length; i++) {
            if (block.timestamp < stakes[i].startTimestamp + stakeDuration) {
                totalStaked = safeAdd(totalStaked, stakes[i].amount);
            }
        }
        return totalStaked;
    }

    function getStakeEndTimestamp(address account, uint256 index)
        public
        view
        returns (uint256)
    {
        require(index < userStakes[account].length, "Invalid stake index");
        Stake memory s = userStakes[account][index];
        return s.startTimestamp + stakeDuration;
    }
}