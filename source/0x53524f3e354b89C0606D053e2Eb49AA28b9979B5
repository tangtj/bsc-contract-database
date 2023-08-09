// SPDX-License-Identifier: GPL-2.0

pragma solidity 0.8.17;

interface IBEP20 {
    function totalSupply() external view returns (uint256);
    function decimals() external view returns (uint8);
    function symbol() external view returns (string memory);
    function name() external view returns (string memory);
    function getOwner() external view returns (address);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address _owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
   
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract EscrowHandler {
    IBEP20 private immutable _token;
    struct OrderItem {
        uint256 orderNumber;
        address depositor;
        address beneficiary;
        uint256 amount;
        uint256 feePercent; // 100 = 1%
        uint256 status; // [0, 1, 2] = [new, done, failed]
    }
    address private _operator;
    address private _owner;
    uint256 private _platformFeePercent;
    uint256 private _totalFee;
    mapping (bytes32 => OrderItem) private orders;

    event Deposited(bytes32 key);
    event Withdrawn(bytes32 key, address beneficiary);
    event OrderProcessed(bytes32 key, uint256 status);
    event OperatorChanged(address indexed newOperator);

    constructor (address _operationToken) {
        _token = IBEP20 (_operationToken);

        _owner = msg.sender;
        _operator = msg.sender;
        _platformFeePercent = 200;
    }

    modifier onlyOperator {
      require(msg.sender == _operator, "EscrowHandler: only for operator");
      _;
    }

    modifier onlyOwner {
      require(msg.sender == _owner, "EscrowHandler: only for owner");
      _;
    }

    function getOwner() external view returns(address) {
        return _owner;
    }

    function getOperator() external view returns(address) {
        return _operator;
    }

    function getOrderItem(bytes32 key) external view returns (OrderItem memory) {
        return orders[key];
    }

    function changeOperator(address newOperator) external onlyOwner {
        _operator = newOperator;
        emit OperatorChanged(newOperator);
    }

    function getFeePercent() external view returns (uint256) {
        return _platformFeePercent;
    }

    function setFeePercent(uint256 percent) external onlyOwner {
        require(percent > 0 && percent < 5000, "EscrowHandler: must less than 5000 (50%)");
        _platformFeePercent = percent;
    }

    function getTotalFee() external view returns (uint256) {
        return _totalFee;
    }

    /*
    *   buyer deposits to escrow (place order)
    */
    function deposit(bytes32 key, uint256 orderNumber, address beneficiary, uint256 amount) external {
        require(orders[key].beneficiary == address(0), "EscrowHandler: transaction existed");
        _token.transferFrom(msg.sender, address(this), amount);
        orders[key] = OrderItem ({
            orderNumber: orderNumber,
            depositor: msg.sender,
            beneficiary: beneficiary,
            amount: amount,
            feePercent: _platformFeePercent,
            status: 0
        });
        emit Deposited(key);
    }

    /*
    *   seller withdraws from escrow (order success, fee applied)
    */
    function withdraw(bytes32 key) external {
        OrderItem storage order = orders[key];
        require(order.amount > 0, "EscrowHandler: zero balance");
        require(order.beneficiary == msg.sender, "EscrowHandler: wrong beneficiary");
        require(order.status == 1, "EscrowHandler: not applicable for withdraw");
        uint256 feeAmount = (order.amount * order.feePercent) / 10000;
        _token.transfer(msg.sender, order.amount - feeAmount);
        _totalFee += feeAmount;//fee = remaining amount.
        order.amount = 0;// reset amount
        emit Withdrawn(key, msg.sender);
    }
    /*
    *   admin withdraw platform fee
    */
    function adminWithdraw(address beneficiary, uint256 amount) onlyOwner external {
        require(_totalFee >= amount, "EscrowHandler: not enough amount to withdraw");
        _token.transfer(beneficiary, amount);
        _totalFee -= amount;
        emit Withdrawn(0, beneficiary);
    }

    /*
    *   buyer withdraw from escrow (order not success, no fee)
    */
    function claim(bytes32 key) external {
        OrderItem storage order = orders[key];
        require(order.amount > 0, "EscrowHandler: zero balance");
        require(order.depositor == msg.sender, "EscrowHandler: wrong depositor");
        require(order.status == 2, "EscrowHandler: not applicable for claim");
        _token.transfer(msg.sender, order.amount);
        order.amount = 0;
        emit Withdrawn(key, msg.sender);
    }

    /*
    *   operator processes order
    */
    function processOrder(bytes32 key, uint256 status) onlyOperator external {
        OrderItem storage order = orders[key];
        require(order.status != status, "EscrowHandler: already processed");
        require(order.amount > 0, "EscrowHandler: zero balance");
        order.status = status;
        emit OrderProcessed(key, status);
    }

    /*
    *   claim tokens accidentially dropped to the vault
    */
    function withdrawVisitingToken(address _visitingToken) external onlyOwner {
        require(_visitingToken != address(_token), "only available to visiting token");
        uint256 amount = IBEP20(_visitingToken).balanceOf(address(this));
        IBEP20(_visitingToken).transfer(msg.sender, amount);
    }
}

/*
    *           *   * * * * * * *   *       *
    * *       * *         *         *     *  
    *   *   *   *         *         *   *    
    *     *     *         *         * *      
    *           *         *         *   *    
    *           *         *         *     *  
    *           *         *         *       *
  contact @mtkungfu on telegram for suggestions
 */