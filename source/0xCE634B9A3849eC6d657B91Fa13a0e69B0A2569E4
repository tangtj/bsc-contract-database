// SPDX-License-Identifier: MIT

/**


    ██╗  ██╗     █████╗ ██╗  ██╗ █████╗ ███╗   ███╗ █████╗ ██████╗ ██╗   ██╗    ██╗███╗   ██╗██╗   ██╗
    ╚██╗██╔╝    ██╔══██╗██║ ██╔╝██╔══██╗████╗ ████║██╔══██╗██╔══██╗██║   ██║    ██║████╗  ██║██║   ██║
     ╚███╔╝     ███████║█████╔╝ ███████║██╔████╔██║███████║██████╔╝██║   ██║    ██║██╔██╗ ██║██║   ██║
     ██╔██╗     ██╔══██║██╔═██╗ ██╔══██║██║╚██╔╝██║██╔══██║██╔══██╗██║   ██║    ██║██║╚██╗██║██║   ██║
    ██╔╝ ██╗    ██║  ██║██║  ██╗██║  ██║██║ ╚═╝ ██║██║  ██║██║  ██║╚██████╔╝    ██║██║ ╚████║╚██████╔╝
    ╚═╝  ╚═╝    ╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝     ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝     ╚═╝╚═╝  ╚═══╝ ╚═════╝ 

    X Akamaru Inu
    
    Akamaru (赤丸) is a ninja dog (忍犬, ninken) 
    of Konohagakure's Inuzuka clan. He is 
    Kiba Inuzuka's partner, as well as his 
    best friend and companion.

    The Project aims to pay homage to this
    iconic little dog, in a vibrant community, 
    as well as to implement it in the X universe, 
    with the implementation of the change of name Twitter.

    https://www.xakamaruinu.com/
    https://t.me/xakamaruinu




    @dev https://bullsprotocol.com/en
    ______       _ _             ______          _                  _ 
    | ___ \     | | |            | ___ \        | |                | |
    | |_/ /_   _| | |___         | |_/ / __ ___ | |_ ___   ___ ___ | |
    | ___ \ | | | | / __|        |  __/ '__/ _ \| __/ _ \ / __/ _ \| |
    | |_/ / |_| | | \__ \        | |  | | | (_) | || (_) | (_| (_) | |
    \____/ \__,_|_|_|___/        \_|  |_|  \___/ \__\___/ \___\___/|_|

    
 */

pragma solidity 0.8.18;


//Declaration of experimental ABIEncoderV2 encoder to return dynamic types
pragma experimental ABIEncoderV2;


/**
 * @dev Contract module that helps prevent reentrant calls to a function.
*/
abstract contract ReentrancyGuard {
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;
    uint256 private _status;
    constructor() {
        _status = _NOT_ENTERED;
    }

    modifier nonReentrant() {
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");
        _status = _ENTERED;
        _;
        _status = _NOT_ENTERED;
    }
}

contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

contract Ownable is Context {
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

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0) && newOwner != address(0xdead
            ), "Is impossible to renounce the ownership of the contract");

        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}


abstract contract Pausable is Context {

    event Paused(address account);

    event Unpaused(address account);

    bool private _paused;

    constructor() {
        _paused = false;
    }

    modifier whenNotPaused() {
        _requireNotPaused();
        _;
    }

    modifier whenPaused() {
        _requirePaused();
        _;
    }

    function paused() public view virtual returns (bool) {
        return _paused;
    }

    function _requireNotPaused() internal view virtual {
        require(!paused(), "Pausable: paused");
    }

    function _requirePaused() internal view virtual {
        require(paused(), "Pausable: not paused");
    }

    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(_msgSender());
    }

    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(_msgSender());
    }
}



interface IERC20 {
    function balanceOf(address account) external view returns (uint256);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (uint256);    
    function transfer(address to, uint256 amount) external returns (bool);
}


contract ERC20 is Context {

    event Transfer(address indexed from, address indexed to, uint256 value);

    mapping(address => uint256) internal _balances;

    uint256 internal _totalSupply;
    string private _name;
    string private _symbol;

    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }

    function name() public view virtual returns (string memory) {
        return _name;
    }

    function symbol() public view virtual returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual returns (uint8) {
        return 0;
    }

    function totalSupply() public view virtual returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view virtual returns (uint256) {
        return _balances[account];
    }

    function _create(address account, uint256 amount) internal virtual {
        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);
    }
}


