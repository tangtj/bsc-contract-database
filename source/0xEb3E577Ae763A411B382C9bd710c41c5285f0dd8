// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract CryptoLabToken {
    string public name = "CryptoLab";
    string public symbol = "CLB";
    uint8 public decimals = 8;
    uint256 public totalSupply = 100000000 * 10 ** uint256(decimals);

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    mapping(address => bool) public isExcludedFromFee;
    address public devWallet;
    address public marketingWallet;

    uint256 public totalFees;
    uint256 public feeDevPercent = 4;
    uint256 public feeMarketingPercent = 4;
    uint256 public feeHoldersPercent = 2;
   


    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event ExcludeFromFee(address indexed account, bool isExcluded);
    event DevWalletUpdated(address indexed newWallet);
    event MarketingWalletUpdated(address indexed newWallet);

    constructor() {
        balanceOf[msg.sender] = totalSupply;
        devWallet = 0x05037b2C799f5F56A855971fa0b0c97b3777F23f;
        marketingWallet = 0x044fe1301a2241daA79840b6983443135795b059;
        isExcludedFromFee[msg.sender] = true;
        isExcludedFromFee[address(this)] = true;
    }

    function transfer(address to, uint256 value) public returns (bool success) {
        _transfer(msg.sender, to, value);
        return true;
    }

    function approve(address spender, uint256 value) public returns (bool success) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) public returns (bool success) {
        _transfer(from, to, value);
        allowance[from][msg.sender] -= value;
        return true;
    }

    function _transfer(address from, address to, uint256 value) internal {
        require(from != address(0) && to != address(0), "Invalid address");
        require(balanceOf[from] >= value, "Insufficient balance");

        uint256 fee = calculateFee(value);
        uint256 transferAmount = value - fee;

        balanceOf[from] -= value;
        balanceOf[to] += transferAmount;
        emit Transfer(from, to, transferAmount);

        if (fee > 0) {
            balanceOf[devWallet] += feeDevPercent * fee / 100;
            balanceOf[marketingWallet] += feeMarketingPercent * fee / 100;
            totalFees += fee;
            emit Transfer(from, devWallet, feeDevPercent * fee / 100);
            emit Transfer(from, marketingWallet, feeMarketingPercent * fee / 100);
        }

        // Distribuir a taxa de holders
        uint256 holdersFee = feeHoldersPercent * fee / 100;
        totalFees += holdersFee;
        emit Transfer(from, address(this), holdersFee);
    }

    function calculateFee(uint256 _amount) public view returns (uint256) {
        if (isExcludedFromFee[msg.sender]) {
            return 0;
        }
        return (_amount * feeDevPercent / 100) + (_amount * feeMarketingPercent / 100) + (_amount * feeHoldersPercent / 100);
    }

    function excludeFromFee(address account, bool exclude) public {
        require(msg.sender == devWallet, "Only dev wallet can exclude from fee");
        isExcludedFromFee[account] = exclude;
        emit ExcludeFromFee(account, exclude);
    }

    function setDevWallet(address newWallet) public {
        require(msg.sender == devWallet, "Only dev wallet can set dev wallet");
        devWallet = newWallet;
        emit DevWalletUpdated(newWallet);
    }

    function setMarketingWallet(address newWallet) public {
        require(msg.sender == marketingWallet, "Only marketing wallet can set marketing wallet");
        marketingWallet = newWallet;
        emit MarketingWalletUpdated(newWallet);
    }


// Variável que irá armazenar o endereço da nova versão do contrato
address public newContractAddress;

// Evento para notificar sobre a atualização do contrato
event ContractUpdated(address indexed newAddress);

// Função para atualizar o contrato
function updateContract(address newContract) public {
    require(msg.sender == devWallet, "Only the current owner can update the contract");
    require(newContract != address(0), "Invalid new contract address");
    
    // Defina o endereço da nova versão do contrato
    newContractAddress = newContract;
    
    // Emita um evento para notificar sobre a atualização
    emit ContractUpdated(newContract);
}

// ...

// Define o preço de um token em Wei (a menor unidade de Ether)
uint256 public tokenPriceInWei;

// Eventos para notificar sobre compras e vendas
event TokensPurchased(address indexed buyer, uint256 amount, uint256 cost);
event TokensSold(address indexed seller, uint256 amount, uint256 revenue);

// Função para permitir a compra de tokens
function buyTokens(uint256 numberOfTokens) public payable {
    require(numberOfTokens > 0, "Number of tokens must be greater than zero");
    require(msg.value >= numberOfTokens * tokenPriceInWei, "Insufficient Ether sent");

    // Calcula o valor a ser pago em Wei
    uint256 cost = numberOfTokens * tokenPriceInWei;

    // Transfere os tokens para o comprador
    balanceOf[msg.sender] += numberOfTokens;

    // Transfere o Ether para as carteiras do desenvolvedor e marketing
    uint256 fee = cost * (feeDevPercent + feeMarketingPercent) / 100;
    balanceOf[devWallet] += fee * feeDevPercent / (feeDevPercent + feeMarketingPercent);
    balanceOf[marketingWallet] += fee * feeMarketingPercent / (feeDevPercent + feeMarketingPercent);

    // Emita eventos para notificar sobre a compra
    emit TokensPurchased(msg.sender, numberOfTokens, cost);

    // Atualiza o total de taxas acumuladas
    totalFees += fee;
}

// Função para permitir a venda de tokens
function sellTokens(uint256 numberOfTokens) public {
    require(numberOfTokens > 0, "Number of tokens must be greater than zero");
    require(balanceOf[msg.sender] >= numberOfTokens, "Insufficient tokens balance");

    // Calcula o valor a ser recebido em Wei
    uint256 revenue = numberOfTokens * tokenPriceInWei;

    // Transfere os tokens do vendedor para o contrato
    balanceOf[msg.sender] -= numberOfTokens;
    balanceOf[address(this)] += numberOfTokens;

    // Transfere o Ether para o vendedor
    payable(msg.sender).transfer(revenue);

    // Emita eventos para notificar sobre a venda
    emit TokensSold(msg.sender, numberOfTokens, revenue);
}


// Declaração da variável que irá armazenar o novo proprietário do contrato
address public newOwner;

// Evento para notificar sobre a renúncia à propriedade
event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

// Função para permitir que o proprietário atual renuncie ao contrato
function transferOwnership(address newContractOwner) public {
    require(msg.sender == devWallet, "Only the current owner can transfer ownership");
    require(newContractOwner != address(0), "Invalid new owner address");

    // Transfira a propriedade do contrato para o novo proprietário
    newOwner = newContractOwner;

    // Emita um evento para notificar sobre a renúncia à propriedade
    emit OwnershipTransferred(msg.sender, newContractOwner);
}

// Função que permite que o novo proprietário aceite a propriedade do contrato
function acceptOwnership() public {
    require(msg.sender == newOwner, "Only the new owner can accept ownership");

    // Emitir um evento para notificar sobre a transferência de propriedade
    emit OwnershipTransferred(devWallet, newOwner);

    // Limpar a variável newOwner
    newOwner = address(0);
}



}