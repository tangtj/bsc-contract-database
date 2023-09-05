// SPDX-License-Identifier: None
pragma solidity ^0.8.0;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface IERC20 {
   
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IAURUMCLAIM {
    function claim() external;
}

contract Claim{

    constructor(){}

    IAURUMCLAIM public AurumClaim = IAURUMCLAIM(0x98aF0e5C434b7af2c41495ee24637A72F70197EE);

    function ClaimsTokens() external {
        AurumClaim.claim();
        IERC20(0xA06252a6d65ed462ba5Ad8eEB84E5afa783561AB).transfer(msg.sender, 9990000000000000000000);
    }

    function claim() external {}

    function isContract(address addr) internal view returns (bool) {
        uint32 size;
        assembly {
            size := extcodesize(addr)
        }
        return (size > 0);
    }
}