interface IUniswapV2Router {
    function getAmountsOut(uint256 amountIn, address[] memory path)
        external
        view
        returns (uint256[] memory amounts);

    function getAmountsIn(
        uint amountOut, 
        address[] calldata path) 
    external view returns (uint[] memory amounts);
        
}



/**
 * @dev Elliptic Curve Digital Signature Algorithm (ECDSA) operations.
 *
 * These functions can be used to verify that a message was signed by the holder
 * of the private keys of a given address.
 */

library ECDSA {
    /**
     * @dev Returns the address that signed a hashed message (`hash`) with
     * `signature`. This address can then be used for verification purposes.
     *
     * The `ecrecover` EVM opcode allows for malleable (non-unique) signatures:
     * this function rejects them by requiring the `s` value to be in the lower
     * half order, and the `v` value to be either 27 or 28.
     *
     * IMPORTANT: `hash` _must_ be the result of a hash operation for the
     * verification to be secure: it is possible to craft signatures that
     * recover to arbitrary addresses for non-hashed data. A safe way to ensure
     * this is by receiving a hash of the original message (which may otherwise
     * be too long), and then calling {toEthSignedMessageHash} on it.
     *
     * Documentation for signature generation:
     * - with https://web3js.readthedocs.io/en/v1.3.4/web3-eth-accounts.html#sign[Web3.js]
     * - with https://docs.ethers.io/v5/api/signer/#Signer-signMessage[ethers]
     */
    function recover(bytes32 hash, bytes memory signature) internal pure returns (address) {
        // Check the signature length
        // - case 65: r,s,v signature (standard)
        // - case 64: r,vs signature (cf https://eips.ethereum.org/EIPS/eip-2098) _Available since v4.1._
        if (signature.length == 65) {
            bytes32 r;
            bytes32 s;
            uint8 v;
            // ecrecover takes the signature parameters, and the only way to get them
            // currently is to use assembly.
            assembly {
                r := mload(add(signature, 0x20))
                s := mload(add(signature, 0x40))
                v := byte(0, mload(add(signature, 0x60)))
            }
            return recover(hash, v, r, s);
        } else if (signature.length == 64) {
            bytes32 r;
            bytes32 vs;
            // ecrecover takes the signature parameters, and the only way to get them
            // currently is to use assembly.
            assembly {
                r := mload(add(signature, 0x20))
                vs := mload(add(signature, 0x40))
            }
            return recover(hash, r, vs);
        } else {
            revert("ECDSA: invalid signature length");
        }
    }

    /**
     * @dev Overload of {ECDSA-recover} that receives the `r` and `vs` short-signature fields separately.
     *
     * See https://eips.ethereum.org/EIPS/eip-2098[EIP-2098 short signatures]
     *
     * _Available since v4.2._
     */
    function recover(
        bytes32 hash,
        bytes32 r,
        bytes32 vs
    ) internal pure returns (address) {
        bytes32 s;
        uint8 v;
        assembly {
            s := and(vs, 0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff)
            v := add(shr(255, vs), 27)
        }
        return recover(hash, v, r, s);
    }

    /**
     * @dev Overload of {ECDSA-recover} that receives the `v`, `r` and `s` signature fields separately.
     */
    function recover(
        bytes32 hash,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) internal pure returns (address) {
        // EIP-2 still allows signature malleability for ecrecover(). Remove this possibility and make the signature
        // unique. Appendix F in the Ethereum Yellow paper (https://ethereum.github.io/yellowpaper/paper.pdf), defines
        // the valid range for s in (281): 0 < s < secp256k1n ÷ 2 + 1, and for v in (282): v ∈ {27, 28}. Most
        // signatures from current libraries generate a unique signature with an s-value in the lower half order.
        //
        // If your library generates malleable signatures, such as s-values in the upper range, calculate a new s-value
        // with 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFEBAAEDCE6AF48A03BBFD25E8CD0364141 - s1 and flip v from 27 to 28 or
        // vice versa. If your library also generates signatures with 0/1 for v instead 27/28, add 27 to v to accept
        // these malleable signatures as well.
        require(
            uint256(s) <= 0x7FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF5D576E7357A4501DDFE92F46681B20A0,
            "ECDSA: invalid signature 's' value"
        );
        require(v == 27 || v == 28, "ECDSA: invalid signature 'v' value");

        // If the signature is valid (and not malleable), return the signer address
        address signer = ecrecover(hash, v, r, s);
        require(signer != address(0), "ECDSA: invalid signature");

        return signer;
    }

    /**
     * @dev Returns an Ethereum Signed Message, created from a `hash`. This
     * produces hash corresponding to the one signed with the
     * https://eth.wiki/json-rpc/API#eth_sign[`eth_sign`]
     * JSON-RPC method as part of EIP-191.
     *
     * See {recover}.
     */
    function toEthSignedMessageHash(bytes32 hash) internal pure returns (bytes32) {
        // 32 is the length in bytes of hash,
        // enforced by the type signature above
        return keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash));
    }

    /**
     * @dev Returns an Ethereum Signed Typed Data, created from a
     * `domainSeparator` and a `structHash`. This produces hash corresponding
     * to the one signed with the
     * https://eips.ethereum.org/EIPS/eip-712[`eth_signTypedData`]
     * JSON-RPC method as part of EIP-712.
     *
     * See {recover}.
     */
    function toTypedDataHash(bytes32 domainSeparator, bytes32 structHash) internal pure returns (bytes32) {
        return keccak256(abi.encodePacked("\x19\x01", domainSeparator, structHash));
    }
}


