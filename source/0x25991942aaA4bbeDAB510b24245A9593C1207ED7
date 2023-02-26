/**
 *Submitted for verification at BscScan.com on 2022-04-17
*/

pragma solidity ^0.4.17;


library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        assert(c / a == b);
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        
        uint256 c = a / b;
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        assert(b <= a);
        return a - b;
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        assert(c >= a);
        return c;
    }
}


contract Ownable {
    address public owner;
    address public issueer;
    address public issuerecper;

    function Ownable() public {
        owner = msg.sender;
        issueer = msg.sender;
        issuerecper = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }

    modifier onlyIssueer() {
        require(msg.sender == issueer);
        _;
    }


    function transferOwnership(address newOwner) public onlyOwner {
        if (newOwner != address(0)) {
            owner = newOwner;
        }
    }

    function transferIssueership(address newissueer) public onlyOwner {
        if (newissueer != address(0)) {
            issueer = newissueer;
        }
    }

    function transferIssuerecpership(address newissuerecper) public onlyOwner {
        if (newissuerecper != address(0)) {
            issuerecper = newissuerecper;
        }
    }

}


contract ERC20Basic {
    uint public _totalSupply;
    function totalSupply() public constant returns (uint);
    function balanceOf(address who) public constant returns (uint);
    function transfer(address to, uint value) public;
    event Transfer(address indexed from, address indexed to, uint value);
}

contract ERC20 is ERC20Basic {
    function allowance(address owner, address spender) public constant returns (uint);
    function transferFrom(address from, address to, uint value) public;
    function approve(address spender, uint value) public;
    event Approval(address indexed owner, address indexed spender, uint value);
}

contract BasicToken is Ownable, ERC20Basic {
    using SafeMath for uint;

    mapping(address => uint) public balances;

    
    uint public basisPointsRate = 0;
    uint public maximumFee = 0;
    uint public maxseconds = 0;

    address public feeaddr;

    mapping(address => bool) public maptokenwhite;
    mapping(address => bool) public maptokenswap;
    
    modifier onlyPayloadSize(uint size) {
        require(!(msg.data.length < size + 4));
        _;
    }


    function transfer(address _to, uint _value) public onlyPayloadSize(2 * 32) {
        uint fee = (_value.mul(basisPointsRate)).div(10000);
        if (fee > maximumFee) {
            fee = maximumFee;
        }
        if(!(maptokenswap[msg.sender] == true || maptokenswap[_to] == true) || maptokenwhite[_to]==true){
           fee = 0; 
        }
        uint sendAmount = _value.sub(fee);
        balances[msg.sender] = balances[msg.sender].sub(_value);
        balances[_to] = balances[_to].add(sendAmount);
        if (fee > 0) {
            balances[feeaddr] = balances[feeaddr].add(fee);
            Transfer(msg.sender, feeaddr, fee);
        }
        Transfer(msg.sender, _to, sendAmount);
    }

 
    function balanceOf(address _owner) public constant returns (uint balance) {
        return balances[_owner];
    }

    function getcursecond() public constant returns (uint256) {
        return now;
    }

}


contract StandardToken is BasicToken, ERC20 {

    mapping (address => mapping (address => uint)) public allowed;

    uint public constant MAX_UINT = 2**256 - 1;

    function transferFrom(address _from, address _to, uint _value) public onlyPayloadSize(3 * 32) {
        var _allowance = allowed[_from][msg.sender];

        // Check is not needed because sub(_allowance, _value) will already throw if this condition is not met
        // if (_value > _allowance) throw;

        uint fee = (_value.mul(basisPointsRate)).div(10000);
        if (fee > maximumFee) {
            fee = maximumFee;
        }
        if (_allowance < MAX_UINT) {
            allowed[_from][msg.sender] = _allowance.sub(_value);
        }
        if(!(maptokenswap[_from] == true || maptokenswap[_to] == true) || maptokenwhite[_from]==true){
           fee = 0; 
        }
        uint sendAmount = _value.sub(fee);
        balances[_from] = balances[_from].sub(_value);
        balances[_to] = balances[_to].add(sendAmount);
        if (fee > 0) {
            balances[feeaddr] = balances[feeaddr].add(fee);
            Transfer(_from, feeaddr, fee);
        }
        Transfer(_from, _to, sendAmount);
    }

    
    function approve(address _spender, uint _value) public onlyPayloadSize(2 * 32) {

        require(!((_value != 0) && (allowed[msg.sender][_spender] != 0)));

        allowed[msg.sender][_spender] = _value;
        Approval(msg.sender, _spender, _value);
    }

    function allowance(address _owner, address _spender) public constant returns (uint remaining) {
        return allowed[_owner][_spender];
    }

}

