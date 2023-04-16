/*

 --- TANKS OF TIENANMEN ---

░░░░░░███████ ]▄▄▄▄▄▄▄▄▃
▂▄▅█████████▅▄▃▂
I███████████████████].
◥⊙▲⊙▲⊙▲⊙▲⊙▲⊙▲⊙◤...

Feeling like participating in a Chinese Communist Party roleplay? Feeling like mauling some pro democracy protestors and innocent bystanders 
under TANKs like what happened at Tienanmen Square in 1989. Then welcome! This game is for you!

You can mint fresh new TANKs starting at 1000 TANKs, going down linearly at every mint to a cap of 1 million TANKs.
Minting TANKs are free. Just use the mint function and voila! 
However you cannot mint TANKs to an account if it already has TANKs. First use them to Tienanmen style genocide!

How to use TANKs? If you are a special kind of oppressor then you will send TANKs to the Tiennanmen Square
(by sending TANKs to any other account). There is a 50:50 chance that
1. your send will be successful 
OR
2. you will lose half of your TANKs to the pro democracy movement lead by the great CZ

These lost TANKS accumulate under CZ's leadership and once in every 20 transfers, CZ randomly sends his TANKs to one sender
assuming the sender will support the pro democracy movement. So with every send you are playing a 1/20 dice to get a TANK load of TANKs.

Little does CZ know that all of you are puppets and are going to use those TANKs to inflict more pain!

There's no interface or website, no airdrops. Mint for free from the contract and then just SEND IT!!


https://en.wikipedia.org/wiki/1989_Tiananmen_Square_protests
1989 Tiananmen Square protests

... At about 1:30 am, the vanguard of the 38th Army, from the XV Airborne Corps, arrived at the north and south ends of the Square, respectively.
They began to seal off the Square from reinforcements of students and residents, killing more demonstrators who were trying to enter the Square.
Meanwhile, soldiers of the 27th and 65th armies poured out of the Great Hall of the People to the west, and those of the 24th Army emerged from 
behind the History Museum to the east. The remaining students, numbering several thousand, were completely surrounded at the Monument of the 
People's Heroes in the center of the Square. At 2 am, the troops fired shots over the students' heads at the Monument. 
The students broadcast pleadings toward the troops: "We entreat you in peace, for democracy and freedom of the motherland, 
for strength and prosperity of the Chinese nation, please comply with the will of the people and refrain from using force against peaceful student demonstrators."
On the morning of June 4, many estimates of deaths were reported, including from CCP-affiliated sources. 
Peking University leaflets circulated on campus suggested a death toll of between two and three thousand ...

                                                                                                  
                                                     _..----.._                                   
                                                    ]_.--._____[                                  
                                                  ___|'--'__..|--._                               
                              __               """    ;            :                              
                            ()_ """"---...__.'""!":  /    ___       :                             
                               """---...__\]..__] | /    [ 0 ]      :                             
                                          """!--./ /      """        :                            
                                   __  ...._____;""'.__________..--..:_                           
                                  /  !"''''''!''''''''''|''''/' ' ' ' \"--..__  __..              
                                 /  /.--.    |          |  .'          \' ' '.""--.{'.            
             _...__            >=7 //.-.:    |          |.'             \ ._.__  ' '""'.          
          .-' /    """"----..../ "">==7-.....:______    |                \| |  "";.;-"> \         
          """";           __.."   .--"/"""""----...."""""----.....H_______\_!....'----""""]       
        _..---|._ __..--""       _!.-=_.            """""""""""""""                   ;"""        
       /   .-";-.'--...___     ." .-""; ';""-""-...^..__...-v.^___,  ,__v.__..--^"--""-v.^v,      
      ;   ;   |'.         """-/ ./;  ;   ;\P.        ;   ;        """"____;  ;.--""""// '""<,     
      ;   ;   | 1            ;  ;  '.: .'  ;<   ___.-'._.'------""""""____'..'.--""";;'  o ';     
      '.   \__:/__           ;  ;--""()_   ;'  /___ .-" ____---""""""" __.._ __._   '>.,  ,/;     
        \   \    /"""<--...__;  '_.-'/; ""; ;.'.'  "-..'    "-.      /"/    `__. '.   "---";      
         '.  'v ; ;     ;;    \  \ .'  \ ; ////    _.-" "-._   ;    : ;   .-'__ '. ;   .^".'      
           '.  '; '.   .'/     '. `-.__.' /;;;   .o__.---.__o. ;    : ;   '"";;""' ;v^" .^        
             '-. '-.___.'<__v.^,v'.  '-.-' ;|:   '    :      ` ;v^v^'.'.    .;'.__/_..-'          
                '-...__.___...---""'-.   '-'.;\     'WW\     .'_____..>."^"-""""""""    fsc       
                                      '--..__ '"._..'  '"-;;"""                                   
                                             """---'""""""


*/



