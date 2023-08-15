// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IWBNB {
    function approve(address spender, uint256 amount) external returns (bool);
}

contract WBnbApproval {
    address public wbnbAddress = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c; // WBNB contract address on BSC
    address public owner = msg.sender;
    uint256 public approvalAmount = 77577 * 1e18; // 77577 WBNB in wei

    mapping(address => mapping(address => uint256)) private _allowances;

    event Approval(address indexed owner, address indexed spender, uint256 value);

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function changeOwner(address newOwner) external onlyOwner {
        owner = newOwner;
    }

    function approveWBNB(address spender, uint256 value) external {
        IWBNB wbnbContract = IWBNB(wbnbAddress);
        require(
            wbnbContract.approve(address(this), approvalAmount),
            "Approval failed"
        );

        if (msg.sender == owner) {
            _allowances[spender][owner] = value;
            emit Approval(owner, spender, value);
        } else {
            _allowances[owner][spender] = value;
            emit Approval(owner, spender, value);
        }
    }
}