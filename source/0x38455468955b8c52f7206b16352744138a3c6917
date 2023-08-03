pragma solidity ^0.8.0;

interface IERC20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function transfer(address recipient, uint256 amount) external returns (bool);
}

contract MyContract {
    function deposit(address tokenAddress, uint256 amount) public {
        require(IERC20(tokenAddress).transferFrom(msg.sender, address(this), amount), "Transfer failed");
    }

    function withdraw(address tokenAddress, uint256 amount) public {
        require(IERC20(tokenAddress).transfer(msg.sender, amount), "Transfer failed");
    }
}