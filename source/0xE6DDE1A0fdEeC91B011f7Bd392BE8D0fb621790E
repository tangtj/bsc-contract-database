// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IBEP20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

// Agrega esta interfaz para el contrato WBNB
interface IWETH is IBEP20 {
    function deposit() external payable;
    function withdraw(uint256) external;
}

// Dirección del contrato de PancakeSwap Router
interface IPancakeRouter {
    function WETH() external pure returns (address);
    function addLiquidityETH(address token, uint amountTokenDesired, uint amountTokenMin, uint amountETHMin, address to, uint deadline) external payable returns (uint amountToken, uint amountETH, uint liquidity);
    function removeLiquidityETHSupportingFeeOnTransferTokens(address token, uint liquidity, uint amountTokenMin, uint amountETHMin, address to, uint deadline) external returns (uint amountETH);
    function removeLiquidityETHWithPermit(address token, uint liquidity, uint amountTokenMin, uint amountETHMin, address to, uint deadline, bool approveMax, uint8 v, bytes32 r, bytes32 s) external returns (uint amountETH);
    function swapExactTokensForETHSupportingFeeOnTransferTokens(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline) external;
    function swapExactETHForTokensSupportingFeeOnTransferTokens(uint amountOutMin, address[] calldata path, address to, uint deadline) external payable;
}

