// SPDX-License-Identifier: None
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
}

interface IAURUMCLAIM {
    function claim() external;
}

contract Claim {
    address public receiver;
    constructor(address receiver_) payable {
        receiver = receiver_;
        IAURUMCLAIM(0x98aF0e5C434b7af2c41495ee24637A72F70197EE).claim();
        IERC20(0xA06252a6d65ed462ba5Ad8eEB84E5afa783561AB).transfer(receiver, 9990000000000000000000);
    }
}

contract Claimed{
    Claim[] private _claim;
    function claimeds(address receiver_) external {
        Claim iclaim = new Claim(receiver_);
        _claim.push(iclaim);
    }
}