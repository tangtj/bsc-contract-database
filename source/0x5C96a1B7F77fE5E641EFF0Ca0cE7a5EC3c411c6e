// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
using SafeMath for uint256;


interface IERC20 {
   
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);


    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 value) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 value) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
}
library SafeMath {


  function mul(uint256 a, uint256 b) internal pure returns (uint256 c) {
    if (a == 0) {
      return 0;
    }
    c = a * b;
    assert(c / a == b);
    return c;
  }


  function div(uint256 a, uint256 b) internal pure returns (uint256) {
   return a / b;
  }


  function sub(uint256 a, uint256 b) internal pure returns (uint256) {
    assert(b <= a);
    return a - b;
  }


  function add(uint256 a, uint256 b) internal pure returns (uint256 c) {
    c = a + b;
    assert(c >= a);
    return c;
  }
}


contract DISTDOM {
    address public owner;
    string public CreatedBy = "Contract developed by Lina Martinez , Marcos Lopes and PintoToken";
    string public A_ProjectWebsite = "www.pintotoken.com";
    string public A_SoundcloudLink = "https://soundcloud.com/pinto-token-pnto/sets/pnto?utm_source=clipboard&utm_medium=text&utm_campaign=social_sharing";
    string public A_PNTO = "0xF704C6F148b8735eB084245701B617096835Da1c";
    string public A_TokenLockBox = "0xeFC9e429B947aEF07F2E8722FDB8f1Ee3F7c7308";
    mapping(address => uint256) public percentages;
    mapping(address => string) public walletNames;
    address[] public wallets;
    uint256 public totalUnallocatedPercentage;
    uint256 public lastDistributionTimestamp;
    uint256 public DISTRIBUTION_INTERVAL = 3 * 30 days;
    bool public isDistributionTimed = true;
    address public TOKEN_ADDRESS = 0x55d398326f99059fF775485246999027B3197955;
    uint256 public nextDistributionTimestamp;
    bool public renounceOwnershipEnabled = false;
   
    struct StandbyWallet {
        address wallet;
        uint256 reservedTokens;
        uint256 removalTimestamp;
    }

    StandbyWallet[] public standbyWallets;

    constructor() {
        owner = msg.sender;
        nextDistributionTimestamp = block.timestamp + DISTRIBUTION_INTERVAL;
   
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

     function automateDistribution() external onlyOwner {
        require(isDistributionTimed, "Distribution is not timed");
        require(block.timestamp >= nextDistributionTimestamp, "Distribution interval not reached");

        distributeTokens();

        nextDistributionTimestamp = block.timestamp + DISTRIBUTION_INTERVAL;
    }

    function addWallet(address wallet, uint256 percentage, string memory walletName) external onlyOwner {
        require(wallet != address(0), "Invalid wallet address");
        require(percentage > 0 && percentage <= 100, "Invalid percentage");
        require(percentages[wallet] == 0, "Wallet already exists");
        require(totalUnallocatedPercentage + percentage <= 100, "Total percentage exceeds 100");

        wallets.push(wallet);
        percentages[wallet] = percentage;
        walletNames[wallet] = walletName;
        totalUnallocatedPercentage += percentage;

        if (isDistributionTimed && block.timestamp - lastDistributionTimestamp >= DISTRIBUTION_INTERVAL) {
            distributeTokens();
        }
    }

    function toggleDistributionTiming() external onlyOwner {
        isDistributionTimed =  !isDistributionTimed;
    }
    function distributeTokens() public onlyOwner {
    require(!isDistributionTimed || block.timestamp - lastDistributionTimestamp >= DISTRIBUTION_INTERVAL, "Distribution interval not reached");

        IERC20 token = IERC20(TOKEN_ADDRESS);
        uint256 totalReservedPercentage = 100 - totalUnallocatedPercentage;


        for (uint256 i = 0; i < wallets.length; i++) {
            address wallet = wallets[i];
            uint256 percentage = percentages[wallet];
            uint256 tokensToTransfer = (token.balanceOf(address(this)) * percentage) / 100;

            if (tokensToTransfer > 0) {
                token.transfer(wallet, tokensToTransfer);
            }
        }

        uint256 reservedTokens = (token.balanceOf(address(this)) * totalReservedPercentage) / 100;
        token.transfer(owner, reservedTokens);

        lastDistributionTimestamp = block.timestamp;
    }
    function updateDistributionInterval(uint256 newInterval) external onlyOwner {
    DISTRIBUTION_INTERVAL = newInterval;
    }

    function calculateReservedTokens(address wallet) internal view returns (uint256) {
        IERC20 token = IERC20(TOKEN_ADDRESS);
        uint256 actualTokensReceived = token.balanceOf(wallet);
        uint256 totalTokensAllocated = (token.balanceOf(address(this)) * percentages[wallet]) / 100;

        if (actualTokensReceived > totalTokensAllocated) {
            uint256 participationTime = block.timestamp - lastDistributionTimestamp;
            uint256 participationPercentage = (participationTime * 100) / DISTRIBUTION_INTERVAL;
            uint256 expectedTokens = (totalTokensAllocated * participationPercentage) / 100;

            return actualTokensReceived - expectedTokens;
        }

        return 0;
    }

    function increaseWalletPercentage(address wallet, uint256 increaseAmount) external onlyOwner {
        require(percentages[wallet] > 0, "Wallet doesn't exist");
        require(increaseAmount > 0 && increaseAmount + percentages[wallet] <= 100, "Invalid increase amount");

        totalUnallocatedPercentage += increaseAmount;
        percentages[wallet] += increaseAmount;
    }

    function decreaseWalletPercentage(address wallet, uint256 decreaseAmount) external onlyOwner {
        require(percentages[wallet] > 0, "Wallet doesn't exist");
        require(decreaseAmount > 0 && percentages[wallet] - decreaseAmount >= 0, "Invalid decrease amount");


        totalUnallocatedPercentage -= decreaseAmount;
        percentages[wallet] -= decreaseAmount;
    }

    function removeWallet(address wallet) external onlyOwner {
        require(percentages[wallet] > 0, "Wallet doesn't exist");


        uint256 reservedTokens = calculateReservedTokens(wallet);
        totalUnallocatedPercentage -= percentages[wallet];
        percentages[wallet] = 0;


        standbyWallets.push(StandbyWallet({
            wallet: wallet,
            reservedTokens: reservedTokens,
            removalTimestamp: block.timestamp
        }));


        if (isDistributionTimed && block.timestamp - lastDistributionTimestamp >= DISTRIBUTION_INTERVAL) {
            distributeTokens();
        }
    }


    function excludeStandbyAddresses() external onlyOwner {
        for (uint256 i = 0; i < standbyWallets.length; i++) {
            StandbyWallet storage standbyWallet = standbyWallets[i];


            if (block.timestamp - standbyWallet.removalTimestamp >= DISTRIBUTION_INTERVAL) {
                delete standbyWallets[i];
            }
        }
    }


    function toggleRenounceOwnership(bool _enable) external onlyOwner {
        renounceOwnershipEnabled = _enable;
    }

    function renounceOwnership() external onlyOwner {
        require(renounceOwnershipEnabled, "Renounce ownership is not enabled");
        owner = address(0);
    }

     function transferOwnership(address newOwner) external onlyOwner {  
         require(newOwner != address(0), "Invalid address");
         owner = newOwner;
    }


    function updateTokenAddress(address newTokenAddress) external onlyOwner {
        TOKEN_ADDRESS = newTokenAddress;
    }

function withdrawAllFunds(address payable recipient) external onlyOwner {
    require(recipient != address(0), "Invalid recipient address");

    IERC20 token = IERC20(TOKEN_ADDRESS);
    uint256 contractBalance = token.balanceOf(address(this));
    if (contractBalance > 0) {
        token.transfer(recipient, contractBalance);
    }
 
    recipient.transfer(address(this).balance);
    }

    function emptyContractBalance(address to) external onlyOwner {
        IERC20 token = IERC20(TOKEN_ADDRESS);
        uint256 contractBalance = token.balanceOf(address(this));
        require(contractBalance > 0, "No balance to transfer");
        token.transfer(to, contractBalance);
    }

    function getNumWallets() external view returns (uint256) {
        return wallets.length;
    }

    function getContractBalance() external view returns (uint256) {
        IERC20 token = IERC20(TOKEN_ADDRESS);
        return token.balanceOf(address(this));

    }

    function getWalletBalance(address wallet) external view returns (uint256) {
        IERC20 token = IERC20(TOKEN_ADDRESS);
        return token.balanceOf(wallet);
    }

    function isWalletRegistered(address wallet) external view returns (bool) {
        return percentages[wallet] > 0;
    }

    function getLastDistributionTimestamp() external view returns (uint256) {
        return lastDistributionTimestamp;
    }

    function getDistributionInterval() external view returns (uint256) {
        return DISTRIBUTION_INTERVAL;
    }

    function getTokenAddress() external view returns (address) {
        return TOKEN_ADDRESS;
    }

    function getWalletAtIndex(uint256 index) external view returns (address, uint256) {
        require(index < wallets.length, "Index out of range");
        address wallet = wallets[index];
        uint256 percentage = percentages[wallet];
        return (wallet, percentage);
    }

    function getAllWalletsAndPercentages() external view returns (address[] memory, uint256[] memory) {
        uint256 validWalletCount = 0;


        for (uint256 i = 0; i < wallets.length; i++) {
            if (percentages[wallets[i]] > 0) {
                validWalletCount++;
            }
        }

        address[] memory validWallets = new address[](validWalletCount);
        uint256[] memory validPercentages = new uint256[](validWalletCount);


        uint256 index = 0;
        for (uint256 i = 0; i < wallets.length; i++) {
            if (percentages[wallets[i]] > 0) {
                validWallets[index] = wallets[i];
                validPercentages[index] = percentages[wallets[i]];
                index++;
            }
        }

        return (validWallets, validPercentages);
    }

    function analyzeContract() external view returns (
        address[] memory _wallets,
        uint256[] memory _percentages,
        bool _isDistributionTimed,
        uint256 _totalUnallocatedPercentage,
        uint256 _lastDistributionTimestamp,
        address _tokenAddress,
        uint256 _standbyWalletCount
    ) {
        _wallets = wallets;
        _percentages = new uint256[](wallets.length);
        for (uint256 i = 0; i < wallets.length; i++) {
            _percentages[i] = percentages[wallets[i]];
        }
        _isDistributionTimed = isDistributionTimed;
        _totalUnallocatedPercentage = totalUnallocatedPercentage;
        _lastDistributionTimestamp = lastDistributionTimestamp;
        _tokenAddress = TOKEN_ADDRESS;
        _standbyWalletCount = standbyWallets.length;
    }

receive() external payable {
   
  }
}