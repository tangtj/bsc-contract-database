// Sources flattened with hardhat v2.10.1 https://hardhat.org

// File contracts/features/Blocklist.sol
// SPDX-License-Identifier: MIT
pragma solidity 0.5.16;

/**
 * @title Blocklist
 * @dev This contract manages a list of addresses and has a simple CRUD
 */
contract Blocklist {
  mapping(address => bool) public operators;
  mapping(address => uint256) private _userIndex;

  address[] private _userList;
  address private _master;

  event addedToBlocklist(address indexed account, address by);
  event removedFromBlocklist(address indexed account, address by);

  modifier onlyOperator() {
    require(operators[msg.sender], "Only operator");
    _;
  }

  constructor() public {
    operators[msg.sender] = true;
    _master = msg.sender;
  }

  // Setters
  function addToBlocklist(address account) external onlyOperator returns (bool) {
    return _addToBlocklist(account);
  }

  function batchAddToBlocklist(address[] calldata accounts) external onlyOperator {
    for (uint256 i = 0; i < accounts.length; i++) {
      require(_addToBlocklist(accounts[i]));
    }
  }

  function removeFromBlocklist(address account) external onlyOperator returns (bool) {
    return _removeFromBlocklist(account);
  }

  function batchRemoveFromBlocklist(address[] calldata accounts) external onlyOperator {
    for (uint256 i = 0; i < accounts.length; i++) {
      require(_removeFromBlocklist(accounts[i]));
    }
  }

  // Getters
  function isBlocklisted(address account) public view returns (bool) {
    if (_userList.length == 0) return false;

    // We don't want to throw when querying for an out-of-bounds index.
    // It can happen when the list has been shrunk after a deletion.
    if (_userIndex[account] >= _userList.length) return false;

    return _userList[_userIndex[account]] == account;
  }

  function areBlocklisted(address accountA, address accountB) public view returns (bool) {
    if (_userList.length == 0) return false;

    if (_userIndex[accountA] < _userList.length) {
      if (_userList[_userIndex[accountA]] == accountA) {
        return true;
      }
    }

    if (_userIndex[accountB] < _userList.length) {
      if (_userList[_userIndex[accountB]] == accountB) {
        return true;
      }
    }

    return false;
  }

  function getFullList() public view returns (address[] memory) {
    return _userList;
  }

  // Private
  function _addToBlocklist(address account) private returns (bool) {
    require(!isBlocklisted(account), "Already in blocklist");

    _userIndex[account] = _userList.length;
    _userList.push(account);

    emit addedToBlocklist(account, msg.sender);

    return true;
  }

  function _removeFromBlocklist(address account) private returns (bool) {
    require(isBlocklisted(account), "Not blocklisted");

    uint256 rowToDelete = _userIndex[account];
    address keyToMove = _userList[_userList.length - 1];
    _userList[rowToDelete] = keyToMove;
    _userIndex[keyToMove] = rowToDelete;
    _userList.length--;

    emit removedFromBlocklist(account, msg.sender);

    return true;
  }

  // Operators
  function setOperator(address account, bool active) public onlyOperator {
    if (account == _master) {
      require(msg.sender == _master, "Only master");
    }

    operators[account] = active;
  }
}