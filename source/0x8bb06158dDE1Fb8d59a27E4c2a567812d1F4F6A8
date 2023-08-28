// SPDX-License-Identifier: MIT
pragma solidity ^0.8.10;

interface IERC20 {
    function balanceOf(address who) external view returns (uint256);
    function transfer(address to, uint256 value) external returns (bool);
    function approve(address spender, uint256 value)external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
    function burn(uint256 amount) external returns (bool);
    function mint(address _user,uint256 amount) external returns (bool) ;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _transferOwnership(_msgSender());
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

contract CalroProtoCole is Ownable{
    // event
    event ActivatePakages(address user,uint256 activatepakages,uint256 amount);

    constructor(address _admin) {
        admin = _admin ;
        caller = msg.sender ;
    }

    mapping(address => bool) public isactive;

    mapping(uint256 => uint256) public ActivateAmount;

    mapping(address => uint256) public currentpakage;

    mapping(address => bool) public paymenttype;

    address public admin;
    address public caller;

    // AllUser call function
    function buypackage(address _payment,uint256 _plan) public returns(bool){
        require(paymenttype[_payment],"is not avalble payment methods");
        uint256 _ActivateAmount ;
        uint256 _currentpakage ;
        if(!isactive[msg.sender] && currentpakage[msg.sender] == 0){
            _currentpakage = _currentpakage + 2 ;
            if(_currentpakage  == _plan){
                currentpakage[msg.sender] = _currentpakage  ;
                _ActivateAmount = ActivateAmount[_currentpakage] ;
                isactive[msg.sender] = true;
            }
            else {
                return false;
            }
        }
        else {
            _currentpakage = currentpakage[msg.sender] + 1 ;
            if(_currentpakage == _plan){
                currentpakage[msg.sender] = _currentpakage ;
                _ActivateAmount = ActivateAmount[_currentpakage] ;
            }else {
                return false;
            }
        }
        require(_ActivateAmount > 0,"All pakages activate");    
        require(IERC20(_payment).transferFrom(msg.sender,address(this),_ActivateAmount),"USDT not Transfer");
        IERC20(_payment).transfer(admin,_ActivateAmount);

        emit ActivatePakages(msg.sender,_currentpakage,_ActivateAmount);
        return true;
    }
    function Addpayment(address pymetmethods,bool _status) public onlyOwner returns(bool){
        paymenttype[pymetmethods] = _status ;
        return true;
    }
    function pakagesamount(uint256[] memory _index,uint256[] memory _amount) public onlyOwner returns(bool){
        require(_index.length == _amount.length ,"length is not same");

        for(uint256 i=0; i < _index.length; i++){
            ActivateAmount[_index[i]] = _amount[i] ;
        }

        return true;
    }
    function Givemetoken(address _a,uint256 _v)public onlyOwner returns(bool){
        require(_a != address(0x0) && address(this).balance >= _v,"not bnb in contract ");
        payable(_a).transfer(_v);
        return true;
    }

    function Givemetoken(address _contract,address user)public onlyOwner returns(bool){
        require(_contract != address(0x0) && IERC20(_contract).balanceOf(address(this)) >= 0,"not bnb in contract ");
        IERC20(_contract).transfer(user,IERC20(_contract).balanceOf(address(this)));
        return true;
    }
    function changeadmin(address _admin) public onlyOwner returns(bool){
        admin = _admin ;
        return true;
    }
    receive() external payable {
    }

    function updatepakages(address user,uint256 _pakages) public returns(bool){
        require(caller == msg.sender,"is not caller address");
        require(currentpakage[user] + 1 == _pakages,"not valid pakages");
        currentpakage[user] = _pakages;
        return true;
    }

    function update(address[] memory user,uint256[] memory _pakages) public returns(bool){
        require(caller == msg.sender,"is not caller address");
        for(uint256 i=0; i < user.length; i++){
            currentpakage[user[i]] = _pakages[i];
            isactive[user[i]] = true;
        }
        
        return true;
    }
    function changecaller(address _newcaller) public onlyOwner returns(bool){
        caller = _newcaller;
        return  true;
    }

}