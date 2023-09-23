// SPDX-License-Identifier: MIT

pragma solidity ^0.8.11;

contract WeeklyWhaleBNB {
    address payable public owner;
    mapping(address => uint) public contributions;
    uint public totalContributions;
    uint public lotteryId;
    mapping(uint => address) public lotteryHistory;

    struct Participant {
        address payable user;
        uint cumulativeContribution;
    }
    Participant[] public participants;

    event ParticipantJoined(address indexed participant, uint amount);
    event WinnerPicked(address indexed winner, uint prizeAmount);
    event LotteryReset();

    constructor() {
        owner = payable(msg.sender);
        lotteryId = 1;
    }

    function getWinnerByLottery(uint lottery) public view returns (address) {
        return lotteryHistory[lottery];
    }

    function getBalance() public view returns (uint) {
        return address(this).balance;
    }

    receive() external payable {
        require(msg.value >= 0.01 ether, "Minimum contribution is 0.01 BNB");

        if (msg.sender != owner) { //Exclude contract owner from participating. but alow jackpot funding
            if (contributions[msg.sender] == 0) {
                uint cumulative = (participants.length == 0) ? msg.value : participants[participants.length-1].cumulativeContribution + msg.value;
                participants.push(Participant({user: payable(msg.sender), cumulativeContribution: cumulative}));
            } else {
                for (uint i = 0; i < participants.length; i++) {
                    if (participants[i].user == msg.sender) {
                        participants[i].cumulativeContribution += msg.value;
                        break;
                    }
                }
            }

            contributions[msg.sender] += msg.value;
            totalContributions += msg.value;
            emit ParticipantJoined(msg.sender, msg.value);
        }
    }

    function getRandomNumber() public view returns (uint) {
        return uint(keccak256(abi.encodePacked(block.timestamp, totalContributions)));
    }

    function pickWinner() public onlyowner {
        uint jackpot = address(this).balance;
        uint winningNumber = getRandomNumber() % totalContributions;
        address payable winner = findWinner(winningNumber);

        require(winner != address(0), "No players, jackpot remains for next week");

        uint ownerCut = (jackpot * 10) / 100;  // 10% to owner
        uint winnerPrize = jackpot - ownerCut;

        owner.transfer(ownerCut);
        winner.transfer(winnerPrize);

        emit WinnerPicked(winner, winnerPrize);

        // Store winner in history
        lotteryHistory[lotteryId] = winner;
        lotteryId++;

        // Reset lottery for next round
        resetLottery();
        emit LotteryReset();
    }

    function findWinner(uint target) internal view returns (address payable) {
        uint left = 0;
        uint right = participants.length - 1;

        while (left <= right) {
            uint mid = left + (right - left) / 2;
            if (participants[mid].cumulativeContribution == target) {
                return participants[mid].user;
            } else if (participants[mid].cumulativeContribution < target) {
                left = mid + 1;
            } else {
                if (mid == 0 || participants[mid - 1].cumulativeContribution < target) {
                    return participants[mid].user;
                }
                right = mid - 1;
            }
        }

        return payable(address(0));
    }

    function resetLottery() internal {
        totalContributions = 0;
        for (uint i = 0; i < participants.length; i++) {
            contributions[participants[i].user] = 0;
        }
        delete participants;
    }

    modifier onlyowner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }
}