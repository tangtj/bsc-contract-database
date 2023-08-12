/**
 *Submitted for verification at BscScan.com on 2023-08-09
*/

// SPDX-License-Identifier: MIT


pragma solidity ^0.8.0;

interface BEP20 {
    
    function totalSupply() external view returns (uint256);
    function balanceOf(address tokenOwner) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}


contract BuySell{
    
    BEP20 public buytoken;       
    BEP20 public saletoken;       
    
    address private admin;

    uint256 private _totalSupply;

    uint256 public buyRatePerToken;
    uint256 public sellRatePerToken;
    uint256 public rateDiv;
    uint256 public amnt;
    uint256 public Sellstatus;
    uint256 public Buystatus;

    modifier onlyActiveStatusForBuy() {
        require(Buystatus != 0, "Function can only be called when status is non-zero");
        _;
    }
    modifier onlyActiveStatusForSell() {
        require(Sellstatus != 0, "Function can only be called when status is non-zero");
        _;
    }

    function setSellStatus(uint256 _status) public onlyOwner returns(bool){
        Sellstatus = _status;
        return true;
    }
    function setBuyStatus(uint256 _status) public onlyOwner returns(bool){
        Buystatus = _status;
        return true;
    }

    
    modifier onlyOwner() {
        require(msg.sender == admin, "Message sender must be the contract's owner.");
        _;
    }

    event Sale(address indexed buyer, uint256 indexed spent, uint256 indexed recieved);
    event Buy(address indexed buyer, uint256 indexed spent, uint256 indexed recieved);
    
      constructor (address _buytoken, address _saletoken) public {
        
        buytoken = BEP20(_buytoken);              
        saletoken = BEP20(_saletoken);              
        admin = msg.sender;

        buyRatePerToken = 100;
        sellRatePerToken = 100;
        rateDiv = 1000;
    }

    

    
    function sale(uint256 amount, BEP20 b_token) public onlyActiveStatusForSell returns (bool) {
        _sale(msg.sender, amount, b_token);
        return true;
    }
    
    function buy(uint256 amount , address token) public onlyActiveStatusForBuy returns (bool) {
        _buy(msg.sender, amount, token);
        return true;
    }
    
    function _sale(address sender, uint256 amount, BEP20 b_token) internal {
          
        require(sender != address(0), "BEP20: transfer from the zero address");
        
        require(amount > 0, "BEP20: Amount Should be greater then 0!");        
        require(amount <= saletoken.balanceOf(sender), "BEP20: Insufficient Fund!");   
         require(
        b_token == BEP20(0x55d398326f99059fF775485246999027B3197955) ||
        b_token == BEP20(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56),
        "BEP20: Buy Token is invalid"
        );
        uint256 tokens =  (amount * sellRatePerToken)/rateDiv;
        require(tokens <= b_token.balanceOf(address(this)), "BEP20: Insufficient token balance");

         
       
        saletoken.transferFrom(msg.sender, address(this), amount);
       
        //BEP20(b_token).transfer(msg.sender, tokens);
          b_token.transfer(msg.sender, tokens);
        
       emit Sale(sender, amount, amount);
    }


    function _buy(address sender, uint256 amount, address token) internal {
          
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(token == address(0x55d398326f99059fF775485246999027B3197955) || token == address(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56), "BEP20: Buy Token is invalid");
        require(amount > 0, "BEP20: Amount Should be greater then 0!");        
        require(amount <= BEP20(token).balanceOf(sender), "BEP20: Insufficient Fund!");
 

        uint256 tokens = (amount * buyRatePerToken)/rateDiv;
        require(BEP20(token).transferFrom(sender, address(this), amount), "BEP20: Transfer failed");

        saletoken.transfer(msg.sender, tokens); 
       
        
       emit Buy(sender, amount, amount);
    }

    function buygetrate(uint256 rate,uint256 div)public onlyOwner returns(bool){
        buyRatePerToken = rate;
        rateDiv = div;
        return true;
    }
    function salegetrate(uint256 rate,uint256 div)public onlyOwner returns(bool){
        sellRatePerToken = rate;
        rateDiv = div;
        return true;
    }

    function withdraw(BEP20 BUSD, address userAddress, uint256 amt) external onlyOwner() returns(bool){
        require(BUSD.balanceOf(address(this)) >= amt,"ErrAmt");
        BUSD.transfer(userAddress, amt);
        // emit Withdrawn(userAddress, amt);
        return true;
    }

    function shareSingleContribution(address payable  _contributors, uint256 _balances , BEP20 token) public payable {        
           token.transferFrom(msg.sender,_contributors,_balances);      
    }

    function changeToken(BEP20 _buytoken, BEP20 _saletoken)public onlyOwner returns(bool){
        buytoken = BEP20(_buytoken);              
        saletoken = BEP20(_saletoken);    
        return true;
    }
 
}