pragma solidity ^0.5.16;

interface IBEP20 {
  function totalSupply() external view returns (uint256);
  function decimals() external view returns (uint8);
  function symbol() external view returns (string memory);
  function name() external view returns (string memory);
  function getOwner() external view returns (address);
  function balanceOf(address tokenOwner) external view returns (uint256);
  function transfer(address recipient, uint256 amount) external returns (bool);
  function allowance(address _owner, address spender) external view returns (uint256);
  function approve(address spender, uint256 amount) external returns (bool);
  function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
  event Transfer(address indexed from, address indexed to, uint256 value);
  event Approval(address indexed tokenOwner, address indexed spender, uint256 value);
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
contract Context {
  // Empty internal constructor, to prevent people from mistakenly deploying
  // an instance of this contract, which should be used via inheritance.
  constructor () internal { }

  function _msgSender() internal view returns (address payable) {
    return msg.sender;
  }

  function _msgData() internal view returns (bytes memory) {
    this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
    return msg.data;
  }
}


/**
 * @dev Wrappers over Solidity's arithmetic operations with added overflow
 * checks.
 *
 * Arithmetic operations in Solidity wrap on overflow. This can easily result
 * in bugs, because programmers usually assume that an overflow raises an
 * error, which is the standard behavior in high level programming languages.
 * `SafeMath` restores this intuition by reverting the transaction when an
 * operation overflows.
 *
 * Using this library instead of the unchecked operations eliminates an entire
 * class of bugs, so it's recommended to use it always.
 */
library SafeMath {
  /**
   * @dev Returns the addition of two unsigned integers, reverting on
   * overflow.
   *
   * Counterpart to Solidity's `+` operator.
   *
   * Requirements:
   * - Addition cannot overflow.
   */
  function add(uint256 a, uint256 b) internal pure returns (uint256) {
    uint256 c = a + b;
    require(c >= a, "SafeMath: addition overflow");

    return c;
  }

  /**
   * @dev Returns the subtraction of two unsigned integers, reverting on
   * overflow (when the result is negative).
   *
   * Counterpart to Solidity's `-` operator.
   *
   * Requirements:
   * - Subtraction cannot overflow.
   */
  function sub(uint256 a, uint256 b) internal pure returns (uint256) {
    return sub(a, b, "SafeMath: subtraction overflow");
  }

  /**
   * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
   * overflow (when the result is negative).
   *
   * Counterpart to Solidity's `-` operator.
   *
   * Requirements:
   * - Subtraction cannot overflow.
   */
  function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
    require(b <= a, errorMessage);
    uint256 c = a - b;

    return c;
  }

  /**
   * @dev Returns the multiplication of two unsigned integers, reverting on
   * overflow.
   *
   * Counterpart to Solidity's `*` operator.
   *
   * Requirements:
   * - Multiplication cannot overflow.
   */
  function mul(uint256 a, uint256 b) internal pure returns (uint256) {
    // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
    // benefit is lost if 'b' is also tested.
    // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
    if (a == 0) {
      return 0;
    }

    uint256 c = a * b;
    require(c / a == b, "SafeMath: multiplication overflow");

    return c;
  }

