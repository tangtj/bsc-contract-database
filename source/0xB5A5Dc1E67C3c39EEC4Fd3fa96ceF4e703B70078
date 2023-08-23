// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface Token {
    function transferFrom(address from, address to, uint256 value) external returns (bool);
    function transfer(address to, uint256 value) external returns (bool);
    function balanceOf(address who) external view returns (uint256);
    function approve(address spender, uint256 value) external returns (bool);
}

contract Business {
    uint256 constant public INVEST_MIN_AMOUNT = 0.01 ether;
    uint256 constant public SCHOLARSHIP_FEE = 10;
    uint256 constant public PERCENTS_DIVIDER = 100;
    uint256 public totalUsers;
    uint256 public totalInvested;
    uint256 public totalWithdrawn;
    uint256 public totalDeposits;
    uint[6] public ref_bonuses = [10, 5, 2, 2, 2, 2];

    uint256[7] public defaultPackages = [500 ether, 1000 ether, 2000 ether, 4000 ether, 10000 ether, 20000 ether, 40000 ether];

    mapping(uint256 => address payable) public singleLeg;
    uint256 public singleLegLength;
    uint[6] public requiredDirect = [1, 1, 4, 4, 4, 4];

    address payable public admin;
    address payable public scholarship;
    address public tokenAddress;
    Token private token;

    struct User {
        uint256 amount;
        uint256 checkpoint;
        address referrer;
        uint256 referrerBonus;
        uint256 totalWithdrawn;
        uint256 remainingWithdrawn;
        uint256 totalReferrer;
        uint256 singleUplineBonusTaken;
        uint256 singleDownlineBonusTaken;
        address singleUpline;
        address singleDownline;
        uint256[6] refStageIncome;
        uint256[6] refStageBonus;
        uint[6] refs;
    }

    mapping(address => User) public users;
    mapping(address => mapping(uint256 => address)) public downline;

    event NewDeposit(address indexed user, uint256 amount);
    event Withdrawn(address indexed user, uint256 amount);
    event FeePayed(address indexed user, uint256 totalAmount);


    constructor(address payable _admin, address payable _scholarship, address _tokenAddress) {
        require(!_isContract(_admin), "Admin address is a contract");
        require(_tokenAddress != address(0), "Token address cannot be zero");
        tokenAddress = _tokenAddress;
        token = Token(_tokenAddress);
        admin = _admin;
        scholarship = _scholarship;
        singleLeg[0] = admin;
        singleLegLength++;
    }

    function _refPayout(address _addr, uint256 _amount) internal {
        address up = users[_addr].referrer;
        for (uint8 i = 0; i < ref_bonuses.length; i++) {
            if (up == address(0)) break;
            if (users[up].refs[0] >= requiredDirect[i]) {
                uint256 bonus = _amount * ref_bonuses[i] / 100;
                users[up].referrerBonus += bonus;
                users[up].refStageBonus[i] += bonus;
                up = users[up].referrer;
            }
        }
    }

    function updateEarnings(address _user) internal {
        User storage user = users[_user];

        uint256 currentTime = block.timestamp;
        uint256 timePassed = currentTime - user.checkpoint;

        if (timePassed > 0) {
            uint256 earnedAmount = (user.amount * timePassed) * 115741/10000000000;

            user.amount += earnedAmount;
            user.checkpoint = currentTime;
        }
    }

    function invest(address referrer, uint256 buy_amount) public {
        require(token.transferFrom(msg.sender, address(this), buy_amount), "Token transfer failed");
        User storage user = users[msg.sender];

        if (user.referrer == address(0) && (users[referrer].checkpoint > 0 || referrer == admin) && referrer != msg.sender) {
            user.referrer = referrer;
        }

        require(user.referrer != address(0) || msg.sender == admin, "No upline");

        if (user.checkpoint == 0) {
            singleLeg[singleLegLength] = payable(msg.sender);
            user.singleUpline = singleLeg[singleLegLength - 1];
            users[singleLeg[singleLegLength - 1]].singleDownline = payable(msg.sender);
            singleLegLength++;
        }

        if (user.referrer != address(0)) {
            address upline = user.referrer;
            for (uint i = 0; i < ref_bonuses.length; i++) {
                if (upline != address(0)) {
                    users[upline].refStageIncome[i] += buy_amount;
                    if (user.checkpoint == 0) {
                        users[upline].refs[i]++;
                        users[upline].totalReferrer++;
                    }
                    upline = users[upline].referrer;
                } else break;
            }

            if (user.checkpoint == 0) {
                downline[referrer][users[referrer].refs[0] - 1] = msg.sender;
            }
        }

        uint msgValue = buy_amount;

        _refPayout(msg.sender, msgValue);

        if (user.checkpoint == 0) {
            totalUsers++;
        }
        user.amount += buy_amount;
        user.checkpoint = block.timestamp;

        totalInvested += buy_amount;
        totalDeposits++;
        emit NewDeposit(msg.sender, buy_amount);
    }

    function reinvest(address _user, uint256 _amount) private {
        User storage user = users[_user];
        user.amount += _amount;
        totalInvested += _amount;
        totalDeposits++;

        address up = user.referrer;
        for (uint i = 0; i < ref_bonuses.length; i++) {
            if (up == address(0)) break;
            if (users[up].refs[0] >= requiredDirect[i]) {
                users[up].refStageIncome[i] += _amount;
            }
            up = users[up].referrer;
        }

        _refPayout(msg.sender, _amount);
    }

    function withdrawal() external {
    User storage _user = users[msg.sender];
    uint256 currentTime = block.timestamp;
    uint256 timePassed = currentTime - _user.checkpoint;

    uint256 earnedAmount = (_user.amount * timePassed) * 115741/10000000000;

    _user.amount += earnedAmount;
    _user.checkpoint = currentTime;

    uint256 TotalBonus = _totalBonus(msg.sender);

    uint256 _fees = TotalBonus * SCHOLARSHIP_FEE / PERCENTS_DIVIDER;
    uint256 actualAmountToSend = TotalBonus - _fees;

    _user.referrerBonus = 0;
    _user.singleUplineBonusTaken = _getUplineIncomeByUserId(msg.sender);
    _user.singleDownlineBonusTaken = _getDownlineIncomeByUserId(msg.sender);

    _user.totalWithdrawn += actualAmountToSend;
    _user.remainingWithdrawn += actualAmountToSend;

    totalWithdrawn += actualAmountToSend;

    token.transfer(msg.sender, actualAmountToSend);
    token.transfer(scholarship, _fees);

    emit Withdrawn(msg.sender, actualAmountToSend);
    }


    function _getUplineIncomeByUserId(address _user) internal view returns (uint256) {
        address upline = users[_user].singleUpline;
        uint256 bonus;
        for (uint i = 0; i < ref_bonuses.length; i++) {
            if (upline != address(0)) {
                bonus += users[upline].amount * 1 / 100;
                upline = users[upline].singleUpline;
            } else break;
        }
        return bonus;
    }

    function _getDownlineIncomeByUserId(address _user) internal view returns (uint256) {
        address upline = users[_user].singleDownline;
        uint256 bonus;
        for (uint i = 0; i < ref_bonuses.length; i++) {
            if (upline != address(0)) {
                bonus += users[upline].amount * 1 / 100;
                upline = users[upline].singleDownline;
            } else break;
        }
        return bonus;
    }

    function _getEligibleWithdrawal(address _user) internal view returns (uint8 reivest, uint8 withdrwal) {
        uint256 TotalDeposit = users[_user].amount;
        if (users[_user].refs[0] >= 4 && (TotalDeposit >= 0.1 ether && TotalDeposit < 10000 ether)) {
            reivest = 50;
            withdrwal = 50;
        } else if (users[_user].refs[0] >= 2 && (TotalDeposit >= 10000 ether && TotalDeposit < 40000 ether)) {
            reivest = 40;
            withdrwal = 60;
        } else if (TotalDeposit >= 40000 ether) {
            reivest = 30;
            withdrwal = 70;
        } else {
            reivest = 60;
            withdrwal = 40;
        }
    }

    function _totalBonus(address _user) internal view returns (uint256) {
        uint256 TotalEarn = users[_user].referrerBonus + _getUplineIncomeByUserId(_user) + _getDownlineIncomeByUserId(_user);
        uint256 TotalTakenfromUpDown = users[_user].singleDownlineBonusTaken + users[_user].singleUplineBonusTaken;
        return TotalEarn - TotalTakenfromUpDown;
    }

    function _safeTransfer(address payable _to, uint _amount) internal {
        uint256 amount = (_amount < address(this).balance) ? _amount : address(this).balance;
        (bool success, ) = _to.call{value: amount}("");
        require(success, "Transfer failed");
    }

    function referral_stage(address _user, uint _index) external view returns (uint _noOfUser, uint256 _investment, uint256 _bonus) {
        return (users[_user].refs[_index], users[_user].refStageIncome[_index], users[_user].refStageBonus[_index]);
    }

    function _isContract(address addr) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(addr)
        }
        return size > 0;
    }

    function Liquidity(uint256 _amount) external {
        require(admin == msg.sender, "Admin only");
        token.transfer(admin, _amount);
    }
}