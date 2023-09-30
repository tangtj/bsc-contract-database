/**
 *Submitted for verification at BscScan.com on 2023-08-24
*/

// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.16;


/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {

    /**
     * @dev Moves a `value` amount of tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 value) external returns (bool);


    /**
     * @dev Moves a `value` amount of tokens from `from` to `to` using the
     * allowance mechanism. `value` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 value) external returns (bool);
    /**
     * @dev Returns the value of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}


abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(_msgSender());
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if the sender is not the owner.
     */
    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby disabling any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}


contract ChatApayoo  is Ownable {
    struct Tower {
        uint256 coins;
        uint256 money;
        uint256 money2;
        uint256 yield;
        uint256 timestamp;
        uint256 hrs;
        address ref;
        uint256[3] refs;
        uint256[3] refDeps;
        uint8[8] chefs;
        uint256 totalCoinsSpend;
        uint256 totalMoneyReceived;
    }
    mapping(address => Tower) public towers;
    uint256 public totalChefs;
    uint256 public totalTowers;
    uint256 public totalInvested;
    address public manager = 0x4439783Efa4B079485ff2e47B7e97825b6801C1a;
    uint public feePercent = 2; // 2%

    address public feeCo1;
    address public feeCo2;
    uint256 public startUNIX;
    uint256[] refPercent = [8,5,2];
    IERC20 public token;
    IERC20 usdt = IERC20(token);
   
    

    constructor(uint256 startDate, IERC20 _token, address _feeCo1, address _feeCo2){
        require(startDate > 0);
        startUNIX = startDate;
        token = _token;
        feeCo1 = _feeCo1;
        feeCo2 = _feeCo2;
    }

    function addCoins(address ref,uint256 tokenAmount) public {
        usdt.transferFrom(msg.sender, address(this), tokenAmount);
        require(block.timestamp > startUNIX, "We are not live yet!");
        uint256 coins = tokenAmount / 1e16;
        require(coins > 0, "Zero coins");
        address user = msg.sender;
        totalInvested += tokenAmount;
        bool isNew;
        if (towers[user].timestamp == 0) {
            totalTowers++;
            ref = towers[ref].timestamp == 0 ? feeCo1 : ref;
            isNew = true;
            towers[user].ref = ref;
            towers[user].timestamp = block.timestamp;
        }
        refEarning(user, coins,isNew);
        towers[user].coins += coins;
        usdt.transfer(feeCo1,(tokenAmount * feePercent) / 100);
        //usdt.transfer(feeCo2,(tokenAmount * feePercent) / 100);
    }

    function refEarning(address user,uint256 coins,bool isNew) internal
    {
        uint8 i =0;
        address ref = towers[user].ref;
        while(i<3){
            if(ref==address(0)){
                break;
            } 
            if(isNew){
                towers[ref].refs[i]++;
            }
            uint256 refTemp = coins * refPercent[i] / 100;
            towers[ref].coins += (refTemp * 70) / 100;
            towers[ref].money += (refTemp * 100 * 30) / 100;
            towers[ref].refDeps[i] += refTemp;
            i++;
            ref = towers[ref].ref;
        }
    }

    function setToken(IERC20 _token) external onlyOwner {
        require(
            address(token) != address(0),
            "Token address cannot be the zero address"
        );
        token = _token;
    }

    function setFeePercent(uint _feePercent) external onlyOwner {
        feePercent = _feePercent;
    }

    function setFeeAccount(address _feeAccount) external onlyOwner {
        feeCo1 = _feeAccount;
    }

    function setFeeAccount2(address _feeAccount2) external onlyOwner {
        feeCo2 = _feeAccount2;
    }

    function withdrawMoney() public {
        address user = msg.sender;
        uint256 money = towers[user].money;
        towers[user].money = 0;
        uint256 amount = money * 1e14;
        usdt.transfer(user,usdt.balanceOf(address(this)) < amount ? usdt.balanceOf(address(this)) : amount);
    }

    function collectMoney() public {
        address user = msg.sender;
        syncTower(user);
        towers[user].hrs = 0;
        towers[user].money += towers[user].money2;
        
        towers[user].money2 = 0;
    }

    function upgradeTower(uint256 floorId) public {
        require(floorId < 8, "Max 8 floors");
        address user = msg.sender;
        if(floorId>0){
        require(towers[user].chefs[floorId-1]>=5,"Need to buy previous tower");
        }
        syncTower(user);
        towers[user].chefs[floorId]++;
        totalChefs++;
        uint256 chefs = towers[user].chefs[floorId];
        towers[user].coins -= getUpgradePrice(floorId, chefs);
        towers[user].totalCoinsSpend += getUpgradePrice(floorId, chefs);
        towers[user].yield += getYield(floorId, chefs);
    }


    function getChefs(address addr) public view returns (uint8[8] memory) {
        return towers[addr].chefs;
    }

    function getRefEarning(address addr) public view returns (uint256[3] memory _refEarning,uint256[3] memory _refCount) {
        return (towers[addr].refDeps,towers[addr].refs);
    }

    function syncTower(address user) internal {
        require(towers[user].timestamp > 0, "User is not registered");
        if (towers[user].yield > 0) {
            uint256 hrs = block.timestamp / 3600 - towers[user].timestamp / 3600;
            if (hrs + towers[user].hrs > 24) {
                hrs = 24 - towers[user].hrs;
            }
            uint256 yield = hrs * towers[user].yield;
            if((towers[user].totalMoneyReceived+yield) > ((towers[user].totalCoinsSpend)*200)){
                 towers[user].money2 += (towers[user].totalCoinsSpend*200)-(towers[user].totalMoneyReceived);
                 towers[user].totalMoneyReceived += (towers[user].totalCoinsSpend*200)-(towers[user].totalMoneyReceived);
                 towers[user].yield = 0;
                 for(uint8 i=0;i<8;i++){
                        towers[user].chefs[i]=0;
                 }
            }else{
                towers[user].money2 += yield;
                towers[user].totalMoneyReceived += yield;
            }
            towers[user].hrs += hrs;
        }
        towers[user].timestamp = block.timestamp;
    }

    function getUpgradePrice(uint256 floorId, uint256 chefId) internal pure returns (uint256) {
        if (chefId == 1) return [500, 1500, 4500, 13500, 40500, 120000, 365000, 1000000][floorId];
        if (chefId == 2) return [625, 1800, 5600, 16800, 50600, 150000, 456000, 1200000][floorId];
        if (chefId == 3) return [780, 2300, 7000, 21000, 63000, 187000, 570000, 1560000][floorId];
        if (chefId == 4) return [970, 3000, 8700, 26000, 79000, 235000, 713000, 2000000][floorId];
        if (chefId == 5) return [1200, 3600, 11000, 33000, 98000, 293000, 890000, 2500000][floorId];
        revert("Incorrect chefId");
    }



    function getYield(uint256 floorId, uint256 chefId) internal pure returns (uint256) {
        if (chefId == 1) return [41, 130, 399, 1220, 3750, 11400, 36200, 104000][floorId];
        if (chefId == 2) return [52, 157, 498, 1530, 4700, 14300, 45500, 126500][floorId];
        if (chefId == 3) return [65, 201, 625, 1920, 5900, 17900, 57200, 167000][floorId];
        if (chefId == 4) return [82, 264, 780, 2380, 7400, 22700, 72500, 216500][floorId];
        if (chefId == 5) return [103, 318, 995, 3050, 9300, 28700, 91500, 275000][floorId];
        revert("Incorrect chefId");
    }
}