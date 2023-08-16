// SPDX-License-Identifier: MIT

pragma solidity 0.8.17;


/*
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with GSN meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}


pragma solidity ^0.8.0;

/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner account will be the one that deploys the contract. This
 * can later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

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
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}


contract BlazexRegistry is Ownable {

    struct Contracts {
        address contractAddress;
        uint112 chainId;
    }
    
    Contracts[] public contractsDeployed;

    struct ContractDetail {
        uint112 createdAt;
        uint8 contractType;
        address createdBy;
        address referralWallet;
        uint256 referralDollarValue;
        uint256 serviceFeeDollarValue;
        address lpPair;
        bytes32 hash;
    }

    address public manager;
    uint256 public gasNeeded;

    modifier onlyManager {
        require(address(msg.sender) == manager, "You are not manager");
        _;
    }

    receive() external payable {
        if(manager.balance < gasNeeded) {
            payable(manager).transfer(gasNeeded - manager.balance);
        }
    }

    mapping(uint256 => mapping(address => ContractDetail)) public contractDetails;

    uint256 public totalReferralDollarValueMainnet;
    uint256 public totalServiceFeeDollarValueMainnet;


    event ContractCreated(address contractAddress, address createdBy, uint8 contractType, uint112 createdAt, uint112 chainId, address referralWallet, uint256 referralDollarValue, uint256 serviceFeeDollarValue, address lpPair, bytes32 hash);

    constructor() { 
    }

    function addContract(address createdBy, address _contractAddress, uint112 createdAt, uint8 contractType, uint112 chainId,  address _referralWallet, uint256 _referralDollarValue, uint256 _serviceFeeDollarValue, address _lpPair, bytes32 _hash) public onlyManager {

        contractsDeployed.push(Contracts(_contractAddress, chainId));
        contractDetails[chainId][_contractAddress] = ContractDetail(createdAt, contractType, createdBy, _referralWallet, _referralDollarValue, _serviceFeeDollarValue, _lpPair, _hash);
        totalReferralDollarValueMainnet += _referralDollarValue;
        totalServiceFeeDollarValueMainnet += _serviceFeeDollarValue;

        if(address(manager).balance < gasNeeded) {
            payable(manager).transfer(gasNeeded - address(manager).balance);
        }

        emit ContractCreated(_contractAddress, createdBy, contractType, createdAt, chainId, _referralWallet, _referralDollarValue, _serviceFeeDollarValue, _lpPair, _hash);
    }

    function getContractDetails(address _contractAddress, uint112 chainId) public view returns(ContractDetail memory) {
        return contractDetails[chainId][_contractAddress];
    }

    function getContractDeployedCount() public view returns(uint256) {
        return contractsDeployed.length;
    }

    function getAllContractDeployed() public view returns(Contracts[] memory) {
        return contractsDeployed;
    }

    function changeManager(address _manager) external onlyOwner() {
        manager = _manager;
    }

    function withdrawEth(uint256 _amount) external payable onlyOwner {
        payable(owner()).transfer(_amount);
    }

    function withdrawAllEth() external payable onlyOwner {
        payable(owner()).transfer(address(this).balance);
    }

    function updateGasNeeded(uint256 _gasCost) external onlyOwner {
        gasNeeded = _gasCost;
    }
}