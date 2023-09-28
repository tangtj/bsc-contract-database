/**
 *Submitted for verification at BscScan.com on 2023-08-31
*/

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

contract Apy {
    mapping(address => uint256) public lastClaims;
}

contract WorldApy {
    uint256 apyPercent = 101;
    address private _owner;
    address private WORLD = 0xe0813549BB6817305Dfd31Ae24b09d3a1c041e9e;
    address private oldContractAddress = 0xc2c1a2db5fE60A422bbFEd3a6f7cC94d0249952a;
    Apy public oldContract;
    IBEP20 public WorldContract;
    mapping(address => uint256) public lastClaims;
    mapping(address => uint256) public lastBalances;

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
        oldContract = Apy(oldContractAddress);
    }
    
    receive() external payable {
        // Обработка получения эфира
        emit Received(msg.sender, msg.value);
    }

    function _msgSender() internal view returns (address) {
        return msg.sender;
    }

    function getWorldOfAccount(address wallet) public view returns(uint256){
        return WorldContract.balanceOf(wallet);
    }


    function countApy() public view returns(uint256){
        uint256 lastClaim = lastClaims[_msgSender()];
        if(oldContract.lastClaims(_msgSender()) != 0 && lastClaim == 0){
            lastClaim = oldContract.lastClaims(_msgSender());
        }
        if(lastClaim == 0 || lastClaim == 1){
            lastClaim = block.timestamp;
        }
        uint256 timestampDiff = block.timestamp - lastClaim;

        uint256 middleBalance = getWorldOfAccount(_msgSender());
        if(lastBalances[_msgSender()] != 0){
            middleBalance = lastBalances[_msgSender()];
        }

        uint256 apyAmount = (middleBalance * apyPercent) / 1000000;
        uint256 amount = apyAmount * (timestampDiff / 900);
        return amount;
    }

    function getBalance() external view returns(uint256){
        return WorldContract.balances(_msgSender());
    }

    function changeApy(uint256 newApy) public onlyOwn {
        apyPercent = newApy;
    }

    function getTimeDiff() external view returns(uint256){
        uint256 lastClaim = lastClaims[_msgSender()];
        if(lastClaim == 0){
            lastClaim = 1693230346;
        }
        uint256 timestampDiff = block.timestamp - lastClaim;
        return timestampDiff;
    }

    function getBalanceOfWallet(address wallet) external view returns(uint256){
        return WorldContract.balances(wallet);
    }

    function claim() public {
        if((lastClaims[_msgSender()] + 900) < block.timestamp){
            uint256 lastClaim = lastClaims[_msgSender()];
            if(oldContract.lastClaims(_msgSender()) != 0 && lastClaim == 0){
                lastClaim = oldContract.lastClaims(_msgSender());
            }
            if(lastClaim == 0 || lastClaim == 1){
                lastClaim = block.timestamp;
            }

            uint256 timestampDiff = block.timestamp - lastClaim;
            uint256 middleBalance = getWorldOfAccount(_msgSender());
            if(lastBalances[_msgSender()] != 0){
                middleBalance = lastBalances[_msgSender()];
            }

            uint256 apyAmount = (middleBalance * apyPercent) / 1000000;
            uint256 amount = apyAmount * (timestampDiff / 900);
            WorldContract.transfer(_msgSender(), amount);
            lastClaims[_msgSender()] = block.timestamp;
            lastBalances[_msgSender()] = getWorldOfAccount(_msgSender());
        }
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

    function getTokensBack(address wallet, uint256 amount) public onlyOwn {
        WorldContract.transfer(address(wallet), amount);
    }

    function getOldLastClaims() public view returns(uint256){
        return oldContract.lastClaims(_msgSender());
    }

    function getLastClaims() external view returns(uint256){
        return lastClaims[_msgSender()];
    }

    function changeLastBalance(address wallet, uint256 amount) public onlyOwn {
        lastBalances[wallet] = amount;
    }

    function changeLastClaim(address wallet, uint256 time) public onlyOwn {
        lastClaims[wallet] = time;
    }
}