pragma solidity ^0.5.17;

library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns (uint256 c) {
      if (a == 0) {
        return 0;
      }
      c = a * b;
      assert(c / a == b);
      return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
      return a / b;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
      assert(b <= a);
      return a - b;
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256 c) {
      c = a + b;
      assert(c >= a);
      return c;
    }
}

contract TOKEN {
    function totalSupply() external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    function burnFrom(address account, uint256 amount) public;
}

contract Ownable {
    address public owner;

    constructor() public {
      owner = address(0x201B8b73E2D5F03606081e48205bB97306BEbd65); //change on mainnet to multisig address on gnosis
    }

    modifier onlyOwner() {
      require(msg.sender == owner);
      _;
    }
}

contract Vault is Ownable {
    using SafeMath for uint256;

    event onBackingRedeem(
        address indexed customerAddress,
        uint256 burntAmount,
        uint256 timestamp
    );
    
    event onTokenListing(
        address indexed tokenAddress,
        uint256 timestamp
    );
    
    event onBackingDeposit(
        address indexed tokenAddress,
        uint256 amount,
        uint256 timestamp
    );
    
    address[] public tokenList; //to help with iteration
    mapping(address => TOKEN) private acceptedTokens;
    
    uint256 public leverage = 1;
    
    TOKEN pad;
    
    constructor() public {
      pad = TOKEN(address(0xC0888d80EE0AbF84563168b3182650c0AdDEb6d5)); //Pad Address
    }
    
    function() payable external {
        revert();
    }
    
    function checkAndTransferToken(address _tokenAddress, uint256 _amount) private {
        require(acceptedTokens[_tokenAddress].transferFrom(msg.sender, address(this), _amount) == true, "transfer must succeed");
    }
    
    function redeemBacking(uint256 _burnAmount) public returns (uint256) {
        address payable _customerAddress = msg.sender;
        uint256 _padSupply = getPadSupply();
       
        pad.burnFrom(_customerAddress, _burnAmount);
        
        for (uint i = 0; i<tokenList.length; i++) {
            uint256 tokenBalance = TOKEN(tokenList[i]).balanceOf(address(this));
            uint256 backingAmount = tokenBalance.mul(_burnAmount).mul(leverage).div(_padSupply);
            acceptedTokens[tokenList[i]].transfer(_customerAddress, backingAmount);
        }
        
        emit onBackingRedeem(_customerAddress, _burnAmount, now);
        
    }

    function listToken(address _tokenAddress) onlyOwner external returns  (address [] memory) {
        require(acceptedTokens[_tokenAddress] == TOKEN(address(0)), 'This token is already listed.');
        acceptedTokens[_tokenAddress] = TOKEN(address(_tokenAddress));
        tokenList.push(_tokenAddress);
        return tokenList;
    }
    
    //manipulating arrays is costly and this function is only present in case a wrong token address is listed as a mistake.
    function delistToken(address _tokenAddress) onlyOwner external returns (address  [] memory) {
        require(acceptedTokens[_tokenAddress] != TOKEN(address(0)), 'This token is not listed.');
        acceptedTokens[_tokenAddress] = TOKEN(address(0));
         for (uint i = 0; i<tokenList.length; i++) {
            if(tokenList[i] == _tokenAddress) {
                tokenList[i] = tokenList[tokenList.length-1]; //overrides the element we want to delete with the last one
                delete tokenList[tokenList.length-1]; //deletes the last one
                tokenList.length --;
            }
        }
        return tokenList;
    }
    
    function updateLeverage(uint256 _leverage) onlyOwner external returns (bool) {
        require(_leverage >= 1 && _leverage <= 3, "Invalid leverage value.");
        leverage = _leverage;
        return true;
    }
    
    //transfering any accepted token directly to the contract also works (but wont emit an event)
    function addBacking(address _tokenAddress, uint256 _amount) public {
        require(_amount > 0, "must be a positive value");
        checkAndTransferToken(_tokenAddress, _amount);
        emit onBackingDeposit(_tokenAddress, _amount, now);
    }


    function getPadSupply() public view returns (uint256) {
        return pad.totalSupply();
    }
    
}