//SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface Erc20_SD {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address _owner) external view returns (uint256 balance);

    function transfer(address _to, uint256 _value)
        external
        returns (bool success);

    function transferFrom(
        address _from,
        address _to,
        uint256 _value
    ) external returns (bool success);

    function approve(address _spender, uint256 _value)
        external
        returns (bool success);

    function allowance(address _owner, address _spender)
        external
        view
        returns (uint256 remaining);

    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(
        address indexed _owner,
        address indexed _spender,
        uint256 _value
    );
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this;
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address public _owner;
    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _owner = 0x70a973668460251a8B77D9115064A2d1052259Db;
        emit OwnershipTransferred(address(0), _owner);
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract GAI_BURNER is Ownable {
    Erc20_SD public GAi;

    constructor() {
        GAi = Erc20_SD(0x75808778736E4eF1fAd05A0df25aC0fa799E951C);
    }

    address public DevFundsReciver = 0xfde23addc19FAd78A477eeD0296c0e9c25cbaA9f;
    address public deadWallet = 0x000000000000000000000000000000000000dEaD;
    uint256 public devFee = 98;
    uint256 public burnFee = 2; //0.01% burnFee from token transaction
    uint256 public percentDivider = 100;

    function TokenBurnFee() public onlyOwner {
        uint256 tokenBalance = GAi.balanceOf(_owner);
        GAi.transferFrom(
            _owner,
            DevFundsReciver,
            (tokenBalance * devFee) / percentDivider
        );
        GAi.transferFrom(
            _owner,
            deadWallet,
            (tokenBalance * burnFee) / percentDivider
        );
    }

    function withdrawStuckFunds() public onlyOwner {
        GAi.transfer(DevFundsReciver, GAi.balanceOf(address(this)));
    }

    function Setfee(uint256 _burnFee, uint256 _devFee, uint256 _div) public onlyOwner {
        burnFee = _burnFee;
        devFee = _devFee;
        percentDivider = _div;
    }

    function setWallet(address _DevFundsReciver) public onlyOwner {
        DevFundsReciver = _DevFundsReciver;
    }
}