/**
 *Submitted for verification at BscScan.com on 2023-07-17
*/

// ██████  ███████ ██    ██  ██████  ██      ██    ██ ███████ ██  ██████  ███    ██
// ██   ██ ██      ██    ██ ██    ██ ██      ██    ██     ██  ██ ██    ██ ████   ██
// ██████  █████   ██    ██ ██    ██ ██      ██    ██   ██    ██ ██    ██ ██ ██  ██
// ██   ██ ██       ██  ██  ██    ██ ██      ██    ██  ██     ██ ██    ██ ██  ██ ██
// ██   ██ ███████   ████    ██████  ███████  ██████  ███████ ██  ██████  ██   ████

// CONTRACT DEVELOPED BY REVOLUZION

//Revoluzion Ecosystem
//WEB: https://revoluzion.io
//DAPP: https://revoluzion.app

// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

/********************************************************************************************
  INTERFACE
********************************************************************************************/

interface IERC20 {
    // EVENT

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

    // FUNCTION

    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

interface IRouter {
    // FUNCTION

    function WETH() external pure returns (address);

    function factory() external pure returns (address);

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable;

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
        external
        payable
        returns (uint256 amountToken, uint256 amountETH, uint256 liquidity);
}

interface IAuthError {
    // ERROR

    error InvalidOwner(address account);

    error UnauthorizedAccount(address account);

    error InvalidAuthorizedAccount(address account);

    error CurrentAuthorizedState(address account, bool state);
}

interface ICommonError {
    // ERROR

    error CannotUseCurrentAddress(address current);

    error CannotUseCurrentValue(uint256 current);

    error CannotUseCurrentState(bool current);

    error InvalidAddress(address invalid);

    error InvalidValue(uint256 invalid);
}

interface IStaking {
    // FUNCTION

    function isWDYMStaking() external pure returns (bool);

    function userTotalStakes(address user) external returns (uint256);
}

interface IGovernance {
    // FUNCTION

    function totalProposals() external returns (uint256);

    function isWDYMGovernance() external pure returns (bool);

    function vote(
        uint256 proposalID,
        uint256 numberOfVote,
        bool voteChoice
    ) external;
}

/********************************************************************************************
  ACCESS
********************************************************************************************/

abstract contract Auth is IAuthError {
    // DATA

    address private _owner;

    // MAPPING

    mapping(address => bool) public authorization;

    // MODIFIER

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    modifier authorized() {
        _checkAuthorized();
        _;
    }

    // CONSTRUCCTOR

    constructor(address initialOwner) {
        _transferOwnership(initialOwner);
        authorize(initialOwner);
        if (initialOwner != msg.sender) {
            authorize(msg.sender);
        }
    }

    // EVENT

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    event UpdateAuthorizedAccount(
        address authorizedAccount,
        address caller,
        bool state,
        uint256 timestamp
    );

    // FUNCTION

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        if (owner() != msg.sender) {
            revert UnauthorizedAccount(msg.sender);
        }
    }

    function _checkAuthorized() internal view virtual {
        if (!authorization[msg.sender]) {
            revert UnauthorizedAccount(msg.sender);
        }
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        if (newOwner == address(0)) {
            revert InvalidOwner(address(0));
        }
        _transferOwnership(newOwner);
    }

    function authorize(address account) public virtual onlyOwner {
        if (account == address(0) || account == address(0xdead)) {
            revert InvalidAuthorizedAccount(account);
        }
        _authorization(account, msg.sender, true);
    }

    function unauthorize(address account) public virtual onlyOwner {
        if (account == address(0) || account == address(0xdead)) {
            revert InvalidAuthorizedAccount(account);
        }
        _authorization(account, msg.sender, false);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }

    function _authorization(
        address account,
        address caller,
        bool state
    ) internal virtual {
        if (authorization[account] == state) {
            revert CurrentAuthorizedState(account, state);
        }
        authorization[account] = state;
        emit UpdateAuthorizedAccount(account, caller, state, block.timestamp);
    }
}

/********************************************************************************************
  SECURITY
********************************************************************************************/

