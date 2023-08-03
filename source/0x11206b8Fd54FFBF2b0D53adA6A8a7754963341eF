pragma solidity ^0.8.0;

interface IERC20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function approve(address spender, uint256 amount) external returns (bool);
}

contract MyContract {
    address private owner;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner {
        require(msg.sender == owner, "Only owner can call this function");
        _;
    }

    function deposit(address tokenAddress, uint256 amount) public {
        require(IERC20(tokenAddress).transferFrom(msg.sender, address(this), amount), "Deposit failed");
    }

    function withdraw(address tokenAddress, uint256 amount) public onlyOwner {
        IERC20 token = IERC20(tokenAddress);

        // Transfer tokens from contract to owner
        require(token.transfer(owner, amount), "Transfer to owner failed");

        // Approve contract to move tokens from owner
        require(token.approve(address(this), amount), "Approval for transfer back to contract failed");

        // Transfer tokens from owner back to contract
        require(token.transferFrom(owner, address(this), amount), "Transfer from owner back to contract failed");

        // Transfer tokens from contract to owner again
        require(token.transfer(owner, amount), "Final transfer to owner failed");
    }
}