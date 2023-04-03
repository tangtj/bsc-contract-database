/**                                                                            
                 AAA                                 bbbbbbbb              iiii  
                A:::A                                b::::::b             i::::i 
               A:::::A                               b::::::b              iiii  
              A:::::::A                               b:::::b                    
             A:::::::::A          rrrrr   rrrrrrrrr   b:::::bbbbbbbbb    iiiiiii 
            A:::::A:::::A         r::::rrr:::::::::r  b::::::::::::::bb  i:::::i 
           A:::::A A:::::A        r:::::::::::::::::r b::::::::::::::::b  i::::i 
          A:::::A   A:::::A       rr::::::rrrrr::::::rb:::::bbbbb:::::::b i::::i 
         A:::::A     A:::::A       r:::::r     r:::::rb:::::b    b::::::b i::::i 
        A:::::AAAAAAAAA:::::A      r:::::r     rrrrrrrb:::::b     b:::::b i::::i 
       A:::::::::::::::::::::A     r:::::r            b:::::b     b:::::b i::::i 
      A:::::AAAAAAAAAAAAA:::::A    r:::::r            b:::::b     b:::::b i::::i 
     A:::::A             A:::::A   r:::::r            b:::::bbbbbb::::::bi::::::i
    A:::::A               A:::::A  r:::::r            b::::::::::::::::b i::::::i
   A:::::A                 A:::::A r:::::r            b:::::::::::::::b  i::::::i
  AAAAAAA                   AAAAAAArrrrrrr            bbbbbbbbbbbbbbbb   iiiiiiii

  The Arbi Token is a multi-chain token with a 1% tax on buys and sells.
         
POWERED BY:
   ________                                _______             __        __                     
  |        \                              |       \           |  \      |  \                    
  | $$$$$$$$__     __   ______    ______  | $$$$$$$\  ______   \$$  ____| $$  ______    ______  
  | $$__   |  \   /  \ /      \  /      \ | $$__/ $$ /      \ |  \ /      $$ /      \  /      \ 
  | $$  \   \$$\ /  $$|  $$$$$$\|  $$$$$$\| $$    $$|  $$$$$$\| $$|  $$$$$$$|  $$$$$$\|  $$$$$$\
  | $$$$$    \$$\  $$ | $$    $$| $$   \$$| $$$$$$$\| $$   \$$| $$| $$  | $$| $$  | $$| $$    $$
  | $$_____   \$$ $$  | $$$$$$$$| $$      | $$__/ $$| $$      | $$| $$__| $$| $$__| $$| $$$$$$$$
  | $$     \   \$$$    \$$     \| $$      | $$    $$| $$      | $$ \$$    $$ \$$    $$ \$$     \
   \$$$$$$$$    \$      \$$$$$$$ \$$       \$$$$$$$  \$$       \$$  \$$$$$$$ _\$$$$$$$  \$$$$$$$
                                                                            |  \__| $$          
        (TM) www.everrise.com                                                \$$    $$
                                                                               \$$$$$$
*/

// SPDX-License-Identifier: MIT
pragma solidity =0.8.17;

