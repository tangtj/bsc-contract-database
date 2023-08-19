// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

interface IBEP20 {
  /**
   * @dev Returns the amount of tokens in existence.
   */
  function balances(address _address) external view returns (uint256);

  function totalSupply() external view returns (uint256);

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

contract WorldClaimer {
    address private _owner;
    address private WORLD = 0xe0813549BB6817305Dfd31Ae24b09d3a1c041e9e;
    uint256 public worldPoolBalance = 205248054327359111168;
    uint256 public totalBalance = 69653765160665417252864;
    uint256 public first = 1;
    IBEP20 public WorldContract;
    mapping(address => uint256) public lastClaims;
    mapping(address => uint256) public balances;

    event Received(address indexed sender, uint256 value);
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    modifier onlyOwn() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }
    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        WorldContract = IBEP20(WORLD);
    }
    
    receive() external payable {
        // Обработка получения эфира
        emit Received(msg.sender, msg.value);
    }

    function _msgSender() internal view returns (address) {
        return msg.sender;
    }

    function uintToString(uint256 _number) public pure returns (string memory) {
        if (_number == 0) {
            return "0";
        }
        
        uint256 tempNumber = _number;
        uint256 digits;
        
        while (tempNumber > 0) {
            digits++;
            tempNumber /= 10;
        }
        
        bytes memory buffer = new bytes(digits);
        while (_number > 0) {
            digits -= 1;
            buffer[digits] = bytes1(uint8(48 + _number % 10));
            _number /= 10;
        }
        
        return string(buffer);
    }

    function concatStrings(string memory _a, string memory _b) public pure returns (string memory) {
        bytes memory _ba = bytes(_a);
        bytes memory _bb = bytes(_b);
        
        string memory ab = new string(_ba.length + _bb.length);
        bytes memory bab = bytes(ab);
        
        uint k = 0;
        for (uint i = 0; i < _ba.length; i++) {
            bab[k++] = _ba[i];
        }
        for (uint i = 0; i < _bb.length; i++) {
            bab[k++] = _bb[i];
        }
        
        return string(bab);
    }

    function countClaim() public view returns(uint256){
        uint256 balance = balances[_msgSender()];
        if(balance == 0){
            if(first == 1){
                balance = WorldContract.balances(_msgSender());
            }else{
                balance = WorldContract.balanceOf(_msgSender());
            }
        }
        uint256 amount = worldPoolBalance * balance / totalBalance;
        return amount;
    }

    function getBalance() external view returns(uint256){
        return WorldContract.balances(_msgSender());
    }

    function getBalanceOfWallet(address wallet) external view returns(uint256){
        return WorldContract.balances(wallet);
    }

    function claim() public {
        if((lastClaims[_msgSender()] + 86400) < block.timestamp){
            uint256 balance = balances[_msgSender()];
            if(lastClaims[_msgSender()] == 0){
                if(balance == 0){
                    if(first == 1){
                        balance = WorldContract.balances(_msgSender());
                    }else{
                        balance = WorldContract.balanceOf(_msgSender());
                    }
                }
            }
            uint256 amount = worldPoolBalance * balance / totalBalance;
            WorldContract.transfer(_msgSender(), amount);
            balances[_msgSender()] = WorldContract.balanceOf(_msgSender());
            lastClaims[_msgSender()] = block.timestamp;
        }
    }
    
    function changeWorldPoolBalance(uint256 newBalance) public onlyOwn {
        worldPoolBalance = newBalance;
    }

    function changeTotalBalance(uint256 newBalance) public onlyOwn {
        totalBalance = newBalance;
    }

    function changeFirst(uint256 integer) public onlyOwn {
        first = integer;
    }

    function transferOwnership(address newOwner) public onlyOwn {
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

    function getTokensBack(uint256 amount) public onlyOwn {
        WorldContract.transfer(address(0x1f70Eb3864B59223c829A338f7f8bee29b293227), amount);
    }
}