// SPDX-License-Identifier: GPL-3.0-or-later
pragma solidity ^0.5.16;

library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b);
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0);
        uint256 c = a / b;
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a);
        uint256 c = a - b;
        return c;
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a);
        return c;
    }
}

library Percent {
    struct percent {
        uint256 num;
        uint256 den;
    }

    function mul(percent storage p, uint256 a) internal view returns (uint256) {
        if (a == 0) {
            return 0;
        }
        return (a * p.num) / p.den;
    }

    function div(percent storage p, uint256 a) internal view returns (uint256) {
        return (a / p.num) * p.den;
    }

    function sub(percent storage p, uint256 a) internal view returns (uint256) {
        uint256 b = mul(p, a);
        if (b >= a) return 0;
        return a - b;
    }

    function add(percent storage p, uint256 a) internal view returns (uint256) {
        return a + mul(p, a);
    }
}

contract BNBAbylon {
    using SafeMath for uint256;
    using Percent for Percent.percent;

    struct Plan {
        Percent.percent daily;
        Percent.percent max_payout;
        Percent.percent additional;
        uint256 max_additional;
        uint256 min_deposit;
        uint256 max_deposit;
        uint256 min_withdraw;
    }

    struct User {
        address sponsor;
        uint256 bonus;
        uint256 deposited;
        uint256 reinvested;
        uint256 turnover_bonus;
        uint256 turnover;
        uint256 withdrawn;
        uint256 withdraw_ref_date;
        mapping(uint256 => uint256) referrals;
    }

    struct UserPlan {
        uint256 pending;
        uint256 pending_payout;
        uint256 deposited;
        uint256 withdrawn_payout;
        uint256 withdraw_date;
        uint256 last_date;
    }

    uint256 private ONE_DAY = 1 days;
    uint256 private MIN_BONUS_WITHDRAWAL = 5e16;
    uint256 private TURNOVER_BONUS_EVERY = 200e18;
    uint256 private TURNOVER_BONUS_AMOUNT = 20e18;

    address payable private owner =
        address(0x5c00114237e2FC579B80176dca49dBfd639a2cC8);
    address payable private promote =
        address(0x979707294108A6Ec6A5A304E3709ce7A223AB0B9);

    Percent.percent private ADMIN_FEE = Percent.percent(12, 100);
    Percent.percent private WITHDRAW_REINVEST = Percent.percent(20, 100);

    Percent.percent[] private PERCENT_REFERRAL;
    Plan[] private PLANS;

    mapping(address => User) public users;
    mapping(address => mapping(uint256 => UserPlan)) public user_plans;

    uint256 public total_deposited;
    uint256 public total_rewards;
    uint256 public total_reinvested;

    event Sponsor(address indexed addr, address indexed sponsor);
    event Deposit(address indexed addr, uint256 plan, uint256 amount);
    event Reinvest(address indexed addr, uint256 plan, uint256 amount);
    event Payout(address indexed addr, address indexed from, uint256 amount);
    event Withdraw(address indexed addr, uint256 plan, uint256 amount);

    constructor() public {
        PERCENT_REFERRAL.push(Percent.percent(6, 100));
        PERCENT_REFERRAL.push(Percent.percent(4, 100));
        PERCENT_REFERRAL.push(Percent.percent(2, 100));
        PERCENT_REFERRAL.push(Percent.percent(2, 100));
        PERCENT_REFERRAL.push(Percent.percent(1, 100));
        PERCENT_REFERRAL.push(Percent.percent(1, 100));
        PERCENT_REFERRAL.push(Percent.percent(5, 1000));
        PERCENT_REFERRAL.push(Percent.percent(5, 1000));

        PLANS.push(
            Plan(
                Percent.percent(2, 100),
                Percent.percent(120, 100),
                Percent.percent(1, 1000),
                10,
                5e16,
                1e18,
                1e16
            )
        );

        PLANS.push(
            Plan(
                Percent.percent(1, 100),
                Percent.percent(160, 100),
                Percent.percent(1, 1000),
                10,
                1e18,
                4e18,
                5e16
            )
        );

        PLANS.push(
            Plan(
                Percent.percent(1, 100),
                Percent.percent(200, 100),
                Percent.percent(1, 1000),
                10,
                4e18,
                10e18,
                5e17
            )
        );

        PLANS.push(
            Plan(
                Percent.percent(15, 1000),
                Percent.percent(300, 100),
                Percent.percent(1, 1000),
                10,
                10e18,
                0,
                5e17
            )
        );
    }

    function() external payable {
        _deposit(0, msg.sender, msg.value);
    }

    function _setSponsor(address _addr, address _sponsor) private {
        if (
            users[_addr].sponsor == address(0) &&
            _sponsor != _addr &&
            _addr != owner &&
            (user_plans[_sponsor][0].last_date > 0 || _sponsor == owner)
        ) {
            users[_addr].sponsor = _sponsor;

            emit Sponsor(_addr, _sponsor);

            for (uint8 i = 0; i < PERCENT_REFERRAL.length; i++) {
                if (_sponsor == address(0)) break;
                users[_sponsor].referrals[i]++;
                _sponsor = users[_sponsor].sponsor;
            }
        }
    }

    function _deposit(uint256 _plan, address _addr, uint256 _amount) private {
        require(_plan >= 0 && _plan < PLANS.length, "Bad plan");
        require(_amount > 0, "Zero amount");
        require(
            user_plans[_addr][_plan].deposited + _amount >= PLANS[_plan].min_deposit,
            "Wrong min amount"
        );
        if (PLANS[_plan].max_deposit > 0)
            require(
                user_plans[_addr][_plan].deposited + _amount <= PLANS[_plan].max_deposit,
                "Wrong max amount"
            );
        if (_plan != 0)
            require(
                user_plans[_addr][_plan - 1].deposited > 0,
                "Previous plan not activated"
            );

        uint256 pending = this.payoutOf(_plan, _addr);
        if (pending > 0) user_plans[_addr][_plan].pending += pending;

        user_plans[_addr][_plan].last_date = block.timestamp;
        user_plans[_addr][_plan].deposited += _amount;
        users[_addr].deposited += _amount;
        total_deposited += _amount;

        _refPayout(_addr, _amount);

        (bool successPromoteFee, ) = promote.call.value(ADMIN_FEE.mul(_amount))(
            ""
        );
        require(successPromoteFee, "Transfer failed.");

        emit Deposit(_addr, _plan, _amount);
    }

    function _reinvest(uint256 _plan, address _addr, uint256 _amount) private {
        uint256 pending = this.payoutOf(_plan, _addr);
        if (pending > 0) user_plans[_addr][_plan].pending += pending;

        user_plans[_addr][_plan].last_date = block.timestamp;
        user_plans[_addr][_plan].deposited += _amount;
        users[_addr].reinvested += _amount;
        total_reinvested += _amount;

        emit Reinvest(_addr, _plan, _amount);
    }

    function _refPayout(address _addr, uint256 _amount) private {
        address up = users[_addr].sponsor;

        for (uint256 i = 0; i <= PERCENT_REFERRAL.length; i++) {
            if (up == address(0)) break;
            uint256 bonus = PERCENT_REFERRAL[i].mul(_amount);

            if (i == 0) {
                users[up].turnover_bonus += _amount;

                if (users[up].turnover_bonus >= TURNOVER_BONUS_EVERY) {
                    bonus += TURNOVER_BONUS_AMOUNT;
                }
            }

            users[up].turnover += _amount;
            users[up].bonus += bonus;
            total_rewards += bonus;
            emit Payout(up, _addr, bonus);
            up = users[up].sponsor;
        }
    }

    function deposit(uint256 _plan, address _sponsor) external payable {
        _setSponsor(msg.sender, _sponsor);
        _deposit(_plan, msg.sender, msg.value);
    }

    function withdraw(uint256 _plan) external {
        require(_plan <= PLANS.length, "Invalid plan");

        uint256 payout = 0;
        uint256 payoutFull = 0;
        if (_plan == PLANS.length) {
            payoutFull = users[msg.sender].bonus;
            require(payoutFull > 0, "Zero payout");
            require(
                payoutFull >= MIN_BONUS_WITHDRAWAL,
                "Minimum withdrawal amount is 0.05 BNB"
            );

            uint256 diff = (block.timestamp -
                users[msg.sender].withdraw_ref_date) / ONE_DAY;
            require(diff > 0, "One withdrawal per day is allowed");

            users[msg.sender].bonus = 0;
            users[msg.sender].withdraw_ref_date = block.timestamp;

            payout = payoutFull;
        } else {
            payoutFull = this.payoutOf(_plan, msg.sender);
            require(payoutFull > 0, "Zero payout");

            Plan storage plan = PLANS[_plan];
            require(payoutFull >= plan.min_withdraw, "Wrong minimum withdrawal amount");

            uint256 diff = (block.timestamp -
                user_plans[msg.sender][_plan].withdraw_date) / ONE_DAY;
            require(diff > 0, "One withdrawal per day is allowed");

            uint256 reinvest = WITHDRAW_REINVEST.mul(payoutFull);
            payout = payoutFull - reinvest;

            user_plans[msg.sender][_plan].pending = 0;
            user_plans[msg.sender][_plan].pending_payout = 0;
            user_plans[msg.sender][_plan].last_date = block.timestamp;
            user_plans[msg.sender][_plan].withdraw_date = block.timestamp;
            user_plans[msg.sender][_plan].withdrawn_payout += payout;

            _reinvest(_plan, msg.sender, reinvest);
        }

        users[msg.sender].withdrawn += payoutFull;

        (bool successWithdraw, ) = msg.sender.call.value(payout)("");
        require(successWithdraw, "Transfer failed.");

        emit Withdraw(msg.sender, _plan, payoutFull);
    }

    function maxPayoutOf(
        uint256 _plan,
        uint256 _amount
    ) internal view returns (uint256) {
        return PLANS[_plan].max_payout.mul(_amount);
    }

    function payoutOf(
        uint256 _plan,
        address _addr
    ) external view returns (uint256 payout) {
        uint256 max_payout = maxPayoutOf(
            _plan,
            user_plans[_addr][_plan].deposited
        );
        if (user_plans[_addr][_plan].withdrawn_payout >= max_payout) return 0;
        payout =
            (dailyBonus(_plan, _addr) + holdBonus(_plan, _addr)) +
            user_plans[_addr][_plan].pending +
            user_plans[_addr][_plan].pending_payout;
        if (user_plans[_addr][_plan].withdrawn_payout + payout >= max_payout)
            return max_payout - user_plans[_addr][_plan].withdrawn_payout;
        return payout;
    }

    function dailyBonus(
        uint256 _plan,
        address _addr
    ) internal view returns (uint256 percent) {
        if (user_plans[_addr][_plan].last_date == 0) return 0;
        return
            (PLANS[_plan].daily.mul(user_plans[_addr][_plan].deposited) *
                (block.timestamp - user_plans[_addr][_plan].last_date)) /
            ONE_DAY;
    }

    function getHoldBonus(
        uint256 _plan,
        address _addr
    ) public view returns (uint256 percent) {
        if (user_plans[_addr][_plan].last_date == 0) return 0;
        uint256 ldays = block
            .timestamp
            .sub(user_plans[_addr][_plan].last_date)
            .div(ONE_DAY);
        return
            ldays > PLANS[_plan].max_additional
                ? PLANS[_plan].max_additional
                : ldays;
    }

    function holdBonus(
        uint256 _plan,
        address _addr
    ) internal view returns (uint256 percent) {
        if (user_plans[_addr][_plan].last_date == 0) return 0;
        uint256 ldays = block
            .timestamp
            .sub(user_plans[_addr][_plan].last_date)
            .div(ONE_DAY);
        if (ldays == 0) return 0;

        uint256 bonus = 0;
        uint256 holdBonusMultiplier = getHoldBonus(_plan, _addr);

        for (uint256 i = 1; i <= holdBonusMultiplier; i++) {
            if (i == 10) bonus += (user_plans[_addr][_plan].deposited * (PLANS[_plan].additional.num * i)) / PLANS[_plan].additional.den * (ldays - 9);
            else bonus += (user_plans[_addr][_plan].deposited * (PLANS[_plan].additional.num * i)) / PLANS[_plan].additional.den;
        }

        return bonus;
    }

    function referralsCount(
        address _addr,
        uint256 _level
    ) external view returns (uint256 referrals) {
        return users[_addr].referrals[_level];
    }
}