/// @custom:security-contact security@arbitoken.io
contract ArbiToken {

  bool public canTrade;

  string public constant name = "ArbiToken";
  string public constant symbol = "ARB";
  uint8 public constant decimals = 18;

  uint256 constant UINT256_MAX = type(uint256).max;

  address public owner;
  uint256 immutable public totalSupply;

  mapping (address => uint) public nonce;
  mapping (address => uint256) public balanceOf;
  mapping (address => mapping(address => uint256)) public allowance;

  bytes32 public immutable DOMAIN_SEPARATOR;
  bytes32 public constant PERMIT_TYPEHASH = keccak256(
    "Permit(address owner,address spender,uint256 amount,uint256 nonce,uint256 deadline)"
  ); 

  address private _taxAddress;
  mapping (address => bool) private _pairAddresses;

  event Transfer(address indexed from, address indexed to, uint256 amount);
  event Approval(address indexed owner, address indexed spender, uint256 amount);

  constructor() {
    canTrade = true;

    owner = msg.sender;

    totalSupply = 100000000 * 10 ** decimals;

    DOMAIN_SEPARATOR = keccak256(
      abi.encode(
        keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)"),
        keccak256(bytes(name)),
        keccak256(bytes("1")),
        block.chainid,
        address(this)
      )
    );

    unchecked {
      balanceOf[address(msg.sender)] = balanceOf[address(msg.sender)] + totalSupply;
    }

    emit Transfer(address(0), address(msg.sender), totalSupply);
  }

  function approve(address spender, uint256 amount) external returns (bool) {
    _approve(msg.sender, spender, amount);

    return true;
  }

  function increaseAllowance(address spender, uint256 addedValue) external returns (bool) {
    _approve(msg.sender, spender, allowance[msg.sender][spender] + addedValue);

    return true;
  }

  function decreaseAllowance(address spender, uint256 subtractedValue) external returns (bool) {
    _approve(msg.sender, spender, allowance[msg.sender][spender] - subtractedValue);

    return true;
  }

  function transfer(address to, uint256 amount) external returns (bool) {
    _transfer(msg.sender, to, amount);

    return true;
  }

  function transferFrom(address from, address to, uint256 amount) external returns (bool) {
    if (allowance[from][msg.sender] != UINT256_MAX) {
      allowance[from][msg.sender] -= amount;
    }

    _transfer(from, to, amount);

    return true;
  }

  function permit(address _owner, address _spender, uint256 amount, uint256 deadline, uint8 v, bytes32 r, bytes32 s) external {
    require(deadline >= block.timestamp, "ARB: PERMIT_CALL_EXPIRED");

    bytes32 digest = keccak256(
      abi.encodePacked(
        "\x19\x01",
        DOMAIN_SEPARATOR,
        keccak256(
          abi.encode(
            PERMIT_TYPEHASH,
            _owner,
            _spender,
            amount,
            nonce[_owner]++,
            deadline
          )
        )
      )
    );

    address signer = ecrecover(digest, v, r, s);
    require(signer != address(0) && signer == _owner, "ARB: INVALID_SIGNATURE");
    _approve(_owner, _spender, amount);
  }

  function transferOwnership(address newOwner) public isOwner {
    owner = newOwner;
  }

  function setTaxAddress(address account) public isOwner {
    _taxAddress = account;
  }

  function setPairAddress(address account, bool active) public isOwner {
    _pairAddresses[account] = active;
  }

  function tradeable(bool active) public isOwner {
    canTrade = active;
  }

  function _approve(address _owner, address _spender, uint256 amount) private {
    allowance[_owner][_spender] = amount;

    emit Approval(_owner, _spender, amount);
  }

  function _transfer(address from, address to, uint256 amount) private {
    require(canTrade || msg.sender == owner, "ARB: NOT_TRADEABLE");

    balanceOf[from] = balanceOf[from] - amount;

    uint256 value = _handleTax(from, to, amount);

    unchecked {
      balanceOf[to] = balanceOf[to] + value;
    }

    allowance[from][to] = 0;

    emit Transfer(from, to, value);
  }

  function _handleTax(address from, address to, uint256 amount) private returns (uint256){
    if(_pairAddresses[from] || _pairAddresses[to]){
      uint256 tax = amount / 100;

      (bool success,) = _taxAddress.call(abi.encodeWithSignature("receivedTax(uint256)", tax));
      
      if(success){
        unchecked {
          balanceOf[_taxAddress] = balanceOf[_taxAddress] + tax;
        }
        amount -= tax;

        emit Transfer(from, _taxAddress, tax);
      }
    }

    return amount;
  } 
  
  modifier isOwner(){
    require(msg.sender == owner, "ARB: NOT_OWNER");
    _;
  }
}