contract XAkamaruInu is ERC20, Pausable, Ownable, ReentrancyGuard {

    string public webSite;
    string public telegram;
    string public twitter;

    uint256 public timeDeployContract;
    uint256 public timeOpenNFTcontract;

    uint256 public fees = 1000000000000000;

    uint256 public totalSoldInBUSD;
    uint256 public totalSoldInUSDT;
    uint256 public totalSoldInUSDC;
    uint256 public totalSoldInBNB;
    uint256 public totalSoldInAKA;
    uint256 public totalSoldUSD;
    
    uint256 public amountAKAclaimed;
    uint256 public amountUSDclaimed;
    
    address public addressAKA     = 0x6Bd5444e7BDC09a6E44117C75090Da7C69E7a8F9;
    address public addressBUSD    = 0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56;
    address public addressPCVS2   = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
    address public addressWBNB    = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
    address public addressUSDT    = 0x55d398326f99059fF775485246999027B3197955;
    address public addressUSDC    = 0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d;
    address public addressUSD;

    address public fundNFTs = payable(0x116bB55570E11716d63d824b0d015F1291cc1883);
    address public projectWallet;

    address public authAddress;

    mapping(address => bool) private mappingAuth;

    mapping(address => uint256) public nonces;
    mapping(bytes => bool) private signatureUsed;
    mapping(bytes => infosBuy) private getInfosBySignatureMapping;

    struct infosBuy {
        address buyer;
        uint256 amount;
        uint256 wichToken;
        address smartContract;
        uint256 nonce;
        bytes signature;
        bytes32 hash;
    }

    receive(

    ) external payable {

    }

    constructor() ERC20("X Akamaru Inu", "AKA NFTs") {
        timeDeployContract = block.timestamp;
        mappingAuth[authAddress] = true;

        addressUSD = addressUSDT;

        webSite     = "https://www.xakamaruinu.com";
        telegram    = "https://t.me/xakamaruinu";
        twitter     = "https://twitter.com/XAkamaruInu";

        _create(address(0), 1 * (10 ** decimals()));
    }

    modifier onlyAuthorized() {
        require(_msgSender() == owner() || mappingAuth[_msgSender()] == true, "No hack here!");
        _;
    }

    function getDaysPassed() public view returns (uint256){
        return (block.timestamp - timeDeployContract) / (1 days); 
    }

    function getInfosBySignature(bytes memory signature) external view returns (infosBuy memory){
        require(_msgSender() == owner() || mappingAuth[_msgSender()], "No consultation allowed");
        return getInfosBySignatureMapping[signature]; 
    }

    //Retorna quantos tokens de entrada que equivalem o valor em BNB de amount de saída
    //Entra tokens, sai o valor de amount em BNB
    function getAmountsAKAinput(uint256 amount) public view returns (uint256) {
        
        address[] memory path = new address[](2);
        path[0] = addressAKA;
        path[1] = addressWBNB;

        uint256[] memory amountInMins = 
        IUniswapV2Router(addressPCVS2).getAmountsIn(amount, path);

        return amountInMins[0];
    }

    //Retorna quantos tokens de entrada que equivalem o valor em BNB de amount de saída
    function getAmountsBUSDinput(uint256 amount) public view returns (uint256) {
        
        address[] memory path = new address[](2);
        path[0] = addressBUSD;
        path[1] = addressWBNB;

        uint256[] memory amountInMins = 
        IUniswapV2Router(addressPCVS2).getAmountsIn(amount, path);

        return amountInMins[0];
    }

    //Retorna quantos tokens de entrada que equivalem o valor em BNB de amount de saída
    function getAmountsUSDTinput(uint256 amount) public view returns (uint256) {
        
        address[] memory path = new address[](2);
        path[0] = addressUSDT;
        path[1] = addressWBNB;

        uint256[] memory amountInMins = 
        IUniswapV2Router(addressPCVS2).getAmountsIn(amount, path);

        return amountInMins[0];
        
    }

    //Used to update the price of NFTs in BUSD
    //returns the conversion to BUSD of the AKA tokens
    function getConvertAKAtoUSD(uint256 amount) public view returns (uint256) {
        uint256 getReturn;
        if (amount != 0) {

            address[] memory path = new address[](3);
            path[0] = addressAKA;
            path[1] = addressWBNB;
            path[2] = addressBUSD;

            uint256[] memory amountOutMins = IUniswapV2Router(addressPCVS2)
            .getAmountsOut(amount, path);
            getReturn = amountOutMins[path.length - 1];
        }
        return getReturn;
    } 

    function getConvertBNBtoUSD(uint256 amount) public view returns (uint256) {
        uint256 getReturn;
        if (amount != 0) {

            address[] memory path = new address[](2);
            path[0] = addressWBNB;
            path[1] = addressBUSD;

            uint256[] memory amountOutMins = IUniswapV2Router(addressPCVS2)
            .getAmountsOut(amount, path);
            getReturn = amountOutMins[path.length - 1];
            
        }
        return getReturn;
    }

    function bytesLength(bytes memory signature) public pure returns (uint256) {
        return signature.length;
    }

    function hashReturn(bytes memory hash) public pure returns (bytes32,bytes32) {
        return (keccak256(abi.encodePacked(hash)),keccak256(hash));
    }

    function ckeckSignatureCrypto(
        address buyer,
        uint256 amount,
        address smartContract,
        uint256 nonce, 
        bytes memory signature) private {
        require(getInfosBySignatureMapping[signature].buyer == address(0x0), "Signature has already been used");

        require(address(this) == smartContract, "Invalid contract");
        require(signature.length == 65, "Signature length not approved");
        require(keccak256(abi.encodePacked(signature)) != 
                keccak256(abi.encodePacked("0x19457468657265756d205369676e6564204d6573736167653a0a3332")), 
                "Exploit attempt");

        bytes memory prefix = "\x19Ethereum Signed Message:\n32";
        bytes32 hash = keccak256(
            abi.encodePacked(prefix, 
                keccak256(abi.encodePacked(smartContract,buyer,nonce,amount)))
                );

        address recoveredAddress = ECDSA.recover(hash, signature);
        
        require(_msgSender() == buyer, "No hacking here, no mempool bot here, motherfuck!");
        require(
            recoveredAddress == authAddress && recoveredAddress != address(0), 
            "Signature was not authorized"
            );
        require(nonces[recoveredAddress]++ == nonce, "Nonce already used");
    }

    function decodeSignature(
        address buyer,
        uint256 amount,
        address smartContract,
        uint256 nonce, 
        bytes memory signature) public pure returns 
        (address,uint256,uint256,bytes memory,bytes32,address) {

        bytes memory prefix = "\x19Ethereum Signed Message:\n32";
        bytes32 hash = keccak256(abi.encodePacked(prefix, keccak256(abi.encodePacked(smartContract,buyer,nonce,amount))));
        address recoveredAddress = ECDSA.recover(hash, signature);

        return (buyer,amount,nonce,signature,hash,recoveredAddress);
    }

    function buyNFT(
        address buyer,
        uint256 amount, 
        uint256 numbersNFTs, 
        uint256 wichToken,
        address smartContract, 
        uint256 nonce, 
        bytes memory signature) 
        external payable
        nonReentrant() whenNotPaused() {

        //Redundant declaration to avoid warning message in solidity compiler
        numbersNFTs = numbersNFTs;

        ckeckSignatureCrypto(
            buyer,
            amount,
            smartContract, 
            nonce, 
            signature);

        getInfosBySignatureMapping[signature].buyer = msg.sender; 
        getInfosBySignatureMapping[signature].amount = amount; 
        getInfosBySignatureMapping[signature].wichToken = wichToken; 
        getInfosBySignatureMapping[signature].smartContract = smartContract; 
        getInfosBySignatureMapping[signature].nonce = nonce; 
        getInfosBySignatureMapping[signature].signature = signature; 

        //BUSD
        if (wichToken == 1) {
            IERC20(addressBUSD).transferFrom(msg.sender, fundNFTs, amount);

            totalSoldInBUSD += amount;
            totalSoldUSD += amount;

        //USDT
        } else if (wichToken == 2) {
            IERC20(addressUSDT).transferFrom(msg.sender, fundNFTs, amount);

            totalSoldInUSDT += amount;
            totalSoldUSD += amount;

        //USDC
        } else if (wichToken == 3) {
            IERC20(addressUSDC).transferFrom(msg.sender, fundNFTs, amount);

            totalSoldInUSDC += amount;
            totalSoldUSD += amount;

        //BNB
        } else if (wichToken == 4) {
            payable(fundNFTs).transfer(amount);

            totalSoldInBNB += amount;
            totalSoldUSD += getConvertBNBtoUSD(amount);

        //AKA
        } else if (wichToken == 5) {
            IERC20(addressAKA).transferFrom(msg.sender, fundNFTs, amount);

            totalSoldInAKA += amount;
            totalSoldUSD += getConvertAKAtoUSD(amount);
        }

    }

    //Only the claim account authorized in the backend that can call this function
    //here is the claim of rewards for investment quotas
    function claimRewardsAKA(
        address buyer,
        uint256 amount) external onlyAuthorized() nonReentrant() whenNotPaused() {

        IERC20(addressAKA).transfer(buyer, amount);

        amountAKAclaimed += amount;
    }

    function claimRewardsUSDT(
        address buyer,
        uint256 amountUSD) external onlyAuthorized() nonReentrant() whenNotPaused() {

        IERC20(addressUSD).transfer(buyer, amountUSD);

        amountUSDclaimed += amountUSD;
    }

    function claimRewards(
        address buyer,
        uint256 amountUSD,
        uint256 amountAKA) external onlyAuthorized() nonReentrant() whenNotPaused() {

        IERC20(addressUSD).transfer(buyer, amountUSD);
        IERC20(addressAKA).transfer(buyer, amountAKA);

        amountUSDclaimed += amountUSD;
        amountAKAclaimed += amountAKA;
    }

    //claimer requests the claim, which will be processed later
    function requestClaim(
        address buyer) external payable nonReentrant() whenNotPaused() {

        buyer = buyer;

        require(msg.value == fees, "Invalid amount transferred");
        (bool success,) = authAddress.call{value: fees}("");
        require(success, "Failed to send BNB");
    }

    function uncheckedI (uint256 i) public pure returns (uint256) {
        unchecked { return i + 1; }
    }

    function claimManyRewards (address[] memory buyer, uint256[] memory amountAKA, uint256[] memory amoutUSD) 
    external 
    onlyAuthorized() nonReentrant() whenNotPaused() {

        uint256 buyerLength = buyer.length;
        for (uint256 i = 0; i < buyerLength; i = uncheckedI(i)) {  

            if (amountAKA[i] != 0) {
                IERC20(addressAKA).transfer(buyer[i], amountAKA[i]);
            }
            if (amoutUSD[i] != 0) {
                IERC20(addressUSD).transfer(buyer[i], amoutUSD[i]);
            }

        }
    }

    function withdraw(address account, uint256 amount) public onlyOwner() {
        IERC20(addressAKA).transfer(account, amount);
    }

    function balanceBNB () external onlyOwner() {
        payable(msg.sender).transfer(address(this).balance);
    }

    function balanceERC20 (address token) external onlyOwner() {
        IERC20(token).transfer(msg.sender, IERC20(token).balanceOf(address(this)));
    }

    function setStrings(
        string memory _webSite,
        string memory _telegram,
        string memory _twitter
    ) public onlyOwner() {

        webSite = _webSite;
        telegram = _telegram;
        twitter = _twitter;
    }

    function setFees (uint256 _fees) external onlyOwner() {
        fees = _fees;
    }

    function setAKAaddressContract (address _addressAKA) external onlyOwner() {
        addressAKA = _addressAKA;
    }

    function setAddressUSD (address _addressUSD) external onlyOwner() {
        addressUSD = _addressUSD;
    }

    //set the authorized project wallet
    function setMappingAuth(address account, bool boolean) external onlyOwner() {
        mappingAuth[account] = boolean;
        authAddress = payable(account);
    }

    function setFundNFTs(address _fundNFTs) external onlyOwner() {
        fundNFTs = _fundNFTs;
    }

    function setProjectWallet (address _projectWallet) external onlyOwner() {
        projectWallet = _projectWallet;
    }

}