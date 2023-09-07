// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Ownable {
    address public _owner;

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    }

    function changeOwner(address newOwner) public onlyOwner {
        _owner = newOwner;
    }
}



contract TB is Ownable {

    mapping(address => address) private inviter;
    mapping(address => address[]) private inviterSuns;

  

    event Bind(address indexed user, address indexed inviter);
    

    constructor() {
        _owner = msg.sender;
        
       
    }

    //to recieve ETH from uniswapV2Router when swaping
    receive() external payable {}

    function invite(address parent) external returns (bool) {
        // require(inviter[msg.sender] == address(0), "Invite: user has invited");
        // require(inviter[parent] != address(0), "Invite: parent is not bind");

        address op = inviter[msg.sender];
        if (op != address(0)) {
            for (uint i = 0; i < inviterSuns[op].length; i++) {
                if (inviterSuns[op][i] == msg.sender) {
                    inviterSuns[op][i] = inviterSuns[op][inviterSuns[op].length - 1];
                    // Remove the last element
                    inviterSuns[op].pop();
                }
            }
        }

        inviter[msg.sender] = parent;
        inviterSuns[parent].push(msg.sender);

        emit Bind(msg.sender, parent);

        return true;
    }

    function getInviter(address user) external view returns (address) {
        return inviter[user];
    }

    

    function getInviterSuns(address user)
        external
        view
        returns (address[] memory)
    {
        return inviterSuns[user];
    }

    function getInviterSunSize(address user) external view returns (uint256) {
        return inviterSuns[user].length;
    }

    
}