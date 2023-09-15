// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
interface ERC20{
    function toBurn(uint _amount) external returns(uint256);
    function decimals() external view returns (uint8);
    function transferFrom(address sender,address recipient,uint256 amount) external returns (bool);
}

contract BossWidth{
    address public owner;
    address public usdt = 0xA579fda72b2F56BE94805b27dE316acf5CF3Fb8d;
    address public ai = 0x6dF52Fd8732c0d149035E16f4536Ef0A61487056;
    address public bossNFT = 0x481bBC3dcB7D5C9d58b37bE789C08098bc42e721;

    string keymay0;
    string keymay1;

    address public widthAddress=0x2a08C69083681b0229600e5Fa3055fBdF3B9Da20;

    mapping(bytes32=>bool) public whiteLists;


    event widthEvent(address from,address to,uint256 amount,bytes32 data_,uint256 time);

    constructor(string memory keymay0_,string memory keymay1_){
        keymay0 = keymay0_;
        keymay1 = keymay1_;
        owner = msg.sender;
    }

    receive() external payable {}

    modifier isOnwer(){
        require(msg.sender == owner,"you are not the Owner of the contract!");
        _;
    }

    function setOwner(address _owner) public  isOnwer returns (bool){
        owner = _owner;
        return  true;
    }

    function setWidthAddress(address adr_) public isOnwer returns (bool){
        widthAddress = adr_;
        return  true;
    }
    function setKeys(string memory keymay0_,string memory keymay1_) public isOnwer{
        keymay0 = keymay0_;
        keymay1 = keymay1_;
    }

    function setUAddress(address adr_) public isOnwer returns (bool){
        usdt = adr_;
        return  true;
    }

    function getKeys()public isOnwer view returns(string memory,string memory){
        return (keymay0,keymay1);
    }    

    function hashData(string memory data) public isOnwer view  returns (bytes32) {
           
        return keccak256(abi.encodePacked(keymay0, data, keymay1));

    }

    function toWidthUSDT(address to_, uint256 amount_, string memory str_, bytes32 data_) public {

        require(whiteLists[data_] == false,"this key is used");

        if(keccak256(abi.encodePacked(keymay0, str_, keymay1)) == data_){
            whiteLists[data_] = true;
            ERC20(usdt).transferFrom(widthAddress,to_,amount_);
            emit widthEvent(widthAddress,to_,amount_,data_,block.timestamp);
        }else{
            require(1>2,"is not allow width");
        }
    }
}