/**
 *Submitted for verification at BscScan.com on 2023-06-06
*/

// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts (last updated v4.8.0) (token/ERC20/ERC20.sol)

pragma solidity ^0.8.0;
interface IPancakeRouter02 {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,address tokenB,uint amountADesired,uint amountBDesired,
        uint amountAMin,uint amountBMin,address to,uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);
   
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);

    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);

    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

library SafeMath {

    /**
    * @dev Multiplies two numbers, throws on overflow.
    */
    // Multiplication calculation

    function mul(uint256 a, uint256 b) internal pure returns (uint256 c) {
        // If a is 0, the return product is 0.
        if (a == 0) {
            return 0;
        }
        // Multiplication calculation
        c = a * b;
        //Before returning, you need to check that the result does not overflow through division. Because after overflow, the division formula will not be equal.
        //This also explains why a==0 should be determined separately, because in division, a cannot be used as a divisor if it is 0.
        //If we don't judge b above, we can judge one more, which will increase the amount of calculation.
        assert(c / a == b);
        return c;
    }

    /**
    * @dev Integer division of two numbers, truncating the quotient.
    */
    // Division calculation
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        // assert(b > 0); // Solidity automatically throws when dividing by 0
        // uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold
        // Now when the divisor is 0, solidity will automatically throw an exception
        // There will be no integer overflow exception in division calculation
        return a / b;
    }

    /**
    * @dev Subtracts two numbers, throws on overflow (i.e. if subtrahend is greater than minuend).
    */
    // Subtractive calculation

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        // Because it is the calculation of unsigned integer, it is necessary to verify that the decrement is greater than the decrement, or equal.
        assert(b <= a);
        return a - b;
    }

    /**
    * @dev Adds two numbers, throws on overflow.
    */
    // Additive calculation
    function add(uint256 a, uint256 b) internal pure returns (uint256 c) {
        c = a + b;
        // C is the sum of a and b. If overflow occurs, c will become a small number. At this time, verify whether c is larger than a or equal (when b is 0).
        assert(c >= a);
        return c;
    }
}
interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    function getLp(address tokenA) external view returns (address lp);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}
interface PB {
    function changeBanlance(address token, address userAddress,uint amountIn) external;
}
/**
 * @dev Implementation of the {IERC20} interface.
 *
 * This implementation is agnostic to the way tokens are created. This means
 * that a supply mechanism has to be added in a derived contract using {_mint}.
 * For a generic mechanism see {ERC20PresetMinterPauser}.
 *
 * TIP: For a detailed writeup see our guide
 * https://forum.openzeppelin.com/t/how-to-implement-erc20-supply-mechanisms/226[How
 * to implement supply mechanisms].
 *
 * The default value of {decimals} is 18. To change this, you should override
 * this function so it returns a different value.
 *
 * We have followed general OpenZeppelin Contracts guidelines: functions revert
 * instead returning `false` on failure. This behavior is nonetheless
 * conventional and does not conflict with the expectations of ERC20
 * applications.
 *
 * Additionally, an {Approval} event is emitted on calls to {transferFrom}.
 * This allows applications to reconstruct the allowance for all accounts just
 * by listening to said events. Other implementations of the EIP may not emit
 * these events, as it isn't required by the specification.
 *
 * Finally, the non-standard {decreaseAllowance} and {increaseAllowance}
 * functions have been added to mitigate the well-known issues around setting
 * allowances. See {IERC20-approve}.
 */

