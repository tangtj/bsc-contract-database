// SPDX-License-Identifier: UNLISCENSED

pragma solidity ^0.8.4;

interface IERC20 {
    function balanceOf(address account) external view returns (uint256);

    function decimals() external view returns (uint8);

    function transfer(address recipient, uint256 amount) external returns (bool);
}

contract SMTFaucet {
    IERC20 public token;
    address public owner;
    uint256 public faucetDripAmount = 1;
    uint256 public rateLimit = 5 minutes; // Rate limit interval

    mapping(address => uint256) public lastRequestTimestamp;

    constructor(address _smtAddress) {
        token = IERC20(_smtAddress);
        owner = msg.sender;
    }

    modifier onlyOwner {
        require(msg.sender == owner, "FaucetError: Caller not owner");
        _;
    }

    function send() external {
        require(token.balanceOf(address(this)) >= faucetDripAmount * 10**token.decimals(), "FaucetError: Insufficient funds");
        require(
            block.timestamp >= lastRequestTimestamp[msg.sender] + rateLimit,
            "FaucetError: Try again later"
        );

        lastRequestTimestamp[msg.sender] = block.timestamp;
        token.transfer(msg.sender, faucetDripAmount * 10**token.decimals());
    }

    function setFaucetDripAmount(uint256 _amount) external onlyOwner {
        faucetDripAmount = _amount;
    }

    function setRateLimit(uint256 _seconds) external onlyOwner {
        rateLimit = _seconds;
    }

    function withdrawTokens(address _receiver, uint256 _amount) external onlyOwner {
        require(token.balanceOf(address(this)) >= _amount, "FaucetError: Insufficient funds");
        token.transfer(_receiver, _amount);
    }
}