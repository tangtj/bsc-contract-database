// SPDX-License-Identifier: MIT
//A pyramidical ROI staking BEP20 contract(BNB staking) with profit- 0.1% per hour/ 2.4% per day/ 16.8% per week/ 72% per 30 days/ 800+% per year.
//
// Group of CyberBank: https://t.me/Cyber_Bank_BNB
//
// SPDX-License-Identifier: MIT
//                           _                     _                  _
//  ___ _ __ ___   __ _ _ __| |_    ___ ___  _ __ | |_ _ __ __ _  ___| |_
// / __| '_ ` _ \ / _` | '__| __|  / __/ _ \| '_ \| __| '__/ _` |/ __| __|
// \__ \ | | | | | (_| | |  | |_  | (_| (_) | | | | |_| | | (_| | (__| |_
// |___/_| |_| |_|\__,_|_|   \__|  \___\___/|_| |_|\__|_|  \__,_|\___|\__|
//
// 
//       ███                ████    ██████  ████            ████       █     █     █  █   █
//     ██   ██              █   █   █       █   █           █   █     █ █    ██    █  █  █
//   ██          █       █  █    █  █       █    █          █    █   █   █   █ █   █  █ █
//  ██            █     █   █████   ██████  █████           █████    █   █   █  █  █  ██
//   ██            █   █    █    █  █       █ █             █    █   █████   █   █ █  █ █ 
//     ██   ██      ███     █   █   █       █  █            █   █   █     █  █    ██  █  █
//       ███         █      ████    ██████  █   █           ████    █     █  █     █  █   █
//                  █
//                 █
//
//
//   ▒▒▒▒▒▒     ▒     ▒▒▒▒    ▒     ▒          ▒▒▒        ▒     ▒▒▒  ▒                          ▓▓▓▓▓        ▓     ▓  ▓▓    ▓
//   ▒         ▒ ▒    ▒   ▒   ▒▒    ▒          ▒  ▒▒     ▒ ▒     ▒   ▒                         ▓     ▓       ▓     ▓       ▓
//   ▒        ▒   ▒   ▒    ▒  ▒ ▒   ▒          ▒    ▒   ▒   ▒    ▒   ▒      ▒       ▒                ▓       ▓     ▓      ▓
//   ▒▒▒▒▒▒   ▒   ▒   ▒▒▒▒▒   ▒  ▒  ▒          ▒    ▒   ▒   ▒    ▒   ▒       ▒     ▒               ▓▓        ▓▓▓▓▓▓▓     ▓
//   ▒        ▒▒▒▒▒   ▒ ▒     ▒   ▒ ▒          ▒    ▒   ▒▒▒▒▒    ▒   ▒        ▒   ▒              ▓▓                ▓    ▓
//   ▒       ▒     ▒  ▒  ▒    ▒    ▒▒          ▒  ▒▒   ▒     ▒   ▒   ▒         ▒▒▒              ▓                  ▓   ▓
//   ▒▒▒▒▒▒  ▒     ▒  ▒   ▒   ▒     ▒          ▒▒▒     ▒     ▒  ▒▒▒  ▒▒▒▒▒      ▒              ▓▓▓▓▓▓▓▓  ▓▓        ▓  ▓    ▓▓
//                                                                             ▒
//                                                                            ▒
//   ███  █     █          ████    █     █  ████     ██
//    █   ██    █          █   █   ██    █  █   █    ██
//    █   █ █   █          █    █  █ █   █  █    █   ██
//    █   █  █  █          █████   █  █  █  █████    ██
//    █   █   █ █          █    █  █   █ █  █    █   ██
//    █   █    ██          █   █   █    ██  █   █   
//   ███  █     █          ████    █     █  ████     ██
//
//
pragma solidity ^0.8.0;

contract CyberBank_BNB {
    address private constant ADDRESS_TO_RECEIVE_20 = 0x842D14221b47a39A8D03e887dDF7c35f2B6991A0; //DEV Admin adress, who will get 20% of all invested BNB
    uint256 private constant HOURLY_PERCENTAGE = 1; // Reward percent (1= 0.1%, 10= 1%, 100= 10%)
    uint256 private constant SECONDS_IN_HOUR = 3600; //amount of seconds per reward
    uint256 private constant PERCENTAGE_TO_CONTRACT = 70; //amount, how much % from users deposit will go to the contract
    uint256 private constant PERCENTAGE_TO_ADDRESS = 20; //amount, how much % from users deposit will go to the DEV Admin
    uint256 private constant PERCENTAGE_TO_INVITER_ADDRESS = 10; //amount, how much % from users deposit will go to the Referal Parent

    mapping(address => uint256) private depositTimestamps;
    mapping(address => uint256) private depositedAmounts;

    event Deposited(address indexed investor, uint256 amount);
    event Withdrawn(address indexed investor, uint256 amount);

    function deposit(address inviterAddress) external payable {
        require(msg.value > 0, "Amount must be greater than zero BNB.");
        require(msg.sender != inviterAddress, "You can not Refer yourself, Please write the wallet adress of your real Inviter");

        uint256 amountToContract = (msg.value * PERCENTAGE_TO_CONTRACT) / 100;
        uint256 amountToAddress = (msg.value * PERCENTAGE_TO_ADDRESS) / 100;
        uint256 amountToInviterAddress = msg.value - amountToContract - amountToAddress;

        payable(ADDRESS_TO_RECEIVE_20).transfer(amountToAddress);
        payable(inviterAddress).transfer(amountToInviterAddress);

        depositedAmounts[msg.sender] += msg.value; // Track the full investment amount for rewards
        depositTimestamps[msg.sender] = block.timestamp;

        emit Deposited(msg.sender, amountToContract);
    }

    function claimRewards() external {
        require(depositTimestamps[msg.sender] > 0, "No deposit found for investor.");
        uint256 hoursElapsed = (block.timestamp - depositTimestamps[msg.sender]) / SECONDS_IN_HOUR;
        uint256 amountToClaim = (depositedAmounts[msg.sender] * HOURLY_PERCENTAGE) / 1000 * hoursElapsed;

        require(amountToClaim <= address(this).balance, "Not enough funds in the contract");

        depositTimestamps[msg.sender] = block.timestamp;
        payable(msg.sender).transfer(amountToClaim);

        emit Withdrawn(msg.sender, amountToClaim);
    }

    // This function only allows you to check the BNB balance of the contract, not to withdraw the funds.
    function getContractBalance() external view returns(uint256) {
        return address(this).balance;
    }
}
// Do Your Own Research!!! All Roi-Crypto Projects Have high risks!
// You are responsible for the invested funds yourself!
// Do not invest borrowed funds or those funds that you will be sorry to lose!
// Invest only the money, with the loss of which there will be no critical situations that will affect your well-being!
// With respect CyberBank creator!