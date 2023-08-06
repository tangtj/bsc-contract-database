// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

interface IERC20 {
   
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}


pragma solidity ^0.8.0;


abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _setOwner(_msgSender());
    }
    function owner() public view virtual returns (address) {
        return _owner;
    }
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }
    function renounceOwnership() public virtual onlyOwner {
        _setOwner(address(0));
    }
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}


pragma solidity ^0.8.0;

contract IDO is Ownable{
    bool public  _MintEnable;

    uint256 public _MintAmount;

    address public _MintToken;

    mapping(address => bool) public mappingmintenable;






    uint256 public _tokenExchangeRate;



    address public _saletoken;

    bool public _presaleEnable;
    
    bool public _inviterEnable;


 
    mapping(address => bool) public teaminviter;

    uint256 public _inviteTeamRate;

    uint256 public _TeamRateinviter;










    constructor(){

        
    }







    function setinviterEnable(bool inviterEnable) onlyOwner external {
        _inviterEnable=inviterEnable;
    }   

    function setSaleToken(address saletoken) onlyOwner external {
        _saletoken = saletoken;
    }
    

    function setTokenExchangeRate(uint256 tokenExchangeRate) onlyOwner external {
        _tokenExchangeRate =  tokenExchangeRate;
    }
    function setSaleEnable(bool presaleenable) onlyOwner external {
        _presaleEnable=presaleenable;
    }
    function setTeamRate(uint256 TeamRate) onlyOwner external {
        _inviteTeamRate=TeamRate;
    }
    function setTeamRateinviter(uint256 TeamRateinviter) onlyOwner external {
        _TeamRateinviter=TeamRateinviter;
    }
    function setTeaminviter (address team) onlyOwner external {
        teaminviter[team]=true;
    }        
    function preSale(address inviter) payable external {
        require(_presaleEnable == true);
        uint256 presaleAmount = msg.value  * _tokenExchangeRate;
        IERC20(_saletoken).transfer(msg.sender,presaleAmount);

        if(_inviterEnable && teaminviter[inviter]==true){
            uint256 invitermount=msg.value * _inviteTeamRate;
            IERC20(_saletoken).transfer(inviter,invitermount);

            uint256 _TeamRateinviterAmount=msg.value * _TeamRateinviter;
            IERC20(_saletoken).transfer(msg.sender,_TeamRateinviterAmount);            
        }
        else{

        }     
        
    }





    function setMintToken(address minttoken) onlyOwner external {
        _MintToken = minttoken;
    }

    function setMintAmount(uint256 mintamount) onlyOwner external {
        _MintAmount = mintamount;
    }
    function setMintEnable(bool mintenable) onlyOwner external {
        _MintEnable = mintenable;
    }


    function Mint() external {
        require(_MintEnable == true);
        require(mappingmintenable[msg.sender] == false);        
        IERC20(_MintToken).transfer(msg.sender,_MintAmount);
        mappingmintenable[msg.sender]=true;
    }


    receive() external payable {}


    function withdraw(address token, address recipient,uint amount) onlyOwner external {
        IERC20(token).transfer(recipient, amount);
    }

    function withdrwaETH() onlyOwner external {
        payable(owner()).transfer(address(this).balance);
    }


}