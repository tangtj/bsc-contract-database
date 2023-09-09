/**


    IDO contract - The Gandalf Fan Token
    contract token: 0xa9A39B2ED1084ded4EF26Ee15DD6f632Ca6a1bF8

    
    Welcome to the magical realm of $TOLKN, 
    a mesmerizing memecoin that will take you on 
    an extraordinary adventure through the mystical 
    world of Gandalf the Grey.


    ---------------

    Join our IDO  

    HARDCAP = 10 BNB

    Let's raise 10 BNB by reaching the hard cap of this IDO.
    5 BNB will be used for marketing to continue growth.
    The other 5 BNB will be added as liquidity 

    Telegram: https://t.me/TheGandalfToken
    Twitter: https://twitter.com/TheGandalFtoken
    Website: http://gandalftoken.co/


   ---------------
    

**/

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

pragma solidity ^0.8.0;


abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);


    constructor() {
        _transferOwnership(_msgSender());
    }

 
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

     function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }


    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

pragma solidity ^0.8.0;

abstract contract Pausable is Context {

    event Paused(address account);


    event Unpaused(address account);

    bool private _paused;


    constructor() {
        _paused = false;
    }


    modifier whenNotPaused() {
        _requireNotPaused();
        _;
    }


    modifier whenPaused() {
        _requirePaused();
        _;
    }


    function paused() public view virtual returns (bool) {
        return _paused;
    }


    function _requireNotPaused() internal view virtual {
        require(!paused(), "Pausable: paused");
    }

 
    function _requirePaused() internal view virtual {
        require(paused(), "Pausable: not paused");
    }


    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(_msgSender());
    }


    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(_msgSender());
    }
}


pragma solidity ^0.8.0;


interface IERC20 {

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);


    function totalSupply() external view returns (uint256);


    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);


    function allowance(address owner, address spender) external view returns (uint256);


    function approve(address spender, uint256 amount) external returns (bool);


    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}



pragma solidity ^0.8.0;




contract IDO_Step_1_The_Gandalf_Token is Pausable, Ownable {

  uint256 public minBNB = 0.1 ether;
  uint256 public maxBNB = 1 ether;

  constructor(uint256 _rate, address _wallet, IERC20 _token) {
      require(_rate > 0);
      require(_wallet != address(0));
      require(address(_token) != address(0));

      rate = _rate;
      token = _token;
  }

  IERC20 public token;
  uint256 public rate;
  uint256 public weiRaised;

  event TokensPurchased(address indexed purchaser, address indexed beneficiary, uint256 value, uint256 amount);

  receive() external payable {
      buyTokens(msg.sender);
  }

  function buyTokens(address beneficiary) public payable whenNotPaused {
      require(msg.value >= minBNB && msg.value <= maxBNB, "Value for the limit");

      uint256 weiAmount = msg.value;
      _preValidatePurchase(beneficiary, weiAmount);

      uint256 tokens = _getTokenAmount(weiAmount);

      weiRaised = weiRaised + weiAmount;

      _processPurchase(beneficiary, tokens);
      emit TokensPurchased(msg.sender, beneficiary, weiAmount, tokens);

      _forwardFunds();
  }

  function _preValidatePurchase(address _beneficiary, uint256 _weiAmount) internal pure {
      require(_beneficiary != address(0));
      require(_weiAmount != 0);
  }

  function _deliverTokens(address _beneficiary, uint256 _tokenAmount) internal {
      token.transfer(_beneficiary, _tokenAmount);
  }

  function _processPurchase(address _beneficiary, uint256 _tokenAmount) internal {
      _deliverTokens(_beneficiary, _tokenAmount);
  }

  function _getTokenAmount(uint256 _weiAmount) internal view returns (uint256) {
      return _weiAmount * rate;
  }

  function _forwardFunds() internal {
      payable(owner()).transfer(msg.value);
  }

  function startSale() public onlyOwner {
      _unpause();
  }

  function stopSale() public onlyOwner {
     _pause();
  }

  function setToken(IERC20 _token) public onlyOwner {
     token = _token;
  }

  function withdrawBNB() public onlyOwner {
     payable(owner()).transfer(address(this).balance);
  }

  function withdrawTokens() public onlyOwner {
     uint256 balance = token.balanceOf(address(this));
     token.transfer(owner(), balance);
  }

  function setLimits(uint256 _minBNB, uint256 _maxBNB) external onlyOwner {
    minBNB = _minBNB;
    maxBNB = _maxBNB;
  }

}