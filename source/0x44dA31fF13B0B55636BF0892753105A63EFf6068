// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.7;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    function decimals() external view returns (uint256);
}

interface IPresaleETH {
    
    event TokensClaimed(address indexed user, uint256 amount, uint256 timestamp);


    event ClaimTimeUpdated(uint256 claimStartTime, uint256 timestamp);

    /// @notice Function can not be called now
    error InvalidTimeframe();

    /// @notice Function can not be called before end of presale
    error PresaleNotEnded();

    /// @notice Trying to buy 0 tokens
    error BuyAtLeastOneToken();

    /// @notice Passed amount is more than amount of tokens remaining for presale
    /// @param tokensRemains - amount of tokens remaining for presale
    error PresaleLimitExceeded(uint256 tokensRemains);

    /// @notice User is in blacklist
    error AddressBlacklisted();

    /// @notice If zero address was passed
    /// @param contractName - name indicator of the corresponding contract
    error ZeroAddress(string contractName);

    /// @notice Passed amount of ETH is not enough to buy requested amount of tokens
    /// @param sent - amount of ETH was sent
    /// @param expected - amount of ETH necessary to buy requested amount of tokens
    error NotEnoughETH(uint256 sent, uint256 expected);

    /// @notice Provided allowance is not enough to buy requested amount of tokens
    /// @param provided - amount of allowance provided to the contract
    /// @param expected - amount of USDT necessary to buy requested amount of tokens
    error NotEnoughAllowance(uint256 provided, uint256 expected);

    /// @notice User already claimed bought tokens
    error AlreadyClaimed();

