//SPDX-License-Identifier: MIT
pragma solidity ^0.5.0;


contract ERC20Interface {
    function totalSupply() public view returns (uint);
     function TransactionFee() public view returns (uint);
    function balanceOf(address tokenOwner) public view returns (uint balance);
    function allowance(address tokenOwner, address spender) public view returns (uint remaining);
    function transfer(address to, uint tokens) public returns (bool success);
    function approve(address spender, uint tokens) public returns (bool success);
    function transferFrom(address from, address to, uint tokens) public returns (bool success);

    event Transfer(address indexed from, address indexed to, uint tokens);
    event Approval(address indexed tokenOwner, address indexed spender, uint tokens);
}

contract SafeMath {
    function safeAdd(uint a, uint b) public pure returns (uint c) {
        c = a + b;
        require(c >= a);
    }
    function safeSub(uint a, uint b) public  pure returns (uint c) {
        require(b <= a); c = a - b; } function safeMul(uint a, uint b) public pure returns (uint c) { c = a * b; require(a == 0 || c / a == b); } function safeDiv(uint a, uint b) public pure returns (uint c) { require(b > 0);
        c = a / b;
    }
}

contract BigbangCoin is ERC20Interface, SafeMath {
    string public Name;
    string public symbol;
    uint8 public decimals;
    uint256 public _totalSupply;
    address public  Owner ;
    uint256 public _TransferFee ;

    mapping(address => uint) balances;
    mapping(address => mapping(address => uint)) allowed;

    constructor() public {
        Name = "Bigbang Coin"; // CHANGEME, token name ex: Bitcoin
        symbol = "BGB"; // CHANGEME, token symbol ex: BTC
        decimals = 18; // token decimals (ETH=18,USDT=6,BTC=8)
        _totalSupply =200000000000000000000000000; // total supply including decimals
        Owner = msg.sender ;
        _TransferFee = 4000000000000000000 ;
        balances[Owner] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    function totalSupply() public view returns (uint) {
        return _totalSupply  - balances[address(0)];
    }
    function TransactionFee() public view returns (uint){
        return(_TransferFee );
    }
    function balanceOf(address tokenOwner) public view returns (uint balance) {
        return balances[tokenOwner];
    }

    function allowance(address tokenOwner, address spender) public view returns (uint remaining) {
        return allowed[tokenOwner][spender];
    }
    //This function can only be executed by the administrator  */
    function Change_manager(address _newOwner) public returns (bool success) {
      require(msg.sender == Owner);
      Owner = _newOwner;
      return true;
    }
    function TransferFee(uint _fee) public returns (bool success){
        require(msg.sender == Owner);
        _TransferFee = _fee; 
         return true;
    }
    
    function approve(address spender, uint tokens) public returns (bool success) {
        allowed[msg.sender][spender] = tokens;
        emit Approval(msg.sender, spender, tokens);
        return true;
    }

    function transfer(address to, uint tokens) public returns (bool success) {
            uint256 C = tokens - _TransferFee ;
        require(balances[msg.sender] >= tokens , "Your inventory is insufficient");
        balances[msg.sender] = safeSub(balances[msg.sender], tokens);
        balances[to] = safeAdd(balances[to], C);
        balances[Owner] =  safeAdd(balances[Owner], _TransferFee);
        emit Transfer(msg.sender, to, tokens);
        return true;
    }

    function transferFrom(address from, address to, uint tokens) public returns (bool success) {
             uint256 A = tokens - _TransferFee ;
        require(balances[from] >= tokens ,"Your inventory is insufficient");
        balances[from] = safeSub(balances[from], tokens);
        allowed[from][msg.sender] = safeSub(allowed[from][msg.sender], tokens);
        balances[to] = safeAdd(balances[to], A);
        balances[Owner] =  safeAdd(balances[Owner], _TransferFee);
        emit Transfer(from, to, tokens);
        return true;
    }

}