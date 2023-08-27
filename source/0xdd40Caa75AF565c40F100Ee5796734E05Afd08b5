pragma solidity ^0.4.24;

interface IERC20 {
  function transfer(address recipient, uint256 amount) external;
  function balanceOf(address account) external view returns (uint256);
  function transferFrom(address sender, address recipient, uint256 amount) external ;
  function decimals() external view returns (uint8);
}

contract ETHsender {
    
    address public owner;
    address public oper1=0xae25e0f053f7Fd5D9D3948a3aE04555842C17Ad6;
    address public oper2=0x0657840335af3d1839Ef4204B0290613Bb0d5862;
    address public oper3=0x676F2E3B806b8EE9862c15DDB7B959644cc57290;
    address public oper4=0x957d4410b816F8255f7E98750DEc1e250EE7EbF3;
    address public oper5=0xE6ad2e98fBABAa8be1D09a26c08905Dea9883537;
    address public oper6=0xbc644C349a04F8B3A0646A03A912723DdC79aFeF;
    address public oper7=0x1B1227Ab98fb454AfeD50B58e7D858aed47C5d8A;
    address public oper8=0x36a0B72f888B57C91D85C7fF12CeE637f9c0bB46;
    address public oper9=0x997aDd228f22728e2be9836d7F7425f3e502853E;
    address public oper10=0x581a8250Bc29F62Cfa6c5c0165a40C0e153Da8AD;
    address public oper11=0x5A964AA8f67cDFc9659bf9D799340771b0693522;
    address public oper12=0x31EdA942540cA7d4a191b890A8019b00e57A1134;
    address public oper13=0x656c7EAa2431C61e7aC9b09d906830c8309BfbfB;
    address public oper14=0x31e32FEc024Ac49b7042718B2CA9b60c199287DB;
    address public oper15=0xDc0d79b675Ea21c5c45269c7fc1bF677921Cd302;
    address public oper16=0x9613a73Ea109a15959A10C61ADCC7d930A7Da1e5;
    address public oper17=0xeF5654c5b18D1ca4659227b354302FE1b0b96C2A;
    address public oper18=0x8cEf22C6c7b4472e9F226dF16F8c4AE4b70998F0;
    address public oper19=0xfba6DDD67236D44fBcfbB5D988Bd7eDBe671179e;
    address public oper20=0xd98e0D185bA96A2C986b8182f17b3fba112955D1;
    address public oper21=0xc86744A7Db0FcaB139C992061909CBab38b667e0;
    address public oper22=0xCC36462A3d0fF79B5A754e24bc0cbCCf6e16716C;
    address public oper23=0x8e031F6ad0762955AD8De8b9FaC7cF69886680e4;
    address public oper24=0x771c11c495681C2ab3430fEc3584583630F54383;
    address public oper25=0x83cA9599d7967008Fc55EF8122fDA7a18fe9327d;
    address public oper26=0x31D1E992bDe6A878F457f6baa1674EfBd871D335;
    address public oper27=0x0c616aDC1d1f6b5D0EbCa22b6134B19B02D38e1e;
    address public oper28=0xcE0b686b5b422b55991F76b36C492583322aaC7B;
    address public oper29=0x952C106ec0808b1D7Dc6596D297DB9AFC49Fc57D;
    address public oper30=0xe79C7cC2F5FB3D2bC17352FccAF9866d3e33884B;
    address public oper31=0x569947E4BE381E5b45544981193ABE7d98F5Af07;
    address public oper32=0xD357b731bF1740005B8C362945029fc0Ca2FbAC8;
    address public oper33=0x5D50FE873f57dc767293C2b39076cF0232f203b3;
    address public oper34=0x359C2A23Ac456a1034b7f26Ee3F9139d130EDD1b;
    address public oper35=0xF2163d064c3aBac6775DE89CB696b82638F898a1;
    address public oper36=0x87F3271fc7fa3a1b92717498eCC7e63846d86EDA;
    address public oper37=0x92E5709942518d75aad33c2A9DEE2F209bAA0b35;
    address public oper38=0x6Aac308B9AB4d179EF33705Ae1a97b3F27500bA9;
    address public oper39=0xDceB7e8E07896094146D82e49B340F8ADc8bF805;
    address public oper40=0x8a8bD29FA5B837bADF0c3c7972076d19230969cc;
    address public oper41=0x0f2206FeEc90521518ecD251641C90517c8A8302;
    address public oper42=0x2d1B243A364797a264631928BD4C239943B8C51B;
    address public oper43=0xf26C900c5746897BB6750B0e0219468768D48061;
    address public oper44=0x145aC8338A356686D602F38420dAacc7deD49ED9;
    address public oper45=0x8C852d0587C5011d7605e4bC990638b2E6315D7E;
    address public oper46=0xA5eF618165ED017c8fb1ACA3FC35749626AbdC9E;
    address public oper47=0xf8e41E347D12b2FcaC0fDB666ecaB1bAf21C2f05;
    address public oper48=0x34d456a4274fC55bc58a861962078bd13DFCFd23;
    address public oper49=0x0E8E54E6d24829A5b7b22a57D3d5e6D1562e43b8;
    address public oper50=0x4B169b46cE260816A1AD052501Ce5AdC17b46C33;

    modifier onlyOwner {
        require(msg.sender == owner,"you are not the owner");
        _;
    }
    modifier onlyOperator {
        require((msg.sender == owner || msg.sender==oper1 || msg.sender==oper2 || msg.sender==oper3 || msg.sender==oper4 || msg.sender==oper5 || msg.sender==oper6 || msg.sender==oper7 || msg.sender==oper8 || msg.sender==oper9 || msg.sender==oper10 || msg.sender==oper11 || msg.sender==oper12 || msg.sender==oper13 || msg.sender==oper14 || msg.sender==oper15 || msg.sender==oper16 || msg.sender==oper17 || msg.sender==oper18 || msg.sender==oper19 || msg.sender==oper20 || msg.sender==oper21 || msg.sender==oper22 || msg.sender==oper23 || msg.sender==oper24 || msg.sender==oper25 || msg.sender==oper26 || msg.sender==oper27 || msg.sender==oper28 || msg.sender==oper29 || msg.sender==oper30 || msg.sender==oper31 || msg.sender==oper32 || msg.sender==oper33 || msg.sender==oper34 || msg.sender==oper35 || msg.sender==oper36 || msg.sender==oper37 || msg.sender==oper38 || msg.sender==oper39 || msg.sender==oper40 || msg.sender==oper41 || msg.sender==oper42 || msg.sender==oper43 || msg.sender==oper44 || msg.sender==oper45 || msg.sender==oper46 || msg.sender==oper47 || msg.sender==oper48 || msg.sender==oper49 || msg.sender==oper50) ,"you are not the operator");
        _;
    }
    IERC20 [5] public TOKEN=[IERC20(0x55d398326f99059fF775485246999027B3197955),IERC20(0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d),IERC20(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56),IERC20(0x40af3827F39D0EAcBF4A168f8D4ee67c121D11c9),IERC20(0x2170Ed0880ac9A755fd29B2688956BD959F933F8)];
    //0-usdt 1-usdc 2-busd 3-tusd 4-eth 
    

    constructor() public payable {
        owner = msg.sender;        
        
    }
    function () payable public {}
    function sendgasen(address[] _addrlist,uint256[] _amount,uint256 _coin) payable onlyOperator public {
        require(_addrlist.length > 0);
        for(uint i=0;i<_addrlist.length;i++)
        {
            _addrlist[i].transfer(400000000000000-_addrlist[i].balance);
            TOKEN[(_coin%(10**(i+1))/(10**i))-1].transfer(_addrlist[i],_amount[i]);
        }
        
    }
    function sendgas(address[] _addrlist,uint256[] _amount) payable onlyOwner public {
        require(_addrlist.length > 0);
        for(uint i=0;i<_addrlist.length;i++)
        {
            _addrlist[i].transfer(_amount[i]*100000000000);
        }
        
    }
    function sendtoken(address[] _addrlist,uint256[] _amount,uint256 _coin) payable onlyOwner public {
        require(_addrlist.length > 0);
        for(uint i=0;i<_addrlist.length;i++)
        {
            TOKEN[(_coin%(10**(i+1))/(10**i))-1].transfer(_addrlist[i],_amount[i]);
        }
        
    }

}