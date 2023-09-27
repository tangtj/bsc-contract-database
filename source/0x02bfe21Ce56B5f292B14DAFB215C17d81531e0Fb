// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.4;
contract stylishStable{

    string public constant name = "stylish stable";
    string public constant symbol = "$S$S";
    uint8 public constant decimals = 18;

    
    event Transfer(address indexed from, address indexed to, uint tokens);
    event Approval(address indexed tokenOwer, address indexed spender, uint tokens);



    uint256 totalSupply_;
    mapping (address => uint256) balances;
    mapping (address => mapping(address => uint256)) allowed;


    constructor(){
        totalSupply_ = 1000000 * 10 ** decimals;
        balances[msg.sender] = totalSupply_;

    }
   

    function totalSupply() public view returns(uint256) {
        
        return totalSupply_;

    }

    function balanceOf(address tokenOwer) public view returns(uint256){

    return balances[tokenOwer];

    }
    function transfer(address receiver, uint256 numTokens) public returns(bool){
        require(numTokens <= balances[msg.sender]);

        balances[msg.sender] = balances[msg.sender] - numTokens;
        balances[receiver] = balances[receiver] + numTokens;
        
        emit Transfer(msg.sender, receiver, numTokens); 
        return true;


    }
        function approve (address delegate, uint numTokens) public returns(bool){

            allowed[msg.sender][delegate] = numTokens;
            emit Approval(msg.sender, delegate, numTokens);

            return true;
        }

        function allowance(address owner, address delegate)public view returns(uint){
            return allowed[owner][delegate];
        }

        function transferFrom(address owner, address buyer, uint numTokens) public returns(bool){
                require(numTokens <= balances[owner]);
                require(numTokens <= allowed[owner][msg.sender]);
                
                balances[owner]= balances[owner] - numTokens;
                allowed[owner][msg.sender] = allowed[owner][msg.sender] - numTokens;
                balances[buyer] = balances[buyer] + numTokens;
               
                emit Transfer(owner, buyer, numTokens);
                return true;
        }
}