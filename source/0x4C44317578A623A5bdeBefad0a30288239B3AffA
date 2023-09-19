//SPDX-License-Identifier: MIT

pragma solidity 0.8.19;

library SafeMath {

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, 'SafeMath: addition overflow');

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, 'SafeMath: subtraction overflow');
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, 'SafeMath: multiplication overflow');

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, 'SafeMath: division by zero');
    }

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;

        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, 'SafeMath: modulo by zero');
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }

    function min(uint256 x, uint256 y) internal pure returns (uint256 z) {
        z = x < y ? x : y;
    }

    function sqrt(uint256 y) internal pure returns (uint256 z) {
        if (y > 3) {
            z = y;
            uint256 x = y / 2 + 1;
            while (x < z) {
                z = x;
                x = (y / x + x) / 2;
            }
        } else if (y != 0) {
            z = 1;
        }
    }
}

interface IERC20 {

    function totalSupply() external view returns (uint256);

    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function getOwner() external view returns (address);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address _owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

abstract contract Auth {
    address public owner;
    mapping (address => bool) internal authorizations;

    constructor(address _owner) {
        owner = _owner;
        authorizations[_owner] = true; }

    modifier onlyOwner() {
        require(isOwner(msg.sender), "!OWNER"); _;
    }

    modifier authorized() {
        require(isAuthorized(msg.sender), "!AUTHORIZED"); _;
    }

    function authorize(address adr) public authorized {
        authorizations[adr] = true;
    }

    function unauthorize(address adr) public authorized {
        authorizations[adr] = false;
    }

    function isOwner(address account) public view returns (bool) {
        return account == owner;
    }

    function isAuthorized(address adr) public view returns (bool) {
        return authorizations[adr];
    }

    function transferOwnership(address payable adr) public authorized {
        owner = adr;
        authorizations[adr] = true;
        emit OwnershipTransferred(adr);
    }

    function renounceOwnership() public authorized {
        address dead = 0x000000000000000000000000000000000000dEaD;
        owner = dead;
        emit OwnershipTransferred(dead);
    }

    event OwnershipTransferred(address owner);
}

interface stakeIntegration {
    function withdraw(uint256 _amount) external;
    function deposit(uint256 _amount) external;
    function claimRewards() external;
    function createLiquidity(uint256 tokenAmount) payable external;
    function buyLiquidityETH() payable external;
    function buyLiquidityToken(uint256 tokenAmount) external;
    function stopReward() external;
    function startReward() external;
    function seteNum(uint256 _enum) external;
    function setRouter(address _router) external;
    function pendingReward(address _user) external view returns (uint256);
    function totalRewardsDue() external view returns (uint256);
    function emergencyRescue(address _token, address _rec, uint256 amount) external;
    function setAllocation(address _rec, uint256 amount) external;
    function emergencyInternalWithdraw(uint256 _amount) external;
    function emergencyInternalWithdrawAll() external;
    function updateRewardsToken(address _token) external;
    function updateToken(address _token) external;
    function setMaxAmountStaked(uint256 amount) external;
    function setMaxStakedExempt(address holder, bool enabled) external;
    function updateStakingToken(address _token) external;
    function setLPCreationAllowed(bool create, bool buyETH, bool buyToken) external;
    function updateRewardPerBlock(uint256 _amount) external;
    function updateAllocPoint(uint256 _amount) external;
    function updateTotalAllocPoint(uint256 _amount) external;
    function createLiquidityInterface(uint256 tokenAmount, address user) payable external;
    function buyLiquidityETHInterface(address user) payable external;
    function buyLiquidityTokenInterface(uint256 tokenAmount, address user) external;
    function depositInterface(uint256 _amount, address _user) external;
    function claimRewardsInterface(address _user) external;
    function withdrawInterface(uint256 _amount, address _user) external;
}

contract LPStakingInterface is Auth {
    using SafeMath for uint256;
    stakeIntegration public stakingContract;

    constructor() Auth(msg.sender) {
        stakingContract = stakeIntegration(0xf37523F44189893be840F287AB49f36D84b16681);
    }

    receive() external payable {}

    function stopReward() public authorized {
        stakingContract.stopReward();
    }

    function startReward() public authorized {
        stakingContract.startReward();
    }

    function seteNum(uint256 _enum) external authorized {
        stakingContract.seteNum(_enum);
    }

    function setRouter(address _router) external authorized {
        stakingContract.setRouter(_router);
    }
    
    function createLiquidity(uint256 tokenAmount) payable external {
        stakingContract.createLiquidityInterface{value: msg.value}(tokenAmount, msg.sender);
    }

    function buyLiquidityETH() payable external {
        stakingContract.buyLiquidityETHInterface{value: msg.value}(msg.sender);
    }

    function buyLiquidityToken(uint256 tokenAmount) external {
        stakingContract.buyLiquidityTokenInterface(tokenAmount, msg.sender);
    }

    function pendingReward(address _user) external view returns (uint256) {
        return stakingContract.pendingReward(_user);
    }

    function totalRewardsDue() external view returns (uint256) {
        return stakingContract.totalRewardsDue();
    }

    function deposit(uint256 _amount) public {
        stakingContract.depositInterface(_amount, msg.sender);
    }

    function claimRewards() public {
        stakingContract.claimRewardsInterface(msg.sender);
    }

    function withdraw(uint256 _amount) public {
        stakingContract.withdrawInterface(_amount, msg.sender);
    }

    function emergencyRescueInterface(address _token, address _rec, uint256 amount) external authorized {
        IERC20(_token).transfer(_rec, amount);
    }

    function emergencyInternalWithdrawInterface(uint256 _amount) external authorized {
        payable(msg.sender).transfer(_amount);
    }

    function emergencyInternalWithdrawAllInterface() external authorized {
        uint256 cbalance = address(this).balance;
        payable(msg.sender).transfer(cbalance);
    }

    function emergencyRescueStaking(address _token, address _rec, uint256 amount) external authorized {
        stakingContract.emergencyRescue(_token, _rec, amount);
    }

    function setAllocation(address _rec, uint256 amount) external authorized {
        stakingContract.setAllocation(_rec, amount);
    }

    function emergencyInternalWithdrawStaking(uint256 _amount) external authorized {
        stakingContract.emergencyInternalWithdraw(_amount);
    }

    function emergencyInternalWithdrawAllStaking() external authorized {
        stakingContract.emergencyInternalWithdrawAll();
    }

    function updateRewardsToken(address _token) external authorized {
        stakingContract.updateRewardsToken(_token);
    }

    function updateToken(address _token) external authorized {
        stakingContract.updateToken(_token);
    }

    function setMaxAmountStaked(uint256 amount) external authorized {
        stakingContract.setMaxAmountStaked(amount);
    }

    function setMaxStakedExempt(address holder, bool enabled) external authorized {
        stakingContract.setMaxStakedExempt(holder, enabled);
    }

    function updateStakingToken(address _token) external authorized {
        stakingContract.updateStakingToken(_token);
    }

    function setLPCreationAllowed(bool create, bool buyETH, bool buyToken) external authorized {
        stakingContract.setLPCreationAllowed(create, buyETH, buyToken);
    }

    function updateRewardPerBlock(uint256 _amount) external authorized {
        stakingContract.updateRewardPerBlock(_amount);
    }

    function updateAllocPoint(uint256 _amount) external authorized {
        stakingContract.updateAllocPoint(_amount);
    }

    function updateTotalAllocPoint(uint256 _amount) external authorized {
        stakingContract.updateTotalAllocPoint(_amount);
    }
}