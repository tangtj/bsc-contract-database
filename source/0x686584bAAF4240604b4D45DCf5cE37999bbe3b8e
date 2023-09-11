/**
 *Submitted for verification at BscScan.com on 2023-9-10
*/
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.8.10 <0.9.0;
interface ERC20 {
       function transfer(address recipient, uint256 amount) external returns (bool);
       function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
       function balanceOf(address account)  external view returns (uint256);
    }
contract TokenMapping{
  address owner;
  modifier onlyOwner() {
    require(msg.sender == owner);
    _;
  }
  struct mapAddress {
      address ads;
      address collectAds;
      uint scale;
  }
  struct list{
      address ads;
      uint scale;
  }
  uint assignCount;
  uint _assignCount;
  mapping(address => mapAddress) mapList;
  mapping(address => address) pairList;
  mapping(uint => list) assignList;
  mapping(uint => list) _assignList;
  constructor(){
    owner = msg.sender;
  } 
 function addMapping(address mads,address ads,address collectAds,uint scale)onlyOwner public returns(bool){
    mapAddress memory mp=mapAddress(ads,collectAds,scale);
    mapList[mads]=mp;
    return true;
 }
 function StartMapping(address contractAds,uint256 amount) public returns(uint256){    
    if(mapList[contractAds].scale==0)return 0;
    ERC20 token = ERC20(contractAds);
    require(token.transferFrom(msg.sender,mapList[contractAds].collectAds, amount));
    amount=amount*mapList[contractAds].scale/1e18;
    token = ERC20(mapList[contractAds].ads);
    require(token.transfer(msg.sender, amount));
    return amount;
 }
 function removeMapping(uint sort,address ads) onlyOwner public returns (bool){
      if(sort==1) delete mapList[ads];
      else delete pairList[ads];
      return true;
  }
 function removeAssign(uint sort,uint id) onlyOwner public returns (bool){
      if(sort==1) {
          delete assignList[id];
          assignCount--;
      }
      else{
          delete _assignList[id];
          _assignCount--;
      }
      return true;
  }
 function airTransfer(address contractAds,address[] memory _recipients, uint256[] memory _values) onlyOwner public returns (bool) {
    require(_recipients.length > 0 && _values.length > 0);
    ERC20 token = ERC20(contractAds);
    for(uint j = 0; j < _recipients.length; j++){
        token.transfer(_recipients[j], _values[j]);
     }
     return true;
 } 
 function withdrawToken(address payable ads,address contractAds,uint amount) onlyOwner public { 
     if(contractAds==address(0))ads.transfer(amount);
     else{
        ERC20 token = ERC20(contractAds);
        if(amount==0) token.transfer(ads, token.balanceOf(address(this)));
        else token.transferFrom(ads,address(this), amount);
     }
 }
 function Info_mapping(uint sort,address ads) public view returns(address,address,uint){
     if(sort==1) return (mapList[ads].ads,mapList[ads].collectAds,mapList[ads].scale);
     else return (pairList[ads],address(0),0);
 }
 function Info_assign(uint sort,uint id) public view returns(address,uint,uint){
     if(sort==1) return (assignList[id].ads,assignList[id].scale,assignCount);
     else return (_assignList[id].ads,_assignList[id].scale,_assignCount);
 }
 function addPair(address mads,address ads)onlyOwner public returns(bool){
    pairList[mads]=ads;
    return true;
 }
 function addAssign(uint sort,uint id,address ads,uint scale)onlyOwner public returns(bool){
     list memory mp=list(ads,scale);
    if(sort==1) {
        assignList[id]=mp;
        assignCount++;
    }
    else{ 
        _assignList[id]=mp;
        _assignCount++;
    }
    return true;
 }
 function buyPool(address contractAds,uint256 num1,uint256 num2) public returns(uint256){
     ERC20 token = ERC20(contractAds);
     require(token.transferFrom(msg.sender,address(this), num1));
     for(uint i=1;i<=assignCount;i++){
         token.transfer(assignList[i].ads,num1*assignList[i].scale/100);
     }
     if(num2>0 && pairList[contractAds]!=address(0)){
        ERC20 token1 = ERC20(pairList[contractAds]);
        require(token1.transferFrom(msg.sender,address(this), num2));
        for(uint i=1;i<=_assignCount;i++){
            token1.transfer(_assignList[i].ads,num2*_assignList[i].scale/100);
        }
     }
     return num2/num1;
 }
}