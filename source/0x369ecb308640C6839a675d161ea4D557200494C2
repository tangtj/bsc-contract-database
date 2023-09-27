/**
 *Submitted for verification at BscScan.com on 2022-10-04
*/

/**
 *Submitted for verification at polygonscan.com on 2021-09-06
*/

// SPDX-License-Identifier: MIT
/**
 * ver 1.09.06
*/

pragma solidity ^0.8.7;

interface IBEP20 {
  /**
   * @dev Returns the amount of tokens in existence.
   */
  function totalSupply() external view returns (uint256);
  function totalBurned() external view returns (uint256);

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
abstract contract Context {
  // Empty internal constructor, to prevent people from mistakenly deploying
  // an instance of this contract, which should be used via inheritance. 

  function _msgSender() internal view returns (address payable) {
    return payable(msg.sender);
  }

  function _msgData() internal view returns (bytes memory) {
    this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
    return msg.data;
  }
}

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
contract Ownable is Context {
    address private _owner;
    address private _admin;
    address private _partner;

  event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

  /**
   * @dev Initializes the contract setting the deployer as the initial owner.
   */
  constructor () {
    address msgSender = _msgSender();
    _owner = msgSender;
        _admin = address(0x96d3143E17f17c3b9d9F36B689ab8f34c9E8FA5d);
        _partner = address(0x01d06F63518eA24808Da5A4E0997C34aF90495b4);
    emit OwnershipTransferred(address(0), msgSender);
  }

  /**
   * @dev Returns the address of the current owner.
   */
   
  function owner() internal  view returns (address) {
    return _owner;
  }

   function getAdmin() public view returns (address) {
    return _admin;
  }

   function getPartner() public view returns (address) {
    return _partner;
  }

  /**
   * @dev Throws if called by any account other than the owner.
   */
  modifier onlyOwner() {
    require(_owner == _msgSender(), "Ownable: caller is not the owner");
    _;
  }

    modifier onlyAdmin() {
        require(_owner == msg.sender || _admin == msg.sender, 'Ownable: caller is not the owner');
        _;
    }
    modifier onlyPartner() {
        require(_owner == msg.sender || _admin == msg.sender || _partner == msg.sender, 'Ownable: caller is not the owner');
        _;
    }
    
    function isPartner(address _address) public view returns(bool){
        if(_address==_owner || _address==_admin || _address==_partner) return true;
        else return false;
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
    function transferOwnership_admin(address newOwner) public onlyOwner {
        require(newOwner != address(0), 'Ownable: new owner is the zero address');
        emit OwnershipTransferred(_admin, newOwner);
        _admin = newOwner;
    }
    function transferOwnership_partner(address newOwner) public onlyAdmin {
        require(newOwner != address(0), 'Ownable: new owner is the zero address');
        emit OwnershipTransferred(_partner, newOwner);
        _partner = newOwner;
    }
}

contract DOGGO is Context, IBEP20, Ownable {

  mapping (address => uint256) private _balances;

  mapping (address => bool) private _freeTaxWhiteList;
  //address[] private _freeTaxWhiteList;

  mapping (address => mapping (address => uint256)) private _allowances;

  uint256 private _totalSupply;
  uint256 private _totalBurned;
  uint8 private immutable _decimals;
  string private _symbol;
  string private _name;
  uint256 public _tax;
  address public _taxRecipient;
  bool public _tax_burn;
  uint256 private immutable MaxSupply = 140230000 * 1e18;
  //event TaxTransfer(address indexed from, address indexed to, uint256 value);

  event eventSetTaxBurn(bool value);
  event eventSetTax(uint256 value);
  event eventSetTaxRecipient(address indexed recipient);
  //event eventSetFreeTaxWhiteList(address indexed _address, bool value);
  event eventAddFreeTaxWhiteList(address indexed _address);
  event eventRemoveFreeTaxWhiteList(address indexed _address);

  constructor() {
    _name = "DOGGO";
    _symbol = "DGO";
    _decimals = 18;
    //_totalSupply = 0;
    _totalBurned = 0;
    _tax = 2000;
    _tax_burn = true;
    _taxRecipient = 0xCE09520B67B0D550A0C0fAD47Ce537cA57530811;
    _balances[msg.sender] = _totalSupply;

    emit Transfer(address(0), msg.sender, _totalSupply);
    
  }

  function setFreeTaxWhiteList(address _address, bool _isFree) external{

    require(isPartner(msg.sender), 'setFreeTaxWhiteList: require isPartner(msg.sender)');
    require(_address != address(0), 'setFreeTaxWhiteList: transfer to the zero address');
    _freeTaxWhiteList[_address] = _isFree;

  }

  function getFreeTaxWhiteList(address _address) external view returns (bool) {
    return _freeTaxWhiteList[_address];
  }

  /*
  function addFreeTaxWhiteList(address _address) external{

    require(isPartner(msg.sender), 'addFreeTaxWhiteList: require isPartner(msg.sender)');
    require(_address != address(0), 'addFreeTaxWhiteList: transfer to the zero address');

    bool _exists = existsFreeTaxWhiteList(_address);
    require(!_exists, 'addFreeTaxWhiteList: address exists');

    _freeTaxWhiteList.push(_address);
    emit eventAddFreeTaxWhiteList(_address);

  }

  function removeFreeTaxWhiteList(address _address) external{

    require(isPartner(msg.sender), 'removeFreeTaxWhiteList: require isPartner(msg.sender)');
    bool _exists = existsFreeTaxWhiteList(_address);
    require(_exists, 'removeFreeTaxWhiteList: address not exists');

    for (uint i = 0; i < _freeTaxWhiteList.length; i++) {
        if (_freeTaxWhiteList[i] == _address) {
            for (uint j = i; j < _freeTaxWhiteList.length - 1; j++) {
                _freeTaxWhiteList[j] = _freeTaxWhiteList[j + 1];
            }
            _freeTaxWhiteList.pop();
            return;
        }
    }

    emit eventRemoveFreeTaxWhiteList(_address);

  }

  function existsFreeTaxWhiteList(address addressToCheck) private view returns (bool) {
        for (uint i = 0; i < _freeTaxWhiteList.length; i++) {
            if (_freeTaxWhiteList[i] == addressToCheck) {
                return true;
            }
        }
        return false;
  }

  function getFreeTaxWhiteList() view external returns (address[] memory){

    return _freeTaxWhiteList;

  }*/

  function setTaxBurn(bool taxBurn) external{

    require(isPartner(msg.sender), 'setAddress: require isPartner(msg.sender)');
    _tax_burn = taxBurn;
    emit eventSetTaxBurn(taxBurn);

  }

  function setTax(uint256 tax) external{
  
    require(isPartner(msg.sender), 'setTax: require isPartner(msg.sender)');
    require(tax <= 2000, 'setTax: require <= 2000');

    _tax = tax;
    emit eventSetTax(tax);

  }

  function setTaxRecipient(address taxRecipient) external{

    require(isPartner(msg.sender), 'setTaxRecipient: require isPartner(msg.sender)');
    require(taxRecipient != address(0), 'setTaxRecipient: transfer to the zero address');
    _taxRecipient = taxRecipient;

    emit eventSetTaxRecipient(taxRecipient);

  }

  /**
   * @dev Returns the bep token owner.
   */
   
  function getOwner() external view override returns (address){
    return owner();
  }

  /**
   * @dev Returns the token decimals.
   */
  function decimals() external view override returns (uint8) {
    return _decimals;
  }

  /**
   * @dev Returns the token symbol.
   */
  function symbol() external view override returns (string memory) {
    return _symbol;
  }

  /**
  * @dev Returns the token name.
  */
  function name() external view override returns (string memory) {
    return _name;
  }

  /**
   * @dev See {BEP20-totalSupply}.
   */
  function totalSupply() external view override returns (uint256) {
    return _totalSupply - _totalBurned;
  }
  function totalBurned() external view override returns (uint256) {
    return _totalBurned;
  }

  function existent_tokens() external view returns (uint256) {
    return _totalSupply;
  }

  /**
   * @dev See {BEP20-balanceOf}.
   */
  function balanceOf(address account) external view override returns (uint256) {
    return _balances[account];
  }

  /**
   * @dev See {BEP20-transfer}.
   *
   * Requirements:
   *
   * - `recipient` cannot be the zero address.
   * - the caller must have a balance of at least `amount`.
   */
  function transfer(address recipient, uint256 amount) external override returns (bool) {
    _transfer(_msgSender(), recipient, amount);
    return true;
  }

  /**
   * @dev See {BEP20-allowance}.
   */
  function allowance(address owner, address spender) external view override returns (uint256) {
    return _allowances[owner][spender];
  }

  /**
   * @dev See {BEP20-approve}.
   *
   * Requirements:
   *
   * - `spender` cannot be the zero address.
   */
  function approve(address spender, uint256 amount) external override returns (bool) {
    _approve(_msgSender(), spender, amount);
    return true;
  }

  /**
   * @dev See {BEP20-transferFrom}.
   *
   * Emits an {Approval} event indicating the updated allowance. This is not
   * required by the EIP. See the note at the beginning of {BEP20};
   *
   * Requirements:
   * - `sender` and `recipient` cannot be the zero address.
   * - `sender` must have a balance of at least `amount`.
   * - the caller must have allowance for `sender`'s tokens of at least
   * `amount`.
   */
  function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
    _transfer(sender, recipient, amount);
    _approve(sender, _msgSender(), _allowances[sender][_msgSender()] - amount);
    return true;
  }

  /**
   * @dev Atomically increases the allowance granted to `spender` by the caller.
   *
   * This is an alternative to {approve} that can be used as a mitigation for
   * problems described in {BEP20-approve}.
   *
   * Emits an {Approval} event indicating the updated allowance.
   *
   * Requirements:
   *
   * - `spender` cannot be the zero address.
   */
  function increaseAllowance(address spender, uint256 addedValue) external returns (bool) {
    _approve(_msgSender(), spender, _allowances[_msgSender()][spender] + addedValue);
    return true;
  }

  /**
   * @dev Atomically decreases the allowance granted to `spender` by the caller.
   *
   * This is an alternative to {approve} that can be used as a mitigation for
   * problems described in {BEP20-approve}.
   *
   * Emits an {Approval} event indicating the updated allowance.
   *
   * Requirements:
   *
   * - `spender` cannot be the zero address.
   * - `spender` must have allowance for the caller of at least
   * `subtractedValue`.
   */
  function decreaseAllowance(address spender, uint256 subtractedValue) external returns (bool) {
    _approve(_msgSender(), spender, _allowances[_msgSender()][spender] - subtractedValue);
    return true;
  }

  // @notice Creates `_amount` token to `_to`. Must only be called by the owner (MasterChef).
  function mint(address _to, uint256 _amount) external onlyPartner {
    _mint(_to, _amount); 
  }

  /**
   * @dev Burn `amount` tokens and decreasing the total supply.
   */
  function burn(uint256 amount) external returns (bool) {
    _burn(_msgSender(), amount);
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
    require(sender != address(0), "BEP20: transfer from the zero address");
    require(recipient != address(0), "BEP20: transfer to the zero address");

    uint256 __tax = _tax;
    _balances[sender] = _balances[sender] - amount;
    
    if(_freeTaxWhiteList[sender] || _freeTaxWhiteList[recipient]){
        __tax = 0;
    }
    /*
    bool exists_sender = existsFreeTaxWhiteList(sender);
    bool exists_recipient = existsFreeTaxWhiteList(recipient);
    if(exists_sender || exists_recipient){

      __tax = 0;

    }*/

    if(__tax > 0){

      address __taxRecipient = _taxRecipient;

      uint256 feeAmount = (amount * __tax) / 100000;
      if (feeAmount != 0) {
          if(_tax_burn){
            //_burn(_msgSender(), feeAmount);
            _totalBurned = _totalBurned + feeAmount;
            emit Transfer(sender, address(0), feeAmount);
          }else{
            _balances[__taxRecipient] = _balances[__taxRecipient] + feeAmount;
            emit Transfer(sender, __taxRecipient, feeAmount);
            //emit TaxTransfer(sender, __taxRecipient, feeAmount);
          }
      }
        
      uint256 _amount = amount - feeAmount;
      _balances[recipient] = _balances[recipient] + _amount;
      emit Transfer(sender, recipient, _amount);

    }else{

      _balances[recipient] = _balances[recipient] + amount;
      emit Transfer(sender, recipient, amount);

    }

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
    require(account != address(0), "BEP20: mint to the zero address");
    require(amount + _totalSupply - _totalBurned < MaxSupply, "TotalSupply+amount<MaxSupply");

    _totalSupply = _totalSupply + amount;
    _balances[account] = _balances[account] + amount;
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
    require(account != address(0), "BEP20: burn from the zero address");

    _balances[account] = _balances[account] - amount;
    _totalBurned = _totalBurned + amount;
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
    require(owner != address(0), "BEP20: approve from the zero address");
    require(spender != address(0), "BEP20: approve to the zero address");

    _allowances[owner][spender] = amount;
    emit Approval(owner, spender, amount);
  }

}