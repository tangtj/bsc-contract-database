// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract AlionDistributor {
    address public USDCAddress;
    address[] public aportadores;
    uint256[] public aportes;
    uint256 public totalAportes;
    uint256 public maxAportadores = 37;
    address public owner;

    mapping(address => mapping(address => address)) public votos;  // antigoEndereco => novoEndereco => aportador
    mapping(address => mapping(address => uint256)) public contadorVotosProposicao; // antigoEndereco => novoEndereco => contador

    constructor(address _USDCAddress) {
        USDCAddress = _USDCAddress;
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Somente o dono pode chamar essa funcao");
        _;
    }

    function adicionarAportador(address _aportador, uint256 _aporte) external onlyOwner {
        require(aportadores.length < maxAportadores, "Numero maximo de aportadores atingido");
        
        aportadores.push(_aportador);
        aportes.push(_aporte);
        totalAportes += _aporte;
    }

    function distribuir(uint256 valor) external {
        require(valor >= 25, "O valor deve ser maior ou igual a 25");
        require(valor % 25 == 0, "O valor deve ser multiplo de 25");

        IERC20 usdcToken = IERC20(USDCAddress);
        uint256 balance = usdcToken.balanceOf(address(this));

        require(balance >= valor * 1e18, "Saldo insuficiente para distribuicao");

        uint256 valorBase = valor * 1e18;

        for (uint i = 0; i < aportadores.length; i++) {
            uint256 valorAportador = (valorBase * aportes[i]) / totalAportes;
            require(usdcToken.transfer(aportadores[i], valorAportador), "Falha na transferencia");
        }
    }

    function votarSubstituicao(address antigoEndereco, address novoEndereco) external {
        require(aportadores[aportadorIndice(msg.sender)] == msg.sender, "Apenas aportadores podem votar");
        require(aportadores[aportadorIndice(antigoEndereco)] == antigoEndereco, "Endereco antigo nao eh um aportador");

        require(votos[antigoEndereco][novoEndereco] != msg.sender, "Voto duplicado");

        votos[antigoEndereco][novoEndereco] = msg.sender;
        contadorVotosProposicao[antigoEndereco][novoEndereco]++;

        if (contadorVotosProposicao[antigoEndereco][novoEndereco] >= 25) {
            _substituirAportador(antigoEndereco, novoEndereco);
        }
    }

    function _substituirAportador(address antigoEndereco, address novoEndereco) internal {
        uint256 indice = aportadorIndice(antigoEndereco);
        aportadores[indice] = novoEndereco;
    }

    function Aportador(uint256 aportador) external view returns(address) {
        return aportadores[aportador]; 
    }

    function Aporte(uint256 aportador) external view returns(uint256) {
        return aportes[aportador];
    }

    function consultarVotosProposicao(address antigoEndereco, address novoEndereco) external view returns (uint256) {
        return contadorVotosProposicao[antigoEndereco][novoEndereco];
    }

    function consultarVotoCarteira(address antigoEndereco, address novoEndereco, address carteiraConsultada) external view returns (bool) {
        return votos[antigoEndereco][novoEndereco] == carteiraConsultada;
    }

    function aportadorIndice(address aportadorEndereco) internal view returns (uint256) {
        for (uint256 i = 0; i < aportadores.length; i++) {
            if (aportadores[i] == aportadorEndereco) {
                return i;
            }
        }
        revert("Aportador nao encontrado");
    }
}