// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

interface IERC20 {
    function decimals() external view returns (uint256);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

abstract contract Ownable {
    address internal _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        address msgSender = msg.sender;
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == msg.sender);
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0));
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}




contract ClassicRefund is Ownable {

    uint256 private constant MAX = ~uint256(0);


    mapping(address => address) public _inviter;
    mapping(address => address[]) public _binders;


    uint256 public totalStakeAmount;
    mapping(address => uint256) public stakeAmount;
    mapping(address => bool) public stakeMember;
    mapping(address => uint256) public stakeType;
    mapping(address => uint256) public stakeTime;
    mapping(address => uint256) public userRateTime;
    mapping(address => uint256) public endTime;
    mapping(address => uint256) public receivedReward;




    mapping(address => uint256) public invitorReward;

    uint256 public constant _mineTimeDebt = 86400;

    address public constant deadAddress = address(0x000000000000000000000000000000000000dEaD);
    address public constant usdt_address = address(0x55d398326f99059fF775485246999027B3197955);
    IERC20 public constant USDT = IERC20(usdt_address);





    function _bindInvitor(address account, address invitor) private  returns(bool) {
        if (invitor != address(0) && invitor != account && _inviter[account] == address(0) && _binders[account].length <= 50) {
            uint256 size;
            assembly {size := extcodesize(invitor)}
            if (size > 0) {
                return false ;
            }else{
                _inviter[account] = invitor;
                _binders[invitor].push(account);
                
                return true;
            }
        }
        else{
            return false;
        }
    }

    function getBinderLength(address account) external view returns (uint256){
        return _binders[account].length;
    }    

    function checkRateChange(address account) public view returns(bool){
        uint256 lowerCount =  _binders[account].length;
        uint256 groupStakeAmount;
        for (uint256 i; i < lowerCount; i++) {
            address lowAddress = _binders[account][i];
            groupStakeAmount += stakeAmount[lowAddress];
            
        }
        if(groupStakeAmount >= 10**22){
            return true;
        }else{
            return false;
        }

    }

    function stake(address invitor,uint256 amount,uint256 stakeDays)  external  {
        require(USDT.allowance(msg.sender,address(this))>=amount && amount > 0);
        require(stakeMember[invitor] || invitor == deadAddress);
        require(!stakeMember[msg.sender]);
        
        bool binded;
        if (invitor != address(0) && invitor != msg.sender && _inviter[msg.sender] == address(0)) {
            binded = _bindInvitor(msg.sender,invitor);
        }
        else if (_inviter[msg.sender] == invitor){
            binded = true;
        }else{
            binded = false;
        }
        require(binded);
        USDT.transferFrom(msg.sender,address(this),amount);
        if(stakeDays == 30){
            endTime[msg.sender] = block.timestamp + 30 days;
            stakeType[msg.sender] = 30;
        }else{
            endTime[msg.sender] = block.timestamp + 50 days;
            stakeType[msg.sender] = 50;
        }
        stakeTime[msg.sender] = block.timestamp;
        stakeMember[msg.sender] = true;
        stakeAmount[msg.sender] = amount;
        procesInvitorReward(msg.sender,amount);
        totalStakeAmount += amount;

        bool isChanged = checkRateChange(msg.sender);
        if(isChanged && userRateTime[msg.sender] == 0){
            userRateTime[msg.sender] = block.timestamp;
        }


    }

    function procesInvitorReward(address account, uint256 amount) private {


        address invitor = _inviter[account];
        uint256 invitorAmount;
        if (address(0) != invitor && deadAddress != invitor) {
            invitorAmount = amount * 8/100;
            invitorReward[invitor] += invitorAmount;
            bool isChanged = checkRateChange(invitor);
            if(isChanged && userRateTime[invitor] == 0){
                userRateTime[invitor] = block.timestamp;
            }

        }

    }


    function checkUserReward(address account) public view returns(uint256){
        uint256 totalReward;
        uint256 claimableReward;

        uint256 countDays;
        uint256 rateTime =  userRateTime[account];
        uint256 rate ;
        uint256 beforeCountDays;
        uint256 afterCountDays;
        uint256 beforeReward;
        uint256 afterReward;
        if(stakeType[account] == 30){
            rate = 130;
        }else{
            rate = 160;
        }
        if(rateTime == 0 ){

            if(block.timestamp < endTime[account]){
                countDays = (block.timestamp - stakeTime[account]) / _mineTimeDebt;
            }else{
                countDays = (endTime[account] - stakeTime[account]) / _mineTimeDebt;
            }

            totalReward = stakeAmount[account] * rate * countDays / 10000;
            claimableReward = totalReward - receivedReward[account];

        }else{

            beforeCountDays = (rateTime - stakeTime[account]) / _mineTimeDebt;
            beforeReward = stakeAmount[account] * rate * beforeCountDays / 10000;           

            if(block.timestamp < endTime[account]){
                afterCountDays = (block.timestamp - rateTime) / _mineTimeDebt;

            }else{
                afterCountDays = (endTime[account] - rateTime) / _mineTimeDebt;

            }
            afterReward = stakeAmount[account] * (rate + 40) * afterCountDays / 10000;
            totalReward = beforeReward + afterReward;
            claimableReward = totalReward - receivedReward[account];


        }
        return claimableReward;


    }


    function unstake()  external  {
        uint256 claimableReward = checkUserReward(msg.sender);
        require(block.timestamp > endTime[msg.sender] && stakeMember[msg.sender] && stakeAmount[msg.sender] >0 && claimableReward==0);
        
        uint256 unstakeAmount = stakeAmount[msg.sender];

        stakeAmount[msg.sender] = 0;
        stakeMember[msg.sender] = false;
        stakeTime[msg.sender] = 0;
        endTime[msg.sender] = 0;
        userRateTime[msg.sender] = 0;
        receivedReward[msg.sender] = 0;
        stakeType[msg.sender] = 0;

        totalStakeAmount -= unstakeAmount;

        USDT.transfer(msg.sender,unstakeAmount);

    }
    



    event Received(address sender, uint256 amount);
    event Sended(address sender, address to,uint256 amount);
    receive() external payable {
        uint256 receivedAmount = msg.value;
        emit Received(msg.sender, receivedAmount);
    }




    function claimBalance(address to) external onlyOwner {
        payable(to).transfer(address(this).balance);
    }

    function claimToken(
        address token,
        uint256 amount,
        address to
    ) external onlyOwner {

        IERC20(token).transfer(to, amount);
    }



    

    function getMineReward()external{
        uint256 claimableReward = checkUserReward(msg.sender);
        require(claimableReward > 0);
        receivedReward[msg.sender] += claimableReward;
        USDT.transfer(msg.sender,claimableReward);

        


    }

    function getInvitorReward()external{
        uint256 totalInvitorReward = invitorReward[msg.sender];
        require(totalInvitorReward > 0);
        USDT.transfer(msg.sender,totalInvitorReward);
        invitorReward[msg.sender] = 0;

        
    }


}