pragma solidity ^0.8.0;

contract AdvancedERC20 {
    string private _name;
    string private _symbol;
    uint8 private _decimals;
    uint256 private _totalSupply;

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping (address=>bool) public frozen ;


    //vesting periods
     uint256 unlock_period1=block.timestamp+180 days;
     uint256 unlock_period2=unlock_period1+90 days;
     uint256 unlock_period3=unlock_period2+90 days;

    modifier whenNotFrozen(address target) {
      require(!frozen[target],"tokens are freeze already");
      _;
    }

    modifier whenFrozen(address target){
      require(frozen[target],"tokens are not freeze");
     _;
    }

     uint8 unlock_stage=1;

     address payable owner;



    constructor(
        string memory name_,
        string memory symbol_,
        uint8 decimals_,
        uint256 totalSupply_
    ) {
        _name = name_;
        _symbol = symbol_;
        _decimals = decimals_;
         owner=payable(msg.sender);
        _totalSupply = totalSupply_ * (10**uint256(decimals_));

        //Initial 30% liquidity is released
        _balances[msg.sender] = (_totalSupply*30)/100;

        //locked liquidity 70% of total supply
        _balances[address(this)] = (_totalSupply*70)/100;

    }
//unlock liquidity on vesting period;
 function unlock_liquidity() public returns(bool){
     
     require(msg.sender==owner,"Access Denied");
     if(unlock_stage==1){
        require(block.timestamp>=unlock_period1,"In lock period");
        uint256 release_amount=(_totalSupply*25)/100;
       _balances[msg.sender]+=release_amount;
       _balances[address(this)]-=release_amount;
        unlock_stage=2;
        emit Transfer(address(this),msg.sender,release_amount);

     }

      if(unlock_stage==2){
        require(block.timestamp>=unlock_period2,"In lock period");
        uint256 release_amount=(_totalSupply*25)/100;
       _balances[msg.sender]+=release_amount;
       _balances[address(this)]-=release_amount;
        unlock_stage=3;
        emit Transfer(address(this),msg.sender,release_amount);

     }

      if(unlock_stage==3){
       require(block.timestamp>=unlock_period3,"In lock period");
       uint256 release_amount=(_totalSupply*20)/100;
       _balances[msg.sender]+=release_amount;
       _balances[address(this)]-=release_amount;
       unlock_stage=0;
      emit Transfer(address(this),msg.sender,release_amount);
     }
     return true;
       
    }



    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount)
        public
        returns (bool)
    {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address _owner, address spender)
        public
        view
        returns (uint256)
    {
        return _allowances[_owner][spender];
    }

    function approve(address spender, uint256 amount)
        public
        returns (bool)
    {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            msg.sender,
            _allowances[sender][msg.sender] - amount
        );
        return true;
    }

    function burn(uint256 amount) public {
        _burn(msg.sender, amount);
    }

    function burnFrom(address account, uint256 amount) public {
        _burn(account, amount);
        _approve(
            account,
            msg.sender,
            _allowances[account][msg.sender] - amount
        );
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(
            _balances[sender] >= amount,
            "ERC20: transfer amount exceeds balance"
        );

        _balances[sender] -= amount;
        _balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);
    }

    function _burn(address account, uint256 amount) internal {
        require(account != address(0), "ERC20: burn from the zero address");
        require(
            _balances[account] >= amount,
            "ERC20: burn amount exceeds balance"
        );

        _balances[account] -= amount;
        _totalSupply -= amount;
        emit Transfer(account, address(0), amount);
    }

    function _approve(
        address _owner,
        address spender,
        uint256 amount
    ) internal {
        require(_owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[_owner][spender] = amount;
        emit Approval(_owner, spender, amount);
    }

        function setOwnerAddress(address payable _owner) public returns (bool)
    {
      require(msg.sender==owner,"Access denied");
       owner = _owner;
        return true;
    }

     function FreezeAcc(address target, bool freeze)  public whenNotFrozen(target) returns (bool) {
    require(msg.sender==owner,"Access Denied");
    freeze = true;
    frozen[target]=freeze;
    emit Freeze(target, true);
    return true;
  }

  function UnfreezeAcc(address target, bool freeze) public whenFrozen(target) returns (bool) {
    require(msg.sender==owner,"Access Denied");
    freeze = false;
    frozen[target]=freeze;
    emit Unfreeze(target, false);
    return true;
  }

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
     event Freeze(address target, bool frozen);
      event Unfreeze(address target, bool frozen);

}