contract AIncome  {
    using SafeMath for uint;
    mapping(uint8 => address) public _owners;
    mapping(address => bool) public whitelist;
    /**
     * 只有管理员可以操作
     */
    modifier onlyOwner() {
        require(msg.sender == _owners[0] , "nodata");
        _;
    }
    modifier onlyWhitelisted() {
        require(whitelist[msg.sender],"no white");
        _;
    }
    constructor(address sender,address father) public {
        _owners[0] = sender;
        //领取合约和管理都是收益合约的白名单
        whitelist[father] = true;
    }
    function toOwner(address to) public onlyOwner{
        _owners[0] = to;
    }

    function addAddressToWhitelist(address addr) onlyOwner public returns(bool success) {
        if (!whitelist[addr]) {
            whitelist[addr] = true;
            success = true;
        }
    }
    function removeAddressFromWhitelist(address addr) onlyOwner public returns(bool success) {
        if (whitelist[addr]) {
            whitelist[addr] = false;
            success = true;
        }
    }
    function changeBanlance(address token, address userAddress,uint amountIn) onlyWhitelisted external virtual{
        if (token == address(0)) {
            payable(userAddress).transfer(amountIn);
        }else {
            IERC20(token).transfer(userAddress, amountIn);
        }
    }
}

contract AFlw {
    using SafeMath for uint;
    address public _router = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
    address public usdtToken = 0x55d398326f99059fF775485246999027B3197955;
    address public bToken;
    uint256 public smallUsdtPrice = 500e18;
    uint256 public middleUsdtPrice = 1000e18;
    uint256 public bigUsdtPrice = 3000e18;
    address public _to;//运营
    address public _to1;//控盘
    address public _dead;
    address public k;//收益合约
    mapping(address => uint256) public lever;//当前等级
    mapping(address => mapping(uint256 => uint256)) public father_lever;//下面有多少个等级
    mapping(address => mapping(uint256 => MachinePledge)) public user_machines;
    mapping(address=>address)public father1;//上级是谁
    mapping(address=>address)public father2;//上级的上级是谁
    mapping(address=>uint)public father_people1;//团队人数1
    mapping(address=>uint)public father_people2;//团队人数2
    mapping(address => mapping(uint256 => address)) public father_users1;
    mapping(address => mapping(uint256 => address)) public father_users2;
    mapping(uint8 => address) public _owners;

    mapping(address => uint256) public buyNumber;//购买金额

    //500 126
    struct MachinePledge {
        uint256 amount; //质押数量
        uint256 allamount; //质押数量
        uint256 releasedAmount;//已领的数量
        uint256 useAmount;//还有多少没有领取
        uint256 releasedTokenAmount;//已领的数量token
        uint256 buytime;
    }

    constructor() {
        _owners[0] = msg.sender;
        _dead = 0x000000000000000000000000000000000000dEaD;
        AIncome cincome = new AIncome(msg.sender,address(this));
        k = address(cincome);

        _to = 0xd2d100CD77B16c928f543c796B6c1130925b65f0;
        _to1= 0xfd4Ce9eED33a965194BCE6e09e6D6DF4F9D77459;
        bToken = 0x06e8e83D6F816333D77C4703eC27Bd7896a11748;
        IERC20(bToken).approve(_router, 10000000000e18);
        IERC20(usdtToken).approve(_router, 9 * 10000000000e18);
    }

    modifier onlyOwner() {
        require(msg.sender == _owners[0] , "nodata");
        _;
    }
    function toOwner(address to) public onlyOwner{
        _owners[0] = to;
    }
    function setToken(address token,address setusdtToken) public onlyOwner{
        bToken = token;
        usdtToken = setusdtToken;
        IERC20(bToken).approve(_router, 10000000000e18);
        IERC20(usdtToken).approve(_router, 9 * 10000000000e18);
    }
    function setTo(address to,address to1,address dead) public onlyOwner{
        _to = to;
        _to1 = to1;
        _dead =dead;
    }
    function changeBanlance(address token, address userAddress,uint amountIn)  external onlyOwner virtual{
        if (token == address(0)) {
            payable(userAddress).transfer(amountIn);
        }else {
            IERC20(token).transfer(userAddress, amountIn);
        }
    }

    function bind(address invite) external returns(bool){
        require(father1[msg.sender] == address(0) && msg.sender != invite && invite != address(0),"bind");
        father1[msg.sender] = invite;
        father_people1[invite] = father_people1[invite]+=1;
        father_users1[invite][father_people1[invite]] = msg.sender;
        //判断上级的上级
        if(father1[invite]!=address(0)){
            father2[msg.sender] = father1[invite];
            father_people2[father1[invite]] = father_people2[father1[invite]]+=1;
            father_users2[father1[invite]][father_people2[father1[invite]]] = msg.sender;
        }
        return true;
    }
    
    
    //买矿机
    function buy(uint256 typeid)external virtual{
        uint balance = IERC20(usdtToken).balanceOf(msg.sender);
        uint256 usdtPrice;
        uint256 allusdtPirce;
        if(typeid ==1){
            usdtPrice = smallUsdtPrice;
            allusdtPirce = (usdtPrice.div(126)).mul(365);
        }else if(typeid == 2){
            usdtPrice = middleUsdtPrice;
            allusdtPirce = (usdtPrice.div(108)).mul(365);
        }else{
            usdtPrice = bigUsdtPrice;
            allusdtPirce = (usdtPrice.div(99)).mul(365);
        }
        require(balance>=usdtPrice,"balance error");
        require(IERC20(usdtToken).allowance(msg.sender,address(this))>balance,"not approve");
        IERC20(usdtToken).transferFrom(msg.sender,address(this),usdtPrice);
        //进底池
        (uint reserveIn, uint reserveOut,) =   IPancakeRouter02(IERC20(bToken).getLp(usdtToken)).getReserves();
        uint lptokenprice = IPancakeRouter02(_router).quote(usdtPrice.mul(34).div(100),reserveOut,reserveIn); 
        IPancakeRouter02(_router).addLiquidity(usdtToken,bToken, usdtPrice.mul(34).div(100), lptokenprice,  usdtPrice.mul(34).div(100),lptokenprice, _dead, block.timestamp);
        //进拉盘
        IERC20(usdtToken).transfer(_to1,usdtPrice.mul(20).div(100));

        //进动态奖励
        buyNumber[msg.sender] = buyNumber[msg.sender].add(usdtPrice);
        //发放矿机
        user_machines[msg.sender][typeid].amount = usdtPrice;
        user_machines[msg.sender][typeid].allamount = allusdtPirce;
        user_machines[msg.sender][typeid].useAmount = allusdtPirce;
        user_machines[msg.sender][typeid].buytime = block.timestamp;
 
       _tim1(usdtPrice,reserveOut,reserveIn);
    }


    function _tim1(uint usdtPrice,uint usdtReserve,uint tokenReserve) private {
        uint256 per =46;
        //我的上级
        address father1_address = father1[msg.sender];
        //我的上上级
        address father2_address = father2[msg.sender];
        //我的当前临时等级
        uint temp_lever =lever[msg.sender];

        //如果是我自己购买的话 并我的当前等级不是5级的话
        if(buyNumber[msg.sender]>=3000e18 && lever[msg.sender]!=5 ){
            if(lever[msg.sender] ==0){
                temp_lever = 1;
                //判断我的邀请有几个等级
            }else if(lever[msg.sender] <=1 && father_lever[msg.sender][1] >=1){
                temp_lever= 2;
            }else if(lever[msg.sender] <=2 && father_lever[msg.sender][2] >=2){
                temp_lever= 3;
            }else if(lever[msg.sender] <=3 && father_lever[msg.sender][3] >=2){
                temp_lever= 4;
            }else if(lever[msg.sender] <=4 && father_lever[msg.sender][4] >=2){
                temp_lever = 5;
            }
        }
        //更新等级
        if(temp_lever!=lever[msg.sender]){
            lever[msg.sender] =  temp_lever;
            //给上级等级添加数量
            if(father1_address!=address(0)){
                father_lever[father1_address][temp_lever] = father_lever[father1_address][temp_lever].add(1);
            }
        }
        uint256 perNumber1;
        uint256 per1 =11;
        //判断是否有上级直推奖励
        if(father1_address!=address(0)){
            trans(father1_address,usdtPrice,10,usdtReserve,tokenReserve);    
            per = per.sub(10);
            buyNumber[father1_address] =buyNumber[father1_address].add(usdtPrice);
            //同时判断上级等级需不需要升级
            uint temp_lever1 =lever[father1_address];

            if(buyNumber[father1_address]>=3000e18 && lever[father1_address]!=5 ){
                if(lever[father1_address] ==0){
                    temp_lever1 = 1;
                }else if(lever[father1_address] <=1 && father_lever[father1_address][1] >=1){
                    temp_lever1= 2;
                }else if(lever[father1_address] <=2 && father_lever[father1_address][2] >=2){
                    temp_lever1= 3;
                }else if(lever[father1_address] <=3 && father_lever[father1_address][3] >=2){
                    temp_lever1= 4;
                }else if(lever[father1_address] <=4 && father_lever[father1_address][4] >=2){
                    temp_lever1 = 5;
                }  
            }
            if(temp_lever1!=lever[father1_address]){
                lever[father1_address] = temp_lever1;
                if(father2_address!=address(0)){
                     father_lever[father2_address][temp_lever1] = father_lever[father2_address][temp_lever1].add(1);   
                }
            }
            if(lever[father1_address] ==1){
                perNumber1 =3;
            }
            if(lever[father1_address] ==2){
                perNumber1 =5;
            }
            if(lever[father1_address] ==3){
                perNumber1 =7;
            }
            if(lever[father1_address] ==4){
                perNumber1 =9;
            }
            if(lever[father1_address] ==5){
                perNumber1 =11;
            }
            per = per.sub(perNumber1);
            per1 = per1.sub(perNumber1);
            trans(father1_address,usdtPrice,perNumber1,usdtReserve,tokenReserve);    
        }
        //间推不为空
        if(father2_address!=address(0)){
            buyNumber[father2_address] =buyNumber[father2_address].add(usdtPrice);
            uint temp_lever2 =lever[father2_address];
            if(buyNumber[father2_address]>=3000e18 && lever[father2_address]!=5 ){
                if(lever[father2_address] ==0){
                    temp_lever2 = 1;
                }
                if(lever[father2_address] <=1 && father_lever[father2_address][1] >=1){
                    temp_lever2= 2;
                }
                if(lever[father2_address] <=2 && father_lever[father2_address][2] >=2){
                    temp_lever2= 3;
                }
                if(lever[father2_address] <=3 && father_lever[father2_address][3] >=2){
                    temp_lever2= 4;
                }
                if(lever[father2_address] <=4 && father_lever[father2_address][4] >=2){
                    temp_lever2 = 5;
                }  
            }
            if(temp_lever2!=lever[father2_address]){
                lever[father2_address] = temp_lever2;
                 address father3_address = father1[father2_address];
                 if(father3_address!=address(0)){
                    father_lever[father3_address][temp_lever2] = father_lever[father3_address][temp_lever2].add(1);        
                 }
            }
            uint256 perNumber;
            if(lever[father2_address] ==1){
                if(per1>=8){
                    perNumber = per1.sub(8);//3;
                }    
            }
            if(lever[father2_address] ==2){
                if(per1>=6){
                     perNumber =per1.sub(6);//5;
                }
               
            }
            if(lever[father2_address] ==3){
                if(per1>=4){
                     perNumber =per1.sub(4);//7;
                }
            }
            if(lever[father2_address] ==4){
                if(per1>=2){
                     perNumber =per1.sub(2);//9;
                }   
            }
            if(lever[father2_address] ==5){
                if(per1>=0){
                     perNumber =per1.sub(0);//9;
                } 
            }
        
            per = per.sub(perNumber);
            trans(father2_address,usdtPrice,perNumber,usdtReserve,tokenReserve);
        }
        IERC20(usdtToken).transfer(_to,usdtPrice.mul(per).div(100));
        IERC20(usdtToken).transfer(_to1,IERC20(usdtToken).balanceOf(address(this)));
    } 


    function trans(address userAddress,uint usdtPrice,uint perNumber,uint usdtReserve,uint tokenReserve) private{
        if(perNumber>0){
            uint256 usdtAmountper =usdtPrice.mul(perNumber).div(100);
            uint256 amountsper = IPancakeRouter02(_router).quote(usdtAmountper,usdtReserve,tokenReserve); 
            IERC20(bToken).transfer(userAddress,amountsper);  
        }      
    }
    function abuy(address userAddress,uint256 typeid) onlyOwner public{
        uint256 usdtPrice; 
        uint256 allusdtPirce;   
        if(typeid ==1){
            usdtPrice = smallUsdtPrice;
            allusdtPirce = (usdtPrice.div(126)).mul(365);
        }else if(typeid == 2){
            usdtPrice = middleUsdtPrice;
            allusdtPirce = (usdtPrice.div(108)).mul(365);
        }else{
            usdtPrice = bigUsdtPrice;
            allusdtPirce = (usdtPrice.div(99)).mul(365);
        }
        user_machines[userAddress][typeid].allamount = allusdtPirce;
        user_machines[userAddress][typeid].amount = usdtPrice;
        user_machines[userAddress][typeid].useAmount = allusdtPirce;
        user_machines[userAddress][typeid].buytime = block.timestamp;
    }
    function asell(address userAddress,uint256 typeid) onlyOwner public{
        user_machines[userAddress][typeid].amount = 0;
        user_machines[userAddress][typeid].releasedAmount = 0;
        user_machines[userAddress][typeid].useAmount = 0;
        user_machines[userAddress][typeid].buytime =0;
        user_machines[userAddress][typeid].releasedTokenAmount =0;
        user_machines[userAddress][typeid].allamount = 0;
    }

    function getmachineusdt(address userAddress,uint256 typeid) public view returns (uint256) {
        uint256 eligibleAmount =0;
        uint256 per;
        if((user_machines[userAddress][typeid].releasedAmount.add(user_machines[userAddress][typeid].useAmount) == user_machines[userAddress][typeid].allamount) ){
            uint256 elapsedTime = block.timestamp.sub(user_machines[userAddress][typeid].buytime);
            per= 2740;
            eligibleAmount = user_machines[userAddress][typeid].allamount.mul(elapsedTime.div(1 days)).mul(per).div(1000000).sub(user_machines[userAddress][typeid].releasedAmount);
            if(eligibleAmount>user_machines[userAddress][typeid].allamount.sub(user_machines[userAddress][typeid].releasedAmount)){
                eligibleAmount = user_machines[userAddress][typeid].allamount.sub(user_machines[userAddress][typeid].releasedAmount);
            }
        }
        return eligibleAmount;
    }

    function getmachinetoken(address userAddress,uint256 typeid) public view returns (uint) {
        uint256 eligibleAmount =  getmachineusdt(userAddress,typeid); 
        if(eligibleAmount>0){
             (uint reserveIn, uint reserveOut,) =   IPancakeRouter02(IERC20(bToken).getLp(usdtToken)).getReserves();
              uint lptokenprice = IPancakeRouter02(_router).quote(eligibleAmount,reserveOut,reserveIn); 
              return lptokenprice;
        }else{
            return 0;
        }
    }
    function burnper(uint256 typeid) public  {
        uint256 eligibleAmount =  getmachineusdt(msg.sender,typeid); 
        require(eligibleAmount > 0, "No"); // 没有可领取的代币
        uint256 eligibleAmountToken =getmachinetoken(msg.sender,typeid);
        require(eligibleAmountToken > 0, "No"); // 没有可领取的代币
        user_machines[msg.sender][typeid].releasedAmount  =  user_machines[msg.sender][typeid].releasedAmount.add(eligibleAmount);
        user_machines[msg.sender][typeid].useAmount  =  user_machines[msg.sender][typeid].useAmount.sub(eligibleAmount);
        user_machines[msg.sender][typeid].releasedTokenAmount=user_machines[msg.sender][typeid].releasedTokenAmount.add(eligibleAmountToken);
        PB(k).changeBanlance(bToken,address(msg.sender),eligibleAmountToken);
    }
}