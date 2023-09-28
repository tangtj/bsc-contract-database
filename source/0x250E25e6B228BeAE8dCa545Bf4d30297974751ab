// SPDX-License-Identifier: MIT

/**
 * 
 * ░█████╗░░█████╗░████████╗░██████╗  ██╗░░░██╗░██████╗  ██████╗░░█████╗░░██████╗░░██████╗
 * ██╔══██╗██╔══██╗╚══██╔══╝██╔════╝  ██║░░░██║██╔════╝  ██╔══██╗██╔══██╗██╔════╝░██╔════╝
 * ██║░░╚═╝███████║░░░██║░░░╚█████╗░  ╚██╗░██╔╝╚█████╗░  ██║░░██║██║░░██║██║░░██╗░╚█████╗░
 * ██║░░██╗██╔══██║░░░██║░░░░╚═══██╗  ░╚████╔╝░░╚═══██╗  ██║░░██║██║░░██║██║░░╚██╗░╚═══██╗
 * ╚█████╔╝██║░░██║░░░██║░░░██████╔╝  ░░╚██╔╝░░██████╔╝  ██████╔╝╚█████╔╝╚██████╔╝██████╔╝
 * ░╚════╝░╚═╝░░╚═╝░░░╚═╝░░░╚═════╝░  ░░░╚═╝░░░╚═════╝░  ╚═════╝░░╚════╝░░╚═════╝░╚═════╝░
 *  ___ _ __ ___   __ _ _ __| |_    ___ ___  _ __ | |_ _ __ __ _  ___| |_
 * / __| '_ ` _ \ / _` | '__| __|  / __/ _ \| '_ \| __| '__/ _` |/ __| __|
 * \__ \ | | | | | (_| | |  | |_  | (_| (_) | | | | |_| | | (_| | (__| |_
 * |___/_| |_| |_|\__,_|_|   \__|  \___\___/|_| |_|\__|_|  \__,_|\___|\__|
 *
 *
 * Cats VS Dogs is an economic investment game built on a decentralized smart contract by Binance Smart Chain.
 *
 * The worldwide war between Cats&Dogs.
 *
 * Accruals profit every day.
 *
 * Works only with USDT BSC-BEP20
 *
 * Web: https://cvsd.fun/
 *
*/

pragma solidity ^0.8.19;

