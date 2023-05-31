// SPDX-License-Identifier: UNLICENSED
pragma solidity = 0.8.19;

/// @notice Simple single owner authorization mixin.
/// @author Solmate (https://github.com/transmissions11/solmate/blob/main/src/auth/Owned.sol)
abstract contract Owned {
    /*//////////////////////////////////////////////////////////////
                                 EVENTS
    //////////////////////////////////////////////////////////////*/

    event OwnershipTransferred(address indexed user, address indexed newOwner);

    /*//////////////////////////////////////////////////////////////
                            OWNERSHIP STORAGE
    //////////////////////////////////////////////////////////////*/

    address public owner;

    modifier onlyOwner() virtual {
        require(msg.sender == owner, "UNAUTHORIZED");

        _;
    }

    /*//////////////////////////////////////////////////////////////
                               CONSTRUCTOR
    //////////////////////////////////////////////////////////////*/

    constructor(address _owner) {
        owner = _owner;

        emit OwnershipTransferred(address(0), _owner);
    }

    /*//////////////////////////////////////////////////////////////
                             OWNERSHIP LOGIC
    //////////////////////////////////////////////////////////////*/

    function transferOwnership(address newOwner) public virtual onlyOwner {
        owner = newOwner;

        emit OwnershipTransferred(msg.sender, newOwner);
    }
}