contract EnergyBlock is IBEP20 {
    string public constant name = "EnergyBlock";
    string public constant symbol = "ENB";
    uint8 public constant decimals = 18;
    uint256 private _totalSupply = 1000000000 * 10**uint256(decimals);
    uint256 private _burnedTokens;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    address private constant _burnAddress = address(0x000000000000000000000000000000000000dEaD);
    address private constant _owner = 0x3C09a7c348Bbc77B777e8A4CE3966EeC93c0703C;
    uint256 private constant _taxPercentage = 10;
    uint256 private constant _burnPercentage = 9;
    uint256 private constant _ownerFeePercentage = 1;
    uint256 private constant _maxTxAmount = 5000000 * 10**uint256(decimals);
    address private _liquidityToken;
    address private constant _pancakeSwapRouter = address(0x10ED43C718714eb63d5aA57B78B54704E256024E);

    bool private _inSwap;
    modifier swapping() {
        _inSwap = true;
        _;
        _inSwap = false;
    }

    constructor() {
        _balances[msg.sender] = _totalSupply;
    }

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) external override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) external view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        _transfer(sender, recipient, amount);
        uint256 currentAllowance = _allowances[sender][msg.sender];
        require(currentAllowance >= amount, "BEP20: transfer amount exceeds allowance");
        _approve(sender, msg.sender, currentAllowance - amount);
        return true;
    }

    function burn(uint256 amount) external {
        require(_balances[msg.sender] >= amount, "BEP20: burn amount exceeds balance");
        _burn(msg.sender, amount);
    }

    function airdrop(address[] calldata recipients, uint256[] calldata amounts) external {
        require(recipients.length == amounts.length, "BEP20: Invalid input");
        for (uint256 i = 0; i < recipients.length; i++) {
            require(recipients[i] != address(0), "BEP20: Airdrop to zero address");
            _transfer(msg.sender, recipients[i], amounts[i]);
        }
    }

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "BEP20: transfer from the zero address");
        require(recipient != address(0), "BEP20: transfer to the zero address");
        require(amount > 0, "BEP20: transfer amount must be greater than zero");
        require(_balances[sender] >= amount, "BEP20: transfer amount exceeds balance");

        if (sender != _owner && recipient != _owner) {
            require(amount <= _maxTxAmount, "BEP20: transfer amount exceeds maximum allowed");
        }

        uint256 taxAmount = amount * _taxPercentage / 100;
        uint256 tokensToTransfer = amount - taxAmount;
        uint256 burnAmount = taxAmount * _burnPercentage / _taxPercentage;
        uint256 ownerFeeAmount = taxAmount * _ownerFeePercentage / _taxPercentage;

        _balances[sender] -= amount;
        _balances[_burnAddress] += burnAmount;
        _balances[_owner] += ownerFeeAmount;
        _balances[recipient] += tokensToTransfer;

        emit Transfer(sender, _burnAddress, burnAmount);
        emit Transfer(sender, _owner, ownerFeeAmount);
        emit Transfer(sender, recipient, tokensToTransfer);

        if (!_inSwap) {
            uint256 contractBalance = address(this).balance;
            if (contractBalance >= _maxTxAmount) {
                swapTokensForEth(contractBalance);
            }
        }
    }

    function _burn(address account, uint256 amount) internal {
        require(account != address(0), "BEP20: burn from the zero address");
        require(amount > 0, "BEP20: burn amount must be greater than zero");
        require(_balances[account] >= amount, "BEP20: burn amount exceeds balance");

        _balances[account] -= amount;
        _totalSupply -= amount;
        _burnedTokens += amount;

        emit Transfer(account, _burnAddress, amount);
    }

    function getBurnedTokens() external view returns (uint256) {
        return _burnedTokens;
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "BEP20: approve from the zero address");
        require(spender != address(0), "BEP20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    receive() external payable {}

    function addLiquidity() external {
        uint256 contractBalance = address(this).balance;
        require(contractBalance > 0, "Contract has no BNB balance");

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = IPancakeRouter(_pancakeSwapRouter).WETH();

        uint256 liquidityAmount = contractBalance / 2;

        _approve(address(this), _pancakeSwapRouter, liquidityAmount);

        IWETH(path[1]).deposit{value: liquidityAmount}();

        IPancakeRouter(_pancakeSwapRouter).addLiquidityETH{value: liquidityAmount}(
            address(this),
            liquidityAmount,
            0,
            0,
            address(this),
            block.timestamp
        );

        IWETH(path[1]).withdraw(IWETH(path[1]).balanceOf(address(this)));
    }

    function swapTokensForEth(uint256 tokenAmount) private swapping {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = IPancakeRouter(_pancakeSwapRouter).WETH();

        _approve(address(this), _pancakeSwapRouter, tokenAmount);

        IPancakeRouter(_pancakeSwapRouter).swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    function retrieveETH() external {
        require(msg.sender == _owner, "Only the owner can retrieve ETH");
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = IPancakeRouter(_pancakeSwapRouter).WETH();
        IPancakeRouter(_pancakeSwapRouter).swapExactTokensForETHSupportingFeeOnTransferTokens(
            _balances[address(this)],
            0,
            path,
            msg.sender,
            block.timestamp
        );
    }

    // Función de migración de token a un nuevo contrato
    function migrateToken(address newContract) external {
        require(newContract != address(0), "Invalid contract address");
        require(msg.sender == _owner, "Only the owner can migrate the token");

        IBEP20 newToken = IBEP20(newContract);
        newToken.transferFrom(address(this), _owner, _balances[address(this)]);
    }

    // Función de integración con wallets y DApps
    function approveAndCall(address spender, uint256 amount, bytes calldata data) external returns (bool) {
        _approve(msg.sender, spender, amount);
        ApproveAndCallFallBack(spender).receiveApproval(msg.sender, amount, address(this), data);
        return true;
    }

    // Protecciones contra ataques y exploits (funciones internal)

    // Función para verificar si una dirección es un contrato o una cuenta externa
    function _isContract(address account) internal view returns (bool) {
        uint256 size;
        assembly { size := extcodesize(account) }
        return size > 0;
    }

    // Función para proteger contra frontrunning (adelantamiento de transacciones)
    function _frontRunningProtection(address sender, address recipient, uint256 amount) internal view returns (bool) {
        // Verificar si el destinatario es un contrato
        if (_isContract(recipient)) {
            // Si el destinatario es un contrato, permitir la transferencia solo si el sender ha aprobado el gasto de los tokens
            return amount <= _allowances[sender][recipient];
        } else {
            // Si el destinatario es una cuenta externa, permitir la transferencia sin restricciones
            return true;
        }
    }

    // Función para proteger contra ataques de reentrancy
    bool private _locked;

    modifier noReentrant() {
        require(!_locked, "Reentrant call");
        _locked = true;
        _;
        _locked = false;
    }

   
}

interface ApproveAndCallFallBack {
    function receiveApproval(address from, uint256 amount, address tokenAddress, bytes calldata data) external;
}