// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract KFCTheChicken {
    using SafeMath for uint256;

    struct Deposit {
        uint256 amount;
        uint256 withdrawn;
        uint64 checkpoint;
        uint8 rate;
        uint8 plan;
        bool refunded;
    }

    mapping(address => Deposit[]) public deposits;
    mapping(address => address) public referrers;
    mapping(address => uint8[]) public plans;
    uint256[5] menus = [0, 0, 0, 0, 0];

    //events
    event NewBie(
        address indexed user,
        address indexed referrer,
        uint256 amount
    );

    event DepositEvent(
        address indexed user,
        address indexed referrer,
        uint256 amount
    );
    event MENU(address indexed user, address indexed referrer, uint8 menu);
    event Refund(address indexed user, uint256 amount);
    event MENUChange(address indexed user, uint8 menu, uint256 depositIndex);
    event Withdraw(address indexed user, uint256 amount);

    address public ceoWallet;
    address public devWallet;
    address public adminWallet;
    address public emergencyContract;
    address public marketingWallet;
    address public bossWallet;
    address public influencerWallet;
    address public owner;

    uint256 public constant DAY = 1 days;
    uint256 public constant BNB = 1 ether;
    uint8 public constant RATE = 5;
    uint256 public constant MAXRATE = 25;
    uint256 public constant REFUND_THRESHOLD = 1 * BNB;

    uint256 public refPercent = 3;
    uint256 public totalInvestment;
    uint256 public launchTime;

    // constructor
    constructor(
        address _devWallet,
        address _ceoWallet,
        address _marketingWallet,
        address _adminWallet,
        address _influencerWallet,
        address _bossWallet
    ) {
        ceoWallet = _ceoWallet;
        devWallet = _devWallet;
        adminWallet = _adminWallet;
        marketingWallet = _marketingWallet;
        bossWallet = _bossWallet;
        influencerWallet = _influencerWallet;

        owner = msg.sender;

        launchTime = block.timestamp;
    }

    function deposit(address _referrer) external payable {
        require(emergencyContract != address(0), "not initilized");
        require(is25DaysPassed(), "not started");
        require(msg.value >= BNB.div(20), "insufficient bnb");
        checkNewUserCore(msg.sender, _referrer);

        deposits[msg.sender].push(
            Deposit(msg.value, 0, uint64(block.timestamp), RATE, 0, false)
        );

        payable(referrers[msg.sender]).transfer(msg.value.mul(3).div(100));
        payOwners(msg.value);
        totalInvestment = totalInvestment.add(msg.value);
        emit DepositEvent(msg.sender, referrers[msg.sender], msg.value);
    }

    function withdraw(uint256 _index) external {
        require(isActive(msg.sender), "Not active");
        Deposit storage dep = deposits[msg.sender][_index];
        require(dep.refunded == false, "refunded");

        uint256 dividends = getDividend(msg.sender, _index);
        uint256 userRate = uint256(dep.rate).add(
            getHoldRate(msg.sender, _index)
        );
        if (userRate > MAXRATE) userRate = MAXRATE;
        uint256 rateDrop = getRateDrop(dep.plan);
        if (dep.plan == 0) userRate = RATE;
        else userRate = userRate.sub(rateDrop);
        userRate = userRate < RATE ? RATE : userRate;

        dep.rate = uint8(userRate);
        dep.checkpoint = uint64(block.timestamp);
        dep.withdrawn = dep.withdrawn.add(dividends);

        payable(marketingWallet).transfer(dividends.mul(2).div(100));
        payable(emergencyContract).transfer(dividends.mul(4).div(100));
        payable(msg.sender).transfer(dividends);

        emit Withdraw(msg.sender, dividends);
    }

    function buyMENU(uint8 menu, address _ref) external payable {
        checkNewUserCore(msg.sender, _ref);
        if (menu == 1) require(msg.value == BNB.div(20), "invalid value");
        else if (menu == 2)
            require(msg.value == BNB.mul(2).div(10), "invalid value");
        else if (menu == 3)
            require(msg.value == BNB.mul(4).div(10), "invalid value");
        else if (menu == 4)
            require(msg.value == BNB.mul(8).div(10), "invalid value");
        else if (menu == 5) require(msg.value == BNB, "invalid value");
        else require(false, "invalid plan");

        plans[msg.sender].push(menu);
        payable(emergencyContract).transfer(msg.value.mul(85).div(100));
        payable(marketingWallet).transfer(msg.value.mul(5).div(100));
        payable(referrers[msg.sender]).transfer(msg.value.mul(10).div(100));
        menus[menu - 1] = menus[menu - 1].add(1);
        emit MENU(msg.sender, referrers[msg.sender], menu);
    }

    function assignMENU(uint8 _menuIndex, uint256 _depIndex) external {
        require(
            _menuIndex < plans[msg.sender].length &&
                _depIndex < deposits[msg.sender].length,
            "invalid index"
        );
        require(
            deposits[msg.sender][_depIndex].amount > 0 &&
                deposits[msg.sender][_depIndex].plan == 0,
            "already set"
        );
        require(
            deposits[msg.sender][_depIndex].withdrawn <
                deposits[msg.sender][_depIndex].amount.mul(2),
            "deposit ended"
        );

        require(plans[msg.sender][_menuIndex] > 0, "no plan");

        deposits[msg.sender][_depIndex].plan = plans[msg.sender][_menuIndex];
        plans[msg.sender][_menuIndex] = 0;

        emit MENUChange(msg.sender, _menuIndex, _depIndex);
    }

    function setEmergencyContract(address _emergency) external {
        require(msg.sender == owner, "access denied");
        require(_emergency != address(0), "invalid address");
        require(emergencyContract == address(0), "already set"); //can be set only once after deployment
        emergencyContract = _emergency;
    }

    function refund(uint256 _depIndex) external {
        require(address(this).balance < REFUND_THRESHOLD, "refund disabled"); //refund enabled when balance is less than 1BNB
        require(block.timestamp >= launchTime.add(35 * DAY), "refund disabled"); //refund enabled 10 days after roi start. It takes 25 days to roi launch
        require(deposits[msg.sender].length > 0, "no deposit");
        require(_depIndex < deposits[msg.sender].length, "invalid index");
        require(
            deposits[msg.sender][_depIndex].refunded == false,
            "already refunded"
        );
        Deposit storage dep = deposits[msg.sender][_depIndex];
        uint256 refundPercent = getRefundPercent(dep.plan);
        uint256 refundAmount = refundPercent.mul(dep.amount).div(100);
        require(dep.withdrawn < refundAmount, "already withdrawn");
        refundAmount = refundAmount.sub(dep.withdrawn);
        dep.refunded = true;

        IKFCEmergency(emergencyContract).refund(msg.sender, refundAmount);

        emit Refund(msg.sender, refundAmount);
    }

    function getRefundPercent(uint8 plan) public pure returns (uint256) {
        if (plan == 0) return 0;
        if (plan == 1) return 35;
        if (plan == 2) return 45;
        if (plan == 3) return 60;
        if (plan == 4) return 75;
        if (plan == 5) return 100;
        return 0;
    }

    function getRateDrop(uint256 _plan) public pure returns (uint256) {
        if (_plan == 1) {
            return 3;
        }
        if (_plan == 2) {
            return 2;
        }
        if (_plan == 3) {
            return 1;
        }
        return 0;
    }

    function getHoldRate(
        address _addr,
        uint256 _index
    ) public view returns (uint256) {
        uint256 intervals = block
            .timestamp
            .sub(deposits[_addr][_index].checkpoint)
            .div(DAY)
            .div(3);
        return intervals > 20 ? 20 : intervals;
    }

    function checkNewUserCore(address _addr, address _referrer) private {
        if (_addr == owner) return;
        if (referrers[_addr] == address(0)) {
            require(
                isActive(_referrer) || _referrer == owner,
                "invalid referrer"
            );
            referrers[_addr] = _referrer;
            emit NewBie(_addr, _referrer, msg.value);
        }
    }

    function payOwners(uint256 _amount) private {
        payable(ceoWallet).transfer(_amount.mul(2).div(100));
        payable(devWallet).transfer(_amount.mul(2).div(100));
        payable(adminWallet).transfer(_amount.mul(1).div(100));
        payable(emergencyContract).transfer(_amount.mul(4).div(100)); //emergency contract
        payable(marketingWallet).transfer(_amount.mul(2).div(100)); //marketing
        payable(influencerWallet).transfer(_amount.mul(1).div(100));
        payable(bossWallet).transfer(_amount.mul(2).div(100));
    }

    function getDividend(
        address _user,
        uint256 _index
    ) public view returns (uint256) {
        Deposit storage dep = deposits[_user][_index];
        if (dep.refunded) return 0;
        uint256 rate = uint256(dep.rate).add(getHoldRate(_user, _index));
        if (rate > 25) rate = 25;
        uint256 dividend = block
            .timestamp
            .sub(dep.checkpoint)
            .mul(dep.amount)
            .mul(rate)
            .div(1000)
            .div(DAY);
        return
            dividend.add(dep.withdrawn) > uint256(dep.amount).mul(2)
                ? dep.amount.mul(2).sub(dep.withdrawn)
                : dividend;
    }

    function isActive(address _user) public view returns (bool) {
        return deposits[_user].length > 0 || plans[_user].length > 0;
    }

    function is25DaysPassed() public view returns (bool) {
        return block.timestamp >= launchTime.add(25 * DAY);
    }

    function getUser(
        address _addr
    )
        public
        view
        returns (
            uint8[] memory userPlans,
            Deposit[] memory userDeposits,
            uint256[] memory userDividends,
            uint8[] memory userHoldRates
        )
    {
        userPlans = plans[_addr];
        userDeposits = deposits[_addr];
        uint256[] memory dividends = new uint256[](userDeposits.length);
        uint8[] memory holdRates = new uint8[](userDeposits.length);
        for (uint i = 0; i < dividends.length; i++) {
            dividends[i] = getDividend(_addr, i);
            holdRates[i] = uint8(getHoldRate(_addr, i));
        }
        userDividends = dividends;
        userHoldRates = holdRates;
    }

    function getContract() public view returns (uint256, uint256, uint256) {
        return (launchTime, address(this).balance, totalInvestment);
    }

    function menuCount() public view returns (uint256[5] memory) {
        return menus;
    }

    receive() external payable {}
}

library SafeMath {
    /**
     * @dev Multiplies two numbers, throws on overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        assert(c / a == b);
        return c;
    }

    /**
     * @dev Integer division of two numbers, truncating the quotient.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        // assert(b > 0); // Solidity automatically throws when dividing by 0
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold
        return c;
    }

    /**
     * @dev Substracts two numbers, throws on overflow (i.e. if subtrahend is greater than minuend).
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        assert(b <= a);
        return a - b;
    }

    /**
     * @dev Adds two numbers, throws on overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        assert(c >= a);
        return c;
    }
}

interface IKFCEmergency {
    function refund(address _addr, uint256 _amount) external;
}