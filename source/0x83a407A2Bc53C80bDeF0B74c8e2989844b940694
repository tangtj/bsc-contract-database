//SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.2;

contract ERC20 {
    mapping(address => uint256) internal _balances;
    mapping(address => mapping(address => uint256)) internal _allowed;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

    uint256 internal _totalSupply;

    function totalSupply() public view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address owner) public view returns (uint256) {
        return _balances[owner];
    }

    function allowance(address owner, address spender)
    public
    view
    returns (uint256)
    {
        return _allowed[owner][spender];
    }

    function transfer(address to, uint256 value) public returns (bool) {
        _transfer(msg.sender, to, value);
        return true;
    }

    function approve(address spender, uint256 value) public returns (bool) {
        _allowed[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) public returns (bool) {
        require(_allowed[from][msg.sender] >= value);
        _allowed[from][msg.sender] = _allowed[from][msg.sender] - (value);
        _transfer(from, to, value);
        return true;
    }

    function _transfer(
        address from,
        address to,
        uint256 value
    ) internal virtual {
        require(to != address(0));
        _balances[from] = _balances[from] - (value);
        _balances[to] = _balances[to] + (value);
        emit Transfer(from, to, value);
    }
}

contract ERC20Mintable is ERC20 {
    string public name;
    string public symbol;
    uint8 public decimals;

    function _mint(address to, uint256 amount) internal {
        _balances[to] = _balances[to] + (amount);
        _totalSupply = _totalSupply + (amount);
        emit Transfer(address(0), to, amount);
    }

    function _burn(address from, uint256 amount) internal {
        _balances[from] = _balances[from] - (amount);
        _totalSupply = _totalSupply - (amount);
        emit Transfer(from, address(0), amount);
    }
}


contract AGT is ERC20 {
    constructor(
        string memory _name,
        string memory _symbol,
        uint256 _set_totalSupply,
        address _defowner
    ) {
        name = _name;
        symbol = _symbol;
        decimals = 18;
        egt = ERC20(0x83a407A2Bc53C80bDeF0B74c8e2989844b940694);
        usdt = ERC20(0x55d398326f99059fF775485246999027B3197955);
        _totalSupply = _set_totalSupply;
        _balances[_defowner] = _totalSupply;
    }

    string public name;
    string public symbol;

    uint8 public decimals;
    uint256 public dividend;
    uint256 public dividend_usdt;

    ERC20 egt;
    ERC20 usdt;
    address public mining_address =
    address(0x4E14703261EC38939Ba0bba8872CE010bc7c2f7c);
    address public bank_address =
    address(0x83a407A2Bc53C80bDeF0B74c8e2989844b940694);
    address egt_withdraw = address(msg.sender);
    address usdt_withdraw = address(msg.sender);

    mapping(address => uint256) public mask;
    mapping(address => uint256) public mask_usdt;

    function distribute(uint256 amount) external {
        require(
            msg.sender == mining_address ||
            msg.sender == bank_address ||
            msg.sender == egt_withdraw
        );
        dividend = dividend + ((amount * (1e18)) / (_totalSupply));
    }

    function update(address holder) public {
        uint256 diff = dividend - (mask[holder]);
        mask[holder] = dividend;
        if (diff > 0) egt.transfer(holder, (diff * _balances[holder]) / (1e18));
    }

    function distribute_usdt(uint256 amount) external {
        require(
            msg.sender == mining_address ||
            msg.sender == bank_address ||
            msg.sender == egt_withdraw
        );
        dividend_usdt = dividend_usdt + ((amount * (1e18)) / (_totalSupply));
    }

    function update_usdt(address holder) public {
        uint256 diff = dividend_usdt - (mask_usdt[holder]);
        mask_usdt[holder] = dividend_usdt;
        if (diff > 0)
            usdt.transfer(holder, (diff * _balances[holder]) / (1e18));
    }

    function _transfer(
        address from,
        address to,
        uint256 value
    ) internal override {
        require(to != address(0));
        update(from);
        update_usdt(from);
        update(to);
        update_usdt(to);
        _balances[from] = _balances[from] - (value);
        _balances[to] = _balances[to] + (value);
        emit Transfer(from, to, value);
    }
}