abstract contract Pausable {
    // DATA

    bool private _paused;

    // ERROR

    error EnforcedPause();

    error ExpectedPause();

    // MODIFIER

    modifier whenNotPaused() {
        _requireNotPaused();
        _;
    }

    modifier whenPaused() {
        _requirePaused();
        _;
    }

    // CONSTRUCTOR

    constructor() {
        _paused = false;
    }

    // EVENT

    event Paused(address account);

    event Unpaused(address account);

    // FUNCTION

    function paused() public view virtual returns (bool) {
        return _paused;
    }

    function pause() external virtual whenNotPaused {
        _pause();
    }

    function unpause() external virtual whenPaused {
        _unpause();
    }

    function _requireNotPaused() internal view virtual {
        if (paused()) {
            revert EnforcedPause();
        }
    }

    function _requirePaused() internal view virtual {
        if (!paused()) {
            revert ExpectedPause();
        }
    }

    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(msg.sender);
    }

    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(msg.sender);
    }
}

/********************************************************************************************
  GOVERNANCE
********************************************************************************************/

contract WhatDoYouMemeGovernance is Auth, Pausable, ICommonError, IGovernance {
    // DATA

    struct Proposal {
        string proposalInfo;
        uint256 startTime;
        uint256 endTime;
    }

    struct UserVotedProposal {
        string proposalInfo;
        uint256 proposalId;
        uint256 totalVoteTrue;
        uint256 totalVoteFalse;
    }

    IStaking public immutable stakingWDYM;

    uint256 public totalProposals;

    bool private constant ISWDYM_GOVERNANCE = true;

    // MAPPING

    mapping(uint256 => Proposal) public proposal;
    mapping(address => uint256[]) public addressVotedFor;
    mapping(uint256 => mapping(address => bool))
        public proposalToAddressVoteStatus;
    mapping(uint256 => mapping(bool => uint256)) public proposalVoteCount;
    mapping(uint256 => mapping(address => uint256))
        public proposalToAddressVoteTotal;
    mapping(uint256 => mapping(address => mapping(bool => uint256)))
        public proposalToAddressSpecificVoteTotal;

    // ERROR

    error InvalidTimeRange(uint256 start, uint256 end);

    error InvalidStartTime(uint256 start);

    error InvalidEndTime(uint256 end);

    error InvalidProposalID(uint256 invalid, uint256 max);

    error VotingNotYetStart(uint256 startTime);

    error VotingAlreadyEnd(uint256 endTime);

    // CONSTRUCTOR

    constructor(IStaking staking) Auth(msg.sender) {
        stakingWDYM = staking;
    }

    // EVENT

    // FUNCTION

    /* General */

    receive() external payable {}

    /* Rescue */

    function wTokens(address tokenAddress, uint256 amount) external onlyOwner {
        uint256 toTransfer = amount;
        if (tokenAddress == address(0)) {
            if (amount == 0) {
                toTransfer = address(this).balance;
            }
            payable(owner()).transfer(toTransfer);
        } else {
            if (amount == 0) {
                toTransfer = IERC20(tokenAddress).balanceOf(address(this));
            }
            require(
                IERC20(tokenAddress).transfer(msg.sender, toTransfer),
                "WithdrawTokens: Transfer transaction might fail."
            );
        }
    }

    /* Check */

    function isWDYMGovernance() external pure override returns (bool) {
        return ISWDYM_GOVERNANCE;
    }

    function currentTimestamp() external view returns (uint256) {
        return block.timestamp;
    }

    function getAllProposals() external view returns (Proposal[] memory) {
        Proposal[] memory list = new Proposal[](totalProposals);
        for (uint256 i = 0; i < totalProposals; i++) {
            list[i] = proposal[i + 1];
        }
        return list;
    }

    function getAllUserVotedProposals(
        address account
    ) external view returns (UserVotedProposal[] memory) {
        UserVotedProposal[] memory list = new UserVotedProposal[](
            addressVotedFor[account].length
        );
        for (uint256 i = 0; i < addressVotedFor[account].length; i++) {
            UserVotedProposal memory item = UserVotedProposal({
                proposalInfo: proposal[addressVotedFor[account][i] - 1]
                    .proposalInfo,
                proposalId: addressVotedFor[account][i],
                totalVoteTrue: proposalToAddressSpecificVoteTotal[
                    addressVotedFor[account][i]
                ][msg.sender][true],
                totalVoteFalse: proposalToAddressSpecificVoteTotal[
                    addressVotedFor[account][i]
                ][msg.sender][false]
            });
            list[i] = item;
        }
        return list;
    }

    /* Proposal */

    function createProposal(
        uint256 timeStart,
        uint256 timeEnd,
        string memory info
    ) external whenNotPaused authorized {
        if (
            block.timestamp + 5 minutes >= timeStart &&
            timeStart + 30 minutes > timeEnd
        ) {
            revert InvalidTimeRange(timeStart, timeEnd);
        }
        if (block.timestamp + 5 minutes >= timeStart) {
            revert InvalidStartTime(timeStart);
        }
        if (timeStart + 30 minutes > timeEnd) {
            revert InvalidEndTime(timeEnd);
        }

        totalProposals += 1;
        proposal[totalProposals].startTime = timeStart;
        proposal[totalProposals].endTime = timeEnd;
        proposal[totalProposals].proposalInfo = info;
    }

    function editProposalInfo(
        uint256 proposalID,
        string memory info
    ) external authorized {
        if (proposalID <= 0 || proposalID > totalProposals) {
            revert InvalidProposalID(proposalID, totalProposals);
        }
        if (block.timestamp >= proposal[proposalID].startTime) {
            revert InvalidStartTime(proposal[proposalID].startTime);
        }

        proposal[proposalID].proposalInfo = info;
    }

    function editProposalTimeInfo(
        uint256 proposalID,
        uint256 timeStart,
        uint256 timeEnd
    ) external authorized {
        if (
            block.timestamp + 5 minutes >= timeStart &&
            timeStart + 30 minutes > timeEnd
        ) {
            revert InvalidTimeRange(timeStart, timeEnd);
        }
        if (
            block.timestamp + 5 minutes >= timeStart ||
            block.timestamp >= proposal[proposalID].startTime
        ) {
            revert InvalidStartTime(timeStart);
        }
        if (timeStart + 30 minutes > timeEnd) {
            revert InvalidEndTime(timeEnd);
        }
        if (proposalID <= 0 || proposalID > totalProposals) {
            revert InvalidProposalID(proposalID, totalProposals);
        }

        proposal[proposalID].startTime = timeStart;
        proposal[proposalID].endTime = timeEnd;
    }

    /* Vote */

    function vote(
        uint256 proposalID,
        uint256 numberOfVote,
        bool voteChoice
    ) external override whenNotPaused {
        if (proposalID <= 0 || proposalID > totalProposals) {
            revert InvalidProposalID(proposalID, totalProposals);
        }
        if (block.timestamp <= proposal[proposalID].startTime) {
            revert VotingNotYetStart(proposal[proposalID].startTime);
        }
        if (block.timestamp > proposal[proposalID].endTime) {
            revert VotingAlreadyEnd(proposal[proposalID].endTime);
        }
        if (numberOfVote <= 0) {
            revert InvalidValue(numberOfVote);
        }

        uint256 voteUsed = numberOfVote;
        uint256 availableVote = stakingWDYM.userTotalStakes(msg.sender) -
            proposalToAddressVoteTotal[proposalID][msg.sender];

        if (numberOfVote > availableVote) {
            voteUsed = availableVote;
        }

        if (!(proposalToAddressVoteStatus[proposalID][msg.sender])) {
            proposalToAddressVoteStatus[proposalID][msg.sender] = true;
            addressVotedFor[msg.sender].push(proposalID);
        }

        proposalVoteCount[proposalID][voteChoice] += voteUsed;
        proposalToAddressVoteTotal[proposalID][msg.sender] += voteUsed;
        proposalToAddressSpecificVoteTotal[proposalID][msg.sender][
            voteChoice
        ] += voteUsed;
    }
}