pragma solidity 0.5.16;

interface IBEP20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function mint(address to, uint256 amount) external returns (bool);
    function burnFrom(address account, uint256 amount) external returns(bool);
}

contract GatewayBSWAP {
    address private _owner;
    address public feeTo;
    IBEP20 public tokenV1;
    IBEP20 public tokenV2;
    uint256 public totalSupply;
    uint256 public stakingPeriod = 31536000;   // user can withdraw staked amount during this period prorate staked time (365 days)
    uint256 public APY = 1000;    // the APY rate with 2 decimals that user get as reword for staking (1000 = 10%)
    mapping (address => uint256) public lastAction;
    mapping (address => uint256) private _swapped;      // total swapped tokens from V1 to V2 
    mapping (address => uint256) private _paidStaked;   // total paid amount of staked (swapped) tokens
    mapping (address => uint256) private _paidRewards;  // total paid rewords (APY)


    constructor (address _feeTo) public {
        _owner = msg.sender;
        feeTo = _feeTo;
    }

    // get user info
    function getInfo(address user) external view returns(
        uint256 swapped,    // Total received from swapping
        uint256 paidRewards,// Total receiver reward from APY
        uint256 paidStaked, // Total withdrawn by user
        uint256 balance,    // Total balance
        uint256 allowed,    // estimate available staked amount to withdraw
        uint256 reward      // estimate available reward to withdraw
    ) {
        swapped = _swapped[user];
        paidRewards = _paidRewards[user];
        paidStaked = _paidStaked[user];
        balance = balanceOf(user);
        (allowed, reward) = readyToWithdraw(user);
    }
    
    function setTokens(address v1, address v2) external returns(bool) {
        require(_owner == msg.sender);
        require(v1!=address(0) && v2 != address(0));
        require(address(tokenV1) == address(0) && address(tokenV2) == address(0));
        tokenV1 = IBEP20(v1);
        tokenV2 = IBEP20(v2);
        return true;
    }

    function setFeeTo (address _addr) external returns(bool) {
        require(_owner == msg.sender);
        feeTo = _addr;
        return true;
    }

    function setStakingDays(uint256 _days) external returns(bool) {
        require(_owner == msg.sender);
        stakingPeriod = _days * 86400;
        return true;
    }

    // _apy in % with 2 decimals (1000 = 10%)
    function setAPY(uint256 _apy) external returns(bool) {
        require(_owner == msg.sender);
        APY = _apy;
        return true;
    }

    function OldToNewToken(uint256 amount) external returns (bool) {
        if (lastAction[msg.sender] != 0) _withdraw(msg.sender);   // paid reward + allowed amount on previous balance
        return _swap(msg.sender, amount);
    }
    
    function swapBehalf(address user, uint256 amount) external returns (bool) {
        require(lastAction[user] == 0, "Only first stake allowed");
        return _swap(user, amount);
    }
    
    function _swap(address user, uint256 amount) internal returns (bool) {
        tokenV1.transferFrom(msg.sender, feeTo, amount);
        tokenV2.mint(address(this), amount);
        lastAction[user] = block.timestamp;
        _swapped[user] += amount;
        return true;
    }

    function withdraw() external returns (bool) {
        require(balanceOf(msg.sender) != 0,"Zero balance");
        _withdraw(msg.sender);
        return true;
    }

    function _withdraw(address user) internal {
        (uint256 allowed, uint256 reward) = readyToWithdraw(user);
        lastAction[user] = block.timestamp;
        _paidStaked[user] += allowed;
        _paidRewards[user] += reward;
        tokenV2.transfer(user, allowed);
        tokenV2.mint(user, reward);
    }

    function readyToWithdraw(address user) public view returns(uint256 allowed, uint256 reward) {
        // the rate (with 18 decimals) per second of initial staked amount that can be withdrawn
        uint256 staked_time = block.timestamp - lastAction[user];
        uint256 unpaid = balanceOf(user);
        reward = unpaid * staked_time * APY / (31536000 * 10000);
        allowed = _swapped[user] * staked_time / stakingPeriod;
        if (allowed > unpaid) allowed = unpaid;
    }

    function balanceOf(address user) public view returns(uint256) {
        return _swapped[user] - _paidStaked[user];
    }
}