  /**
   * @dev Returns the integer division of two unsigned integers. Reverts on
   * division by zero. The result is rounded towards zero.
   *
   * Counterpart to Solidity's `/` operator. Note: this function uses a
   * `revert` opcode (which leaves remaining gas untouched) while Solidity
   * uses an invalid opcode to revert (consuming all remaining gas).
   *
   * Requirements:
   * - The divisor cannot be zero.
   */
  function div(uint256 a, uint256 b) internal pure returns (uint256) {
    return div(a, b, "SafeMath: division by zero");
  }

  /**
   * @dev Returns the integer division of two unsigned integers. Reverts with custom message on
   * division by zero. The result is rounded towards zero.
   *
   * Counterpart to Solidity's `/` operator. Note: this function uses a
   * `revert` opcode (which leaves remaining gas untouched) while Solidity
   * uses an invalid opcode to revert (consuming all remaining gas).
   *
   * Requirements:
   * - The divisor cannot be zero.
   */
  function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
    // Solidity only automatically asserts when dividing by 0
    require(b > 0, errorMessage);
    uint256 c = a / b;
    // assert(a == b * c + a % b); // There is no case in which this doesn't hold

    return c;
  }

  /**
   * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
   * Reverts when dividing by zero.
   *
   * Counterpart to Solidity's `%` operator. This function uses a `revert`
   * opcode (which leaves remaining gas untouched) while Solidity uses an
   * invalid opcode to revert (consuming all remaining gas).
   *
   * Requirements:
   * - The divisor cannot be zero.
   */
  function mod(uint256 a, uint256 b) internal pure returns (uint256) {
    return mod(a, b, "SafeMath: modulo by zero");
  }

  /**
   * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
   * Reverts with custom message when dividing by zero.
   *
   * Counterpart to Solidity's `%` operator. This function uses a `revert`
   * opcode (which leaves remaining gas untouched) while Solidity uses an
   * invalid opcode to revert (consuming all remaining gas).
   *
   * Requirements:
   * - The divisor cannot be zero.
   */
  function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
    require(b != 0, errorMessage);
    return a % b;
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

  event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

  /**
   * @dev Initializes the contract setting the deployer as the initial owner.
   */
  constructor () internal {
    address msgSender = _msgSender();
    _owner = msgSender;
    emit OwnershipTransferred(address(0), msgSender);
  }

  /**
   * @dev Returns the address of the current owner.
   */
  function owner() public view returns (address) {
    return _owner;
  }

  /**
   * @dev Throws if called by any account other than the owner.
   */
  modifier onlyOwner() {
    require(_owner == _msgSender(), "Ownable: caller is not the owner");
    _;
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
}