contract EGT is ERC20Mintable {
    ERC20 public constant bsc_eth =
    ERC20(0x2170Ed0880ac9A755fd29B2688956BD959F933F8);
    AGT public constant agt = AGT(0x986BF6Fc67A1FCB3fD20f2a590598A076aD5ACd9);
    address public constant defaultRef =
    0x8230dBd7b8a7Ad774ddbA2eFb0bFE2CaB0d4f8F2;


    uint256[7] refferer_percent = [3, 2, 1, 1, 1, 1, 1];

    mapping(address => address) public ref;
    mapping(address => bool) public isMember;

    event Register(address indexed client, address indexed referrer);

    constructor() {
        symbol = "EGT";
        name = "ETH Gearing Token";
        decimals = 18;
        isMember[defaultRef] = true;
    }


    function register(address _referrer) public {
        require(!isMember[msg.sender]);
        if (!isMember[_referrer]) _referrer = defaultRef;

        ref[msg.sender] = _referrer;
        isMember[msg.sender] = true;

        emit Register(msg.sender, _referrer);
    }

   
    function buy(uint256 amount) public {
        require(isMember[msg.sender]);

   
        bsc_eth.transferFrom(msg.sender, address(this), amount);
        
        _mint(msg.sender, amount * 1000);
        
        _mint(address(agt), amount * 100);
        
        agt.distribute(amount * 100);

        address referrer = ref[msg.sender];
        
        for (uint256 i = 0; i < refferer_percent.length; i++) {
           
            if (
                referrer ==
                address(0x0000000000000000000000000000000000000000) ||
                referrer == defaultRef
            ) {
                referrer = defaultRef;
            }
           
            _mint(referrer, (amount * 10 * refferer_percent[i]));
         
            referrer = ref[referrer];
        }
    }

    
    function sell(uint256 amount) public {
        require(balanceOf(msg.sender) >= amount);
        _burn(msg.sender, amount);
        uint256 send_bsc_eth_amount = amount / 1200;
        bsc_eth.transfer(msg.sender, send_bsc_eth_amount);
    }
}