    /// @notice No tokens were purchased by this user
    error NothingToClaim();
}
/// @title Presale contract for Chancer token
contract BYBPresale is IPresaleETH{
    
    /// @notice Address of token contract
    address public immutable saleToken;
    
    /// @notice Last stage index
    uint8 public constant MAX_STAGE_INDEX = 11;

    /// @notice Total amount of purchased tokens
    uint256 public totalTokensSold;

    /// @notice Timestamp when purchased tokens claim starts
    uint256 public claimStartTime;

    /// @notice Timestamp when presale starts
    uint256 public saleStartTime;

    /// @notice Timestamp when presale ends
    uint256 public saleEndTime;

    /// @notice Array representing cap values of totalTokensSold for each presale stage
    uint32[12] public limitPerStage;

    /// @notice Sale prices for each stage
    uint64[12] public pricePerStage;

    /// @notice Index of current stage
    uint8 public currentStage;

    /// @notice claim duration time 
    uint256 public claimDurationTime = 60 * 60 * 24 * 15;

    /// @notice TGE % 
    uint256 public tgePer = 15;

    /// @notice caim per 
    uint256 public claimPer = 15;

    /// @notice Owner Addess
    address public owner;

    /// @notice addInvestorOwner Addess
    address private addInvestorOwner;

    event TokensBought(
        address indexed user,
        uint256 amount,
        uint256 indexed referrerId,
        uint256 timestamp
    );

    /// @notice Stores the number of tokens purchased by each user that have not yet been claimed
    mapping(address => uint256) public purchasedTokens;
    mapping(address => uint256) public remainingPurchasedTokens;

    /// @notice Indicates whether the user already claimed or not
    mapping(address => uint256) public preClaimTime;
    mapping(address => bool) public hasTgeGenereted;

    /// @notice Checks that it is now possible to purchase passed amount tokens
    /// @param amount - the number of tokens to verify the possibility of purchase
    modifier verifyPurchase(uint256 amount) {
        if (block.timestamp < saleStartTime || block.timestamp >= saleEndTime) revert InvalidTimeframe();  
        if (amount == 0) revert BuyAtLeastOneToken();
        if (amount + totalTokensSold > limitPerStage[MAX_STAGE_INDEX])
            revert PresaleLimitExceeded(limitPerStage[MAX_STAGE_INDEX] - totalTokensSold);
        _;
    }

    /// @notice Creates the contract
    /// @param _saleToken        - Address of presailing token
    /// @param _addInvestorOwner - addInvestorOwner of presailing token
    /// @param _limitPerStage    - Array representing cap values of totalTokensSold for each presale stage
    /// @param _pricePerStage    - Array of prices for each presale stage
    /// @param _saleStartTime    - Sale start time
    /// @param _saleEndTime      - Sale end time
    constructor(
        address _saleToken,
        address _addInvestorOwner,
        uint256 _saleStartTime,
        uint256 _saleEndTime,
        uint32[12] memory _limitPerStage,
        uint64[12] memory _pricePerStage
    ) {
        if (_saleToken == address(0)) revert ZeroAddress("Aggregator");
        owner = msg.sender;
        saleToken = _saleToken;
        addInvestorOwner = _addInvestorOwner;
        limitPerStage = _limitPerStage;
        pricePerStage = _pricePerStage;
        saleStartTime = _saleStartTime;
        saleEndTime = _saleEndTime;
    }
    modifier onlyOwner() {
        require(owner == msg.sender , "Only owner access");
        _;
    }
    modifier onlyInvestorOwner() {
        require(addInvestorOwner == msg.sender , "Only investor owner access");
        _;
    }

    /// @notice To update the sale start and end times
    /// @param _saleStartTime - New sales start time
    /// @param _saleEndTime   - New sales end time
    function configureSaleTimeframe(uint256 _saleStartTime, uint256 _saleEndTime) external onlyOwner {
        if (saleStartTime != _saleStartTime) saleStartTime = _saleStartTime;
        if (saleEndTime != _saleEndTime) saleEndTime = _saleEndTime;
        // emit SaleTimeUpdated(_saleStartTime, _saleEndTime, block.timestamp);
    }

    /// @notice To set the claim start time
    /// @param _claimStartTime - claim start time
    /// @notice Function also makes sure that presale have enough sale token balance
    /// @dev Function can be executed only after the end of the presale, so totalTokensSold value here is final and will not change
    function configureClaim(uint256 _claimStartTime) external onlyOwner {
        if (block.timestamp < saleEndTime) revert  PresaleNotEnded();
        require(IERC20(saleToken).balanceOf(address(this)) >= totalTokensSold * 1e18, "Not enough tokens on contract");
        claimStartTime = _claimStartTime;
        emit ClaimTimeUpdated(_claimStartTime, block.timestamp);
    }

    function configureClaimDuration(uint256 _claimDurationTimeInSeconds) external onlyOwner {
        claimDurationTime = _claimDurationTimeInSeconds;
    }

    function configureClaimPercentage(uint256 _claimPercentage) external onlyOwner {
        claimPer = _claimPercentage;
    }

    function configureTGEPercentage(uint256 _tgePercentage) external onlyOwner {
        tgePer = _tgePercentage;
    }


    /// @notice To buy into a presale using ETH with referrer
    /// @param _amount - Amount of tokens to buy
    /// @param _referrerId - id of the referrer
    function buyToken(
        address _investor,
        uint256 _amount,
        uint256 _referrerId
    ) public verifyPurchase(_amount) onlyInvestorOwner {
        totalTokensSold += _amount;
        purchasedTokens[_investor] += _amount * 1e18;
        remainingPurchasedTokens[_investor] += _amount * 1e18;
        uint8 stageAfterPurchase = _getStageByTotalSoldAmount();
        if (stageAfterPurchase > currentStage) currentStage = stageAfterPurchase;
        emit TokensBought(_investor, _amount,  _referrerId, block.timestamp);
    }

    function TgeGenerete() external  {
        if (block.timestamp < claimStartTime || claimStartTime == 0) revert InvalidTimeframe();
        if (hasTgeGenereted[msg.sender]) revert AlreadyClaimed();
        uint256 amount = purchasedTokens[msg.sender];
        if (amount == 0) revert NothingToClaim();
        uint256 tgAmount = amount * tgePer / 100;
        remainingPurchasedTokens[msg.sender] = amount - tgAmount; 
        preClaimTime[msg.sender] = block.timestamp;
        hasTgeGenereted[msg.sender] = true;
        IERC20(saleToken).transfer(msg.sender, tgAmount );
        emit TokensClaimed(msg.sender, tgAmount, block.timestamp);
    }
// preClaimTime
    /// @notice To claim tokens after claiming starts
    function claim() external  {
        if (block.timestamp < claimStartTime || claimStartTime == 0) revert InvalidTimeframe();
        if (!hasTgeGenereted[msg.sender]) revert("TGE is not generate");
        if (preClaimTime[msg.sender] + claimDurationTime > block.timestamp) revert InvalidTimeframe();
        uint256 claimAmount = remainingPurchasedTokens[msg.sender] * claimPer / 100;
        if (claimAmount == 0) revert NothingToClaim();
        remainingPurchasedTokens[msg.sender] = remainingPurchasedTokens[msg.sender] - claimAmount;
        preClaimTime[msg.sender] = block.timestamp;
        IERC20(saleToken).transfer(msg.sender, claimAmount);
        emit TokensClaimed(msg.sender, claimAmount, block.timestamp);
    }

    /// @notice Returns price for current stage
    function getCurrentPrice() external view returns (uint256) {
        return pricePerStage[currentStage];
    }
    
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        owner = newOwner;
    }
    function modifyStageRate(uint256 _stage,uint64 _rate) public virtual onlyOwner {
        pricePerStage[_stage] = _rate;
    }

    /// @notice Returns amount of tokens sold on current stage
    function getSoldOnCurrentStage() external view returns (uint256) {
        return totalTokensSold - ((currentStage == 0) ? 0 : limitPerStage[currentStage - 1]);
    }

    /// @notice Returns presale last stage token amount limit
    function getTotalPresaleAmount() external view returns (uint256) {
        return limitPerStage[MAX_STAGE_INDEX];
    }

    /// @notice Returns total price of sold tokens
    function totalSoldPrice() external view returns (uint256) {
        return _calculatePriceInUSDTForConditions(totalTokensSold, 0, 0);
    }

    /// @notice Helper function to calculate price in ETH and USDT for given amount
    /// @param _amount - Amount of tokens to buy
    /// @return priceInETH - price for passed amount of tokens in ETH in 1e18 format
    /// @return priceInUSDT - price for passed amount of tokens in USDT in 1e6 format
   
    /// @notice For sending ETH from contract
    /// @param _recipient - Recipient address
    /// @param _ethAmount - Amount of ETH to send in wei
    function _sendValue(address payable _recipient, uint256 _ethAmount) external {
        require(address(this).balance >= _ethAmount, "Low balance");
        (bool success, ) = _recipient.call{ value: _ethAmount }("");
        require(success, "ETH Payment failed");
    }

    /// @notice Recursively calculate USDT cost for specified conditions
    /// @param _amount           - Amount of tokens to calculate price
    /// @param _currentStage     - Starting stage to calculate price
    /// @param _totalTokensSold  - Starting total token sold amount to calculate price
    function _calculatePriceInUSDTForConditions(
        uint256 _amount,
        uint256 _currentStage,
        uint256 _totalTokensSold
    ) internal view returns (uint256 cost) {
        if (_totalTokensSold + _amount <= limitPerStage[_currentStage]) {
            cost = _amount * pricePerStage[_currentStage];
        } else {
            uint256 currentStageAmount = limitPerStage[_currentStage] - _totalTokensSold;
            uint256 nextStageAmount = _amount - currentStageAmount;
            cost =
                currentStageAmount *
                pricePerStage[_currentStage] +
                _calculatePriceInUSDTForConditions(nextStageAmount, _currentStage + 1, limitPerStage[_currentStage]);
        }

        return cost;
    }

    /// @notice Calculate current stage index from total tokens sold amount
    function _getStageByTotalSoldAmount() internal view returns (uint8) {
        uint8 stageIndex = MAX_STAGE_INDEX;
        uint256 totalTokensSold_ = totalTokensSold;
        while (stageIndex > 0) {
            if (limitPerStage[stageIndex - 1] <= totalTokensSold_) break;
            stageIndex -= 1;
        }
        return stageIndex;
    }
}