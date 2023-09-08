// SPDX-License-Identifier: MIT

pragma solidity ^0.6.0;
pragma experimental ABIEncoderV2;



library SafeMath {
   
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }
 
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}


/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address recipient, uint256 amount) external returns (bool);

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
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

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
}



library Address {
    
    function isContract(address account) internal view returns (bool) {
        // This method relies in extcodesize, which returns 0 for contracts in
        // construction, since the code is only stored at the end of the
        // constructor execution.

        uint256 size;
        // solhint-disable-next-line no-inline-assembly
        assembly { size := extcodesize(account) }
        return size > 0;
    }

   
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success, ) = recipient.call{ value: amount }("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
      return functionCall(target, data, "Address: low-level call failed");
    }

    
    function functionCall(address target, bytes memory data, string memory errorMessage) internal returns (bytes memory) {
        return _functionCallWithValue(target, data, 0, errorMessage);
    }

    function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }

    /**
     * @dev Same as {xref-Address-functionCallWithValue-address-bytes-uint256-}[`functionCallWithValue`], but
     * with `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCallWithValue(address target, bytes memory data, uint256 value, string memory errorMessage) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        return _functionCallWithValue(target, data, value, errorMessage);
    }

    function _functionCallWithValue(address target, bytes memory data, uint256 weiValue, string memory errorMessage) private returns (bytes memory) {
        require(isContract(target), "Address: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.call{ value: weiValue }(data);
        if (success) {
            return returndata;
        } else {
            // Look for revert reason and bubble it up if present
            if (returndata.length > 0) {
                // The easiest way to bubble the revert reason is using memory via assembly

                // solhint-disable-next-line no-inline-assembly
                assembly {
                    let returndata_size := mload(returndata)
                    revert(add(32, returndata), returndata_size)
                }
            } else {
                revert(errorMessage);
            }
        }
    }
}



library SafeERC20 {

    using SafeMath for uint256;
    using Address for address;

    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    function safeApprove(IERC20 token, address spender, uint256 value) internal {
        require((value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    function safeIncreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 newAllowance = token.allowance(address(this), spender).add(value);
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }

    function safeDecreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 newAllowance = token.allowance(address(this), spender).sub(value, "SafeERC20: decreased allowance below zero");
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, newAllowance));
    }


    function _callOptionalReturn(IERC20 token, bytes memory data) private {
    
        bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
        if (returndata.length > 0) { // Return data is optional
            // solhint-disable-next-line max-line-length
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}

interface levelcorePlan {
    function transfersecondlvl(address userAddress, uint amount) external;
    function transfertoken(address useraddress,uint amount) external;
    function getuseraddress(address _user) external view returns(address[] memory );
}



contract Metakingxcore  {

    using SafeERC20 for IERC20;

    address public contractOwner;
    address public rewardwallet;
    bool public locked;
    IERC20 public depositToken;
    uint public BASIC_PRICE;
    address public id1;

    mapping(address => User) public users;
    mapping(uint => address) public userIds;
    mapping(uint256 => uint) public levelPrice;
    mapping(uint => address) public idToAddress;

    uint256 public LAST_LEVEL;
    uint public lastUserId;
    address lastReinvestAddress;
    address public levelplanAddress;
    uint public XcoreAndLevelContractTotAmount;

    struct User {
        uint id;
        address referrer;
        bool exists;   
        uint amount;
        uint usercycles;
        mapping(uint256 => bool) activeX6Levels;
        mapping(uint256 => mapping(uint256 => X6)) x6Matrix;
        mapping(uint256 => X3) x3Matrix;
        uint8 userlevels;
        mapping(uint256 => uint8) cycles;
        mapping(uint256 => uint256) userEarnAmount;
        mapping(uint256 => uint256) totalTeamMembers;
    }

    struct X3 {
        address currentReferrer;
        address[] referrals;
        bool blocked;
        uint reinvestCount;
    }


    struct X6 {
        address currentReferrer;
        firstrefValue[] firstLevelReferrals;
        secondrefValue[] secondLevelReferrals;
        uint128 reinvestCount;
        uint256 currentReferrerIndex;
        bool blocked;
        address closedPart;
    }

    struct firstrefValue {
        address refAddress;
        uint refId;
        uint timestamp;
        uint level;
        uint256 amount;
    }

    struct secondrefValue {
        address refAddress;
        uint refId;
        uint timestamp;
        uint level;
        uint256 amount;
    }

    modifier onlyContractOwner() { 
        require(msg.sender == contractOwner, "onlyOwner"); 
        _; 
    }

    modifier onlyUnlocked() { 
        require(!locked || msg.sender == contractOwner); 
        _; 
    }

    event Registration(address indexed user, address indexed referrer, uint indexed userId, uint referrerId);
    event Reinvest(address indexed user, address indexed currentReferrer, address indexed caller, uint8 matrix, uint8 level);
    event NewUserPlace(address indexed user, address indexed referrer, uint8 matrix, uint8 level, uint8 place);
    event MissedEthReceive(address indexed receiver, address indexed from, uint256 level);
    event ReleasedETH(address indexed from, address indexed to, uint256 level, uint value);
    event StoredETH(address indexed from, address indexed to, uint256 level, uint value);
    event SentExtraEthDividends(address indexed from, address indexed receiver, uint8 matrix, uint8 level);
    event MissedEthReceive(address indexed receiver, address indexed from, uint8 matrix, uint8 level);

    constructor(address _ownerAddress, 
        IERC20 _depositTokenAddress,
        address _levelplanAddress,
        address _rewardwallet) public {
        BASIC_PRICE = 15e18;
        LAST_LEVEL = 12;

        levelPrice[1] = BASIC_PRICE;
        levelPrice[2] = 30e18;
        levelPrice[3] = 100e18;
        levelPrice[4] = 200e18;
        levelPrice[5] = 300e18;
        levelPrice[6] = 500e18;
        levelPrice[7] = 1000e18;
        levelPrice[8] = 1500e18;
        levelPrice[9] = 2500e18;
        levelPrice[10] = 5000e18;
        
        id1 = _ownerAddress;
        
        User memory user = User({
            id: 1,
            referrer: address(0),
            exists: true,
            amount:0,
            userlevels:1,
            usercycles:1
        });
        
        users[_ownerAddress] = user;
        idToAddress[1] = _ownerAddress;
        
        for (uint256 i = 1; i <= LAST_LEVEL; i++) {
            users[_ownerAddress].activeX6Levels[i] = true;
        }
        
        userIds[1] = _ownerAddress;

        lastUserId = 2;

        depositToken = _depositTokenAddress;
        contractOwner = msg.sender;
        levelplanAddress = _levelplanAddress;
        rewardwallet = _rewardwallet;

        locked = true;
    }

    function ownerchange(address _AdminAddress) external onlyContractOwner() {
        id1 = _AdminAddress;
    }

    function ActivelevelChange(address _UserAddress,address _referAddress) external onlyContractOwner() {
        require(!isUserExists(_UserAddress), "user exists");
        User memory user = User({
            id: lastUserId,
            referrer: _referAddress,
            exists: true,
            amount:0,
            userlevels:1,
            usercycles:1
        });
        
        users[_UserAddress] = user;
        idToAddress[lastUserId] = _UserAddress;
        userIds[lastUserId] = _UserAddress;
        for (uint256 i = 1; i <= LAST_LEVEL; i++) {
            users[_UserAddress].activeX6Levels[i] = true;
        }
        lastUserId++;
    }

    function changeLock() external onlyContractOwner() {
        locked = !locked;
    }

    function buyNewLevelFor(address userAddress, uint8 level) external onlyUnlocked() {
        _buyNewLevel(userAddress, level);
    }

    function registrationFor(address userAddress, address referrerAddress) external onlyUnlocked() returns(uint256,address) {
        (uint256 id,address refer) = registration(userAddress, referrerAddress);
       return (id,refer);   
    }

    function registrationExt(address userAddress, address referrerAddress) external onlyUnlocked()  {
        require(userAddress != referrerAddress ,"Not a valid address");
        registration(userAddress,referrerAddress);
    }

    function usercompletelevel(address _userAddress, uint8 level) external view returns(bool)  {
        bool active = users[_userAddress].activeX6Levels[level];
        return active;
    }

    function finduplineReferrer(address userAddress, uint256 level) public view returns(address) {
        while (true) {
           address referrer = users[userAddress].referrer;
            if(referrer == id1) {
                return id1;
            }
            if (users[referrer].activeX6Levels[level]) {
                return users[userAddress].referrer;
            }
            if(referrer == address(0)) {
                return id1;
            }
            userAddress = referrer;
        }
        return id1;
    }

    function _buyNewLevel(address _userAddress, uint8 level) internal {
        require(isUserExists(_userAddress), "user is not exists. Register first.");
        address freeX6Referrer = finduplineReferrer(_userAddress, level);  

        uint rewardpercent = (levelPrice[level] * 10)/100;
        uint usrrewardsprn = levelPrice[level] - rewardpercent;
        uint refersamount = ((usrrewardsprn/2) * 50 )/100;

        depositToken.safeTransferFrom(_userAddress,rewardwallet, rewardpercent);
        depositToken.safeTransferFrom(_userAddress,levelplanAddress, usrrewardsprn/2);

    
        XcoreAndLevelContractTotAmount = XcoreAndLevelContractTotAmount + levelPrice[level];
        levelcorePlan(levelplanAddress).transfersecondlvl(_userAddress,usrrewardsprn/2);
        
        require(level > 1 && level <= LAST_LEVEL, "invalid level");
       
        require(users[_userAddress].activeX6Levels[level-1], "buy previous level first");
        require(!users[_userAddress].activeX6Levels[level], "level already activated"); 
        uint8 cycleNum = users[_userAddress].cycles[level];

        if (users[_userAddress].x6Matrix[level-1][cycleNum].blocked) {
            users[_userAddress].x6Matrix[level-1][cycleNum].blocked = false;
        }
            
        users[_userAddress].activeX6Levels[level] = true;
        updateX6Referrer(_userAddress, freeX6Referrer, level, cycleNum, refersamount);
    }


    function registration(address userAddress,address referrerAddress) private returns(uint ,address) {
        XcoreAndLevelContractTotAmount = XcoreAndLevelContractTotAmount + BASIC_PRICE;
         
        require(!isUserExists(userAddress), "user exists");
        User memory user = User({
            id: lastUserId,
            referrer: referrerAddress,
            exists: true,
            amount:BASIC_PRICE,
            userlevels:1,
            usercycles:1
        });
        users[userAddress] = user;

        idToAddress[lastUserId] = userAddress;
        users[userAddress].activeX6Levels[1] = true;

        uint rewardpercent = (levelPrice[1] * 10)/100;
        uint usrrewardsprn = levelPrice[1] - rewardpercent;

        depositToken.safeTransferFrom(userAddress,rewardwallet, rewardpercent);
        depositToken.safeTransferFrom(userAddress,levelplanAddress, usrrewardsprn/2);
        levelcorePlan(levelplanAddress).transfertoken(userAddress,usrrewardsprn/2);
        uint refersamount = ((usrrewardsprn/2) * 50 )/100;

        lastUserId++;
        uint8 lvlNumber = users[referrerAddress].userlevels;
        uint8 level = users[userAddress].userlevels;
        users[referrerAddress].userEarnAmount[level] = users[referrerAddress].userEarnAmount[level] + refersamount;
        users[referrerAddress].totalTeamMembers[level] = users[referrerAddress].totalTeamMembers[level]+1;
        
        address currentReferrer = findFreeX6Referrer(userAddress, users[userAddress].userlevels);
      
        updateX6Referrer(userAddress, currentReferrer, users[userAddress].userlevels ,users[referrerAddress].cycles[lvlNumber], refersamount);
      
        emit Registration(userAddress, referrerAddress, users[userAddress].id, users[referrerAddress].id);
        return (users[userAddress].id,users[userAddress].referrer);
    }

    function getcycles(address useraddress,uint levels) public view returns(uint) {
        return users[useraddress].cycles[levels];
    }

    function userlevsl(address useraddress) public view returns(uint) {
        return users[useraddress].userlevels;
    }

    function returnusrearn(address _userAddress,uint8 level) public view returns(uint,uint) {
        uint amts = users[_userAddress].userEarnAmount[level];
        uint Teammembers = users[_userAddress].totalTeamMembers[level];
        return (amts,Teammembers);
    }
    

    function updateX6Referrer(address userAddress, address referrerAddress, uint8 level, uint8 cycles, uint256 refersamount) private {
        if (users[referrerAddress].x6Matrix[level][cycles].firstLevelReferrals.length < 3) {
            firstrefValue memory newReferralfirst = firstrefValue({
                refAddress: userAddress,
                refId: users[userAddress].id,
                timestamp:block.timestamp,
                level:level,
                amount:refersamount
            });
            users[referrerAddress].x6Matrix[level][cycles].firstLevelReferrals.push(newReferralfirst);
            //set current level
            users[userAddress].x6Matrix[level][cycles].currentReferrer = referrerAddress;
            if (referrerAddress == id1) {
                return sendETHDividends(userAddress,level);
            }

            address ref = users[referrerAddress].x6Matrix[level][cycles].currentReferrer;             
            uint len = users[ref].x6Matrix[level][cycles].firstLevelReferrals.length;
            return updateX6ReferrerSecondLevel(userAddress, ref, level, cycles,refersamount);
        }
        
        updateX6ReferrerSecondLevel(userAddress, referrerAddress, level, cycles,refersamount);
    }



    function updateX6(address userAddress, address referrerAddress, uint8 level, bool x2, uint8 cycles, uint256 refersamount) private {
         firstrefValue memory newReferralfirst = firstrefValue({
                refAddress: userAddress,
                refId: users[userAddress].id,
                timestamp: block.timestamp,
                level: level,
                amount: refersamount
            });
        if (!x2) {
            address usr = getFirstLevelReferral(referrerAddress,0,level,cycles);
            
            users[usr].x6Matrix[level][cycles].firstLevelReferrals.push(newReferralfirst);
           
            //set current level
            users[userAddress].x6Matrix[level][cycles].currentReferrer = getFirstLevelReferral(referrerAddress,0,level,cycles);
        } else {
             address usr2 = getFirstLevelReferral(referrerAddress,1,level,cycles);
            users[usr2].x6Matrix[level][cycles].firstLevelReferrals.push(newReferralfirst);
            //set current level
            users[userAddress].x6Matrix[level][cycles].currentReferrer = getFirstLevelReferral(referrerAddress,1,level,cycles);
        }
    }


    function updateX6ReferrerSecondLevel(address userAddress, address referrerAddress, uint8 level, uint8 cycles, uint256 refersamount) private {
        if (users[referrerAddress].x6Matrix[level][cycles].secondLevelReferrals.length < 9) {
            secondrefValue memory newReferralsecond = secondrefValue({
                refAddress: userAddress,
                refId: users[userAddress].id,
                timestamp:block.timestamp,
                level:level,
                amount:refersamount
            });
            users[referrerAddress].x6Matrix[level][cycles].secondLevelReferrals.push(newReferralsecond);
            users[referrerAddress].userEarnAmount[level] = users[referrerAddress].userEarnAmount[level] + ((levelPrice[level]/2) * 50 )/100;
            users[referrerAddress].totalTeamMembers[level] = users[referrerAddress].totalTeamMembers[level]+1;
            return sendETHDividends(userAddress, level);
        }

        if(users[referrerAddress].activeX6Levels[level]) {
            users[referrerAddress].cycles[level]++;
        }

        if (referrerAddress != id1) {
            address freeReferrerAddress = finduplineReferrer(userAddress,level+1);
            updateX6Referrer(userAddress, freeReferrerAddress, level,users[freeReferrerAddress].cycles[level], refersamount);
        } 
        if(referrerAddress == id1) {
            updateX6Referrer(userAddress, referrerAddress, level,users[referrerAddress].cycles[level],refersamount); 
        }
    }


    function sendETHDividends(address userAddress,uint8 level) private  {
        address freeX6Referrer = finduplineReferrer(userAddress, level); 
        address uplineref = users[freeX6Referrer].referrer;
        uint rewardpercent = (levelPrice[level] * 10)/100;
        uint usrrewardsprn = levelPrice[level] - rewardpercent;

        uint refersamount = ((usrrewardsprn/2) * 50 )/100;
        if(uplineref != address(0) && uplineref != userAddress) {
            depositToken.safeTransferFrom(msg.sender,freeX6Referrer, refersamount);
            depositToken.safeTransferFrom(msg.sender,uplineref, refersamount);
        }
        else {
            depositToken.safeTransferFrom(msg.sender,id1, usrrewardsprn/2);
        }
    }


    function findFreeX6Referrer(address userAddress, uint256 level) public view returns(address) {
        while (true) {
            if (users[users[userAddress].referrer].activeX6Levels[level]) {
                return users[userAddress].referrer;
            }
            userAddress = users[userAddress].referrer;
        }
    }

    function isUserExists(address user) public view returns (bool) {
        return (users[user].id != 0);
    }

    function usersX6Matrix(address userAddress, uint8 level, uint8 cycles) public view returns(address, firstrefValue[] memory, secondrefValue[] memory,uint,uint) {
        return (
            users[userAddress].x6Matrix[level][cycles].currentReferrer,
            users[userAddress].x6Matrix[level][cycles].firstLevelReferrals,
            users[userAddress].x6Matrix[level][cycles].secondLevelReferrals,
            users[userAddress].userlevels,
            users[userAddress].cycles[level]);
    }

    function getFirstLevelReferral(address _referrer, uint256 index,uint level, uint cycles) public view returns (address) {
        firstrefValue storage referral = users[_referrer].x6Matrix[level][cycles].firstLevelReferrals[index];
        return (referral.refAddress);
    }

    function getSecondLevelReferral(address _referrer, uint256 index,uint level, uint cycles) public view returns (address) {
        secondrefValue storage referral = users[_referrer].x6Matrix[level][cycles].secondLevelReferrals[index];
        return (referral.refAddress);
    }



    function changelevelplnaddress(address _levelplanAddress) external onlyContractOwner() {
        levelplanAddress = _levelplanAddress;
    }


    function withdrawLostTokens(address tokenAddress) public onlyContractOwner() {
        if (tokenAddress == address(0)) {
            payable(contractOwner).transfer(address(this).balance);
        } else {
            IERC20(tokenAddress).transfer(contractOwner, IERC20(tokenAddress).balanceOf(address(this)));
        }
    }

    function changeRewardWallet(address _rewardWallet) external onlyContractOwner() {
        rewardwallet = _rewardWallet;
    }


    function getBronzerank(address _userAddress) public view returns(address[] memory) {
        uint ids = users[_userAddress].id;
        address[] memory directReferrals = new address[](lastUserId - ids-1);
        uint256 count = 0;

        for(uint256 i = ids+1; i < lastUserId; i++) {
           address refaddr = idToAddress[i];
           if(users[refaddr].referrer == _userAddress) {
                directReferrals[count] = refaddr;
                count++;
           }
        }
        address[] memory actualReferrals = new address[](count);
        for (uint256 i = 0; i < count; i++) {
            actualReferrals[i] = directReferrals[i];
        }
        return actualReferrals;
    }

    function getindirectref(address _userAddress) public view returns(address[] memory) {
        uint ids = users[_userAddress].id;
        address[] memory Referraladdr = new address[](lastUserId - ids-1);
        uint256 count = 0;
        while (ids < lastUserId) {
            address[] memory directRefferrals = getBronzerank(idToAddress[ids]);
            for (uint i = 0; i < directRefferrals.length; i++) {
                Referraladdr[count] = directRefferrals[i];
                count++;
            }
            ids++;
        }
        address[] memory actualReferrals = new address[](count);
        for (uint256 i = 0; i < count; i++) {
            actualReferrals[i] = Referraladdr[i];
        }
        return actualReferrals;
    }

    function getsilverrank(address _userAddress) public  view returns(address[] memory) {
        address[] memory getalladdr = getindirectref(_userAddress);
        address[] memory getsilveraddress = new address[](getalladdr.length);
        
        uint silverCount = 0;
        for(uint i=0; i< getalladdr.length; i++) {
            address[] memory getrefbronze = getBronzerank(getalladdr[i]);
            if(getrefbronze.length > 4 ) {
                getsilveraddress[i] = getalladdr[i];
                silverCount++;
            }
        }
        address[] memory actualReferrals = new address[](silverCount);
        for (uint256 i = 0; i < silverCount; i++) {
            actualReferrals[i] = getsilveraddress[i];
        }
        return actualReferrals;
    }

    function goldrank(address _userAddress) public view returns(address[] memory) {
        address[] memory getalladdr = getindirectref(_userAddress);
        address[] memory getgoldaddress = new address[](getalladdr.length);
        uint goldCount = 0;
        if(getalladdr.length >= 100 && users[_userAddress].activeX6Levels[5]) {
            for(uint i=0; i < getalladdr.length; i++) {
                address[] memory getrefbronze = getsilverrank(getalladdr[i]);
                if(getrefbronze.length > 2 ) {
                    getgoldaddress[i] = getalladdr[i]; 
                    goldCount++;
                }
            }
        }
        address[] memory actualReferrals = new address[](goldCount);
        for (uint256 i = 0; i < goldCount; i++) {
            actualReferrals[i] = getgoldaddress[i];
        }
        return actualReferrals;
    }

    function platinumRank(address _userAddress) public view returns(address[] memory) {
        address[] memory getalladdr = getindirectref(_userAddress);
        address[] memory getgoldaddress = new address[](getalladdr.length);
        uint platinumCount = 0;
        if(getalladdr.length >= 500 && users[_userAddress].activeX6Levels[7]) {
            for(uint i=0; i < getalladdr.length; i++) {
                address[] memory getrefgold = goldrank(getalladdr[i]);
                if(getrefgold.length > 2 ) {
                    getgoldaddress[i] = getalladdr[i]; 
                    platinumCount++;
                }
            }
        }
        address[] memory actualReferrals = new address[](platinumCount);
        for (uint256 i = 0; i < platinumCount; i++) {
            actualReferrals[i] = getgoldaddress[i];
        }
        return actualReferrals;
    }

    function diamondRank(address _userAddress) public view returns(address[] memory) {
        address[] memory getalladdr = getindirectref(_userAddress);
        address[] memory getgoldaddress = new address[](getalladdr.length);
        uint diamondCount = 0;
        if(getalladdr.length >= 2500 && users[_userAddress].activeX6Levels[10]) {
            for(uint i=0; i < getalladdr.length; i++) {
                address[] memory getrefplatinum = platinumRank(getalladdr[i]);
                if(getrefplatinum.length > 1 ) {
                    getgoldaddress[i] = getalladdr[i]; 
                    diamondCount++;
                }
            }
        }
        address[] memory actualReferrals = new address[](diamondCount);
        for (uint256 i = 0; i < diamondCount; i++) {
            actualReferrals[i] = getgoldaddress[i];
        }
        return actualReferrals;
    }

}