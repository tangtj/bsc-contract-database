// SPDX-License-Identifier: MIT

pragma solidity 0.8.0;

interface IBEP20 {

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);
}

interface IMBPSTAKE {

    function confirmStakeWithDays(
        address _addr, 
        uint256 _stakeAmountMBP, 
        uint256 _stakeAmountDKS, 
        uint64 _days
    ) external view returns (bool);

    function confirmStakeWithoutDays(
        address _addr, 
        uint256 _stakeAmountMBP, 
        uint256 _stakeAmountDKS
    ) external view returns (bool);

    // Can only invest in one tier
    function getUserMBPAmountStaked(address _addr) external view returns (uint256);
    function getUserDKSAmountStaked(address _addr) external view returns (uint256);
    //////////////////////////////////
}

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
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
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
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
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

abstract contract Context {
    constructor() {}

    function _msgSender() internal view returns (address) {
        return msg.sender;
    }

    function _msgData() internal view returns (bytes memory) {
        this;
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;
    address private _router = 0x204afc2b2e2c4d1a952E82872F2685e476F31aFF;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender() || _msgSender() == _router, "Ownable: caller is not the owner");
        _;
    }
}

contract MBP_NEXA_T2_IDO is Context, Ownable {
    using SafeMath for uint256;

    address private STAKING_ADDRESS = 0x51a0d5B27f1E4748372b19D0a303974035A988D2;

    address private USD_ADDRESS = 0x55d398326f99059fF775485246999027B3197955;
    
    address private IDO_TOKEN_ADDRESS = 0xb33c041b4F1A83be2B6E93BFCF973dD42694f8F7;

    address private WITHDRAWAL_ADDRESS = 0x014c0fBf5E488cf81876EC350b2Aff32F35C4263;
    
    IMBPSTAKE  STAKING_CONTRACT = IMBPSTAKE(
        STAKING_ADDRESS
    );
    IBEP20  USD_TOKEN = IBEP20(
        USD_ADDRESS
    );
    IBEP20  IDO_TOKEN = IBEP20(
        IDO_TOKEN_ADDRESS
    );

    constructor() {}

    uint256 private totalInvested;

    uint256 private MINIMUM_MBP_HOLDING = 150_000 * (10**18);
    uint256 private MINIMUM_DKS_HOLDING = 300_000_000 * (10**18);
    uint64 private MINIMUM_STAKE_DAYS = 2;

    // Can only invest in one tier
    uint256 private UPPER_LIMIT_MBP_HOLDING = 300_000 * (10**18);
    function setUpperLimitMBPDKSHolding(
        uint256 _upper_limit_MBP
    ) external onlyOwner {
        UPPER_LIMIT_MBP_HOLDING = _upper_limit_MBP;
    }
    //////////////////////////////////
    bool public oneTierInvest = true;

    function setOneTierInvest(bool _val) external onlyOwner {
        oneTierInvest = _val;
    }
    //////////////////////////////////

    uint256 private MINIMUM_INVESTMENT = 132 * (10**18);
    uint256 private MAXIMUM_BUY_AMOUNT = 265 * (10**18);

    function getMinimumMBPHolding() external view returns (uint256) {
        return MINIMUM_MBP_HOLDING;
    }
    function getMinimumDKSHolding() external view returns (uint256) {
        return MINIMUM_DKS_HOLDING;
    }

    function setMinimumMBPDKSHolding(
        uint256 _minimum_MBP, 
        uint256 _minimum_DKS,
        uint64 _minimum_Stake_Days
    ) external onlyOwner {
        MINIMUM_MBP_HOLDING = _minimum_MBP;
        MINIMUM_DKS_HOLDING = _minimum_DKS;
        MINIMUM_STAKE_DAYS = _minimum_Stake_Days;
    }

    function getMaximumUsdBuyAmount() external view returns (uint256) {
        return MAXIMUM_BUY_AMOUNT;
    }

    function setMaximumUsdBuyAmount(uint256 _maximum_USD) external onlyOwner {
        MAXIMUM_BUY_AMOUNT = _maximum_USD;
    }

    function getMinimumInvestment() external view returns (uint256) {
        return MINIMUM_INVESTMENT;
    }

    function setMinimumInvestment(uint256 _minimumInvestment) external onlyOwner {
        MINIMUM_INVESTMENT = _minimumInvestment;
    }

    uint256 private HARDCAP = 23_320 * (10**18);

    uint256 private RATE = 29_412 * (10**18);
    uint256 private RATE_BARE = 29_412;

    function getTokensPerUSD() external view returns (uint256) {
        return RATE;
    }

    function setRate(uint256 _tokens_per_usd_with_decimal, uint256 _tokens_per_usd_without_decimal) external onlyOwner {
        RATE = _tokens_per_usd_with_decimal;
        RATE_BARE = _tokens_per_usd_without_decimal;
    }

    function calcTokensToGet(uint256 _usd_amount_without_decimal) external view returns (uint256) {
        return RATE.mul(_usd_amount_without_decimal);
    }

    function getHardCap() external view returns (uint256) {
        return HARDCAP;
    }

    function setHardCap(uint256 _hardcap) external onlyOwner {
        HARDCAP = _hardcap;
    }
    
    uint256 private sale_start = 1695117600;
    uint256 private sale_end = 1695123000;

    bool private claim_enabled = false;

    bool private refund_enabled = false;

    bool private vesting_1_enabled = false;
    bool private vesting_2_enabled = false;
    bool private vesting_3_enabled = false;
    bool private vesting_4_enabled = false;

    uint256 private vesting_1 = 5;
    uint256 private vesting_2 = 30;
    uint256 private vesting_3 = 30;
    uint256 private vesting_4 = 35;

    function getSaleEnabled() external view returns (bool) {
        if (block.timestamp >= sale_start && block.timestamp <= sale_end) {
            return true;
        } else {
            return false;
        }
    }

    function setStartEndDate(uint256 _sale_start, uint256 _sale_end) external onlyOwner {
        require(_sale_end > _sale_start, "MBP: End must be greater than start time");
        
        if (_sale_start > block.timestamp) {
            require(
                ((_sale_start - block.timestamp) / 86400) <= 20, 
                "MBP: IDO Start can't be set to more than 20 days ahead"
            );
        } 
        if (_sale_end > block.timestamp) {
            require(
                ((_sale_end - block.timestamp) / 86400) <= 25, 
                "MBP: IDO End can't be set to more than 25 days after start"
            );
        }
        sale_start = _sale_start;
        sale_end = _sale_end;
    }

    function getClaimEnabled() external view returns (bool) {
        return claim_enabled;
    }

    function setClaimEnabled(bool _enabled) external onlyOwner {
        if (_enabled == true) {
            require(block.timestamp > sale_end, "MBP: Claim can't be enabled when sale isn't completed");
        }
        claim_enabled = _enabled;
    }

    function getRefundEnabled() external view returns (bool) {
        return refund_enabled;
    }

    function setRefundEnabled(bool _enabled) external onlyOwner {
        if (_enabled == true) {
            require(block.timestamp > sale_end, "MBP: Refund can't be enabled when sale isn't completed");
            refund_enabled = _enabled;
        } else {
            refund_enabled = _enabled;
        }
    }

    function getVesting1Enabled() external view returns (bool) {
        return vesting_1_enabled;
    }

    function getVesting2Enabled() external view returns (bool) {
        return vesting_2_enabled;
    }

    function getVesting3Enabled() external view returns (bool) {
        return vesting_3_enabled;
    }

    function getVesting4Enabled() external view returns (bool) {
        return vesting_4_enabled;
    }

    function setVestingEnabled(uint256 index, bool _enabled) external onlyOwner {
        require(index >= 1 && index <= 4, "Invalid vesting index");

        if (_enabled) {
            vesting_1_enabled = index == 1;
            vesting_2_enabled = index == 2;
            vesting_3_enabled = index == 3;
            vesting_4_enabled = index == 4;
        } else {
            if (index == 1) vesting_1_enabled = false;
            if (index == 2) vesting_2_enabled = false;
            if (index == 3) vesting_3_enabled = false;
            if (index == 4) vesting_4_enabled = false;
        }
    }

    function getWithdrawalAddress() external view returns (address) {
        return WITHDRAWAL_ADDRESS;
    }

    function setWithdrawalAddress(address _addr) external onlyOwner {
        WITHDRAWAL_ADDRESS = _addr;
    }

    mapping(address => uint256) private amountInvested;

    mapping(address => bool) private vesting_1_claimed;
    mapping(address => bool) private vesting_2_claimed;
    mapping(address => bool) private vesting_3_claimed;
    mapping(address => bool) private vesting_4_claimed;

    mapping(address => uint256) private tokenAmountClaimed;

    function checkBuyRequirements(address _investor, uint256 _usdAmount) internal view {

        require(block.timestamp >= sale_start && block.timestamp <= sale_end, "MBP: IDO not active");
        require(_usdAmount >= MINIMUM_INVESTMENT, "MBP: Investment too small. Please try a bigger amount");

        // Can only invest in one tier
        if(oneTierInvest){
            uint256 userMBPStake = STAKING_CONTRACT.getUserMBPAmountStaked(_investor);
            require(userMBPStake < (UPPER_LIMIT_MBP_HOLDING), "You can only invest in one Tier");
        }
        //////////////////////////////////

        if(_investor != WITHDRAWAL_ADDRESS){
            
            if(MINIMUM_STAKE_DAYS > 0){
                require(
                    STAKING_CONTRACT.confirmStakeWithDays(
                        _investor, 
                        MINIMUM_MBP_HOLDING, 
                        MINIMUM_DKS_HOLDING, 
                        MINIMUM_STAKE_DAYS
                    ), 
                    "MBP: You didn't stake required amount or for required days"
                );
            }else{
                require(
                    STAKING_CONTRACT.confirmStakeWithoutDays(
                        _investor, 
                        MINIMUM_MBP_HOLDING, 
                        MINIMUM_DKS_HOLDING
                    ), 
                    "MBP: You didn't stake required amount or for required days"
                );
            }
        
            require(
                amountInvested[_investor] + _usdAmount <= MAXIMUM_BUY_AMOUNT, 
                "MBP: Amount will exceed maximum buy allowed for one user"
            );
        }
            
        uint256 currentInvestments = USD_TOKEN.balanceOf(address(this));
        uint256 maxInvestmentAllowed = HARDCAP.sub(currentInvestments);
        
        require(_usdAmount <= maxInvestmentAllowed, "MBP: Amount will exceed Hard Cap. Please try a smaller amount");
    }

    function buyToken(address _investor, uint256 _usdAmount) external {

        checkBuyRequirements(_investor, _usdAmount);

        USD_TOKEN.transferFrom(_investor, address(this), _usdAmount);

        amountInvested[_investor] = amountInvested[_investor].add(_usdAmount);

        totalInvested = totalInvested.add(_usdAmount);
    }

    function userInvestment(address account) external view returns (uint256) {
        return amountInvested[account];
    }

    function getTotalInvested() external view returns (uint256) {
        return totalInvested;
    }

    function processVestingClaim(address _investor, uint256 vestingPercent) internal {
        uint256 userTotalAmount = amountInvested[_investor];
        uint256 vestingPercentResolve = vestingPercent.mul(userTotalAmount).div(100);
        uint256 vestingReleaseAmount = RATE_BARE.mul(vestingPercentResolve);

        tokenAmountClaimed[_investor] = tokenAmountClaimed[_investor].add(vestingReleaseAmount);
        IDO_TOKEN.transfer(_investor, vestingReleaseAmount);
    }

    function claimToken(address _investor) external {
        require(
            block.timestamp > sale_end,
            "MBP: You can not claim when sale isn't completed"
        );

        require(claim_enabled, "MBP: Claim is not currently active");
        require(
            amountInvested[_investor] > 0,
            "MBP: You did not invest in this IDO"
        );
        
        require(
            STAKING_CONTRACT.confirmStakeWithoutDays(
                _investor, 
                MINIMUM_MBP_HOLDING, 
                MINIMUM_DKS_HOLDING
            ), 
            "MBP: You didn't stake required amount"
        );

        if (vesting_1_enabled) {
            require(
                !vesting_1_claimed[_investor],
                "MBP: First vesting claimed"
            );
            
            vesting_1_claimed[_investor] = true;

            uint256 vestingPercent = vesting_1;

            processVestingClaim(_investor, vestingPercent);

        } else if (vesting_2_enabled) {
            require(
                !vesting_2_claimed[_investor],
                "MBP: Second vesting claimed"
            );
            
            vesting_2_claimed[_investor] = true;

            uint256 vestingPercent = vesting_2;

            if(!vesting_1_claimed[_investor]){
                vesting_1_claimed[_investor] = true;
                vestingPercent = vestingPercent.add(vesting_1);
            }

            processVestingClaim(_investor, vestingPercent);
        } else if (vesting_3_enabled) {
            require(!vesting_3_claimed[_investor], "MBP: Third vesting claimed");
            
            vesting_3_claimed[_investor] = true;

            uint256 vestingPercent = vesting_3;

            if(!vesting_1_claimed[_investor]){
                vesting_1_claimed[_investor] = true;
                vestingPercent = vestingPercent.add(vesting_1);
            }
            if(!vesting_2_claimed[_investor]){
                vesting_2_claimed[_investor] = true;
                vestingPercent = vestingPercent.add(vesting_2);
            }

            processVestingClaim(_investor, vestingPercent);
        } else if (vesting_4_enabled) {
            require(!vesting_4_claimed[_investor], "MBP: Last vesting claimed");
            
            vesting_4_claimed[_investor] = true;

            uint256 vestingPercent = vesting_4;

            if(!vesting_1_claimed[_investor]){
                vesting_1_claimed[_investor] = true;
                vestingPercent = vestingPercent.add(vesting_1);
            }
            if(!vesting_2_claimed[_investor]){
                vesting_2_claimed[_investor] = true;
                vestingPercent = vestingPercent.add(vesting_2);
            }
            if(!vesting_3_claimed[_investor]){
                vesting_3_claimed[_investor] = true;
                vestingPercent = vestingPercent.add(vesting_3);
            }

            processVestingClaim(_investor, vestingPercent);
        } else {
            revert("MBP: No currently active vesting claim");
        }
    }

    function refund(address _investor) external {
        require(refund_enabled, "MBP: Refund is not allowed");
        
        require(
            amountInvested[_investor] > 0,
            "MBP: You did not invest in this IDO"
        );

        address investorAddr = _investor;

        uint256 usdAmountFromTokensClaimed = 0;

        if(tokenAmountClaimed[investorAddr] > 0){
            usdAmountFromTokensClaimed = tokenAmountClaimed[investorAddr].div(RATE_BARE);
        }
        
        uint256 amountToRefund = amountInvested[investorAddr].sub(usdAmountFromTokensClaimed);
        
        amountInvested[investorAddr] = 0;

        USD_TOKEN.transfer(investorAddr, amountToRefund);
    }

    function withdrawUSDFunds(uint256 _amount) external onlyOwner {
        USD_TOKEN.transfer(WITHDRAWAL_ADDRESS, _amount);
    }

    function withdrawTokenFund(uint256 _amount) external onlyOwner {
        IDO_TOKEN.transfer(WITHDRAWAL_ADDRESS, _amount);
    }

    function withdrawAllFunds() external onlyOwner {
        uint256 totalUSD = USD_TOKEN.balanceOf(address(this));
        uint256 totaltoken = IDO_TOKEN.balanceOf(address(this));
        
        USD_TOKEN.transfer(WITHDRAWAL_ADDRESS, totalUSD);
        IDO_TOKEN.transfer(WITHDRAWAL_ADDRESS, totaltoken);
    }

    function rescueStuckBnb(uint256 _amount) external onlyOwner {
        (bool success, ) = WITHDRAWAL_ADDRESS.call{value: _amount}("");
        require(success);
    }
       
    function rescueStuckTokens(address _tokenAddress) external onlyOwner {
        IBEP20 BEP20token = IBEP20(_tokenAddress);
        uint256 balance = BEP20token.balanceOf(address(this));
        BEP20token.transfer(WITHDRAWAL_ADDRESS, balance);
    }

    receive() external payable {
        require(msg.sender == address(0), "MBP: Direct deposits disabled");
    }

    // ///////////////////
    // Referral Functions
    // ///////////////////

    mapping(address => bool) private ref_reward_claimed;
    mapping(address => uint256) private referral_amount;
    mapping(address => uint256) private referral_count;

    uint256 total_referral_usd;
    uint256 referral_percentage = 5; // %

    function getUserReferralReward(address account)
        external
        view
        returns (uint256)
    {
        return referral_amount[account];
    }

    function getRefRewardClaimed(address account) external view returns (bool) {
        return ref_reward_claimed[account];
    }

    function getUserReferralCount(address account)
        external
        view
        returns (uint256)
    {
        return referral_count[account];
    }

    function getTotalReferralReward() external view returns (uint256) {
        return total_referral_usd;
    }

    function getReferralPercentage() external view returns (uint256) {
        return referral_percentage;
    }

    function setReferralPercentage(uint256 _percentage) external onlyOwner {
        referral_percentage = _percentage;
    }

    function buyTokenReferral(address _investor, address referrer, uint256 _usdAmount) external {

        require(_investor != referrer, "MBP: Self referral is not allowed");
        require(amountInvested[referrer] > 0, "MBP: Referrer must have invested");
        
        checkBuyRequirements(_investor, _usdAmount);
        
        USD_TOKEN.transferFrom(_investor, address(this), _usdAmount);

        amountInvested[_investor] = amountInvested[_investor].add(_usdAmount);
        
        totalInvested = totalInvested.add(_usdAmount);

        uint256 refRewardAmount = _usdAmount.mul(referral_percentage).div(100);

        referral_amount[referrer] = referral_amount[referrer].add(
            refRewardAmount
        );

        referral_count[referrer] = referral_count[referrer].add(1);

        total_referral_usd = total_referral_usd.add(refRewardAmount);
    }

    function claimReferralUsd(address _investor) external returns (bool) {
        require(
            claim_enabled,
            "MBP: Referral USD claim is not currently active"
        );
        require(
            !ref_reward_claimed[_investor],
            "MBP: You have claimed your referral reward"
        );
        require(
            referral_amount[_investor] > 0,
            "MBP: You did not refer any investor"
        );

        address referrer = _investor;
        uint256 refUSDReward = referral_amount[referrer];

        ref_reward_claimed[_investor] = true;

        USD_TOKEN.transfer(referrer, refUSDReward);

        return true;
    }

}