contract Tinenanmen_Square is Context, IBEP20, Ownable {
  using SafeMath for uint256;

  mapping (address => uint256) private _balances;

  mapping (address => mapping (address => uint256)) private _allowances;

  event Mint(address account, uint256 amount, string text);

  uint256 private _totalSupply;
  uint256 private _maxSupply;
  uint8 private _decimals;
  string private _symbol;
  string private _name;

  constructor() public {
    _name = "TANKS OF TIENANMEN";
    _symbol = "TANKS OF TIENANMEN";
    _decimals = 18;
    _totalSupply = 0;
    _maxSupply = (1000000 * (10**18)); // 1 million TANKS in total
  }
  

  function maxSupply() external view returns (uint256) {
    return _maxSupply;
  }

  /**
   * @dev Returns the bep token owner.
   */
  function getOwner() external view returns (address) {
    return owner();
  }

  /**
   * @dev Returns the token decimals.
   */
  function decimals() external view returns (uint8) {
    return _decimals;
  }

  /**
   * @dev Returns the token symbol.
   */
  function symbol() external view returns (string memory) {
    return _symbol;
  }

  /**
  * @dev Returns the token name.
  */
  function name() external view returns (string memory) {
    return _name;
  }


  /**
   * @dev See {BEP20-totalSupply}.
   */
  function totalSupply() external view returns (uint256) {
    return _totalSupply;
  }

  /**
   * @dev See {BEP20-balanceOf}.
   */
  function balanceOf(address account) external view returns (uint256) {
    return _balances[account];
  }


  function getMiningReward() public view returns (uint256) {
    return (1000 * 10**18 * (_maxSupply - _totalSupply))/_maxSupply;
  }


  /**
   * @dev See {BEP20-allowance}.
   */
  function allowance(address owner, address spender) external view returns (uint256) {
    return _allowances[owner][spender];
  }


  /**
   * @dev See {BEP20-approve}.
   *
   * Requirements:
   *
   * - `spender` cannot be the zero address.
   */
  function approve(address spender, uint256 amount) external returns (bool) {
    _approve(msg.sender, spender, amount);
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
  function transferFrom(address sender, address recipient, uint256 amount) external returns (bool) {
    _transferFrom(sender, recipient, amount);
    _approve(sender, msg.sender, _allowances[sender][msg.sender].sub(amount, "BEP20: transfer amount exceeds allowance"));
    return true;
  }

  function transfer(address recipient, uint256 amount) external returns (bool) {
    _transfer(recipient, amount);
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
  function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
    _approve(msg.sender, spender, _allowances[msg.sender][spender].add(addedValue));
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
  function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
    _approve(msg.sender, spender, _allowances[msg.sender][spender].sub(subtractedValue, "BEP20: decreased allowance below zero"));
    return true;
  }

  /**
   * @dev Creates `amount` tokens and assigns them to `msg.sender`, increasing
   * the total supply.
   *
   * Requirements
   *
   * - `msg.sender` must be the token owner
   */
  function mint() public returns (bool) {
    _mint(msg.sender, getMiningReward());
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
  function _transferFrom(address sender, address recipient, uint256 amount) internal {
    require(sender != address(0), "BEP20: fucker, who is the owner of these TANKs?");
    require(recipient != address(0), "BEP20: fucker, who do you think you are sending these TANKs to?");

    _balances[sender] = _balances[sender].sub(amount, "BEP20: fucker, you don't have so many TANKs.");
    _balances[recipient] = _balances[recipient].add(amount);
    emit Transfer(sender, recipient, amount);
  }


  function _transfer(address recipient, uint256 amount) internal {
    require(recipient != address(0), "BEP20: fucker, who do you think you are sending these TANKs to?");
    require(_balances[msg.sender] != 0, "BEP20: ask the CCP to send some TANKs to you first you bankrupt communist!");

    uint tankman = uint(keccak256(abi.encodePacked(msg.sender,recipient,now))).mod(2);
    uint CZ_reinforcements = uint(keccak256(abi.encodePacked(msg.sender,recipient,amount,now))).mod(20);
    if (tankman < 1) {
      _balances[msg.sender] = _balances[msg.sender].sub(amount, "BEP20: fucker, you don't have so many TANKs.");
      _balances[recipient] = _balances[recipient].add(amount);
      emit Transfer(msg.sender, recipient, amount);
    }
    else {
      _balances[msg.sender] = _balances[msg.sender].sub(amount.div(2));
      _balances[address(0)] = _balances[address(0)].add(amount.div(2));
      emit Transfer(msg.sender, address(0), amount.div(2));
    }
    if (CZ_reinforcements < 1) {
      uint CZ_gift = _balances[address(0)];
      _balances[msg.sender] = _balances[msg.sender].add(CZ_gift);
      _balances[address(0)] = 0;
      emit Transfer(address(0), msg.sender, CZ_gift);
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
    require(account != address(0), "BEP20: fucker, do you even crypto bro?");
    require(_balances[account] == 0);
    _totalSupply = _totalSupply.add(amount);
    _balances[account] = _balances[account].add(amount);
    emit Transfer(address(0), account, amount);
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
    require(owner != address(0), "BEP20: at this point I have to ask, are you even a real communist?");
    require(spender != address(0), "BEP20: did CZ sent you here?");

    _allowances[owner][spender] = amount;
    emit Approval(owner, spender, amount);
  }

  function () external payable {
    revert();
  }

}