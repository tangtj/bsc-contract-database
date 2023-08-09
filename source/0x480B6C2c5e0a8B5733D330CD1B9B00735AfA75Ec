pragma solidity ^0.5.16;

contract TSLA {
    string  public constant name     = "TraitSniper.com Locked Airdrop";
    string  public constant symbol   = "TSLA";
    uint8   public constant decimals = 18;
    address public constant TS       = 0x9879406C2EF6578CEB59009D64151Ef3f225830b;
    
    function totalSupply() external view returns(uint s) {
        (s,,) = ITS(TS).airInfo(address(-1));
    }
    
    function balanceOf(address who) external view returns(uint b) {
        (,,b) = ITS(TS).airInfo(who);
    }
}

interface ITS {
    function airInfo(address who) external view returns(uint airClaimed, uint airUnlocked, uint airRest);
}