interface IERC20 {
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
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

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
    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

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
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

struct Listing {
    uint256 price;
    uint256 amount;
    uint256 totalAmount;
    uint256 index;
    uint256 time;
    address buyer;
}
struct SellListing {
    uint256 itemId;
    uint256 index;
    uint256 price;
    uint256 amount;
    uint256 time;
    address buyer;
    address seller;
}

contract Marketplace is Owned {
    event ItemListed(address indexed buyer, uint256 amount, uint256 price);
    event ItemSell(address indexed seller,address buyer,uint256 price,uint256 amount);
    mapping(uint256 id => Listing) public items;
    mapping(address  => uint256[]) public itemOrder;
    mapping(uint256 id => SellListing) public sellOrders;
    mapping(address => uint256[]) public buyOrder;  
    mapping(address => uint256[]) public sellOrder;  
    mapping(address => uint256) public sellAmount;  


    uint256 public itemCount = 0;
    uint256 public sellCount = 0;
    uint256 public charge = 5;
    uint256 public space = 0;  
    address public charge_address;
    uint256 public amount_double_sell = 10 * 1e18; 
    uint256 public amount_double_buy = 100 * 1e18;   
    uint256 public amount_max_buy = 5000 * 1e18; 
    uint256 public amount_max_sell = 5000 * 1e18; 
    uint256 public amount_max_count_day = 1;
    uint256 public count_hour = 24;
    uint256 public new_price = 2000;  
    uint256 public currenyId = 1;  
    uint256 public total_buy_coin = 0;  
    uint256 public per_amount_price = 50000 * 1e18;
    uint256 public per_up_price = 0;  
    uint256 public prev_up_amount = 0;  
    uint256 public limit_scale = 105;
    mapping(address => uint256) private limitAmount;  
    mapping(address => uint256) public inviteLimit;  

    mapping (address => address) public inviter;
    uint256[] public inviteRadio = [200,100];


    IERC20 public constant coin = IERC20(0x50ab0D88045F540b8B79C8A7Dc25790dB493BBC5);
    IERC20 public constant usdt = IERC20(0x55d398326f99059fF775485246999027B3197955);

    constructor() Owned(msg.sender) {
        charge_address = msg.sender;
    }

     function setCharge(uint256 _charge) external onlyOwner {
        charge = _charge;
    }
    function setAmountDoubleBuy(uint256 _double) external onlyOwner {
        amount_double_buy = _double;
    }
    function setAmountDoubleSell(uint256 _double) external onlyOwner {
        amount_double_sell = _double;
    }
    function setAmountMaxBuy(uint256 _amount) external onlyOwner {
        amount_max_buy = _amount;
    }
    function setAmountMaxSell(uint256 _amount) external onlyOwner {
        amount_max_sell = _amount;
    }
    function setCountHour(uint256 _hour) external onlyOwner{
        count_hour = _hour;
    }
    function setAmountMaxCountDay(uint256 _acount) external onlyOwner{
        amount_max_count_day = _acount;
    }
    function setSpace(uint256 _hour) external onlyOwner {
        space = _hour;
    }
    function setChargeAddress(address _charge_address) external onlyOwner {
        charge_address = _charge_address;
    }
    function setNewPrice(uint256 _price) external onlyOwner {
        new_price = _price;
    }
    function setPreAmountPrice(uint256 _amount) external onlyOwner {
        per_amount_price = _amount;
    }
    function setPreUpPrice(uint256 _upPrice) external onlyOwner {
        per_up_price = _upPrice;
    }
    function setLimitScale(uint256 _limitScale) external onlyOwner {
        limit_scale = _limitScale;
    }
    function setLimitAmount(address acount,uint256 amount) external onlyOwner{
        limitAmount[acount] = amount;
    }
    function setInvite(address account,address invite) external onlyOwner{
        inviter[account] = invite;
    }
    function setPerUpAmount(uint256 amount) external onlyOwner{
        prev_up_amount = amount;
    }
    function setInviteRadio(uint256 one,uint256 two) external  onlyOwner{
        inviteRadio[0] = one;
        inviteRadio[1] = two;
    }
    function getLimitAmount(address acount) external view onlyOwner returns (uint256){
        return limitAmount[acount];
    }
    
    function listItem(uint256 _amount,address invite) external  returns (uint256)  {
        require(_amount % amount_double_buy == 0 && _amount > 0 && _amount <= amount_max_buy,"Illegal amount ");  
        bool flag = (inviter[msg.sender] != address(0) || (invite != msg.sender && invite != address(0) && inviter[invite] != address(0)));
        require(flag ,"The recommender does not exist");
        require(getOrderByDay() < amount_max_count_day, "Only one order can be placed within  hours");
        if(per_up_price == 0){
            addItem(_amount);
        }else{
            if(_amount + total_buy_coin - prev_up_amount < per_amount_price){
                addItem(_amount);
            }else{
                uint256 clac_amount = _amount;
                while(clac_amount > 0){
                    uint256 buy = prev_up_amount + per_amount_price - total_buy_coin;
                    if(buy > clac_amount){
                        addItem(clac_amount);
                        break ;
                    }else{
                        addItem(buy);
                        prev_up_amount += per_amount_price;
                        new_price += per_up_price;
                        clac_amount -= buy;
                    }   
                }
            }
        }
        if(inviter[msg.sender] == address(0)){    
            inviter[msg.sender] = invite;
        }
        caclInviteLimit(_amount*limit_scale/100 -_amount);
        usdt.transferFrom(msg.sender, address(this), _amount);
        emit ItemListed(msg.sender, _amount, new_price/100);
        return total_buy_coin;
    }

    function addItem(uint256 _amount) internal  returns (uint) {
        itemCount++;
        Listing  memory item = Listing(new_price,_amount,_amount,itemCount,block.timestamp,msg.sender);
        items[itemCount] = item;
        total_buy_coin += _amount;
        itemOrder[msg.sender].push(itemCount);
        return itemCount;
    }

    function caclInviteLimit(uint256 amount) internal{
        address a1 = inviter[msg.sender];
        address a2 = inviter[a1];
        addlimit(a1, amount*inviteRadio[0]/1000);
        addlimit(a2, amount*inviteRadio[1]/1000);
    }
    function getOrderByDay() internal view returns (uint256 ) {
        uint256[] memory _itemOrder = itemOrder[msg.sender];
        uint256 count = 0;
        for(uint256 i = 0;i<_itemOrder.length;i++){
            if(block.timestamp < items[_itemOrder[i]].time + (count_hour * 1 hours)){
                count++;
            }
        }
        return count;
    }

    function addlimit(address account,uint256 _limit)internal {
        if(itemOrder[account].length > 0){
            inviteLimit[account] = inviteLimit[account]+_limit;
        }
    }

    function getLimits() external  view returns (uint256){
        uint256[] memory ids = buyOrder[msg.sender];
        uint256 totalLimit = 0;
        for(uint256 i = 0;i<ids.length;i++){
            totalLimit += sellOrders[ids[i]].amount;
        }
        return totalLimit*limit_scale/100;
    }

    function getLimit() internal  view returns (uint256){
        uint256[] memory ids = buyOrder[msg.sender];
        uint256 limit = 0;
        for(uint256 i = 0;i<ids.length;i++){
            if(block.timestamp >= (sellOrders[ids[i]].time + (space * 1 hours))){
                limit += sellOrders[ids[i]].amount;
            }
        }
        return limit*limit_scale/100 ;
    }

    function sellItem(uint256 _amount) external returns(SellListing memory ){
        require(itemCount > 0 && currenyId <= itemCount,"Not buy order");
        require(_amount % amount_double_sell == 0 && _amount > 0 && _amount <= amount_max_sell,"Illegal amount ");
        require(items[currenyId].buyer != msg.sender,"Cannot sell to oneself");    
        require(limitAmount[msg.sender] + getLimit() +  inviteLimit[msg.sender] -sellAmount[msg.sender] >= _amount,"Insufficient credit limit");
        Listing  memory listedItem = items[currenyId];
        uint256 index = currenyId;
        if(_amount >= listedItem.amount){  
            _amount = listedItem.amount;
            items[currenyId].amount = 0;
            currenyId++;
        }else{
            items[currenyId].amount -= _amount;
        }
        sellCount++;
        SellListing  memory sell = SellListing(index,sellCount,listedItem.price,_amount,block.timestamp,listedItem.buyer,msg.sender);
        sellOrders[sellCount] = sell;
        buyOrder[listedItem.buyer].push(sellCount); 
        sellOrder[msg.sender].push(sellCount);
        sellAmount[msg.sender] += _amount;
        usdt.approve(msg.sender, _amount);
        usdt.transfer(msg.sender, _amount * (1000-charge)/1000);
        if(charge>0){
            usdt.transfer(charge_address, _amount * charge/1000);  
        } 
        coin.transferFrom(msg.sender, listedItem.buyer, (_amount/listedItem.price * 100));
        emit ItemSell(msg.sender, listedItem.buyer, listedItem.price, _amount);
        return sell;
    }

    

    function pay(uint256 _amount) external onlyOwner{
        usdt.approve(msg.sender, _amount);
        usdt.transfer(msg.sender, _amount);
    }
    function payCoin(uint256 _amount) external onlyOwner{
        coin.approve(msg.sender, _amount);
        coin.transfer(msg.sender, _amount);
    }

    function itemsWithLength(
        uint256 start,
        uint256 end
    ) external view returns (Listing[] memory) {
        Listing[] memory nitem = new Listing[](end - start+1);
        uint256 index = 0;
        for (uint256 i = start; i <= end && i <= itemCount; i++) {
            nitem[index] = items[i];   
            index++;  
        }
        return nitem;
    }

    function sellsWithLength(
        uint256 start,
        uint256 end
    ) external view returns (SellListing[] memory) {
        SellListing[] memory nitem = new SellListing[](end - start+1);
        uint256 index = 0;
        for (uint256 i = start; i <= end && i <= sellCount; i++) {
            nitem[index] = sellOrders[i]; 
            index++;    
        }
        return nitem;
    }

    function tradeById(uint256 id) external view returns (SellListing memory) {
        return sellOrders[id];
    }

    function itemById(uint256 id) external view returns (Listing memory) {
        return items[id];
    }

    function buyOrderByAddress() external  view returns (SellListing[] memory){
        uint256[] memory ids = buyOrder[msg.sender];
        if(ids.length>0){
            SellListing[] memory nitem = new SellListing[](ids.length);
            for(uint256 i = 0;i<ids.length;i++){
                nitem[i] = sellOrders[ids[i]];
            }
            return nitem;
        }else{
            return new SellListing[](0);
        }
        
    }
    function sellOrderByAddress() external  view returns (SellListing[] memory){
        uint256[] memory ids = sellOrder[msg.sender];
        if(ids.length>0){
            SellListing[] memory nitem = new SellListing[](ids.length);
            for(uint256 i = 0;i<ids.length;i++){
                nitem[i] = sellOrders[ids[i]];
            }
            return nitem;
        }else{
            return new SellListing[](0);
        }
    }
}