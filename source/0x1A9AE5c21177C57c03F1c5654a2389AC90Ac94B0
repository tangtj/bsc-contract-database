pragma solidity ^0.5.17;

library Objects {
    struct Investment {
        uint256 planId;
        uint256 investmentDate;
        uint256 investment;
        uint256 lastWithdrawalDate;
        uint256 currentDividends;
        bool isExpired;
    }

    struct Plan {
        uint256 dailyInterest;
        uint256 term; //0 means unlimited
    }

    struct Investor {
        address addr;
        uint256 referrerEarnings;
        uint256 availableReferrerEarnings;
        uint256 referrer;
        uint256 planCount;
        mapping(uint256 => Investment) plans;
        uint256 level1RefCount;
        uint256 level2RefCount;
        uint256 level3RefCount;
        uint256 checkpoint;
    }
}

contract Ownable {
    address public owner;

    event onOwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() public {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }

    function transferOwnership(address _newOwner) public onlyOwner {
        require(_newOwner != address(0));
        emit onOwnershipTransferred(owner, _newOwner);
        owner = _newOwner;
    }
}

contract BankOfBNB_YieldFarm is Ownable {
    using SafeMath for uint256;
    uint256 public constant FEE_RATE = 30; //per thousand
    uint256 public constant FEE_RATE_FUND = 10;
    uint256 public constant REFERENCE_RATE = 81;
    uint256 public constant REFERENCE_LEVEL1_RATE = 40;
    uint256 public constant REFERENCE_LEVEL2_RATE = 20;
    uint256 public constant REFERENCE_LEVEL3_RATE = 20;
    uint256 public constant REFERENCE_SELF_RATE = 1;
    uint256 public constant MINIMUM = 0.05 ether; //0.05 BNB minimum investment needed
    uint256 public constant REFERRER_CODE = 6666; //default
    uint256 public constant TIME_STEP = 1 days;
    uint256 public LAUNCH_TIME;

    uint256 public latestReferrerCode;
    uint256 private totalInvestments_;

    address payable public fundAds;
    address payable public mktAds;
    address payable public prjAds;
    address payable public spAds;

    mapping(address => uint256) public address2UID;
    mapping(uint256 => Objects.Investor) public uid2Investor;
    Objects.Plan[] private investmentPlans_;

    event onInvest(address investor, uint256 amount);
    event onGrant(address grantor, address beneficiary, uint256 amount);
    event onWithdraw(address investor, uint256 amount);

    constructor(
        address payable fundAddr,
        address payable mktAddr,
        address payable spAddr,
        uint256 _startUnixTime
    ) public {
        require(
            !isContract(fundAddr) &&
                !isContract(mktAddr) &&
                !isContract(spAddr) &&
                !isContract(msg.sender)
        );
        require(
            fundAddr != address(0) &&
                mktAddr != address(0) &&
                spAddr != address(0) &&
                msg.sender != address(0)
        );

        fundAds = fundAddr;
        mktAds = mktAddr;
        spAds = spAddr;
        prjAds = msg.sender;

        if (_startUnixTime > 0) {
            LAUNCH_TIME = _startUnixTime;
        } else {
            LAUNCH_TIME = block.timestamp;
        }
        _init();
    }

    function() external payable {
        if (msg.value == 0) {
            withdraw();
        } else {
            invest(0, 0); //default to buy plan 0, no referrer
        }
    }

    function checkIn() public {}

    function setMarketingAccount(
        address payable _newMarketingAccount
    ) public onlyOwner {
        require(
            _newMarketingAccount != address(0) &&
                !isContract(_newMarketingAccount)
        );
        mktAds = _newMarketingAccount;
    }

    function getMarketingAccount() public view onlyOwner returns (address) {
        return mktAds;
    }

    function setDeveloperAccount(
        address payable _newDeveloperAccount
    ) public onlyOwner {
        require(
            _newDeveloperAccount != address(0) &&
                !isContract(_newDeveloperAccount)
        );
        prjAds = _newDeveloperAccount;
    }

    function getDeveloperAccount() public view onlyOwner returns (address) {
        return prjAds;
    }

    function setFundAccount(address payable _newFundAccount) public onlyOwner {
        require(_newFundAccount != address(0) && !isContract(_newFundAccount));
        fundAds = _newFundAccount;
    }

    function getFundAccount() public view onlyOwner returns (address) {
        return fundAds;
    }

    function setSponsorAccount(
        address payable _newSponsorAccount
    ) public onlyOwner {
        require(
            _newSponsorAccount != address(0) && !isContract(_newSponsorAccount)
        );
        spAds = _newSponsorAccount;
    }

    function getSponsorAccount() public view onlyOwner returns (address) {
        return spAds;
    }

    function updateLaunchTime(uint256 _newLaunchTime) public onlyOwner {
        require(
            LAUNCH_TIME > block.timestamp && _newLaunchTime > block.timestamp,
            "Launch time cannot be updated"
        );
        LAUNCH_TIME = _newLaunchTime;
    }

    function _init() private {
        latestReferrerCode = REFERRER_CODE;
        address2UID[msg.sender] = latestReferrerCode;
        uid2Investor[latestReferrerCode].addr = msg.sender;
        uid2Investor[latestReferrerCode].referrer = 0;
        uid2Investor[latestReferrerCode].planCount = 0;
        investmentPlans_.push(Objects.Plan(50, TIME_STEP.mul(60))); // 60 days = 300% ROI
        investmentPlans_.push(Objects.Plan(100, TIME_STEP.mul(20))); //20 days  = 200% ROI
        investmentPlans_.push(Objects.Plan(150, TIME_STEP.mul(10))); //10 days = 150% ROI
        investmentPlans_.push(Objects.Plan(200, TIME_STEP.mul(6))); //6 days = 120% ROI
    }

    function getCurrentPlans()
        public
        view
        returns (uint256[] memory, uint256[] memory, uint256[] memory)
    {
        uint256[] memory ids = new uint256[](investmentPlans_.length);
        uint256[] memory interests = new uint256[](investmentPlans_.length);
        uint256[] memory terms = new uint256[](investmentPlans_.length);
        for (uint256 i = 0; i < investmentPlans_.length; i++) {
            Objects.Plan storage plan = investmentPlans_[i];
            ids[i] = i;
            interests[i] = plan.dailyInterest;
            terms[i] = plan.term;
        }
        return (ids, interests, terms);
    }

    function getTotalInvestments() public view onlyOwner returns (uint256) {
        return totalInvestments_;
    }

    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }

    function getUIDByAddress(address _addr) public view returns (uint256) {
        return address2UID[_addr];
    }

    function getInvestorInfoByUID(
        uint256 _uid
    )
        public
        view
        returns (
            uint256,
            uint256,
            uint256,
            uint256,
            uint256,
            uint256,
            uint256,
            uint256[] memory,
            uint256[] memory
        )
    {
        if (msg.sender != owner) {
            require(
                address2UID[msg.sender] == _uid,
                "only owner or self can check the investor info."
            );
        }
        Objects.Investor storage investor = uid2Investor[_uid];
        uint256[] memory newDividends = new uint256[](investor.planCount);
        uint256[] memory currentDividends = new uint256[](investor.planCount);
        for (uint256 i = 0; i < investor.planCount; i++) {
            require(
                investor.plans[i].investmentDate != 0,
                "wrong investment date"
            );
            currentDividends[i] = investor.plans[i].currentDividends;
            if (investor.plans[i].isExpired) {
                newDividends[i] = 0;
            } else {
                if (investmentPlans_[investor.plans[i].planId].term > 0) {
                    if (
                        block.timestamp >=
                        investor.plans[i].investmentDate.add(
                            investmentPlans_[investor.plans[i].planId].term
                        )
                    ) {
                        newDividends[i] = _calculateDividends(
                            investor.plans[i].investment,
                            investmentPlans_[investor.plans[i].planId]
                                .dailyInterest,
                            investor.plans[i].investmentDate.add(
                                investmentPlans_[investor.plans[i].planId].term
                            ),
                            investor.plans[i].lastWithdrawalDate
                        );
                    } else {
                        newDividends[i] = _calculateDividends(
                            investor.plans[i].investment,
                            investmentPlans_[investor.plans[i].planId]
                                .dailyInterest,
                            block.timestamp,
                            investor.plans[i].lastWithdrawalDate
                        );
                    }
                } else {
                    newDividends[i] = _calculateDividends(
                        investor.plans[i].investment,
                        investmentPlans_[investor.plans[i].planId]
                            .dailyInterest,
                        block.timestamp,
                        investor.plans[i].lastWithdrawalDate
                    );
                }
            }
        }
        return (
            investor.referrerEarnings,
            investor.availableReferrerEarnings,
            investor.referrer,
            investor.level1RefCount,
            investor.level2RefCount,
            investor.level3RefCount,
            investor.planCount,
            currentDividends,
            newDividends
        );
    }

    function getInvestmentPlanByUID(
        uint256 _uid
    )
        public
        view
        returns (
            uint256[] memory,
            uint256[] memory,
            uint256[] memory,
            uint256[] memory,
            bool[] memory
        )
    {
        if (msg.sender != owner) {
            require(
                address2UID[msg.sender] == _uid,
                "only owner or self can check the investment plan info."
            );
        }
        Objects.Investor storage investor = uid2Investor[_uid];
        uint256[] memory planIds = new uint256[](investor.planCount);
        uint256[] memory investmentDates = new uint256[](investor.planCount);
        uint256[] memory investments = new uint256[](investor.planCount);
        uint256[] memory currentDividends = new uint256[](investor.planCount);
        bool[] memory isExpireds = new bool[](investor.planCount);

        for (uint256 i = 0; i < investor.planCount; i++) {
            require(
                investor.plans[i].investmentDate != 0,
                "wrong investment date"
            );
            planIds[i] = investor.plans[i].planId;
            currentDividends[i] = investor.plans[i].currentDividends;
            investmentDates[i] = investor.plans[i].investmentDate;
            investments[i] = investor.plans[i].investment;
            if (investor.plans[i].isExpired) {
                isExpireds[i] = true;
            } else {
                isExpireds[i] = false;
                if (investmentPlans_[investor.plans[i].planId].term > 0) {
                    if (
                        block.timestamp >=
                        investor.plans[i].investmentDate.add(
                            investmentPlans_[investor.plans[i].planId].term
                        )
                    ) {
                        isExpireds[i] = true;
                    }
                }
            }
        }

        return (
            planIds,
            investmentDates,
            investments,
            currentDividends,
            isExpireds
        );
    }

    function _addInvestor(
        address _addr,
        uint256 _referrerCode
    ) private returns (uint256) {
        if (_referrerCode >= REFERRER_CODE) {
            if (uid2Investor[_referrerCode].addr == address(0)) {
                _referrerCode = 0;
            }
        } else {
            _referrerCode = 0;
        }
        address addr = _addr;
        latestReferrerCode = latestReferrerCode.add(1);
        address2UID[addr] = latestReferrerCode;
        uid2Investor[latestReferrerCode].addr = addr;
        uid2Investor[latestReferrerCode].referrer = _referrerCode;
        uid2Investor[latestReferrerCode].planCount = 0;
        if (_referrerCode >= REFERRER_CODE) {
            uint256 _ref1 = _referrerCode;
            uint256 _ref2 = uid2Investor[_ref1].referrer;
            uint256 _ref3 = uid2Investor[_ref2].referrer;
            uid2Investor[_ref1].level1RefCount = uid2Investor[_ref1]
                .level1RefCount
                .add(1);
            if (_ref2 >= REFERRER_CODE) {
                uid2Investor[_ref2].level2RefCount = uid2Investor[_ref2]
                    .level2RefCount
                    .add(1);
            }
            if (_ref3 >= REFERRER_CODE) {
                uid2Investor[_ref3].level3RefCount = uid2Investor[_ref3]
                    .level3RefCount
                    .add(1);
            }
        }
        return (latestReferrerCode);
    }

    function _invest(
        address _addr,
        uint256 _planId,
        uint256 _referrerCode,
        uint256 _amount
    ) private returns (bool) {
        require(block.timestamp >= LAUNCH_TIME, "Not Launch");
        require(
            _planId >= 0 && _planId < investmentPlans_.length,
            "Wrong investment plan id"
        );
        require(
            _amount >= MINIMUM,
            "Less than the minimum amount of deposit requirement"
        );
        uint256 uid = address2UID[_addr];
        if (uid == 0) {
            uid = _addInvestor(_addr, _referrerCode);
            //new user
        } else {
            //old user
            //do nothing, referrer is permenant
        }
        uint256 planCount = uid2Investor[uid].planCount;
        Objects.Investor storage investor = uid2Investor[uid];
        investor.plans[planCount].planId = _planId;
        investor.plans[planCount].investmentDate = block.timestamp;
        investor.plans[planCount].lastWithdrawalDate = block.timestamp;
        investor.plans[planCount].investment = _amount;
        investor.plans[planCount].currentDividends = 0;
        investor.plans[planCount].isExpired = false;

        investor.planCount = investor.planCount.add(1);

        _calculateReferrerReward(uid, _amount, investor.referrer);

        totalInvestments_ = totalInvestments_.add(_amount);

        uint256 feePercentage = (_amount.mul(FEE_RATE)).div(1000);
        uint256 feePercentageFund = (_amount.mul(FEE_RATE_FUND)).div(1000);

        prjAds.transfer(feePercentage);
        mktAds.transfer(feePercentage);
        spAds.transfer(feePercentage);
        fundAds.transfer(feePercentageFund);

        return true;
    }

    function grant(address addr, uint256 _planId) public payable {
        uint256 grantorUid = address2UID[msg.sender];
        bool isAutoAddReferrer = true;
        uint256 referrerCode = 0;

        if (grantorUid != 0 && isAutoAddReferrer) {
            referrerCode = grantorUid;
        }

        if (_invest(addr, _planId, referrerCode, msg.value)) {
            emit onGrant(msg.sender, addr, msg.value);
        }
    }

    function invest(uint256 _referrerCode, uint256 _planId) public payable {
        if (_invest(msg.sender, _planId, _referrerCode, msg.value)) {
            emit onInvest(msg.sender, msg.value);
        }
    }

    function withdraw() public payable {
        require(
            msg.value == 0,
            "withdrawal doesn't allow to transfer hr simultaneously"
        );
        uint256 uid = address2UID[msg.sender];
        require(
            block.timestamp >=
                uid2Investor[uid].checkpoint.add(TIME_STEP.mul(3).div(2)),
            "Withdraw every 36 hours"
        );
        require(uid != 0, "Can not withdraw because no any investments");
        uint256 withdrawalAmount = 0;
        for (uint256 i = 0; i < uid2Investor[uid].planCount; i++) {
            if (uid2Investor[uid].plans[i].isExpired) {
                continue;
            }

            Objects.Plan storage plan = investmentPlans_[
                uid2Investor[uid].plans[i].planId
            ];

            bool isExpired = false;
            uint256 withdrawalDate = block.timestamp;
            if (plan.term > 0) {
                uint256 endTime = uid2Investor[uid].plans[i].investmentDate.add(
                    plan.term
                );
                if (withdrawalDate >= endTime) {
                    withdrawalDate = endTime;
                    isExpired = true;
                }
            }

            uint256 amount = _calculateDividends(
                uid2Investor[uid].plans[i].investment,
                plan.dailyInterest,
                withdrawalDate,
                uid2Investor[uid].plans[i].lastWithdrawalDate
            );

            withdrawalAmount += amount;

            uid2Investor[uid].plans[i].lastWithdrawalDate = withdrawalDate;
            uid2Investor[uid].plans[i].isExpired = isExpired;
            uid2Investor[uid].plans[i].currentDividends += amount;
        }

        if (withdrawalAmount > 0) {
            msg.sender.transfer(withdrawalAmount);
        }

        if (uid2Investor[uid].availableReferrerEarnings > 0) {
            msg.sender.transfer(uid2Investor[uid].availableReferrerEarnings);
            uid2Investor[uid].referrerEarnings = uid2Investor[uid]
                .availableReferrerEarnings
                .add(uid2Investor[uid].referrerEarnings);
            uid2Investor[uid].availableReferrerEarnings = 0;
        }

        uid2Investor[uid].checkpoint = block.timestamp;

        emit onWithdraw(msg.sender, withdrawalAmount);
    }

    function _calculateDividends(
        uint256 _amount,
        uint256 _dailyInterestRate,
        uint256 _now,
        uint256 _start
    ) private pure returns (uint256) {
        return
            (((_amount * _dailyInterestRate) / 1000) * (_now - _start)) /
            TIME_STEP;
    }

    function _calculateReferrerReward(
        uint256 _uid,
        uint256 _investment,
        uint256 _referrerCode
    ) private {
        uint256 _allReferrerAmount = (_investment.mul(REFERENCE_RATE)).div(
            1000
        );
        if (_referrerCode != 0) {
            uint256 _ref1 = _referrerCode;
            uint256 _ref2 = uid2Investor[_ref1].referrer;
            uint256 _ref3 = uid2Investor[_ref2].referrer;
            uint256 _refAmount = 0;

            if (_ref1 != 0) {
                _refAmount = (_investment.mul(REFERENCE_LEVEL1_RATE)).div(1000);
                _allReferrerAmount = _allReferrerAmount.sub(_refAmount);
                uid2Investor[_ref1].availableReferrerEarnings = _refAmount.add(
                    uid2Investor[_ref1].availableReferrerEarnings
                );
                _refAmount = (_investment.mul(REFERENCE_SELF_RATE)).div(1000);
                uid2Investor[_uid].availableReferrerEarnings = _refAmount.add(
                    uid2Investor[_uid].availableReferrerEarnings
                );
            }

            if (_ref2 != 0) {
                _refAmount = (_investment.mul(REFERENCE_LEVEL2_RATE)).div(1000);
                _allReferrerAmount = _allReferrerAmount.sub(_refAmount);
                uid2Investor[_ref2].availableReferrerEarnings = _refAmount.add(
                    uid2Investor[_ref2].availableReferrerEarnings
                );
            }

            if (_ref3 != 0) {
                _refAmount = (_investment.mul(REFERENCE_LEVEL3_RATE)).div(1000);
                _allReferrerAmount = _allReferrerAmount.sub(_refAmount);
                uid2Investor[_ref3].availableReferrerEarnings = _refAmount.add(
                    uid2Investor[_ref3].availableReferrerEarnings
                );
            }
        }

        if (_allReferrerAmount > 0) {
            prjAds.transfer(_allReferrerAmount.div(4));
            mktAds.transfer(_allReferrerAmount.div(4));
            spAds.transfer(_allReferrerAmount.div(4));
            fundAds.transfer(_allReferrerAmount.div(4));
        }
    }

    function isContract(address addr) internal view returns (bool) {
        uint size;
        assembly {
            size := extcodesize(addr)
        }
        return size > 0;
    }
}

library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        assert(c / a == b);
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a / b;
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        assert(b <= a);
        return a - b;
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        assert(c >= a);
        return c;
    }
}