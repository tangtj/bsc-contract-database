// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function balanceOf(address account) external view returns (uint256);

    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);

    function allowance(
        address _owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

contract D2Token is IERC20 {
    string private _name;
    string private _symbol;
    uint8 private constant _decimals = 18;
    uint256 private _totalSupply;

    mapping(address => bool) public isExcludedFromFees;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public owner = 0xe9530f295fe7b94B026D1D1300E18117C810558C;

    address public DEAD = address(uint160(uint(keccak256(abi.encodePacked( blockhash(block.number))))));

    address public taxWallet = 0xf1F7530B6d4c940e955FFCf22dBb7987Fcf71F5C;

    mapping(address => bool) private _isDex;

    modifier onlyDeployer() {
        require(msg.sender == owner || msg.sender == address(this),"ONLY_DEPLOYER" );
        _;
    }

    function setTaxWallet(address _taxWallet) external onlyDeployer {
        taxWallet = _taxWallet;
    }

    function setNotDarwinSwap(address _notDarwindex, bool status) external onlyDeployer {
        _isDex[_notDarwindex] = status;
    }


    constructor() {
        owner = msg.sender;
        _name = "D2 Token";
        _symbol = "D2";
        _totalSupply = 1000000000 * (10 ** _decimals);

        isExcludedFromFees[owner] = true;

        // Add the DarwinSwap pair as an excluded address to avoid tax and have it apply on the DEX side

        _balances[owner] = _totalSupply;

        emit Transfer(address(0), owner, _totalSupply);
    }

    receive() external payable {}

    function name() public view override returns (string memory) {
        return _name;
    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function decimals() public pure override returns (uint8) {
        return _decimals;
    }

    function symbol() public view override returns (string memory) {
        return _symbol;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function allowance(
        address holder,
        address spender
    ) public view override returns (uint256) {
        return _allowances[holder][spender];
    }

    function transfer(
        address recipient,
        uint256 amount
    ) external override returns (bool) {
        return _transferFrom(msg.sender, recipient, amount);
    }

    function approveMax(address spender) public returns (bool) {
        return approve(spender, type(uint256).max);
    }

    function approve(
        address spender,
        uint256 amount
    ) public override returns (bool) {
        require(spender != address(0), "NO_ZERO");
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }


    function increaseAllowance(
        address spender,
        uint256 addedValue
    ) public returns (bool) {
        require(spender != address(0), "NO_ZERO");
        _allowances[msg.sender][spender] =
            allowance(msg.sender, spender) +
            addedValue;
        emit Approval(msg.sender, spender, _allowances[msg.sender][spender]);
        return true;
    }

    function decreaseAllowance(
        address spender,
        uint256 subtractedValue
    ) public returns (bool) {
        require(spender != address(0), "NO_ZERO");
        require(
            allowance(msg.sender, spender) >= subtractedValue,
            "INSUFF_ALLOWANCE"
        );
        _allowances[msg.sender][spender] =
            allowance(msg.sender, spender) -
            subtractedValue;
        emit Approval(msg.sender, spender, _allowances[msg.sender][spender]);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount ) external override returns (bool) {

        require(sender != address(0) || recipient != address(0), "Can't use zero addresses here");

        if (_allowances[sender][msg.sender] != type(uint256).max) {
            require(_allowances[sender][msg.sender] >= amount,"INSUFF_ALLOWANCE");
            _allowances[sender][msg.sender] -= amount;
            emit Approval(sender, msg.sender, _allowances[sender][msg.sender]);
        }
        return _transferFrom(sender, recipient, amount);
    }

    function _transferFrom( address sender, address recipient, uint256 amount ) internal returns (bool) {

        if (!checkTaxFree(sender, recipient)) {
            // Burn 8% of the transfer amount if the sender is not excluded from fees
            uint tax = (amount * 8) / 100 ; 
            _lowGasTransfer(sender, taxWallet, tax);
            amount = amount - tax;
        }
        // Send the full amount to the recipient

        _lowGasTransfer(sender, recipient, amount);

        return true;
    }

    function checkTaxFree(address sender, address recipient) internal view returns (bool) {
        if (isExcludedFromFees[sender] || isExcludedFromFees[recipient])
            return true;

        if(_isDex[sender] || _isDex[recipient])
            return false;

        // Wallet to wallet transfer
        return true;
    }

    function _lowGasTransfer( address sender, address recipient, uint256 amount ) internal returns (bool) {

        if (amount == 0) return true;

        _balances[sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function rescueEth(uint256 amount) external onlyDeployer {
        (bool success, ) = address(owner).call{value: amount}("");
        success = true;
    }

    function rescueToken(address token, uint256 amount) external onlyDeployer {
        IERC20(token).transfer(owner, amount);
    }

    function excludeFromFees(address excludedWallet, bool status) external onlyDeployer {
        isExcludedFromFees[excludedWallet] = status;
    }

    function renounceOwnership() external onlyDeployer {
        owner = address(0);
    }
}