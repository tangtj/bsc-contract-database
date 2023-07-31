// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
}

contract FreelanceContract {   

    mapping(uint256 => Order) public orders;
    mapping(uint256 => uint256) public escrowBalances;
    mapping(uint256 => Proposal[]) public orderProposals;
    mapping(address => bool) public bannedCustomers;
    mapping(address => bool) public bannedExecutors;
    mapping(address => bool) public admins;
    mapping(address => uint256) public customerOrderCount;
    mapping(address => uint256) public executorOrderCount;
    mapping(address => uint256) public executorWrongOrderCount;

    event OrderCreated(address indexed creator, uint256 indexed orderIndex);
    event PersonalOrderCreated(address customer, uint256 orderId, address assignedExecutor);
    event OrderCompleted(uint256 orderId);
    event OrderCancelled(uint256 orderId);
    event IpfsHashSet(uint256 orderId, string ipfsHash);
    event ProposalSubmitted(uint256 orderId, address executor);
    event ProposalAccepted(uint256 orderId, address executor);
    event TokensDeposited(uint256 orderId, uint256 amount);
    event TokensWithdrawn(uint256 orderId, uint256 amount);
    event AdminAdded(address adminAddress);
    event AdminRemoved(address adminAddress);
    event PaymentRejected(uint256 orderId);
    event OrderConfirmed(uint256 orderId);
    event ExecutorUnbanned(address indexed executor);
    event CustomerUnbanned(address indexed customer);
    event ExecutorBanned( address indexed executor);
    event CustomerBanned( address indexed customer);

    uint256 public orderCount;
    uint256 public orderIndex;
    address public owner;
    IERC20 private _token;

    constructor(address tokenAddress) {
        _token = IERC20(tokenAddress);
        owner = msg.sender;
        admins[owner] = true;
    }

    modifier onlyAdmin() {
        require(admins[msg.sender] || msg.sender == owner, "Only admins or the owner can perform this action");
        _;
    }

    modifier onlyCustomerOrAdmin(uint256 orderId) {
        Order storage order = orders[orderId];
        require(
            msg.sender == order.customer || msg.sender == owner || admins[msg.sender],
            "Only the order customer, owner or admin can perform this action"
        );
        _;
    }

    struct Order {
        address payable customer;
        string description;
        string fullDescription;
        string category;
        uint256 price;
        bool completed;
        bool rejected;
        bool confirmed;
        address payable assignedExecutor;
        string ipfsHash;
    }

    struct Proposal {
        address payable executor;
        uint256 orderId;
        string message;
        bool accepted;
    }

    function addAdmin(address adminAddress) public onlyAdmin {
        admins[adminAddress] = true;
        emit AdminAdded(adminAddress);
    }

    function removeAdmin(address adminAddress) public onlyAdmin {
        require(adminAddress != owner, "Cannot remove contract owner as admin");
        admins[adminAddress] = false;
        emit AdminRemoved(adminAddress);
    }

    function createOrder(
            string memory _description,
            string memory _fullDescription,
            string memory _category,
            uint256 _price,
            address payable _assignedExecutor
        ) public {
            uint256 newOrderIndex = orderCount;
            orders[newOrderIndex] = Order(
                payable(msg.sender),
                _description,
                _fullDescription,
                _category,
                _price,
                false,
                false,
                false,
                _assignedExecutor,
                ""
            );
            orderCount++;
            customerOrderCount[msg.sender]++;
            if (_assignedExecutor != address(0)) {
                emit PersonalOrderCreated(msg.sender, newOrderIndex, _assignedExecutor);
            } else {
                emit OrderCreated(msg.sender, newOrderIndex);
            }
    }

    function submitProposal(uint256 orderId, string memory message) public {
        Order storage order = orders[orderId];
        require(order.assignedExecutor == address(0), "Order is a personal order");
        uint256 proposalCount = orderProposals[orderId].length;
        for (uint256 i = 0; i < proposalCount; i++) {
            if (orderProposals[orderId][i].executor == msg.sender) {
                revert("You have already submitted a proposal for this order");
            }
        }
        orderProposals[orderId].push(Proposal(payable(msg.sender), orderId, message, false));
        emit ProposalSubmitted(orderId, msg.sender);
    }

    function acceptProposal(uint256 orderId, uint256 proposalIndex) public {
        Order storage order = orders[orderId];
        require(msg.sender == order.customer, "Only the customer can accept a proposal");
        require(order.assignedExecutor == address(0), "Order is a personal order");
        Proposal storage proposal = orderProposals[orderId][proposalIndex];
        require(!proposal.accepted, "Proposal is already accepted");
        proposal.accepted = true;
        order.assignedExecutor = proposal.executor;
        emit ProposalAccepted(orderId, proposal.executor);
    }

    function getOrderProposals(uint256 orderId) public view returns (Proposal[] memory) {
        return orderProposals[orderId];
    }

    function cancelOrder(uint256 orderId) public {
        Order storage order = orders[orderId];
        require(!order.completed, "Order is completed");
        require(msg.sender == order.customer, "Only the customer can cancel the order");
        order.completed = true;
        emit OrderCancelled(orderId);
    }

    function setIpfsHash(uint256 orderId, string memory ipfsHash) public {
        require(msg.sender == orders[orderId].customer, "Only the order customer can set the IPFS hash");
        orders[orderId].ipfsHash = ipfsHash;
        emit IpfsHashSet(orderId, ipfsHash);
    }

    function depositTokens(uint256 orderId, uint256 amount) public {
        Order storage order = orders[orderId];
        require(msg.sender == order.customer, "Only the order customer can deposit tokens");
        require(_token.allowance(msg.sender, address(this)) >= amount, "Insufficient allowance");
        require(_token.transferFrom(msg.sender, address(this), amount), "Transfer failed");
        escrowBalances[orderId] += amount;
        emit TokensDeposited(orderId, amount);
    }

    function rejectPayment(uint256 orderId) public {
        Order storage order = orders[orderId];
        require(msg.sender == order.assignedExecutor, "Only the assigned executor can reject payment");
        require(!order.completed, "Order is already completed");
        require(!order.rejected, "Payment is already rejected");
        order.rejected = true;
        emit PaymentRejected(orderId); 
    }

    function confirmCompletion(uint256 orderId) public {
        Order storage order = orders[orderId];
        require(msg.sender == order.assignedExecutor, "Only the assigned executor can confirm completion");
        require(!order.completed, "Order is already completed");
        require(!order.rejected, "Payment was rejected by the assigned executor");
        require(!order.confirmed, "Order is already confirmed");
        order.confirmed = true;
        emit OrderConfirmed(orderId);
    }

    function withdrawTokensToExecutor(uint256 orderId) public onlyCustomerOrAdmin(orderId) {
        Order storage order = orders[orderId];
        require(order.confirmed, "Executor has not confirmed the order yet");
        require(order.assignedExecutor != address(0), "No assigned executor");
        uint256 amount = escrowBalances[orderId];
        require(amount > 0, "No tokens to withdraw");
        escrowBalances[orderId] = 0;
        require(_token.transfer(order.assignedExecutor, amount), "Transfer failed");
        order.completed = true;
        emit TokensWithdrawn(orderId, amount);
        executorOrderCount[order.assignedExecutor]++;
    }

    function withdrawTokens(uint256 orderId) public onlyCustomerOrAdmin(orderId) {
        Order storage order = orders[orderId];
        require(order.rejected, "Executor has not rejected the order yet");
        uint256 amount = escrowBalances[orderId];
        require(amount > 0, "No tokens to withdraw");
        escrowBalances[orderId] = 0;
        require(_token.transfer(order.customer, amount), "Transfer failed");
        order.completed = true;
        emit TokensWithdrawn(orderId, amount);
        executorWrongOrderCount[order.assignedExecutor]++;
    }

    function adminConfirmCompletion(uint256 orderId) public onlyAdmin {
        Order storage order = orders[orderId];
        order.confirmed = !order.confirmed;
        emit OrderConfirmed(orderId);
    }

    function adminRejectPayment(uint256 orderId) public onlyAdmin {
        Order storage order = orders[orderId];
        order.rejected = !order.rejected;
        emit PaymentRejected(orderId); 
    }

    function adminCompleted(uint256 orderId) public onlyAdmin {
        Order storage order = orders[orderId];
        order.completed = !order.completed;
        emit OrderCompleted(orderId); 
    }

    function banCustomer(address customer) public onlyAdmin {
        bannedCustomers[customer] = true;
        emit CustomerBanned(customer);
    }

    function banExecutor(address executor) public onlyAdmin {
        bannedExecutors[executor] = true;
        emit ExecutorBanned(executor);
    }

    function unbanCustomer(address customer) public onlyAdmin {
        bannedCustomers[customer] = false;
        emit CustomerUnbanned(customer);
    }

    function unbanExecutor(address executor) public onlyAdmin {
        bannedExecutors[executor] = false;
        emit ExecutorUnbanned(executor);
    }
    
}