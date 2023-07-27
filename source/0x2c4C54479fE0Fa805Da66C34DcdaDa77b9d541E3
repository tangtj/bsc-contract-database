// SPDX-License-Identifier: MIT

/*
 Linktree: https://linktr.ee/CutYourDickoff_
 Twitter: https://twitter.com/cutyourdickoff
 Youtube: https://youtube.com/@DICK-Crypto
 Website: https://cutyourdickoff.men
 Email: support@cutyourdickoff.men
 Telegram:
  - Group: https://t.me/cutyourdickoff
  - Channel: https://t.me/cutmydickoff


     ##### /    ##   ###            ###                                                     ##### ##                        /                                     
  ######  /  #####    ###            ###                                                 /#####  /##      #               #/                                      
 /#   /  /     #####   ###            ##                                               //    /  / ###    ###              ##                                      
/    /  ##     # ##      ##           ##                                              /     /  /   ###    #               ##                                      
    /  ###     #         ##           ##                                                   /  /     ###                   ##                                      
   ##   ##     #         ##    /##    ##      /###      /###   ### /### /###     /##      ## ##      ## ###       /###    ##  /##      /##  ###  /###     /###    
   ##   ##     #         ##   / ###   ##     / ###  /  / ###  / ##/ ###/ /##  / / ###     ## ##      ##  ###     / ###  / ## / ###    / ###  ###/ #### / / #### / 
   ##   ##     #         ##  /   ###  ##    /   ###/  /   ###/   ##  ###/ ###/ /   ###    ## ##      ##   ##    /   ###/  ##/   /    /   ###  ##   ###/ ##  ###/  
   ##   ##     #         ## ##    ### ##   ##        ##    ##    ##   ##   ## ##    ###   ## ##      ##   ##   ##         ##   /    ##    ### ##       ####       
   ##   ##     #         ## ########  ##   ##        ##    ##    ##   ##   ## ########    ## ##      ##   ##   ##         ##  /     ########  ##         ###      
    ##  ##     #         ## #######   ##   ##        ##    ##    ##   ##   ## #######     #  ##      ##   ##   ##         ## ##     #######   ##           ###    
     ## #      #         /  ##        ##   ##        ##    ##    ##   ##   ## ##             /       /    ##   ##         ######    ##        ##             ###  
      ###      /##      /   ####    / ##   ###     / ##    ##    ##   ##   ## ####    / /###/       /     ##   ###     /  ##  ###   ####    / ##        /###  ##  
       #######/ #######/     ######/  ### / ######/   ######     ###  ###  ### ######/ /   ########/      ### / ######/   ##   ### / ######/  ###      / #### /   
         ####     ####        #####    ##/   #####     ####       ###  ###  ### ##### /       ####         ##/   #####     ##   ##/   #####    ###        ###/    
                                                                                      #                                                                           
                                                                                       ##                                                                         
*/

pragma solidity ^0.8.0;

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

contract DickPoolReward {
    IERC20 public token;
    address public owner;
    struct StakeInfo {
        uint256 amount;
        uint256 time;
    }
    mapping(address => StakeInfo) public staker;
    address[] public stakers;
    mapping(address => uint256) public balanceOf;
    uint256 amountStake = 600_000_000_000 * 10**18;
    event Deposit(address indexed sender, uint256 amount);
    event Withdraw(address indexed sender, address indexed receiver, uint256 amount);

    constructor() {
        owner = msg.sender;
    }
    function deposit() public payable {
        balanceOf[msg.sender] += msg.value;
        emit Deposit(msg.sender, msg.value);
    }
    function stake() public {
        require(token.balanceOf(msg.sender) >= amountStake, "not enough token");
        require(staker[msg.sender].amount == 0, "already staked");
        token.transferFrom(msg.sender, address(this), amountStake);
        staker[msg.sender].amount = amountStake;
        staker[msg.sender].time = block.timestamp;
        stakers.push(msg.sender);
    }
    function unstake() public {
        require(staker[msg.sender].amount > 0, "not staked");
        uint256 reward = _calculateReward(msg.sender);
        payable(msg.sender).transfer(reward);
        emit Withdraw(address(this), msg.sender, reward);
        token.transfer(msg.sender, staker[msg.sender].amount);
        staker[msg.sender].amount = 0;
        staker[msg.sender].time = 0;
        for (uint256 i = 0; i < stakers.length; i++) {
            if (stakers[i] == msg.sender) {
                stakers[i] = stakers[stakers.length - 1];
                stakers.pop();
                break;
            }
        }
    }
    function claimReward() public {
        require(staker[msg.sender].amount > 0, "not staked");
        uint256 reward = _calculateReward(msg.sender);
        payable(msg.sender).transfer(reward);
        emit Withdraw(address(this), msg.sender, reward);
        staker[msg.sender].time = block.timestamp;
    }
    function setToken(address _token) public {
        require(msg.sender == owner, "only owner");
        token = IERC20(_token);
    }
    function setAmountStake(uint256 _amount) public {
        require(msg.sender == owner, "only owner");
        amountStake = _amount;
    }
    function getRewardInfo(address _staker) public view returns(uint256) {
        return _calculateReward(_staker);
    }
    function getTotalStaker() public view returns(uint256) {
        return _getTotalStaker();
    }
    function _getTotalStaker() private view returns(uint256) {
        return stakers.length;
    }
    function _calculateReward(address _staker) private view returns(uint256) {
        if (staker[_staker].amount == 0) {
            return 0;
        }
        uint256 time = block.timestamp - staker[_staker].time;
        if (time == 0) {
            return 0;
        }
        uint256 totalStakers = _getTotalStaker();
        if (totalStakers == 0) {
            return 0;
        }
        uint256 reward = address(this).balance / (31536000 / time) / totalStakers;
        return reward;
    }
}