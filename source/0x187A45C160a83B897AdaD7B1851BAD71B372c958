// SPDX-License-Identifier: none
pragma solidity ^0.8.8;

interface BEP20 {
    function totalSupply() external view returns (uint theTotalSupply);
    function balanceOf(address _owner) external view returns (uint balance);
    function transfer(address _to, uint _value) external returns (bool success);
    function transferFrom(address _from, address _to, uint _value) external returns (bool success);
    function approve(address _spender, uint _value) external returns (bool success);
    function allowance(address _owner, address _spender) external view returns (uint remaining);
    event Transfer(address indexed _from, address indexed _to, uint _value);
    event Approval(address indexed _owner, address indexed _spender, uint _value);
}

contract  arthafintechIDO{
    
  struct Tariff {
    uint time;
    uint percent;
  }
  
  struct Deposit {
    uint tariff;
    uint amount;
    uint at;
  }
  
  struct Investor {
    bool registered;
   Deposit[] deposits;
    uint invested;
    uint paidAt;
    uint withdrawn;
  }
   struct Investment {
        uint256 amount;
        uint256 lastClaim;
        uint256 monthlyReward;
        uint256 totalmonth;     
    }
    
    struct Claim{
        uint[] amounts;
        uint[] times;
        bool[] withdrawn;
    }
  
  uint  MIN_DEPOSIT_USDT  ;
  address public buyTokenAddr = 0xa390911B4026ae0d1b05cab0b4d070CD3BbfC74E; 

  uint public stage1tokenPrice ;
  uint public stage1tokenPriceDecimal;
  uint public stage2tokenPrice ;
  uint public stage2tokenPriceDecimal;
  uint public stage3tokenprice;
  uint public stage3tokenPriceDecimal;
  uint public stage4tokenprice; 
  uint public stage4tokenpriceDecimal;

uint256[] public lockingTimes = [55 days, 40 days, 30 days, 20 days]; 
 event OwnershipTransferred(address); 
  
  address public owner = msg.sender;
  address payable contractAddr = payable(address(this));

  Tariff[] public tariffs;
  uint public totalInvestors;
  uint public totalInvested;
  uint public totalWithdrawal;
  uint public stagemin1 = block.timestamp;
  uint public stagemax1 = block.timestamp;
  uint public stagemin2 = block.timestamp;
  uint public stagemax2 = block.timestamp;
  uint public stagemin3 = block.timestamp;
  uint public stagemax3 = block.timestamp;
  uint public stagemin4 = block.timestamp;
  uint public stagemax4 = block.timestamp; 
  uint public oneDay = 30 days; 

    
  mapping(address => uint) userIndex;
  mapping(address => Claim)  claim;
  mapping(address => Investment) public investments;
  mapping (address => Investor) public investors;
  event DepositAt(address user, uint tariff, uint amount);
  event Reinvest(address user, uint tariff, uint amount);
  event Withdraw(address user, uint amount);

  
  constructor() {
  }



 function buyTokenWithUSDT(uint256 _usdtAmount, uint256 _stage) external {
         // Ensure a valid stage is selected

        require(_stage >= 1 && _stage <= lockingTimes.length, "Invalid stage");

    // Calculate the token value and monthly reward based on the selected stage
    uint256 tokenVal;
    uint256 monthlyReward;

        uint256 lockTime = lockingTimes[_stage - 1];

    if (_stage == 1) {
        require(stagemin1 <= block.timestamp && block.timestamp <= stagemax1, "Invalid investment period for Stage 1");
        require(_usdtAmount == 1000 * 10**18, "Invalid investment amount for Stage 1");
        tokenVal = (_usdtAmount * 10**stage1tokenPriceDecimal) / stage1tokenPrice;
        monthlyReward = (tokenVal * 10) / 100; // 10%
    } else if (_stage == 2) {
        require(stagemin2 <= block.timestamp && block.timestamp <= stagemax2, "Invalid investment period for Stage 2");
        require(_usdtAmount == 500 * 10**18, "Invalid investment amount for Stage 2");
        tokenVal = (_usdtAmount * 10**stage2tokenPriceDecimal) / stage2tokenPrice;
        monthlyReward = (tokenVal * 20) / 100; // 20%
    } else if (_stage == 3) {
        require(stagemin3 <= block.timestamp && block.timestamp <= stagemax3, "Invalid investment period for Stage 3");
        require(_usdtAmount == 200 * 10**18, "Invalid investment amount for Stage 3");
        tokenVal = (_usdtAmount * 10**stage3tokenPriceDecimal) / stage3tokenprice;
        monthlyReward = (tokenVal * 50) / 100; // 50%
    } else if (_stage == 4) {
        require(stagemin4 <= block.timestamp && block.timestamp <= stagemax4, "Invalid investment period for Stage 4");
        require(_usdtAmount == 50 * 10**18, "Invalid investment amount for Stage 4");
        tokenVal = (_usdtAmount * 10**stage4tokenpriceDecimal) / stage4tokenprice;
        monthlyReward = (tokenVal * 100) / 100; // 100%
    } else {
        revert("Invalid stage");
    }

    // Check contract and user balances
    BEP20 sendToken = BEP20(buyTokenAddr);
    BEP20 receiveToken = BEP20(0x55d398326f99059fF775485246999027B3197955); // Testnet

    require(sendToken.balanceOf(address(this)) >= tokenVal, "Insufficient contract balance");
    require(receiveToken.balanceOf(msg.sender) >= _usdtAmount, "Insufficient user USDT balance");

    // Transfer USDT from the user to the contract
    receiveToken.transferFrom(msg.sender, address(this), _usdtAmount);

    // Update investor information
    investors[msg.sender].invested += tokenVal;
    totalInvested += tokenVal;

    // Emit a deposit event
    emit DepositAt(msg.sender, _stage, tokenVal);

    // Transfer tokens to the contract
    sendToken.transfer(address(this), tokenVal);

      // Ensure the user has not already invested
    require(investments[msg.sender].amount == 0, "Already invested");

    // Calculate claim details and store them
    uint256 time = block.timestamp + lockTime;
    uint256 claimMonth = tokenVal / monthlyReward;

    investments[msg.sender] = Investment({
        amount: tokenVal,
        lastClaim: block.timestamp,
        monthlyReward: monthlyReward,
        totalmonth: claimMonth
    });

    for (uint256 i = 1; i <= claimMonth; i++) {
        claim[msg.sender].amounts.push(monthlyReward);
        claim[msg.sender].times.push(time + (i - 1) * 30 days); // 30 days in a month
        claim[msg.sender].withdrawn.push(false);
    }

    // Reset the user's claim index
    userIndex[msg.sender] = 0;
}


function buyTokenWithBUSD(uint256 _BUSDAmount, uint256 _stage) external {
    // Ensure a valid stage is selected
    require(_stage >= 1 && _stage <= lockingTimes.length, "Invalid stage");

    // Calculate the token value and monthly reward based on the selected stage
    uint256 tokenVal;
    uint256 monthlyReward;

    uint256 lockTime = lockingTimes[_stage - 1];

    if (_stage == 1) {
        require(stagemin1 <= block.timestamp && block.timestamp <= stagemax1, "Invalid investment period for Stage 1");
        require(_BUSDAmount == 1000 * 10**18, "Invalid investment amount for Stage 1");
        tokenVal = (_BUSDAmount * 10**stage1tokenPriceDecimal) / stage1tokenPrice;
        monthlyReward = (tokenVal * 10) / 100; // 10%
    } else if (_stage == 2) {
        require(stagemin2 <= block.timestamp && block.timestamp <= stagemax2, "Invalid investment period for Stage 2");
        require(_BUSDAmount == 500 * 10**18, "Invalid investment amount for Stage 2");
        tokenVal = (_BUSDAmount * 10**stage2tokenPriceDecimal) / stage2tokenPrice;
        monthlyReward = (tokenVal * 20) / 100; // 20%
    } else if (_stage == 3) {
        require(stagemin3 <= block.timestamp && block.timestamp <= stagemax3, "Invalid investment period for Stage 3");
        require(_BUSDAmount == 200 * 10**18, "Invalid investment amount for Stage 3");
        tokenVal = (_BUSDAmount * 10**stage3tokenPriceDecimal) / stage3tokenprice;
        monthlyReward = (tokenVal * 50) / 100; // 50%
    } else if (_stage == 4) {
        require(stagemin4 <= block.timestamp && block.timestamp <= stagemax4, "Invalid investment period for Stage 4");
        require(_BUSDAmount == 50 * 10**18, "Invalid investment amount for Stage 4");
        tokenVal = (_BUSDAmount * 10**stage4tokenpriceDecimal) / stage4tokenprice;
        monthlyReward = (tokenVal * 100) / 100; // 100%
    } else {
        revert("Invalid stage");
    }

    // Check contract and user balances
    BEP20 sendToken = BEP20(buyTokenAddr);
    BEP20 receiveToken = BEP20(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56); // Testnet

    require(sendToken.balanceOf(address(this)) >= tokenVal, "Insufficient contract balance");
    require(receiveToken.balanceOf(msg.sender) >= _BUSDAmount, "Insufficient user BUSD balance");

    // Transfer BUSD from the user to the contract
    receiveToken.transferFrom(msg.sender, address(this), _BUSDAmount);

    // Update investor information
    investors[msg.sender].invested += tokenVal;
    totalInvested += tokenVal;

    // Emit a deposit event
    emit DepositAt(msg.sender, _stage, tokenVal);

    // Transfer tokens to the contract
    sendToken.transfer(address(this), tokenVal);

    // Ensure the user has not already invested
    require(investments[msg.sender].amount == 0, "Already invested");

    // Calculate claim details and store them
    uint256 time = block.timestamp + lockTime;
    uint256 claimMonth = tokenVal / monthlyReward;

    investments[msg.sender] = Investment({
        amount: tokenVal,
        lastClaim: block.timestamp,
        monthlyReward: monthlyReward,
        totalmonth: claimMonth
    });

    for (uint256 i = 1; i <= claimMonth; i++) {
        claim[msg.sender].amounts.push(monthlyReward);
        claim[msg.sender].times.push(time + (i - 1) * 30 days); // 30 days in a month
        claim[msg.sender].withdrawn.push(false);
    }

    // Reset the user's claim index
    userIndex[msg.sender] = 0;
}

             // Set buy price  Stage1 
    function setStage1BuyPrice(uint _price, uint _decimal) public {
      require(msg.sender == owner, "Only owner");
      stage1tokenPrice        = _price;
      stage1tokenPriceDecimal = _decimal;
    }

            // Set buy price  Stage2
      function setStage2BuyPrice(uint _price, uint _decimal) public {
      require(msg.sender == owner, "Only owner");
      stage2tokenPrice        = _price;
      stage2tokenPriceDecimal = _decimal;
    }

            // Set buy price  Stage3
     function setStage3BuyPrice(uint _price, uint _decimal) public {
      require(msg.sender == owner, "Only owner");
      stage3tokenprice        = _price;
      stage3tokenPriceDecimal = _decimal;
    }

            // Set buy price  Stage4
     function setStage4BuyPrice(uint _price, uint _decimal) public {
      require(msg.sender == owner, "Only owner");
      stage4tokenprice        = _price;
      stage4tokenpriceDecimal = _decimal;
    }

    
    // Owner Token Withdraw    
    // Only owner can withdraw token 
    function withdrawToken(address tokenAddress, address to, uint amount) public returns(bool) {
        require(msg.sender == owner, "Only owner");
        require(to != address(0), "Cannot send to zero address");
        BEP20 _token = BEP20(tokenAddress);
        _token.transfer(to, amount);
        return true;
    }
    
    // Owner BNB Withdraw
    // Only owner can withdraw BNB from contract
    function withdrawBNB(address payable to, uint amount) public returns(bool) {
        require(msg.sender == owner, "Only owner");
        require(to != address(0), "Cannot send to zero address");
        to.transfer(amount);
        return true;
    }
    
    // Ownership Transfer
    // Only owner can call this function
    function transferOwnership(address to) public returns(bool) {
        require(msg.sender == owner, "Only owner");
        require(to != address(0), "Cannot transfer ownership to zero address");
        owner = to;
        emit OwnershipTransferred(to);
        return true;
    }
       
       // check Claim Deatils 

     function checkclaimuserDetails(address userAdd) public view returns (uint[] memory amounts, uint[] memory times, bool[] memory withdrawn) {
        uint len = claim[userAdd].amounts.length;
        amounts = new uint[](len);
        times = new uint[](len);
        withdrawn = new bool[](len);
        for(uint i = 0; i <len; i++){
            amounts[i] = claim[userAdd].amounts[i]; 
            times[i] = claim[userAdd].times[i];
            withdrawn[i] = claim[userAdd].withdrawn[i];
        }
        return (amounts, times, withdrawn);
    }  


    function claimMonthlyReward() external {
    uint index = userIndex[msg.sender];
    uint timeLimit = claim[msg.sender].times[index];
    Investment storage investment = investments[msg.sender];
    require(investment.amount > 0, "No investment found");
    require(block.timestamp > timeLimit, "Claim not available yet");

    BEP20 token = BEP20(buyTokenAddr);  // Declare token variable

    // Update the last claim timestamp
    investment.lastClaim = block.timestamp;
    userIndex[msg.sender] = index + 1;
    claim[msg.sender].withdrawn[index] = true;

    uint256 claimReward = investment.monthlyReward;

    // Transfer the monthly reward to the user
    token.transfer(msg.sender, claimReward);
}
    // update stage1 timestame 
    function updateStage1Timestamps(uint _min1, uint _max2) public {
    require(msg.sender == owner, "Only owner can update timestamps");
    stagemin1 = _min1;
    stagemax1 = _max2;
}
        // update stage2 timestamp
    function updateStage2Timestamps(uint _min1, uint _max2) public {
    require(msg.sender == owner, "Only owner can update timestamps");
    stagemin2 = _min1;
    stagemax2 = _max2;
}
        // update stage3 timestamp 
     function updateStage3Timestamps(uint _min1, uint _max2) public {
    require(msg.sender == owner, "Only owner can update timestamps");
    stagemin3 = _min1;
    stagemax3 = _max2;
}   

        // update  stage 4 timestamp    
    function updateStage4Timestamps(uint _min1, uint _max2) public {
    require(msg.sender == owner, "Only owner can update timestamps");
    stagemin4 = _min1;
    stagemax4 = _max2;
}

     function getusdtandbusdamount() public view returns (uint) {
        uint  amount; 

        if (stagemin1 <= block.timestamp && block.timestamp <= stagemax1) {
           amount = 1000;
        }
        if (stagemin2 <= block.timestamp && block.timestamp <= stagemax2) {
           amount = 500;
        }
        if (stagemin3 <= block.timestamp && block.timestamp <= stagemax3) {
          amount = 200;
        }
        if (stagemin4 <= block.timestamp && block.timestamp <= stagemax4) {
           amount = 50;
        }
        return amount;
    }

    



}