/**
 *Submitted for verification at BscScan.com on 2023-09-10
*/

// SPDX-License-Identifier: MIT

pragma solidity 0.8.19;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function transferFrom(address sender,address recipient,uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner,address indexed spender,uint256 value);
}

contract permission {

    address private _owner;
    mapping(address => mapping(string => bytes32)) private _permit;

    modifier forRole(string memory str) {
        require(checkpermit(msg.sender,str),"Permit Revert!");
        _;
    }

    constructor() {
        newpermit(msg.sender,"owner");
        newpermit(msg.sender,"permit");
        _owner = msg.sender;
    }

    function owner() public view returns (address) { return _owner; }
    function newpermit(address adr,string memory str) internal { _permit[adr][str] = bytes32(keccak256(abi.encode(adr,str))); }
    function clearpermit(address adr,string memory str) internal { _permit[adr][str] = bytes32(keccak256(abi.encode("null"))); }
    function checkpermit(address adr,string memory str) public view returns (bool) {
        if(_permit[adr][str]==bytes32(keccak256(abi.encode(adr,str)))){ return true; }else{ return false; }
    }

    function grantRole(address adr,string memory role) public forRole("owner") returns (bool) { newpermit(adr,role); return true; }
    function revokeRole(address adr,string memory role) public forRole("owner") returns (bool) { clearpermit(adr,role); return true; }

    function transferOwnership(address adr) public forRole("owner") returns (bool) {
        newpermit(adr,"owner");
        clearpermit(msg.sender,"owner");
        _owner = adr;
        return true;
    }

    function renounceOwnership() public forRole("owner") returns (bool) {
        newpermit(address(0),"owner");
        clearpermit(msg.sender,"owner");
        _owner = address(0);
        return true;
    }
}

contract MarketingWalletSplit is permission {

    address[] wallet = [
        0x1424E93C144A7C459eE71aE1d15E1C82A5D73938,
        0xecbD0f18fAD82304F1328d045E6BcB7dFCAF215b,
        0x9262902E682C53dC3a2c2c6324201BacF204F579,
        0x8e30E8e2C7e1d84Bb1857369BF9A52d4aE822D3C,
        0xf16364ca8AD0A2f7cc6C8cB9DcAAbD143d403d0f,
        0xAff55DF8069C8832a9135FA48cd462fde00e53c5,
        0xE3bd7749Bd001287A396d6301630473A0c05FAcF
    ];

    uint256[] value = [60,10,10,10,10,10,10];

    uint256 public split = 7;
    uint256 public denominator = 120;

    address[] contributors;

    bool public autoReacts = false;

    constructor() {}

    function getWalletValue() public view returns (address[] memory wallet_,uint256[] memory value_,uint256 denominator_) {
        return (wallet,value,denominator);
    }

    function updateWalletValue(address[] memory _wallet,uint256[] memory _value,uint256 _denominator) public forRole("owner") returns (bool) {
        wallet = _wallet;
        value = _value;
        denominator = _denominator;
        split = wallet.length;
        return true;
    }

    function reacts() external returns (bool) {
        if(autoReacts){
            _distribute();
        }
        return true;
    }

    function manualDistribute() external returns (bool) {
        _distribute();
        return true;
    }

    function _distribute() internal {
        if(split>0){
            uint256 amount = address(this).balance;
            for(uint256 i=0; i < split; i++){
                sendValue(wallet[i],amount * value[i] / denominator);
            }
        }
    }

    function toggleReacts() public forRole("owner") returns (bool) {
        autoReacts = !autoReacts;
        return true;
    }

    function purgeToken(address token,uint256 amount) public forRole("owner") returns (bool) {
        IERC20(token).transfer(msg.sender,amount);
        return true;
    }

    function purgeETH() public forRole("owner") returns (bool) {
        sendValue(owner(),address(this).balance);
        return true;
    }

    function sendValue(address receiver,uint256 amount) internal {
        (bool success,) = receiver.call{ value: amount }("");
        require(success, "!fail to send eth");
    }

    receive() external payable {}
}