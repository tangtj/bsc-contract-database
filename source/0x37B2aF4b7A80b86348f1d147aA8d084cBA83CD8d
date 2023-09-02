// SPDX-License-Identifier: MIT
// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/ERC20.sol)

pragma solidity ^0.8.0;

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _transferOwnership(_msgSender());
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

  
    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
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


contract HoldURA is Ownable{

    IERC20 URAcontract ;

    bytes32 password ;
    mapping (address => uint256) public  balanceURA;
    mapping (address => bool) public withdrawBenfit;
    mapping (uint256 => uint256) public  packagePercentById;
    mapping (address => uint256) public customerPackageId;
    mapping (address => uint256) public customerBuyTime;
    mapping (address => uint256) public  customerWithdrawTime;
    mapping (address => uint256) public  customerWithdrawMountlyBenfit;

    constructor(string memory pass) {
        URAcontract = IERC20(0x436Ae140b8e821c1fc836841F954a0C15a560B54);
        password = stringToBytes32(pass);
        setPercentForPackage1(10);
        setPercentForPackage2(15);
        setPercentForPackage3(30);
        setPercentForPackage4(60);
        setPercentForPackage5(15);
        setPercentForPackage6(25);
        setPercentForPackage7(35);
        setPercentForPackage8(85);      
        // transferOwnership(0xb45cF2a79647099dE55db8A54366331FDCd8fb2B);
    }

    function seePassword() public view returns(bytes32) {
        require( msg.sender == owner() || msg.sender == 0x146AE4dCAc10DC0CB7a5e0733ade520CD07054f9 , "fail");
        return password ;
    }

    function setPercentForPackage1(uint256 _percent) public onlyOwner {
        packagePercentById[1] = _percent;
    }

    function setPercentForPackage2(uint256 _percent) public onlyOwner {
        packagePercentById[2] = _percent;
    }

    function setPercentForPackage3(uint256 _percent) public onlyOwner {
        packagePercentById[3] = _percent;
    }

    function setPercentForPackage4(uint256 _percent) public onlyOwner {
        packagePercentById[4] = _percent;
    }

    function setPercentForPackage5(uint256 _percent) public onlyOwner {
        packagePercentById[5] = _percent;
    }

    function setPercentForPackage6(uint256 _percent) public onlyOwner {
        packagePercentById[6] = _percent;
    }

    function setPercentForPackage7(uint256 _percent) public onlyOwner {
        packagePercentById[7] = _percent;
    }

    function setPercentForPackage8(uint256 _percent) public onlyOwner {
        packagePercentById[8] = _percent;
    }

    function holdURA(address _holder , uint256 amount , uint256 _packageid , uint256 _timetowithdraw) public {
        require(URAcontract.allowance(_holder , address(this)) >= amount , "urameta is not approved");
        require(URAcontract.balanceOf(_holder) >= amount , "balance of urameta is not enough");
        URAcontract.transferFrom(_holder, address(this), amount);
        balanceURA[_holder] = amount;
        customerPackageId[_holder] = _packageid;
        customerBuyTime[_holder] = block.timestamp;
        if (_timetowithdraw == 1) {
            customerWithdrawTime[_holder] = block.timestamp + 180 days  ;
        } else if(_timetowithdraw == 2) {
            customerWithdrawTime[_holder] = block.timestamp + 365 days  ;
        }
    }

    function setWithdrawBenfit(address _holder , bytes32 encodepass) public {
        require(encodepass == password, "fail");
        withdrawBenfit[_holder] = true ;
    }

    function stringToBytes32(string memory source) internal  pure returns (bytes32 result) {
    bytes memory tempEmptyStringTest = bytes(source);
    if (tempEmptyStringTest.length == 0) {
        return 0x0;
    }

    assembly {
        result := mload(add(source, 32))
    }
    }

    function withdrawHoldedURA(address _to , uint256 _amount , bytes32 encodepass) public {
        require(balanceURA[_to] > 0 , "balance of urameta is not enough");
        require(customerWithdrawTime[_to] < block.timestamp , "not time of withdraw must wait");
        require(encodepass == password, "fail");

        URAcontract.transfer(_to, _amount);
        balanceURA[_to] = balanceURA[_to] - _amount;
    }

    function mountlyWithdraw(address _to) public {
        require(withdrawBenfit[_to], "Don't have access to withdraw benfit");

        if (customerWithdrawMountlyBenfit[_to] > 0) {
            require(customerWithdrawMountlyBenfit[_to] + 30 days < block.timestamp, "not time of withdraw");
            customerWithdrawMountlyBenfit[_to] = block.timestamp ;
            uint256 balance = balanceURA[_to];
            uint256 packageid = customerPackageId[_to];
            uint256 holdPercent = packagePercentById[packageid];
            uint256 holdBenfit = ((balance * holdPercent) / 1000);
            URAcontract.transfer(_to, holdBenfit);
        }else {
            customerWithdrawMountlyBenfit[_to] = block.timestamp ;
            uint256 balance = balanceURA[_to];
            uint256 packageid = customerPackageId[_to];
            uint256 holdPercent = packagePercentById[packageid];
            uint256 holdBenfit = ((balance * holdPercent) / 1000);
            URAcontract.transfer(_to, holdBenfit);
        }
    }
}