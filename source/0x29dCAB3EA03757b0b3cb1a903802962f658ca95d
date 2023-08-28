// SPDX-License-Identifier: Unlicensed
pragma solidity ^0.8.0;

library TransferHelper {
    function safeTransferFrom(address token, address from, address to, uint256 value) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FROM_FAILED');
    }

    function safeTransfer(address token, address to, uint256 value) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FAILED');
    }
}

interface IERC20 {
    function balanceOf(address who) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);
}


contract TokenMigrate {

    event Migrated(address indexed from, uint256 amount);
	event TokenWithdraw(address indexed user, address token, uint256 amount);
    address public oldTokenContract;
    address public newTokenContract;
    address public owner;
    uint256 public totalMigratedTokens;

    mapping(address => bool) public isAllowed;
    mapping(address => uint256) public allowedAmount;

    constructor (address _oldToken, address _newToken) {
        oldTokenContract = _oldToken;
        newTokenContract = _newToken;
        owner = msg.sender;
    }

    modifier onlyOwner(address _owner) {
        require(owner == _owner, "Only the owner can use the function");
        _;
    }

    modifier approvedOnly(address _approved) {
        require(isAllowed[_approved] == true, "Not an allowed address");
        _;
    }

    function swapOldToNewToken(uint256 amount) public approvedOnly(msg.sender) returns (uint256 swapped_tokens) {
        require(allowedAmount[msg.sender] >= amount, "Not enough allowed amount to migrate, contact owners");

        require(IERC20(newTokenContract).balanceOf(address(this)) >= amount, "This contract does not have enough NEW Tokens, contact owners");
        require(IERC20(oldTokenContract).balanceOf(msg.sender) >= amount, "Not enough OLD Tokens");
        require(IERC20(oldTokenContract).allowance(msg.sender, address(this)) >= amount, "Go to OLD Token contract and approve this contract");

        // Get users initial balance of both the new and old token
        uint256 userOldTokenInitialBalance = IERC20(oldTokenContract).balanceOf(msg.sender);
        uint256 userNewTokenInitialBalance = IERC20(newTokenContract).balanceOf(msg.sender);

        // Get this contracts initial balance of both the new and old token
        uint256 contractOldTokenInitialBalance = IERC20(oldTokenContract).balanceOf(address(this));
        uint256 contractNewTokenInitialBalance = IERC20(newTokenContract).balanceOf(address(this));

        // Transfer OLD token from the user to this contract and NEW token from this contract to the user
        TransferHelper.safeTransferFrom(oldTokenContract, msg.sender, address(this), amount);
        TransferHelper.safeTransfer(newTokenContract, msg.sender, amount);

        // Make sure all balances is correct after the transfer
        require(IERC20(oldTokenContract).balanceOf(address(this)) == (contractOldTokenInitialBalance + amount), "Something went wrong transfer OLD token to this contract");
        require(IERC20(newTokenContract).balanceOf(address(this)) == (contractNewTokenInitialBalance - amount), "Something went wrong transfer NEW token to the user");
        require(IERC20(oldTokenContract).balanceOf(msg.sender) == (userOldTokenInitialBalance - amount), "Something went wrong, user OLD token balance is not correct");
        require(IERC20(newTokenContract).balanceOf(msg.sender) == (userNewTokenInitialBalance + amount), "Something went wrong, user NEW token balance is not correct");

        //remove the amount from the approved user amount
        allowedAmount[msg.sender] -= amount;
        //if the user migrated all his funds, set him as a non approved user
        if(allowedAmount[msg.sender] == 0) {
            isAllowed[msg.sender] = false;
        }

        
        emit Migrated(msg.sender, amount);
        totalMigratedTokens += amount;
        return amount;
    }

    function changeOwner(address _newOwner) public onlyOwner(msg.sender) {
        require(_newOwner != address(0), "Can not set the zero address as owner");

        owner = _newOwner;
    }

   function approveNewUsers(address[] memory _newUsers, uint256[] memory _userAmounts) public onlyOwner(msg.sender) {

        require(_newUsers.length == _userAmounts.length, "users and amounts array need to be same length");

        for(uint256 i = 0; i < _newUsers.length; i++) {
            require(_newUsers[i] != address(0), "Can not set the zero address as user");

            isAllowed[_newUsers[i]] = true;
            allowedAmount[_newUsers[i]] = _userAmounts[i];
        }
    }

    function removeApprovedUser(address _removeUser) public onlyOwner(msg.sender) {
        require(_removeUser != address(0), "Can not remove the zero address");
        require(isAllowed[_removeUser] == true, "The user is not approved");

        isAllowed[_removeUser] = false;
        allowedAmount[_removeUser] = 0;
    }
	
	function tokenWithdraw(address _token, uint256 _amount) public onlyOwner(msg.sender) {
      require(_token != oldTokenContract, "Can not remove the old token from the contract");  
      
      TransferHelper.safeTransfer(_token, owner, _amount);

emit TokenWithdraw(owner, _token, _amount);
    }
}