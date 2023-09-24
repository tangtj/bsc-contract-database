// Sources flattened with hardhat v2.13.0 https://hardhat.org

// File contracts/interface/IScrowBay.sol

// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.9;

/**
  @dev this is the interface of the users.
 */

interface IScrowBay {
    enum Status {
        Locked,
        Pending,
        Resolved,
        Cancelled,
        Disputed,
        Released
    }

    struct Transaction {
        uint256 id;
        bytes32 title;
        address buyer;
        address seller;
        address token;
        uint256 amount;
        uint256 time;
        Status status;
    }

    function createTransaction(
        bytes32 _title,
        address _seller,
        address _token,
        uint256 _amount
    ) external returns (uint256);

    function getTransaction(
        uint256 _transactionId
    ) external view returns (Transaction memory);

    function releaseTransaction(uint256 _transactionId) external;

    function cancelTransaction(uint256 _transactionId) external;

    function lock(uint256 _transactionId) external;

    function execute(
        uint256 _transactionId,
        uint256 buyerPercent,
        uint256 sellerPercent
    ) external;

    function dispute(uint256 _transactionId) external;

    function getTransactions() external view returns (Transaction[] memory);

    function getBuyerTransactions() external view returns (uint256[] memory);

    function getSellerTransactions() external view returns (uint256[] memory);

    function setProtocolFee(uint256 _fee) external;

    function setDisputeFee(uint256 _fee) external;

    function setAdmin(address _address) external;

    function setFeeAddress(address _address) external;
}


// File contracts/interface/tokens/IERC20.sol

pragma solidity ^0.8.9;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}


// File contracts/contract/ScrowBay.sol

pragma solidity ^0.8.9;
contract ScrowBay is IScrowBay {
    Transaction[] public transactions;
    address private feeCollector;
    address private admin;
    uint256 private protocolFee = 50;
    uint256 private disputeFee = 20;
    mapping(address => uint256[]) private buyerTransactions;
    mapping(address => uint256[]) private sellerTransactions;

    constructor() {
        feeCollector = msg.sender;
        admin = msg.sender;
    }

    function setProtocolFee(uint256 _fee) public override {
        require(msg.sender == admin, "Not Admin");
        protocolFee = _fee;
    }

    function setDisputeFee(uint256 _fee) public override {
        require(msg.sender == admin, "Not Admin");
        disputeFee = _fee;
    }

    function setAdmin(address _address) public override {
        require(msg.sender == admin, "Not Admin");
        admin = _address;
    }

    function setFeeAddress(address _address) public override {
        require(msg.sender == admin, "Not Admin");
        feeCollector = _address;
    }

    function createTransaction(
        bytes32 _title,
        address _seller,
        address _token,
        uint256 _amount
    ) public override returns (uint256) {
        require(_seller != address(0), "Can not send token to zero address.");
        IERC20 token = IERC20(_token);
        require(
            token.transferFrom(msg.sender, address(this), _amount),
            "Token transfer failed."
        );

        Transaction memory newTransaction = Transaction(
            transactions.length,
            _title,
            msg.sender,
            _seller,
            _token,
            _amount,
            block.timestamp,
            Status.Pending
        );

        transactions.push(newTransaction);
        uint256 id = transactions.length - 1;
        buyerTransactions[msg.sender].push(id);
        sellerTransactions[_seller].push(id);
        return id;
    }

    function getTransaction(
        uint256 _transactionId
    ) public view override returns (Transaction memory) {
        Transaction memory transaction = transactions[_transactionId];
        return transaction;
    }

    function releaseTransaction(uint256 _transactionId) public override {
        Transaction storage transaction = transactions[_transactionId];
        require(
            msg.sender == transaction.buyer,
            "You are not a buyer of this transaction."
        );
        require(
            transaction.status == Status.Pending,
            "Only can release when the transaction is pending."
        );
        IERC20 token = IERC20(transaction.token);
        require(
            token.transfer(
                transaction.seller,
                (transaction.amount * (1000 - protocolFee)) / 1000
            ),
            "Failed token transfer"
        );
        require(
            token.transfer(
                feeCollector,
                (transaction.amount * protocolFee) / 1000
            ),
            "Failed token transfer"
        );
        transaction.time = block.timestamp;
        transaction.status = Status.Released;
    }

    function cancelTransaction(uint256 _transactionId) public override {
        Transaction storage transaction = transactions[_transactionId];
        require(
            msg.sender == transaction.buyer,
            "You are not a buyer of this transaction."
        );
        require(
            transaction.status == Status.Locked,
            "This is not Locked transaction"
        );
        IERC20 token = IERC20(transaction.token);
        require(
            token.transfer(
                transaction.buyer,
                (transaction.amount * (1000 - disputeFee)) / 1000
            ),
            "Failed token transfer"
        );
        require(
            token.transfer(
                feeCollector,
                (transaction.amount * disputeFee) / 1000
            ),
            "Failed token transfer"
        );
        transaction.status = Status.Cancelled;
    }

    function lock(uint256 _transactionId) public override {
        require(msg.sender == admin, "You are not admin.");
        Transaction storage transaction = transactions[_transactionId];
        require(
            transaction.status == Status.Disputed,
            "Admin can lock to disputed transactions only."
        );
        transaction.status = Status.Locked;
    }

    function execute(
        uint256 _transactionId,
        uint256 buyerPercent,
        uint256 sellerPercent
    ) public override {
        require(msg.sender == admin, "You are not admin.");
        Transaction storage transaction = transactions[_transactionId];
        require(
            transaction.status == Status.Disputed,
            "Admin can lock to disputed transactions only."
        );
        IERC20 token = IERC20(transaction.token);
        require(
            token.transfer(
                transaction.buyer,
                (transaction.amount * buyerPercent) / 1000
            ),
            "Failed token transfer."
        );
        require(
            token.transfer(
                transaction.seller,
                (transaction.amount * sellerPercent) / 1000
            ),
            "Failed token transfer"
        );
        require(
            token.transfer(
                feeCollector,
                (transaction.amount * (protocolFee + disputeFee)) / 1000
            ),
            "Failed token transfer"
        );
        transaction.status = Status.Resolved;
    }

    function dispute(uint256 _transactionId) public override {
        Transaction storage transaction = transactions[_transactionId];
        require(
            msg.sender == transaction.buyer || msg.sender == transaction.seller,
            "You have no authority to this transaction."
        );
        require(
            transaction.status == Status.Pending,
            "You can only call when the status is pending."
        );
        transaction.status = Status.Disputed;
    }

    function getTransactions()
        public
        view
        override
        returns (Transaction[] memory)
    {
        require(msg.sender == admin, "Only Admin can see the transactions.");
        return transactions;
    }

    function getBuyerTransactions()
        public
        view
        override
        returns (uint256[] memory)
    {
        return buyerTransactions[msg.sender];
    }

    function getSellerTransactions()
        public
        view
        override
        returns (uint256[] memory)
    {
        return sellerTransactions[msg.sender];
    }
}