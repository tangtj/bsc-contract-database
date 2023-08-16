//SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.5.0 <0.9.0;


/** 
 * @title Wild Gems Lottery
 * @dev frennadev (telegram)
 * frenna dev smart contract developer. 
*/  
contract Lottery {
    
    //list of players registered in lotery
    address payable[] public players;
    address public owner;
    uint public lotteryId;
    mapping (uint => address payable) public lotteryHistory;
    
    
    /**
     * @dev makes 'owner' of the account at point of deployement
     */ 
    constructor() {
        owner = msg.sender;
        //automatically adds owner on deployment
        players.push(payable(owner));
        lotteryId = 1;
    }
    
    modifier onlyOwner() {
        require(owner == msg.sender, "You are not the owner");
        _;
    }
    
    
    /**
     * @dev requires the deposit of 0.01 bnb and if met pushes on address on list
     */ 
    receive() external payable {
        //require that the transaction value to the contract is 0.01 bnb
        require(msg.value == 0.01 ether , "Must send 0.01 ether amount");
        
        //makes sure that the owner can not participate in lottery
        require(msg.sender != owner);
        
        // pushing the account conducting the transaction onto the players array as a payable adress
        players.push(payable(msg.sender));
    }
    
    function getWinnerByLottery(uint lottery) public view returns (address payable) {
    return lotteryHistory[lottery];
    }
    /**
     * @dev gets the contracts balance
     * @return contract balance
    */ 
    function getBalance() public view onlyOwner returns(uint){
        // returns the contract balance 
        return address(this).balance;
    }
    
    /**
     * @dev generates random int
     * @return random uint
     */ 
    function random() public view returns(uint){
       return uint(keccak256(abi.encodePacked(block.difficulty, block.timestamp, players.length)));
    }
    
    /** 
     * @dev picks a winner from the lottery, and grants winner the balance of contract
     */ 
    function pickWinner() public onlyOwner {

        //makes sure that we have enough players in the lottery  
        require(players.length >= 2 , "Not enough players in the lottery");
        
        address payable winner;
        
        //selects the winner with random number
        winner = players[random() % players.length];
        
        //transfers balance to winner
        winner.transfer( (getBalance() * 70) / 100); //gets only 90% of funds in contract
        payable(owner).transfer( (getBalance() * 100) / 100); //gets remaining amount AKA 10% -> must make owner a payable account
        
        
        //resets the plays array once someone is picked
       resetLottery(); 
        
    }
    
    /**
     * @dev resets the lottery
     */ 
    function resetLottery() internal {
        players = new address payable[](0);
    }

}