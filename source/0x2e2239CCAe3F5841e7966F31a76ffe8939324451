//SPDX-License-Identifier: UNLICENSED
pragma solidity 0.7.6;

contract USDTSTAR {
    using SafeMath for uint256;
    IBEP20 public token;

    uint256 public constant INVEST_MIN_AMOUNT = 100e18; //100$
    uint256[] public reward = [1000e18,3000e18,12000e18,48000e18];
    uint256[] public reward_level_business_condition = [5000e18,20000e18,80000e18,320000e18];
    uint256[] public GI_PERCENT = [5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5,5];
    uint256 public constant BASE_PERCENT = 50; // 0.5% per day
    uint256 public constant PERCENTS_DIVIDER = 10000;
    uint256 public constant TIME_STEP = 1 days;
    uint256 public LAUNCH_TIME;
    uint256 public totalInvested;
    uint256 public totalWithdrawn;
    uint256 public totalDeposits;
    uint256 gi_bonus;
    address payable public marketingAddress;
     address payable public w1;
      address payable public w2;
     address payable public projectAddress;
    address payable public BepSegment;


    struct Deposit {
        uint256 amount;
        uint256 start;
        uint256 halt_timestamp;
    }

    struct User {
        Deposit[] deposits;
        uint256 checkpoint;
        address payable referrer;
        uint256 direct_amount;
        uint256 gi_bonus;
        uint256 total_gi_bonus;
        uint256 id;
        uint256 reward_earned;
        uint256 available;
        uint256 withdrawn;
        mapping(uint8 => uint256) structure;
        mapping(uint8 => uint256) level_business;
        mapping(uint8 => bool) rewards;
        uint256 total_direct_bonus;
        uint256 total_invested;
        uint256 reward_available;
    }

    mapping(address => User) public users;

    event Newbie(address user);
    event NewDeposit(address indexed user, uint256 amount);
    event Withdrawn(address indexed user, uint256 amount);
    event RefBonus(
        address indexed referrer,
        address indexed referral,
        uint256 indexed level,
        uint256 amount
    );
    event FeePayed(address indexed user, uint256 totalAmount);

    modifier beforeStarted() {
        require(block.timestamp >= LAUNCH_TIME, "!beforeStarted");
        _;
    }

    constructor(address payable marketingAddr,address payable _projectAddress,IBEP20 tokenAdd, address payable _w1, address payable _w2) {
        require(!isContract(marketingAddr), "!marketingAddr");
        BepSegment = msg.sender;
        marketingAddress = marketingAddr;
        projectAddress = _projectAddress;
        token = tokenAdd;
        w1 = _w1;
        w2 =  _w2;

        
    }

    function invest(address payable referrer,uint256 token_quantity) public payable beforeStarted() {

        uint256 tokenWei = token_quantity * 10 ** 18;

        require(tokenWei >= INVEST_MIN_AMOUNT, "!INVEST_MIN_AMOUNT");

        
            token.transferFrom(msg.sender, address(this), tokenWei);
        
        

        User storage user = users[msg.sender];


        if (user.deposits.length > 0) {
            require(tokenWei >= user.total_invested, "Top Up with same and above ");
         }

           token.transfer(projectAddress,tokenWei.mul(500).div(PERCENTS_DIVIDER));
            token.transfer(marketingAddress,tokenWei.mul(500).div(PERCENTS_DIVIDER));
        
        _setUpline(msg.sender, referrer,tokenWei);

        address upline  = user.referrer;
        uint256 direct_amt = tokenWei.mul(500).div(PERCENTS_DIVIDER);
       
       if(direct_amt > users[upline].available )
       {
           direct_amt = users[upline].available;
       }
        users[upline].direct_amount += direct_amt;
        users[upline].total_direct_bonus += direct_amt;
        
        distribute_reward(msg.sender);

        if (user.deposits.length == 0) {
            user.checkpoint = block.timestamp;
            user.withdrawn = 0;
            emit Newbie(msg.sender);
        }

        user.total_invested += tokenWei;

        uint256 triple = tokenWei.mul(3);
        user.available += triple;
        

        if(user.deposits.length > 0)
        {
            

                for(uint256 i=user.deposits.length; i > 0 ; i--)
                {
                    if( user.deposits[i - 1].halt_timestamp == 0)
                    {
                        user.deposits[i - 1].halt_timestamp = block.timestamp;
                    }
                    
                }
            
        }
    

        user.deposits.push(Deposit(tokenWei, block.timestamp, 0));

        totalInvested = totalInvested.add(tokenWei);
        totalDeposits = totalDeposits.add(1);
        emit NewDeposit(msg.sender,tokenWei);
        
    }

      function _setUpline(address _addr, address payable _upline,uint256 amount) private {
        if(users[_addr].referrer == address(0)) {//first time entry
            if(users[_upline].deposits.length == 0) {//no deposite from my upline
                _upline = BepSegment;
            }
            users[_addr].referrer = _upline;
            for(uint8 i = 0; i < GI_PERCENT.length; i++) {
                users[_upline].structure[i]++;
                 users[_upline].level_business[i] += amount;
                _upline = users[_upline].referrer;
                if(_upline == address(0)) break;
            }
        }
        
         else
             {
                _upline = users[_addr].referrer;
            for( uint8 i = 0; i < GI_PERCENT.length; i++) {
                     users[_upline].level_business[i] += amount;
                    _upline = users[_upline].referrer;
                    if(_upline == address(0)) break;
                }
        }
        
    }

    function distribute_reward(address _addr) private {

        address payable _upline = users[_addr].referrer;

        for (uint8 i = 0 ; i < reward.length; i ++ )
        {
            if(users[_upline].level_business[0] >= reward_level_business_condition[i]   &&  users[_upline].rewards[i] == false)
            {
                    users[_upline].rewards[i] = true;
                   // users[_upline].reward_earned += reward[i];
                    users[_upline].reward_available +=reward[i];
                    //token.transfer(_upline,reward[i]);
            }

            //  _upline = users[_upline].referrer;
            // if(_upline == address(0)) break;
        }

    }

    function withdraw() public beforeStarted() {

        require(
            getTimer(msg.sender) < block.timestamp,
            "withdrawal is available only once every 24 hours"
        );
        User storage user = users[msg.sender];
        
        uint256 totalAmount;
        uint256 dividends;

        require(user.available > 0,"You have reached your 3x limit");


        for (uint256 i = 0; i < user.deposits.length; i++) {
            if (user.available > 0) {

               if(user.deposits[i].halt_timestamp > 0)
                {

                    dividends = (
                            user.deposits[i].amount.mul(BASE_PERCENT).div(
                                PERCENTS_DIVIDER
                            )
                        )
                            .mul(user.deposits[i].halt_timestamp.sub(user.deposits[i].start))
                            .div(TIME_STEP);

                  
                }
                else if(user.deposits[i].halt_timestamp == 0){
                    if (user.deposits[i].start > user.checkpoint) {
                        dividends = (
                            user.deposits[i].amount.mul(BASE_PERCENT).div(
                                PERCENTS_DIVIDER
                            )
                        )
                            .mul(block.timestamp.sub(user.deposits[i].start))
                            .div(TIME_STEP);

                    } else {
                        dividends = (
                            user.deposits[i].amount.mul(BASE_PERCENT).div(
                                PERCENTS_DIVIDER
                            )
                        )
                            .mul(block.timestamp.sub(user.checkpoint))
                            .div(TIME_STEP);

                    }

                    

                }

                
                            totalAmount = totalAmount.add(dividends);
                    
            }
        }


        // As you get the income oh haltpackage , delete them , just take current package



        if(user.deposits.length > 0)
        {
                for(uint256 i= user.deposits.length; i > 0 ; i-- )
                {

                    if(user.deposits[i -1 ].halt_timestamp > 0)
                    {
                            delete user.deposits[i-1];
                    }
                        
                } 
        }

            


        uint256 min_check_value = totalAmount.add(user.direct_amount);
         min_check_value += user.gi_bonus;
        require(min_check_value  > 20e18, "Min withdraw is 20$");

         _send_gi(msg.sender,totalAmount);

        totalAmount += user.direct_amount;
        totalAmount += user.gi_bonus;
        totalAmount += user.reward_available;

       

        if (user.available < totalAmount) {
            totalAmount = user.available;

            delete user.deposits;
        }

        uint256 fees = totalAmount.mul(1000).div(PERCENTS_DIVIDER); // 10 % withdrawal deduction
        token.transfer(w1,fees.div(2));
        token.transfer(w2,fees.div(2));
        user.withdrawn = user.withdrawn.add(totalAmount);
        user.available = user.available.sub(totalAmount);
        totalAmount -= fees;
        user.checkpoint = block.timestamp;
        token.transfer(msg.sender,totalAmount);

        user.total_gi_bonus  += user.gi_bonus; 
        user.reward_earned += user.reward_available;
       
        user.direct_amount = 0;
        user.gi_bonus = 0;
        user.reward_available = 0;

        totalWithdrawn = totalWithdrawn.add(totalAmount);

        emit Withdrawn(msg.sender, totalAmount);
    }


     function getTimer(address userAddress) public view returns (uint256) {
        return users[userAddress].checkpoint.add(24 hours);  
    }


    function _send_gi(address _addr, uint256 _amount) private {
        address up = users[_addr].referrer;

        for(uint8 i = 0; i < GI_PERCENT.length; i++) {
            if(up == address(0)) break;

            if((i< users[up].structure[0] && users[up].available > 0))
            {
                uint256 bonus = _amount.mul(GI_PERCENT[i]).div(100);
                
                if(bonus > users[up].available)
                {
                    bonus = users[up].available;
                }
            
                users[up].gi_bonus += bonus;
                gi_bonus += bonus;
 
                
            }
            up = users[up].referrer;
        }
    }

    function getUserDividends(address userAddress) public view returns (uint256)
    {
        User storage user = users[userAddress];


        uint256 totalDividends;
        uint256 dividends;

        for (uint256 i = 0; i < user.deposits.length; i++) {
            if (user.available > 0) {
                if(user.deposits[i].halt_timestamp > 0)
                {

                    dividends = (
                            user.deposits[i].amount.mul(BASE_PERCENT).div(
                                PERCENTS_DIVIDER
                            )
                        )
                            .mul(user.deposits[i].halt_timestamp.sub(user.deposits[i].start))
                            .div(TIME_STEP);

                   
                }
                else if(user.deposits[i].halt_timestamp == 0){
                    if (user.deposits[i].start > user.checkpoint) {
                        dividends = (
                            user.deposits[i].amount.mul(BASE_PERCENT).div(
                                PERCENTS_DIVIDER
                            )
                        )
                            .mul(block.timestamp.sub(user.deposits[i].start))
                            .div(TIME_STEP);

               
                    } else {
                        dividends = (
                            user.deposits[i].amount.mul(BASE_PERCENT).div(
                                PERCENTS_DIVIDER
                            )
                        )
                            .mul(block.timestamp.sub(user.checkpoint))
                            .div(TIME_STEP);
                    }

                

                }

                 totalDividends = totalDividends.add(dividends);
            
            }
        }

        if (totalDividends > user.available) {
            totalDividends = user.available;
        }

        return totalDividends;
    }

    function getContractBalance() public view returns (uint256) {
        return address(this).balance;
    }

    function getUserCheckpoint(address userAddress)
        public
        view
        returns (uint256)
    {
        return users[userAddress].checkpoint;
    }

    function getUserReferrer(address userAddress)
        public
        view
        returns (address)
    {
        return users[userAddress].referrer;
    }

    function getUserReferralBonus(address userAddress)
        public
        view
        returns (uint256, uint256)
    {
        return (users[userAddress].gi_bonus,users[userAddress].total_direct_bonus);
    }

    function getUserAvailable(address userAddress)
        public
        view
        returns (uint256)
    {
        return getUserDividends(userAddress);
    }

    function getAvailable(address userAddress) public view returns (uint256) {
        return users[userAddress].available;
    }

    function getUserAmountOfReferrals(address userAddress)
        public
        view
        returns (
            uint256[] memory structure,
            uint256[] memory levelBusiness
        )
    {

        uint256[] memory _structure = new uint256[](GI_PERCENT.length);
        uint256[] memory _levelBusiness = new uint256[](GI_PERCENT.length);
        for(uint8 i = 0; i < GI_PERCENT.length; i++) {
            _structure[i] = users[userAddress].structure[i];
            _levelBusiness[i] = users[userAddress].level_business[i];
        }
        return (
             _structure,_levelBusiness

        );
    }


     function getrewardinfo(address userAddress)
        public
        view
        returns (
            bool[] memory reward_info
        )
    {


        bool[] memory _reward_info = new bool[](reward.length);

        for(uint8 i = 0; i < reward.length; i++) {
            _reward_info[i] = users[userAddress].rewards[i];
            
        }
        return (
            _reward_info

        );
    }

    function getChainID() public pure returns (uint256) {
        uint256 id;
        assembly {
            id := chainid()
        }
        return id;
    }


    function getUserDepositInfo(address userAddress, uint256 index)
        public
        view
        returns (uint256, uint256, uint256)
    {
        User storage user = users[userAddress];

        return (user.deposits[index].amount, user.deposits[index].start,  user.deposits[index].halt_timestamp);
    }

   
    function getUserAmountOfDeposits(address userAddress)
        public
        view
        returns (uint256)
    {
        return users[userAddress].deposits.length;
    }

    function getUserTotalDeposits(address userAddress)
        public
        view
        returns (uint256)
    {
        User storage user = users[userAddress];

        uint256 amount;

        for (uint256 i = 0; i < user.deposits.length; i++) {
            amount = amount.add(user.deposits[i].amount);
        }

        return amount;
    }

    function getUserTotalWithdrawn(address userAddress)
        public
        view
        returns (uint256)
    {
        User storage user = users[userAddress];

        return user.withdrawn;
    }

    function isContract(address addr) internal view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(addr)
        }
        return size > 0;
    }
}


library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        uint256 c = a - b;

        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: division by zero");
        uint256 c = a / b;

        return c;
    }
}
interface IBEP20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address BepSegment, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed BepSegment, address indexed spender, uint256 value);
}