// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

contract MultiSigWallet {
    event Deposit(address indexed sender, uint amount, uint balance);
    
    event AddOwner(address indexed sender,address indexed owner);
    event RemoveOwner(address indexed sender,address indexed owner);

    event SubmitTransaction(
        address indexed owner,
        TxType txType,
        uint indexed txIndex,
        address indexed to,
        uint value,
        bytes data
    );
    event ConfirmTransaction(address indexed owner, uint indexed txIndex);
    event RevokeConfirmation(address indexed owner, uint indexed txIndex);
    event ExecuteTransaction(address indexed owner, uint indexed txIndex);
    
    enum TxType {transfer, addOwner, removeOwner }
    address[] public owners;
    mapping(address => bool) public isOwner;

    uint public transferConfirmationsRequired = 70; // 70/100 70%
    uint public addOwnerConfirmationsRequired = 70; // 70/100 70%
    uint public removeOwnerConfirmationsRequired = 90; // 90/100 | 90%

    bytes4 private _transferSelector = bytes4(keccak256("transfer(address,uint256)"));
    struct Transaction {
        TxType txType;
        address baseToken;
        address to;
        uint value;
        bytes data;
        bool executed;
        uint numConfirmations;
    }

    // mapping from tx index => owner => bool
    mapping(uint => mapping(address => bool)) public isConfirmed;

    Transaction[] public transactions;

    modifier onlyOwner() {
        require(isOwner[msg.sender], "not owner");
        _;
    }

    modifier txExists(uint _txIndex) {
        require(_txIndex < transactions.length, "tx does not exist");
        _;
    }

    modifier notExecuted(uint _txIndex) {
        require(!transactions[_txIndex].executed, "tx already executed");
        _;
    }

    modifier notConfirmed(uint _txIndex) {
        require(!isConfirmed[_txIndex][msg.sender], "tx already confirmed");
        _;
    }

    constructor(address[] memory _owners) {
        require(_owners.length > 0, "owners required");

        for (uint i = 0; i < _owners.length; i++) {
            addOwner(_owners[i]);
        }
    }

    receive() external payable {
        emit Deposit(msg.sender, msg.value, address(this).balance);
    }

    function submitTransaction(
        TxType _txType,
        address _contract,
        address _to,
        uint _value,
        bytes memory _data
    ) public onlyOwner {
        uint txIndex = transactions.length;

        transactions.push(
            Transaction({
                txType: _txType,
                baseToken: _contract,
                to: _to,
                value: _value,
                data: _data,
                executed: false,
                numConfirmations: 0
            })
        );

        emit SubmitTransaction(msg.sender,_txType, txIndex, _to, _value, _data);
    }

    function confirmTransaction(
        uint _txIndex
    ) public onlyOwner txExists(_txIndex) notExecuted(_txIndex) notConfirmed(_txIndex) {
        Transaction storage transaction = transactions[_txIndex];
        transaction.numConfirmations += 1;
        isConfirmed[_txIndex][msg.sender] = true;

        emit ConfirmTransaction(msg.sender, _txIndex);
    }

    function executeTransaction(
        uint _txIndex
    ) public onlyOwner txExists(_txIndex) notExecuted(_txIndex) {
        Transaction storage transaction = transactions[_txIndex];
        if (transaction.txType == TxType.transfer) {
            require(
                transaction.numConfirmations >= (transferConfirmationsRequired*owners.length)/100,
                "cannot execute tx"
            );

            transaction.executed = true;
            if (transaction.baseToken == address(0) ) {
                (bool success, ) = transaction.to.call{value: transaction.value}(
                    transaction.data
                );
                require(success, "tx failed");
            }else {
                (bool success, ) = transaction.baseToken.call(abi.encodeWithSelector(_transferSelector, transaction.to, transaction.value));
                require(success, "tx failed");
            }
        }else if (transaction.txType == TxType.addOwner) {
            require(
                transaction.numConfirmations >= (addOwnerConfirmationsRequired*owners.length)/100,
                "cannot execute tx"
            );
            transaction.executed = true;
            bool success = addOwner(transaction.to);
            require(success, "tx failed");
        }else if (transaction.txType == TxType.removeOwner) {
            require(
                transaction.numConfirmations >= (removeOwnerConfirmationsRequired*owners.length)/100,
                "cannot execute tx"
            );
            transaction.executed = true;
            bool success = removeOwner(transaction.to);
            require(success, "tx failed");
        }else {
            require(false,"transaction not supported");
        }
        
        emit ExecuteTransaction(msg.sender, _txIndex);
    }

    function revokeConfirmation(uint _txIndex) public onlyOwner txExists(_txIndex) notExecuted(_txIndex) {
        Transaction storage transaction = transactions[_txIndex];

        require(isConfirmed[_txIndex][msg.sender], "tx not confirmed");

        transaction.numConfirmations -= 1;
        isConfirmed[_txIndex][msg.sender] = false;

        emit RevokeConfirmation(msg.sender, _txIndex);
    }

    function getOwners() public view returns (address[] memory) {
        return owners;
    }

    function getTransactionCount() public view returns (uint) {
        return transactions.length;
    }

    function getTransaction(
        uint _txIndex
    )
        public
        view
        returns (
            TxType txType,
            address to,
            uint value,
            bytes memory data,
            bool executed,
            uint numConfirmations
        )
    {
        Transaction storage transaction = transactions[_txIndex];

        return (
            transaction.txType,
            transaction.to,
            transaction.value,
            transaction.data,
            transaction.executed,
            transaction.numConfirmations
        );
    }

    function addOwner(address owner) internal returns (bool) {
        require(owner != address(0), "invalid owner");
        require(!isOwner[owner], "owner not unique");
        isOwner[owner] = true;
        owners.push(owner);
        emit AddOwner(msg.sender,owner);
        return true;
    }

    function removeOwner(address owner) internal returns (bool) {
        require(owner != address(0), "invalid owner");
        require(isOwner[owner], "owner is not exist");
        delete isOwner[owner];
        bool found;
        for (uint256 i = 0; i < owners.length - 1; i++) {
            if (owners[i]== owner) {
                found = true;
            }
            if (found) {
                owners[i] = owners[i + 1];
            }
        }
        owners.pop();
        emit RemoveOwner(msg.sender,owner);
        return true;
    }
}