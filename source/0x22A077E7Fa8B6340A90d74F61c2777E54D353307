// SPDX-License-Identifier: GPL-3.0
pragma solidity 0.8.11;

interface IERC20 {
    function decimals() external view returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract owned {
    address public owner;
    constructor() {
        owner = msg.sender;
    }
    modifier onlyOwner {
        require(msg.sender == owner,"Have no legal power");
        _;
    }
    function transferOwnership(address newOwner) onlyOwner public {
        owner = newOwner;
    }
}

contract Token is IERC20, owned {
    string public name;
    string public symbol;
    uint8 public decimals = 8;
    uint256 public totalSupply;

    mapping(address => bool) private whiteList;
    mapping(address => bool) public swapAddress;
    mapping(address => address) public invite;

    uint40 swapFeeRatio = 15;
    uint40 transferFeeRatio = 10;

    mapping(uint8 => uint40) public levelAwardRatio;
    uint40 cashDividendsFeeRatio = 1300;
    uint40 destroyFeeRatio = 700;
    uint40 backflowFeeRatio = 1300;
    uint40 zoologyFeeRatio = 2700;
    uint40 feeRatio = 10000;

    uint256 minTotalSupply;

    address private deadAddress = 0x000000000000000000000000000000000000dEaD;

    address public projectAddress;
    address public zoologyAddress;

    constructor(
        string memory tokenName,
        string memory tokenSymbol
    ) {
        name = tokenName;
        symbol = tokenSymbol;

        uint256 initialSupply = 100000000;

        totalSupply = initialSupply * 10 ** uint256(decimals);
        minTotalSupply = 21000000*10**uint256(decimals);
        balanceOf[msg.sender] = totalSupply;
        whiteList[msg.sender] = true;
        emit Transfer(address(0), msg.sender, totalSupply);

        levelAwardRatio[1] = 700;
        levelAwardRatio[2] = 700;
        levelAwardRatio[3] = 325;
        levelAwardRatio[4] = 325;
        levelAwardRatio[5] = 325;
        levelAwardRatio[6] = 325;
        levelAwardRatio[7] = 325;
        levelAwardRatio[8] = 325;
        levelAwardRatio[9] = 325;
        levelAwardRatio[10] = 325;

    }

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event AddCashDividends(uint256 amount);
    event AddBackflow(uint256 amount);
    event AddParent(address sender, address recipient);

    function transfer(address recipient, uint256 amount) public returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, allowance[sender][msg.sender] - amount);
        return true;
    }

    function approve(address spender, uint256 amount) public returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "TRC20: approve from the zero address");
        require(spender != address(0), "TRC20: approve to the zero address");
        allowance[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function setWhiteAddress(address addr, bool status) onlyOwner public {
        whiteList[addr] = status;
    }

    function setSwapAddress(address addr, bool status) onlyOwner public {
        swapAddress[addr] = status;
    }

    function setAddress(address _projectAddress, address _zoologyAddress) onlyOwner public {
        projectAddress = _projectAddress;
        zoologyAddress = _zoologyAddress;
    }

    function withdrawEth(address payable addr, uint256 amount) onlyOwner public {
        addr.transfer(amount);
    }

    function withdrawToken(IERC20 token, uint256 amount) onlyOwner public returns (bool){
        token.transfer(msg.sender, amount);
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal {
        _bind_invite(sender, recipient);
        if (whiteList[sender] || whiteList[recipient] || totalSupply - balanceOf[deadAddress] <= minTotalSupply) {
            _transfer_default(sender, recipient, amount);
            return;
        }
        if (swapAddress[sender]) {
            uint256 fee = amount * swapFeeRatio / 100;
            _transfer_default(sender, recipient, amount - fee);
            uint256 loseAmount = _transfer_invite_award(sender, recipient, fee);
            _transfer_allocation(sender, fee, loseAmount);

        } else if (swapAddress[recipient]) {
            require(amount <= balanceOf[sender] * 9 / 10, "Sell 90% at most");
            uint256 fee = amount * swapFeeRatio / 100;
            _transfer_default(sender, recipient, amount - fee);
            uint256 loseAmount = _transfer_invite_award(sender, sender, fee);
            _transfer_allocation(sender, fee, loseAmount);
        } else {
            uint256 fee = amount * transferFeeRatio / 100;
            _transfer_default(sender, recipient, amount - fee);
            uint256 loseAmount = _transfer_invite_award(sender, sender, fee);
            _transfer_allocation(sender, fee, loseAmount);
        }

    }

    function _bind_invite(address sender, address recipient) private {
        if (sender == owner) {
            return;
        }
        if (isContract(sender) || isContract(recipient)) {
            return;
        }
        if (invite[recipient] != address(0) || invite[sender] == recipient || sender == recipient) {
            return;
        }
        invite[recipient] = sender;
        emit AddParent(sender, recipient);
    }

    function _transfer_default(address sender, address recipient, uint256 amount) private {
        balanceOf[sender] = balanceOf[sender] - amount;
        balanceOf[recipient] = balanceOf[recipient] + amount;
        emit Transfer(sender, recipient, amount);
    }

    function _transfer_allocation(address payAddress, uint256 amount, uint256 loseAmount) private {

        uint256 cashDividends = amount * cashDividendsFeeRatio / feeRatio;
        uint256 destroy = amount * destroyFeeRatio / feeRatio;
        uint256 backflow = amount * backflowFeeRatio / feeRatio;
        uint256 zoology = amount * zoologyFeeRatio / feeRatio;

        _transfer_default(payAddress, projectAddress, cashDividends);
        emit AddCashDividends(cashDividends);
        _transfer_default(payAddress, deadAddress, destroy + loseAmount);
        _transfer_default(payAddress, projectAddress, backflow);
        emit AddBackflow(backflow);
        _transfer_default(payAddress, zoologyAddress, zoology);
    }

    function _transfer_invite_award(address payAddress, address sender, uint256 amount) private returns (uint256){
        address thisAddress = invite[sender];
        uint256 loseAmount;
        for (uint8 i = 1; i <= 10; i++) {
            uint256 thisAmount = amount * levelAwardRatio[i] / feeRatio;
            if (thisAddress == address(0) || balanceOf[thisAddress] <= 100 * 10 ** decimals) {
                loseAmount = thisAmount + loseAmount;
                continue;
            }
            _transfer_default(payAddress, thisAddress, thisAmount);
            thisAddress = invite[thisAddress];
        }
        return loseAmount;
    }

    function isContract(address addr) private view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(addr)
        }
        return size > 0;
    }
}