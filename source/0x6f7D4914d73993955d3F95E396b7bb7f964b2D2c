/**
 *Submitted for verification at BscScan.com on 2023-07-10
*/

// SPDX-License-Identifier: MIT

pragma solidity 0.8.19;

contract ClassicMiner {

    uint256 private LAUNCH_DATE = 1689955200; //TESTNET VALUE
    uint256 public RENOUNCE_FEE_DATE = 1692633600; //30 days from launch TESTNET VALUE
    uint256 _dumpingfactor;
    uint256 private EGGS_TO_HATCH_1MINERS = 2592000;
    uint256 private PSN = 10000;
    uint256 private PSNH = 5000;
    uint256 public JACKPOT = 0.5 ether;
    address public WINNER;
    uint256 public TIME_JACK = LAUNCH_DATE;
    uint256 public LIMIT_ROI = 350; //limit to 3.5% daily

    uint256 public MIN_DEPOSIT_TO_WIN = 0.1 ether;

    address payable private recAdd;
    address payable private rec2Add;
    address payable private mktAdd;
    address payable private mktAdd2;

    mapping (address => uint256) private hatcheryMiners;
    mapping (address => uint256) private claimedEggs;
    mapping (address => uint256) private lastHatch;
    mapping (address => address) private referrals;
    mapping (address => uint256) public totalDeposited;
    mapping (address => uint256) public totalWithdraws;
    uint256 public marketEggs = 108000000000;
    uint256 public dumpedEggs;
    
    constructor(address _rec2 , address _mkt , address _mkt2) {
        require(!isContract(msg.sender) && !isContract(_rec2) && !isContract(_mkt));
        recAdd = payable(msg.sender);
        rec2Add = payable(_rec2);
        mktAdd = payable(_mkt);
		mktAdd2 = payable(_mkt2);
    }
    
    function hatchEggs() public {
        require(isLaunched() && !isContract(msg.sender));
        sendPot();
        uint256 _ROI = returnROI(msg.sender);
        uint256 eggsUsed = getMyEggs(msg.sender);
        uint256 newMiners = eggsUsed / EGGS_TO_HATCH_1MINERS;

        //if ROI is below 1 or 2% daily gives a bonus
        if (_ROI <= 100){
            hatcheryMiners[msg.sender] = hatcheryMiners[msg.sender] + newMiners + (newMiners * 10 / 100);
        } else if (_ROI >= LIMIT_ROI){
            eggsUsed = eggsUsed * LIMIT_ROI / _ROI;
            newMiners = newMiners * LIMIT_ROI / _ROI;
            hatcheryMiners[msg.sender] = hatcheryMiners[msg.sender] + newMiners;
        } else {
            hatcheryMiners[msg.sender] = hatcheryMiners[msg.sender] + newMiners;
        }

        claimedEggs[msg.sender] = 0;
        lastHatch[msg.sender] = block.timestamp;
        marketEggs = marketEggs + (eggsUsed / 5);
    }

    
    function sellEggs() public {
        require(isLaunched() && !isContract(msg.sender));

        sendPot();
        uint256 _ROI = returnROI(msg.sender);

        uint256 hasEggs = getMyEggs(msg.sender);
        
        //if ROI is above 3.5% only the same proportion of the eggs is dumped
        if (_ROI >= LIMIT_ROI) {
            hasEggs = hasEggs * LIMIT_ROI / _ROI;
            //hatcheryMiners[msg.sender] -= hatcheryMiners[msg.sender] * 10 * (block.timestamp - lastHatch[msg.sender]) / 1 days / 1000; //lost 0.5% of the miners daily
            hatcheryMiners[msg.sender] -= hasEggs / EGGS_TO_HATCH_1MINERS / 3; // 1/3 of miners are burnt
        }

        uint256 eggValue = calculateEggSell(hasEggs);

        totalWithdraws[msg.sender] = totalWithdraws[msg.sender] + eggValue;
        claimedEggs[msg.sender] = 0;
        lastHatch[msg.sender] = block.timestamp;

        marketEggs = marketEggs + hasEggs;
        dumpedEggs += hasEggs;
        _dumpingfactor = 100000 + (200000 * dumpedEggs / marketEggs);
        payable(msg.sender).transfer(eggValue);
    }
    
    function beanRewards(address adr) public view returns(uint256) {
        uint256 hasEggs = getMyEggs(adr);
        if (hasEggs == 0){
            return 0;
        }
        uint256 eggValue = calculateEggSell(hasEggs);
        return eggValue;
    }

    //if more than 1 days has passed send the jackpot to the winner
    function sendPot() internal {
        uint256 NOW = block.timestamp;
        if(NOW >= TIME_JACK + 1 days) {
			TIME_JACK = block.timestamp;
            uint256 _balance = getBalance();
            _balance > 50 ether ? JACKPOT = _balance / 100 : JACKPOT = 0.5 ether;
            payable(WINNER).transfer(JACKPOT);
        } 
    }
    
    function buyEggs(address ref) public payable {
        require(isLaunched() && !isContract(msg.sender));
        require(msg.value > 0.01 ether);

        sendPot();

        referrals[msg.sender] != address(0) && referrals[msg.sender] != msg.sender ? referrals[msg.sender] = ref : address(0);
        //Keep track of a users total deposits in BNB
        totalDeposited[msg.sender] = totalDeposited[msg.sender] + msg.value;

        uint256 eggsBought = calculateEggBuy(msg.value, (address(this).balance - msg.value));
        if (dumpedEggs > 0){
            uint256 _dumped;
            eggsBought = eggsBought * _dumpingfactor / 100000;
            eggsBought >= dumpedEggs ? _dumped = eggsBought : _dumped = dumpedEggs;
            dumpedEggs > eggsBought ? dumpedEggs -= eggsBought : dumpedEggs = 0;
        }
        //if more than 30 days have passed taxes are erased
        if (block.timestamp <= RENOUNCE_FEE_DATE){
            eggsBought = eggsBought - (eggsBought * 5 / 100);
            payFee(msg.value);
        } 
        claimedEggs[msg.sender] = claimedEggs[msg.sender] + eggsBought;

        //If referral exist and value is 0.1 BNB or more then referrer gets more miners.

        if (msg.value >= 0.1 ether){
            WINNER = msg.sender;
            TIME_JACK = block.timestamp;
            if (ref != address(0) && !isContract(ref)){
            //hatcheryMiners[ref] = hatcheryMiners[ref] + (eggsBought * 3 / EGGS_TO_HATCH_1MINERS / 100);
            payable(ref).transfer(msg.value * 3 / 100);
            }
        }
        hatchEggs();
        } 

    function getWinnerInfo() public view returns(address,uint256){
        return (WINNER,TIME_JACK);
    }

    function isLaunched() public view returns(bool) {
        return block.timestamp >= LAUNCH_DATE;
    }

    function isContract(address _user) public view returns(bool) {
        return address(_user).code.length != 0;
    }
    
    function calculateTrade(uint256 _eggs,uint256 _mkteggs, uint256 _crrbalance) private view returns(uint256) {
        return (PSN * _crrbalance) / (PSNH + (((PSN * _mkteggs) + (PSNH * _eggs)) / _eggs));
    }
    
    function calculateEggSell(uint256 eggs) public view returns(uint256) {
        uint256 _calcTrade = calculateTrade(eggs,marketEggs,address(this).balance);
        uint256 _limitdaily = totalDeposited[msg.sender] * LIMIT_ROI / 10000;
        uint256 _realcap = (block.timestamp - lastHatch[msg.sender]) * _limitdaily / 1 days;
        if (_calcTrade > _realcap){
        return _realcap;
        } else {
        return _calcTrade;
        } 
    }

    function returnROI(address user) public view returns(uint256) {
        uint256 hasEggs = getMyEggs(user);
        uint256 _calcTrade = calculateTrade(hasEggs,marketEggs,address(this).balance);
        uint256 _daily =  _calcTrade / (block.timestamp - lastHatch[user]) * 1 days ;
        uint256 _dailyROI = 10000 * _daily / totalDeposited[user] ;
        return _dailyROI;
    }
    
    function dumpingFact() public view returns(uint256) {
        return _dumpingfactor;
    }

    function calculateEggBuy(uint256 eth,uint256 contractBalance) public view returns(uint256) {
        return calculateTrade(eth,contractBalance,marketEggs);
    }
    
    function calculateEggBuySimple(uint256 eth) public view returns(uint256) {
        return calculateEggBuy(eth,address(this).balance);
    }
    
    function payFee(uint256 amount) private {
        uint256 fee = amount / 100;
        recAdd.transfer(fee); //1%
        rec2Add.transfer(fee); //1%
        mktAdd.transfer(fee * 25 / 10); //2.5%
		mktAdd2.transfer(fee / 2); //0.5%
    }

    
    function getBalance() public view returns(uint256) {
        return address(this).balance;
    }
    
    function getMyMiners(address adr) public view returns(uint256) {
        return hatcheryMiners[adr];
    }
    
    function getMyEggs(address adr) public view returns(uint256) {
        return claimedEggs[adr] + getEggsSinceLastHatch(adr);
    }
    
    function getEggsSinceLastHatch(address adr) public view returns(uint256) {
        uint256 secondsPassed = min(EGGS_TO_HATCH_1MINERS , (block.timestamp - lastHatch[adr]));
        return secondsPassed * hatcheryMiners[adr];
    }
    
    function min(uint256 a, uint256 b) private pure returns (uint256) {
        return a < b ? a : b;
    }
}