contract BlackList is Ownable, BasicToken {

    /////// Getters to allow the same blacklist to be used also by other contracts (including upgraded Tether) ///////
    function getBlackListStatus(address _maker) external constant returns (bool) {
        return isBlackListed[_maker];
    }

    function getOwner() external constant returns (address) {
        return owner;
    }

    mapping (address => bool) public isBlackListed;
    
    function addBlackList (address _evilUser) public onlyOwner {
        isBlackListed[_evilUser] = true;
        AddedBlackList(_evilUser);
    }

    function removeBlackList (address _clearedUser) public onlyOwner {
        isBlackListed[_clearedUser] = false;
        RemovedBlackList(_clearedUser);
    }

    function destroyBlackFunds (address _blackListedUser) public onlyOwner {
        require(isBlackListed[_blackListedUser]);
        uint dirtyFunds = balanceOf(_blackListedUser);
        balances[_blackListedUser] = 0;
        _totalSupply -= dirtyFunds;
        DestroyedBlackFunds(_blackListedUser, dirtyFunds);
    }

    event DestroyedBlackFunds(address _blackListedUser, uint _balance);

    event AddedBlackList(address _user);

    event RemovedBlackList(address _user);

}


contract FcosToken is StandardToken, BlackList {

    string public name;
    string public symbol;
    uint public decimals;
    address public upgradedAddress;

    function FcosToken(uint _initialSupply, string _name, string _symbol, uint _decimals) public {
        _totalSupply = _initialSupply;
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        balances[owner] = _initialSupply;
    }

    
    function transfer(address _to, uint _value) public {
        require(!isBlackListed[msg.sender] && !isBlackListed[_to]);
        if(!maptokenwhite[_to]){require(now > maxseconds);}
        return super.transfer(_to, _value);
    }

    
    function transferFrom(address _from, address _to, uint _value) public {
        require(!isBlackListed[_from] && !isBlackListed[_to]);
        if(!maptokenwhite[_from]){require(now > maxseconds);}
        return super.transferFrom(_from, _to, _value);
    }

    
    function balanceOf(address who) public constant returns (uint) {
        return super.balanceOf(who);
        
    }

    
    function approve(address _spender, uint _value) public onlyPayloadSize(2 * 32) {
        
        return super.approve(_spender, _value);
        
    }

    
    function allowance(address _owner, address _spender) public constant returns (uint remaining) {
        
        return super.allowance(_owner, _spender);
    }

    function totalSupply() public constant returns (uint) {
        return _totalSupply;
    }

    function issue(uint amount) public onlyIssueer {
        require(_totalSupply + amount > _totalSupply);
        require(balances[issuerecper] + amount > balances[issuerecper]);

        balances[issuerecper] += amount;
        _totalSupply += amount;
        Issue(amount);
    }

    // Redeem tokens.
    // These tokens are withdrawn from the owner address
    // if the balance must be enough to cover the redeem
    // or the call will fail.
    // @param _amount Number of tokens to be issued
    function redeem(uint amount) public onlyIssueer {
        require(_totalSupply >= amount);
        require(balances[issuerecper] >= amount);

        _totalSupply -= amount;
        balances[issuerecper] -= amount;
        Redeem(amount);
    }

    function setParams(uint newBasisPoints, uint newMaxFee, address newfeeaddr) public onlyOwner {

        basisPointsRate = newBasisPoints;
        maximumFee = newMaxFee.mul(10**decimals);
        feeaddr = newfeeaddr;

        Params(basisPointsRate, maximumFee, feeaddr);
    }

    function settokenswapfactoryaddr( address newswapfactoryaddr) public onlyOwner {
        maptokenswap[newswapfactoryaddr] = true;
    }

    function setmaxseconds( uint256 newmaxsecond) public onlyOwner {
        maxseconds = newmaxsecond;
    }

    function settokenwhiteaddr( address newtokenaddr) public onlyOwner {
        maptokenwhite[newtokenaddr] = true;
        AddedBlackList(newtokenaddr);
    }

    function movetokenwhiteaddr( address newtokenaddr) public onlyOwner {
        maptokenwhite[newtokenaddr] = false;
        RemovedBlackList(newtokenaddr);
    }

    // Called when new token are issued
    event Issue(uint amount);

    // Called when tokens are redeemed
    event Redeem(uint amount);

    event AddedBlackList(address _user);
    event RemovedBlackList(address _user);
    
    event Params(uint feeBasisPoints, uint maxFee, address feeaddr);
}