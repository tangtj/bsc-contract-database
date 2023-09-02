// SPDX-License-Identifier: MIT

pragma solidity ^0.8.6;

interface IERC20Uniswap {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);
}

abstract contract ReentrancyGuard {
    uint256 private constant NOT_ENTERED = 1;
    uint256 private constant ENTERED = 2;
    uint256 private _status;
    error ReentrancyGuardReentrantCall();
    constructor() {
        _status = NOT_ENTERED;
    }

    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {

        if (_status == ENTERED) {
            revert ReentrancyGuardReentrantCall();
        }
        _status = ENTERED;
    }

    function _nonReentrantAfter() private {
        _status = NOT_ENTERED;
    }

    function _reentrancyGuardEntered() internal view returns (bool) {
        return _status == ENTERED;
    }
}

contract QWebStakingDAO is IERC20Uniswap, ReentrancyGuard {
    address public owner;
    IERC20Uniswap public stakingToken;
    bool public emergencyFlag = false;

    uint256 public defaultStakingTime = 3 weeks;
    uint256 public minimumStakingTime = 1 days;

    mapping(address => uint256) public stakedAmount;
    mapping(uint256 => mapping(address => bool)) public hasVoted;
    mapping(address => uint256) public activeProposals;
    mapping(address => bool) public activeStakers;
    mapping(address => uint256) public stakedTimestamps;

    uint256 public totalStaked;

    string public override name = "Quantums DAO Shares";
    string public override symbol = "QDS";
    uint8 public override decimals = 18;
    uint256 public override totalSupply;
    mapping(address => uint256) public override balanceOf;
    mapping(address => mapping(address => uint256)) public override allowance;

    struct Proposal {
        uint256 proposalId;
        address proposer;
        address beneficiary;
        uint256 capital;
        uint256 endTime;
        uint256 votesFor;
        uint256 votesAgainst;
        bool executed;
    }

    Proposal[] public proposals;
    address[] public stakers;

    event Staked(address indexed user, uint256 amount);
    event Unstaked(address indexed user, uint256 amount);
    event ProposalCreated(uint256 proposalId, address indexed proposer, address beneficiary, uint256 capital);
    event Voted(uint256 proposalId, address indexed user, bool inFavor);
    event ProposalExecuted(uint256 proposalId);

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");
        _;
    }

    modifier onlyEOA() {
        require(tx.origin == msg.sender, "Must be an EOA");
        require(address(msg.sender).code.length == 0, "Must be an EOA");
        _;
    }

    constructor(address _stakingToken) {
        stakingToken = IERC20Uniswap(_stakingToken);
        owner = msg.sender;
    }

    function stake(uint256 amount) external onlyEOA nonReentrant {
        require(amount > 0, "Cannot stake zero tokens.");

        stakingToken.transferFrom(msg.sender, address(this), amount);
        stakedAmount[msg.sender] += amount;
        totalStaked += amount;

        if (!activeStakers[msg.sender]) {
            stakers.push(msg.sender);
            activeStakers[msg.sender] = true;
        }

        stakedTimestamps[msg.sender] = block.timestamp;
        mint(msg.sender, amount);
        emit Staked(msg.sender, amount);
    }

    function unstake() external onlyEOA nonReentrant {
        uint256 userStake = stakedAmount[msg.sender];
        require(userStake > 0, "You do not have any tokens staked.");

        stakedAmount[msg.sender] = 0;
        totalStaked -= userStake;

        burn(msg.sender, userStake);

        stakingToken.transfer(msg.sender, userStake);

        activeStakers[msg.sender] = false;

        emit Unstaked(msg.sender, userStake);
    }

    function emergencyUnstake() external {
        require(emergencyFlag, "We are not in an emergency");
        uint256 amount = stakedAmount[msg.sender];
        burn(msg.sender, amount);
        stakedAmount[msg.sender] = 0;
        stakingToken.transfer(msg.sender, amount);
    }

    function createProposal(address _beneficiary, uint256 _amount, uint256 _duration) external {
        require(balanceOf[msg.sender] > 0, "Must hold DAO tokens to create a proposal.");
        require(activeProposals[msg.sender] < 3, "Can have at most 3 active proposals.");

        uint256 endTime = block.timestamp + _duration;
        proposals.push(Proposal({
            proposalId: proposals.length,
            proposer: msg.sender,
            beneficiary: _beneficiary,
            capital: _amount,
            endTime: endTime,
            votesFor: 0,
            votesAgainst: 0,
            executed: false
        }));

        activeProposals[msg.sender] += 1;

        emit ProposalCreated(proposals.length, msg.sender, _beneficiary, _amount);
    }

    function vote(uint256 _proposalId, bool _inFavor) external {
        require(balanceOf[msg.sender] > 0, "Must hold DAO tokens to vote!");
        require(_proposalId < proposals.length, "Invalid proposal ID!");

        Proposal storage proposal = proposals[_proposalId];
        require(block.timestamp <= proposal.endTime, "Voting for this proposal has ended!");
        require(!hasVoted[_proposalId][msg.sender], "Already voted on this proposal!");

        if (_inFavor) {
            proposal.votesFor += balanceOf[msg.sender];
        } else {
            proposal.votesAgainst += balanceOf[msg.sender];
        }

        hasVoted[_proposalId][msg.sender] = true;

        emit Voted(_proposalId, msg.sender, _inFavor);
    }

    function executeProposal(uint _proposalId) external {
        require(balanceOf[msg.sender] > 0, "Must hold DAO tokens to execute!");
        require(_proposalId < proposals.length, "Invalid proposal ID");
        Proposal storage proposal = proposals[_proposalId];
        require(block.timestamp > proposal.endTime, "Proposal still active");
        require(!proposal.executed, "Proposal already executed");
        require(proposal.votesFor > proposal.votesAgainst, "Proposal not approved");

        proposal.executed = true;
        activeProposals[proposal.proposer] -= 1;
        emit ProposalExecuted(_proposalId);
    }

    function distributeRewards() external onlyOwner {
        uint256 totalReward = address(this).balance;
        for (uint256 i = 0; i < stakers.length; i++) {
            address staker = stakers[i];
            if (activeStakers[staker] && block.timestamp >= stakedTimestamps[staker] + minimumStakingTime) {
                uint256 reward = totalReward * balanceOf[staker] / totalSupply;
                payable(staker).transfer(reward);
            }
        }
    }

    function depositEther() external payable {
        require(msg.value > 0, "Must send Ether");
    }

    function withdrawEther() external onlyOwner {
        payable(owner).transfer(address(this).balance);
    }

    function setEmergencyFlag(bool _status) external onlyOwner {
        emergencyFlag = _status;
    }

    function setDefaultStakingTime(uint256 _time) external onlyOwner {
        defaultStakingTime = _time;
    }

    function setMinimumStakingTime(uint256 _time) external onlyOwner {
        minimumStakingTime = _time;
    }

    function mint(address account, uint256 amount) internal {
        balanceOf[account] += amount;
        totalSupply += amount;
        emit Transfer(address(0), account, amount);
    }

    function burn(address account, uint256 amount) internal {
        balanceOf[account] -= amount;
        totalSupply -= amount;
        emit Transfer(account, address(0), amount);
    }

    function transfer(address, uint256) public pure override returns (bool) {
        revert("QDS transfers are disabled");
    }

    function transferFrom(address, address, uint256) public pure override returns (bool) {
        revert("QDS transfers are disabled");
    }

    function approve(address /*spender*/, uint256 /*amount*/) public pure override returns (bool) {
        revert("QDS approvals are disabled");
    }
}