interface USDT {
    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

contract CatsVsDogs{
    address immutable public owner;
    address immutable public marketing;
    mapping(address => uint256) fraction;
    mapping(uint256 => address) cats;
    mapping(uint256 => address) dogs;
    mapping(address => address) public ref;
    mapping(address => uint256) level;
    mapping(address => uint256) totalProfit;
    mapping(address => uint256) lastProfit;
    mapping(address => uint256) lastUpgrade;
    mapping(address => uint256) loseProfit;
    mapping(address => uint256) countRefProfit1;
    mapping(address => uint256) countRefProfit2;
    mapping(address => uint256) countRefProfit3;
    uint256 allCats;
    uint256 allDogs;
    mapping(uint256 => uint256) public price;
    mapping(uint256 => uint256) public maxProfit;
    mapping(uint256 => uint256) public dayProfit;
    uint256 immutable public blockUpgrade;
    uint256 immutable timeBlock;
    uint256 immutable public refProfit1;
    uint256 immutable public refProfit2;
    uint256 immutable public refProfit3;
    uint256 immutable public ownerProfit;
    uint256 immutable public marketingProfit;
    uint256 immutable public battleProfit;
    uint256 immutable public battleProfitApponent;
    uint256 immutable public battleMax;
    uint256 immutable public battleMin;
    uint256 public countBattle;
    USDT usdt;

    event SendMoney(address indexed to, uint256 sum);
    event Battle(address indexed who, address indexed whom, uint256 level, uint256 sum, uint256 res);

    constructor() {
        owner = msg.sender;
        price[0] = 5000;
        price[1] = 5000;
        price[2] = 10000;
        price[3] = 10000;
        price[4] = 10000;
        price[5] = 10000;
        price[6] = 10000;
        price[7] = 10000;
        price[8] = 10000;
        price[9] = 10000;
        price[10] = 10000;
        dayProfit[0] = 100;
        dayProfit[1] = 210;
        dayProfit[2] = 440;
        dayProfit[3] = 690;
        dayProfit[4] = 960;
        dayProfit[5] = 1250;
        dayProfit[6] = 1560;
        dayProfit[7] = 1890;
        dayProfit[8] = 2240;
        dayProfit[9] = 2610;
        dayProfit[10] = 3000;
        maxProfit[0] = 7000;
        maxProfit[1] = 14700;
        maxProfit[2] = 30800;
        maxProfit[3] = 48300;
        maxProfit[4] = 67200;
        maxProfit[5] = 87500;
        maxProfit[6] = 109200;
        maxProfit[7] = 132300;
        maxProfit[8] = 156800;
        maxProfit[9] = 182700;
        maxProfit[10] = 210000;
        refProfit1 = 5;
        refProfit2 = 3;
        refProfit3 = 1;
        ownerProfit = 3;
        marketingProfit = 3;
        marketing = 0xc77AdD063ddEdE8DBFB8eED29Bacc6fB366A7Af4;
        ref[owner] = owner;
        timeBlock = 1 days;
        blockUpgrade = timeBlock * 5;
        battleMin = 10;
        battleMax = 20;
        battleProfit = 150;
        battleProfitApponent = 10;
        usdt = USDT(0x55d398326f99059fF775485246999027B3197955);
    }

    function registration(uint256 _fraction) public{
        /**
        * @dev Registration without referral.
        * _fraction - value is 1 - cats, 2 - dogs
        */
        registration(_fraction, owner);
    }

    function registration(uint256 _fraction, address _ref) public{
        /**
        * @dev Registration with referral.
        * _ref - referral address
        * _fraction - value is 1 - cats, 2 - dogs
        */
        require(msg.sender == tx.origin,"Function can only be called by a user account.");
        uint256 _price = price[0] * (10 ** 16);
        require(_fraction == 1 || _fraction == 2, "Incorrect fraction");
        require(usdt.balanceOf(msg.sender) >= _price, "Balance USDT is low");
        require(usdt.allowance(msg.sender, address(this)) >= _price, "USDT is not approve");
        require(fraction[msg.sender] == 0, "User in game");
        require((fraction[_ref] > 0 && _ref != msg.sender) || msg.sender == owner, "Incorrect referral");
        if(usdt.transferFrom(msg.sender, address(this), _price)){
            fraction[msg.sender] = _fraction;
            level[msg.sender] = 0;
            ref[msg.sender] = _ref;
            lastProfit[msg.sender] = block.timestamp;
            lastUpgrade[msg.sender] = block.timestamp;
            if(_fraction == 1){
                cats[allCats] = msg.sender;
                allCats++;
            }else{
                dogs[allDogs] = msg.sender;
                allDogs++;
            }
            sendMoney(owner, _price * ownerProfit / 100);
            sendMoney(marketing, _price * marketingProfit / 100);
            sendRefProfit(msg.sender, _price);
        }
    }

    function upgrade() public{
        /**
        * @dev Upgrade pet. Max level - 10
        */
        require(msg.sender == tx.origin,"Function can only be called by a user account.");
        uint256 _price = price[level[msg.sender]+1] * (10 ** 16);
        require(fraction[msg.sender] > 0, "User not registration");
        require(usdt.balanceOf(msg.sender) >= _price, "Balance USDT is low");
        require(usdt.allowance(msg.sender, address(this)) >= _price, "USDT is not approve");
        require(level[msg.sender] < 10, "Max level");
        require(block.timestamp - lastUpgrade[msg.sender] > blockUpgrade, "Block time Upgrade");
        if(usdt.transferFrom(msg.sender, address(this), _price)){
            getProfit();
            level[msg.sender] += 1;
            lastUpgrade[msg.sender] = block.timestamp;
            sendMoney(owner, _price * ownerProfit / 100);
            sendMoney(marketing, _price * marketingProfit / 100);
            sendRefProfit(msg.sender, _price);
        }
    }

    function sendRefProfit(address _address, uint256 _value) private{
        /**
        * @dev Send referral profit
        * _address - address from referral
        * _value - count USDT 
        */
        address _ref = ref[_address];
        uint256 _level = level[_address];
        if(level[_ref] >= _level){
            countRefProfit1[_ref] += _value * refProfit1 / 100;
            sendMoney(_ref, _value * refProfit1 / 100);
        }
        else
            loseProfit[_ref] += _value * refProfit1 / 100;
        _ref = ref[_ref];
        if(level[_ref] >= _level){
            countRefProfit2[_ref] += _value * refProfit2 / 100;
            sendMoney(_ref, _value * refProfit2 / 100);
        }
        else
            loseProfit[_ref] += _value * refProfit2 / 100;
        _ref = ref[_ref];
        if(level[_ref] >= _level){
            countRefProfit3[_ref] += _value * refProfit3 / 100;
            sendMoney(_ref, _value * refProfit3 / 100);
        }
        else
            loseProfit[_ref] += _value * refProfit3 / 100;
    }   

    function countProfit(address _address) public view returns(uint256 sum){
        /**
        * @dev Count profit for address
        * _address - user address
        * Returns free profit
        */
        if(fraction[_address] == 0)
            return 0;
        uint256 _level = level[_address];
        uint256 _profit = ((block.timestamp - lastProfit[_address]) / timeBlock) * dayProfit[_level];
        if(_profit + totalProfit[_address] <= maxProfit[_level])
            return _profit;
        return maxProfit[_level] - totalProfit[_address];
    }

    function getProfit() public{
        /**
        * @dev Whitdraw profit
        */
        require(msg.sender == tx.origin,"Function can only be called by a user account.");
        uint256 _countProfit = countProfit(msg.sender);
        if(_countProfit > 0){
            totalProfit[msg.sender] += _countProfit;
            lastProfit[msg.sender] = block.timestamp;
            sendMoney(msg.sender, _countProfit * (10 ** 16));
        }
    }

    function sendMoney(address _address, uint256 _sum) private{
        /**
        * @dev Send USDT
        * _address - address to
        * _sum - sum for send
        */
        if(usdt.transfer(_address, _sum))
            emit SendMoney(_address, _sum);
    }

    function getReferrals(address _address) public view returns(
        uint256,
        uint256,
        uint256,
        uint256,
        uint256,
        uint256,
        uint256
    ){
        /**
        * @dev Get list referrals
        * _address - user address
        * Returns (
        *    Count referrals level 1, 
        *    Count referrals level 2, 
        *    Count referrals level 3, 
        *    Lose profit,
        *    Count profit from referrals level 1,
        *    Count profit from referrals level 2,
        *    Count profit from referrals level 3
        *    )
        */
        uint256 _countRef1 = 0;
        uint256 _countRef2 = 0;
        uint256 _countRef3 = 0;
        address _address_l = _address;
        for(uint256 i = 0; i < allCats; i++){
            if(_address_l == ref[cats[i]])
                _countRef1++;
            if(_address_l == ref[ref[cats[i]]])
                _countRef2++;
            if(_address_l == ref[ref[ref[cats[i]]]])
                _countRef3++;
        }
        for(uint256 i = 0; i < allDogs; i++){
            if(_address_l == ref[dogs[i]])
                _countRef1++;
            if(_address_l == ref[ref[dogs[i]]])
                _countRef2++;
            if(_address_l == ref[ref[ref[dogs[i]]]])
                _countRef3++;
        }
        return (
            _countRef1, 
            _countRef2, 
            _countRef3,
            loseProfit[_address_l],
            countRefProfit1[_address_l],
            countRefProfit2[_address_l],
            countRefProfit3[_address_l]
            );
    }

    function getStatistics() public view returns(
        uint256 countCats,
        uint256 countDogs,
        uint256 balance,
        uint256 countBattles
    ){
        /**
        * @dev Get statistics
        * Returns (
        *    Count all cats, 
        *    Count all dogs, 
        *    Count USDT balance contract, 
        *    Count battles
        *    )
        */
        return (allCats, allDogs, usdt.balanceOf(address(this)), countBattle);
    }

    function getProfile(address _address) public view returns(
        uint256,
        uint256,
        uint256,
        uint256,
        uint256
    ){
        /**
        * @dev Get user's profile
        * _address - user address
        * Returns (
        *    User's fraction, 
        *    User's level, 
        *    Total user's profit, 
        *    Free user's profit,
        *    Last time upgrade
        *    )
        */
        return (
            fraction[_address], 
            level[_address], 
            totalProfit[_address], 
            countProfit(_address), 
            lastUpgrade[_address]
            );
    }

    function random(uint256 max, uint256 salt) private view returns(uint256) {
        /**
        * @dev Random generation
        * _max - max value
        * _salt - salt
        * Returns random value
        */
        return uint256(keccak256(abi.encodePacked(block.timestamp * salt, block.prevrandao, msg.sender))) % max;
    }

    function randomBattle(uint256 sum) public{
        /**
        * @dev Start random battle
        * _sum - sum for battle, This value between maxBattle and minBattle
        */
        uint256 _sum = sum * (10 ** 18);
        require(fraction[msg.sender] > 0, "User not registration");
        require(sum <=  battleMax, "Incorrect sum");
        require(sum >= battleMin, "Incorrect sum");
        require(usdt.balanceOf(msg.sender) >= _sum, "Balance USDT is low");
        require(usdt.allowance(msg.sender, address(this)) >= _sum, "USDT is not approve");
        if(usdt.transferFrom(msg.sender, address(this), _sum)){
            uint256 _flag = random(2, countBattle);
            address _apponent;
            if(fraction[msg.sender] == 1){
                _apponent = dogs[random(allDogs, countBattle)];
            }else{
                _apponent = cats[random(allCats, countBattle)];
            }
            if(_flag == 0){
                sendMoney(_apponent, _sum * battleProfitApponent / 100);
                sendMoney(owner, _sum * 5 / 100);
                sendMoney(marketing, _sum * 5 / 100);
            }else{
                sendMoney(msg.sender, _sum * battleProfit / 100);
            }
            countBattle++;
            emit Battle(msg.sender, _apponent, level[_apponent], sum, _flag);
        }
    }
}