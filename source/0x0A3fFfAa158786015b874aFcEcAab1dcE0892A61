//SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

interface IERC20 {
  function transferFrom(address from, address to, uint256 amount) external;
  function transfer(address to, uint256 amount) external;
}

contract RainBotDeposit {
  address public owner;

  event Deposit(uint256 user, uint256 amount, address token);
  event Withdraw(uint256 user, address receiver, uint256 amount, address token);

  constructor(address newOwner){
    owner = newOwner;
  }

  modifier onlyOwner(){
    require(msg.sender == owner);
    _;
  }

  function transferOwnership(address _new) external onlyOwner {
    owner = _new;
  }

  function deposit(uint256 user, uint256 amount, address token) external {
    IERC20(token).transferFrom(msg.sender, address(this), amount);
    emit Deposit(user, amount, token);
  }

  function withdraw(uint256 user, address receiver, uint256 amount, address token) external onlyOwner {
    IERC20(token).transfer(receiver, amount);
    emit Withdraw(user, receiver, amount, token);
  }

  function recover(address token, address to, uint256 amount) external onlyOwner {
    if (token == address(0)){
        payable(to).call{value: address(this).balance}("");
    } else {
        IERC20(token).transfer(to, amount);
    }
  }
}