contract EGT_MINING {
    struct Order {
    
        uint256 amount;
        
        uint256 rate_amount;
        
        uint256 start_egt;
       
        uint256 start_usdt;
      
        uint256 last_egt;
       
        uint256 last_usdt;
       
        uint256 egt_amount;
        
        uint256 usdt_amount;
       
        uint256 egt_reffer_amount;
        
        uint256 usdt_reffer_amount;
        
        uint256 egt_agt_amount;
     
        uint256 usdt_agt_amount;
    }

    struct TakeOrder {
       
        address member;
      
        uint256 amount;
       
        uint256 create_time;
    }

    ERC20 public constant egt =
    ERC20(0x83a407A2Bc53C80bDeF0B74c8e2989844b940694);
    ERC20 public constant usdt =
    ERC20(0x55d398326f99059fF775485246999027B3197955);
    AGT public constant agt1 = AGT(0x986BF6Fc67A1FCB3fD20f2a590598A076aD5ACd9);
    AGT public constant agt3 = AGT(0x26a9574fa1d0479F5Ed44AEDDbb8058b4AFb13E5);

   
    uint256 public usdt_start = 303;

    uint256[7] to_refferer_percent = [3, 2, 1, 1, 1, 1, 1];

    mapping(address => uint256) public count;
  
    mapping(address => mapping(uint256 => Order)) public Orders;
   
    mapping(address => bool) public isMember;
   
    mapping(address => address) public ref;
    
    mapping(address => uint256) public ref_amount;
  
    mapping(address => uint256) public ref_egt_amount;

   
    mapping(address => uint256) public ref_take_total_egt;
  
    mapping(address => uint256) public ref_take_orders_egt;
    
    mapping(address => uint256) public ref_take_total_usdt;
  
    mapping(address => uint256) public ref_take_orders_usdt;
   
    mapping(address => mapping(uint256 => TakeOrder)) public take_orders_egt;
    
    mapping(address => mapping(uint256 => TakeOrder)) public take_orders_usdt;

    address public admin = address(msg.sender);
    address public defaultRef = 0x8230dBd7b8a7Ad774ddbA2eFb0bFE2CaB0d4f8F2;

    event Register(address indexed client, address indexed referrer);


    function time_base() public view returns (uint256 _now) {
        return block.timestamp / 1 days;
    }

   
    function register(address _referrer) public {
        require(!isMember[msg.sender]);
        if (!isMember[_referrer]) _referrer = defaultRef;

        
        ref[msg.sender] = _referrer;
        
        ref_amount[_referrer] += 1;
        isMember[msg.sender] = true;

        emit Register(msg.sender, _referrer);
    }


    function count_rate(uint256 _amount, uint256 _eth_price)
    private
    pure
    returns (uint256 rate)
    {
        rate = (_eth_price * _amount) / 1200;
    }

 
    function check_invest_rule(uint256 _amount, uint256 _eth_price)
    internal
    view
    {
        
        require(_amount >= 100e18);
      
        require(_amount <= 32000e18);
      
        require(count_rate(_amount, _eth_price) > 0);
      
        require(isMember[msg.sender]);
    }

   
    function invest(uint256 _amount, uint256 _eth_price) public {
        check_invest_rule(_amount, _eth_price);
       
        count[msg.sender] += 1;
       
        ref_egt_amount[ref[msg.sender]] += _amount;
        Order storage order = Orders[msg.sender][count[msg.sender]];
     
        order.amount = _amount;
        order.rate_amount = count_rate(_amount, _eth_price);
       
        order.egt_amount = (_amount * 3939) / 10000;
      
        order.egt_reffer_amount = (_amount * 303) / 4000;
        
        order.egt_agt_amount = (_amount * 303) / 10000;

        
        order.usdt_amount = (order.rate_amount * 35061) / 10000;
       
        order.usdt_reffer_amount = (order.rate_amount * 2697) / 4000;
       
        order.usdt_agt_amount = (order.rate_amount * 2697) / 10000;

        order.start_egt = time_base() + 1;
        order.last_egt = time_base() + 1;
      
        order.start_usdt = time_base() + 1 + usdt_start;
        order.last_usdt = time_base() + 1 + usdt_start;

       
        require(
            egt.transferFrom(msg.sender, address(this), order.amount),
            "transfer to contract error"
        );
        require(
            egt.transferFrom(msg.sender, address(agt1), order.amount / 100),
            "transfer to AGT1 error"
        );
        
        agt1.distribute(order.amount / 100);
       
        require(
            egt.transfer(address(agt3), order.amount / 2),
            "transfer to AGT3 error"
        );
        
        agt3.distribute(order.amount / 2);
    }

    
    function find_refferer()
    internal
    view
    returns (address[7] memory _find_refferer)
    {
       
        address refferer = ref[msg.sender];

       
        if (ref_egt_amount[refferer] >= 500e18) {
            _find_refferer[0] = refferer;
        }
        
        refferer = ref[refferer];

     
        if (ref_egt_amount[refferer] >= 1000e18) {
            _find_refferer[1] = refferer;
        }
       
        refferer = ref[refferer];

        
        if (ref_egt_amount[refferer] >= 2000e18) {
            _find_refferer[2] = refferer;
        }
     
        refferer = ref[refferer];

        
        if (ref_egt_amount[refferer] >= 4000e18) {
            _find_refferer[3] = refferer;
        }
        
        refferer = ref[refferer];

       
        if (ref_egt_amount[refferer] >= 8000e18) {
            _find_refferer[4] = refferer;
        }
        
        refferer = ref[refferer];

       
        if (ref_egt_amount[refferer] >= 16000e18) {
            _find_refferer[5] = refferer;
        }
        
        refferer = ref[refferer];

       
        if (ref_egt_amount[refferer] >= 32000e18) {
            _find_refferer[6] = refferer;
        }
    }

    
    function refferer_take_record(
        uint256 _token_type,
        address _member,
        address _reffer,
        uint256 _amount
    ) internal {
        if (_token_type == 1) {
            ref_take_orders_egt[_reffer] += 1;
            ref_take_total_egt[_reffer] += _amount;
            TakeOrder storage order = take_orders_egt[_reffer][
                            ref_take_orders_egt[_reffer]
                ];
            order.member = _member;
            order.amount = _amount;
            order.create_time = time_base();
        }
        if (_token_type == 2) {
            ref_take_orders_usdt[_reffer] += 1;
            ref_take_total_usdt[_reffer] += _amount;
            TakeOrder storage order = take_orders_usdt[_reffer][
                            ref_take_orders_usdt[_reffer]
                ];
            order.member = _member;
            order.amount = _amount;
            order.create_time = time_base();
        }
    }

   
    function check_amount(uint256 _token_type, uint256 _index)
    public
    view
    returns (
        uint256 to_user,
        uint256 to_agt,
        uint256[7] memory to_refferer,
        address[7] memory user_refferer
    )
    {
        Order storage order = Orders[msg.sender][_index];
        user_refferer = find_refferer();
        uint256 today = time_base();
        uint256 token_amount;
        uint256 last_day;
        if (_token_type == 1) {
            token_amount = order.amount;
            last_day = order.last_egt;
        } else if (_token_type == 2) {
            token_amount = order.rate_amount;
            last_day = order.last_usdt;
        }
        
        to_user = ((token_amount * 13) / 10000) * (today - (last_day));
        
        to_agt = (token_amount / 10000) * (today - (last_day));
        
        for (uint256 i = 0; i < to_refferer.length; i++) {
            to_refferer[i] =
                (((token_amount / 4000) * to_refferer_percent[i]) / 10) *
                (today - (last_day));
        }
    }


    function claim_to_agt(
        uint256 _token_type,
        uint256 user_amount,
        uint256 to_agt
    ) internal returns (uint256 fee) {
        fee = user_amount / 100;
        if (_token_type == 1) {
          
            require(egt.transfer(address(agt1), to_agt));
            
            require(egt.transfer(address(agt1), fee));
          
            agt1.distribute(to_agt + fee);
        } else if (_token_type == 2) {
            
            require(usdt.transfer(address(agt1), to_agt));
           
            require(usdt.transfer(address(agt1), fee));
           
            agt1.distribute_usdt(to_agt + fee);
        }
    }

  
    function claim_egt(uint256 index) public {
        Order storage order = Orders[msg.sender][index];
        uint256 today = time_base();
       
        (
            uint256 to_user,
            uint256 to_agt,
            uint256[7] memory to_refferer,
            address[7] memory user_refferrer
        ) = check_amount(1, index);
     
        order.last_egt = today;
       
        require(order.egt_amount != 0 && today >= order.start_egt);

        
        if (to_user != 0 && to_user <= order.egt_amount) {
            
            uint256 fee = claim_to_agt(1, to_user, to_agt);
            
            require(egt.transfer(msg.sender, to_user - fee));
           
            order.egt_amount = order.egt_amount - to_user;
           
            order.egt_agt_amount = order.egt_agt_amount - to_agt;
           
            for (uint256 i = 0; i < user_refferrer.length; i++) {
                if (
                    user_refferrer[i] ==
                    address(0x0000000000000000000000000000000000000000)
                ) {
                    user_refferrer[i] = defaultRef;
                }
                
                require(egt.transfer(user_refferrer[i], to_refferer[i]));
                
                refferer_take_record(
                    1,
                    msg.sender,
                    user_refferrer[i],
                    to_refferer[i]
                );
                order.egt_reffer_amount =
                    order.egt_reffer_amount -
                    to_refferer[i];
            }

            
        } else if (to_user > order.egt_amount && order.egt_amount != 0) {
           
            uint256 fee = claim_to_agt(
                1,
                order.egt_amount,
                order.egt_agt_amount
            );
           
            require(egt.transfer(msg.sender, order.egt_amount - fee));
          
            for (uint256 i = 0; i < user_refferrer.length; i++) {
                if (
                    user_refferrer[i] ==
                    address(0x0000000000000000000000000000000000000000)
                ) {
                    user_refferrer[i] = defaultRef;
                }
                
                uint256 to_refferer_count = (order.egt_reffer_amount *
                    to_refferer_percent[i]) / 10;
               
                require(egt.transfer(user_refferrer[i], to_refferer_count));
               
                refferer_take_record(
                    1,
                    msg.sender,
                    user_refferrer[i],
                    to_refferer_count
                );
            }

          
            order.egt_amount = 0;
            order.egt_agt_amount = 0;
            order.egt_reffer_amount = 0;
            
        } else {
            revert("reward points balance is zero");
        }
    }

   
    function claim_usdt(uint256 index) public {
        Order storage order = Orders[msg.sender][index];
        uint256 today = time_base();
        
        (
            uint256 to_user,
            uint256 to_agt,
            uint256[7] memory to_refferer,
            address[7] memory user_refferrer
        ) = check_amount(2, index);
        
        order.last_usdt = today;
       
        require(order.usdt_amount != 0 && today >= order.start_usdt);

        
        if (to_user != 0 && to_user <= order.usdt_amount) {
           
            uint256 fee = claim_to_agt(2, to_user, to_agt);
            
            require(usdt.transfer(msg.sender, to_user - fee));
            
            order.usdt_amount = order.usdt_amount - to_user;
            
            order.usdt_agt_amount = order.usdt_agt_amount - to_agt;
           
            for (uint256 i = 0; i < user_refferrer.length; i++) {
                if (
                    user_refferrer[i] ==
                    address(0x0000000000000000000000000000000000000000)
                ) {
                    user_refferrer[i] = defaultRef;
                }
                
                require(usdt.transfer(user_refferrer[i], to_refferer[i]));
               
                refferer_take_record(
                    2,
                    msg.sender,
                    user_refferrer[i],
                    to_refferer[i]
                );
                order.usdt_reffer_amount =
                    order.usdt_reffer_amount -
                    to_refferer[i];
            }

            
        } else if (to_user > order.usdt_amount && order.usdt_amount != 0) {
            
            uint256 fee = claim_to_agt(
                2,
                order.usdt_amount,
                order.usdt_agt_amount
            );
           
            require(usdt.transfer(msg.sender, order.usdt_amount - fee));
           
            for (uint256 i = 0; i < user_refferrer.length; i++) {
                if (
                    user_refferrer[i] ==
                    address(0x0000000000000000000000000000000000000000)
                ) {
                    user_refferrer[i] = defaultRef;
                }
                
                uint256 to_refferer_count = (order.usdt_reffer_amount *
                    to_refferer_percent[i]) / 10;
                require(usdt.transfer(user_refferrer[i], to_refferer_count));
                
                refferer_take_record(
                    2,
                    msg.sender,
                    user_refferrer[i],
                    to_refferer_count
                );
            }

           
            order.usdt_amount = 0;
            order.usdt_reffer_amount = 0;
            order.usdt_agt_amount = 0;
            
        } else {
            revert("reward points balance is zero");
        }
    }
}