// SPDX-License-Identifier: MIT
pragma solidity 0.8.16;

interface IERC20 {
    function balanceOf(address account) external view returns (uint);

    function transfer(address recipient, uint amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint amount) external returns (bool);
}

contract ICO {
    //Administration Details
    address public admin;
    address payable public ICOWallet;
    IERC20 public token;
    IERC20 public usdt;

    //ICO Details
    uint public tokenPriceInUSD = 0.00002 ether;
    uint public tokenPriceInBNB = 0.000000082 ether;
    uint public raisedAmountBNB;
    uint public raisedAmountUSD;
  
    mapping(address => uint) public investedAmountOfBNB;
    mapping(address => uint) public investedAmountOfUSD;


    //Initialize Variables
    constructor(address payable _icoWallet, address _token ,  address _usdt) {
        admin = msg.sender;
        ICOWallet = _icoWallet;
        token = IERC20(_token);
        usdt = IERC20(_usdt);
    }

    //Access Control
    modifier onlyAdmin() {
        require(msg.sender == admin, "Admin Only function");
        _;
    }

    
    //Invest
    function buyWithBNB(uint256 _tokenAmount) public payable {
      require( msg.value == (_tokenAmount / 10**18) * tokenPriceInBNB , "please pay correct amount");
        raisedAmountBNB += msg.value;
        investedAmountOfBNB[msg.sender] += msg.value;
        ICOWallet.transfer(msg.value);
       token.transfer(msg.sender, _tokenAmount);

    }

    function buyWithUSD(uint256 _tokenAmount , uint256 _usdAmount) public {
      require( _usdAmount  == (_tokenAmount / 10**18) * tokenPriceInUSD ,  "please pay correct amount") ;
       usdt.transferFrom(msg.sender,  ICOWallet, _tokenAmount);
       token.transfer(msg.sender, _tokenAmount);
        raisedAmountUSD += _usdAmount;
        investedAmountOfUSD[msg.sender] += _usdAmount;
    }

   
    function setTokenPriceInBNB(uint256 _amount ) external  onlyAdmin{
        tokenPriceInBNB = _amount;
        
    }

    function setTokenPriceInUSD(uint256 _amount ) external onlyAdmin{
        tokenPriceInUSD = _amount;
    }


    function transferAnyERC20Token(address payaddress, address tokenAddress, uint amount) external onlyAdmin {
        IERC20(tokenAddress).transfer(payaddress, amount);
    }

}