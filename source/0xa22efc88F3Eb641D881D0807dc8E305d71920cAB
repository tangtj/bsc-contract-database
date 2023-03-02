// SPDX-License-Identifier: MIT
pragma solidity ^0.8.6;

interface IToken {
    function approve(address spender, uint256 amount) external returns (bool);
}

interface ISwap {
    function swapExactTokensForETH(uint256 amountIn, uint256 amountOutMin, address[] memory path, address receiver, uint256 deadline) external;
}

/// @title BNB Distributor
/// @author @fbslo (@fbsloXBT)
/// @notice Contract to distribute BNB rewards.

contract Distributor {
    /// @notice Owner address who can change settings
    address public owner;
    /// @notice Signer address, used to sign messages off-chain
    address public signer;
    /// @notice Admins can swap received tokens to BNB
    address[] public admins;
    /// @notice Number of admin addresses
    uint256 public numberOfAdmins;
    /// @notice true if contracts are allowed to interact, otherwise only EOAs can call it
    bool public allowContracts;
    
    /// @notice Nonces for each user, used to prevent replay attacks
    mapping(address => uint256) public nonces;
    /// @notice Total amount claimed by user
    mapping (address => uint256) public claimed;
    /// @notice Mapping of admin addresses
    mapping (address => bool) public isAdmin;
    /// @notice Approved swap router addresses
    mapping (address => bool) public isRouter;
    
    /// @notice An event thats emitted when user claims rewards
    event Claim(address indexed user, uint256 amount);
    /// @notice An event thats emitted when BNB rewards are deposited
    event Deposit(address indexed sender, uint256 amount);
    /// @notice Emitted when an admin is added or removed
    event SetAdmin(address newAdmin, bool addNew, uint256 index);
    /// @notice Emited when new router contract is added/removed
    event SetRouter(address router, bool added);
    
    /**
     * @notice Construct a new Distributor contract
     * @param newSigner The address with signers rights
     * @param newAdmins Array of new admin addresses
     * @param routers Array of new exchange router addresses
     */
    constructor(address newSigner, address[] memory newAdmins, address[] memory routers){
        owner = msg.sender;
        signer = newSigner;
        allowContracts = false;
        
        for (uint256 i = 0; i < newAdmins.length; i++){
            isAdmin[newAdmins[i]] = true;
            admins.push(newAdmins[i]);
            numberOfAdmins += 1;
            emit SetAdmin(newAdmins[i], true, 0);
        }
        
        for (uint256 i = 0; i < routers.length; i++){
            isRouter[routers[i]] = true;
            emit SetRouter(routers[i], true);
        }
    }
    
    /// @notice Fallback function used to receive BNB
    fallback() external payable {
        emit Deposit(msg.sender, msg.value);
    }
    
    /// @notice Fallback function used to receive BNB
    receive() external payable {
        emit Deposit(msg.sender, msg.value);
    }
    
    /**
     * @notice Claim BNB rewards using signature generate off-chain
     * @param user Address of the user
     * @param amount BNB amount that user wants to claim
     * @param nonce Number only used once 
     * @param signature Signature signed by signer
     */   
    function claim(address payable user, uint256 amount, uint256 nonce, bytes memory signature) external {
        bytes32 hash = getEthereumMessageHash(getMessageHash(user, amount, nonce));
        address signedBy = recoverSigner(hash, signature);
        
        require(signedBy == signer, 'Not signed by signer');
        require(nonces[user] == nonce, 'Nonce does not match');
        if (!allowContracts) require(msg.sender == tx.origin, 'No smart contracts allowed');
        
        nonces[user] += 1;
        claimed[user] += amount;
        
        user.transfer(amount);

        emit Claim(user, amount);
    }

    /**
     * @notice Call external contract
     * @param target Address of the contract we want to call
     * @param value BNB amount we want to send
     * @param signature Function signature
     * @param data Encoded call data
     */      
    function call(address target, uint value, string memory signature, bytes memory data) external {
        require(msg.sender == owner, '!owner');
        bytes memory callData;

        if (bytes(signature).length == 0) {
            callData = data;
        } else {
            callData = abi.encodePacked(bytes4(keccak256(bytes(signature))), data);
        }

        (bool success,) = target.call{value:value}(callData);
        require(success, "Transaction execution reverted.");
    }

    /**
     * @notice Swap tokens to BNB
     * @param tokenAddress Address of the token we want to sell
     * @param router Address of the exchange router contract
     * @param amountIn Amount to sell
     * @param amountOutMin Minimum received amount
     * @param path Path to swap tokens through
     */  
    function swap(address tokenAddress, address router, uint amountIn, uint256 amountOutMin, address[] memory path) external {
        require(isAdmin[msg.sender], '!admin');
        require(isRouter[router], 'Not router');
        require(IToken(tokenAddress).approve(address(router), amountIn), 'Approve tx failed');
        ISwap(router).swapExactTokensForETH(amountIn, amountOutMin, path, msg.sender, block.timestamp);
    }
    
    /**
     * @notice Change settings
     * @param newOwner Address of the new owner
     * @param newSigner Address of the new signer
     * @param newAllowContracts Boolean, true if contracts are allowed
     * @param routers Array of new router contracts
     * @param addRouters True if we are adding new router addresses, false otherwise
     */   
    function settings(address newOwner, address newSigner, bool newAllowContracts, address[] memory routers, bool addRouters) external {
        require(msg.sender == owner, '!owner');
        require(newOwner != address(0), "Owner != 0x0");
        require(newSigner != address(0), "Signer != 0x0");
        
        owner = newOwner;
        allowContracts = newAllowContracts;
        signer = newSigner;
        
        
        for (uint256 i = 0; i < routers.length; i++){
            isRouter[routers[i]] = addRouters;
            emit SetRouter(routers[i], true);
        }
    }

    /**
     * @notice Add or remove admin address
     * @param adminAddress Address of the  admin we want to add or remove
     * @param addNew True if we are adding new admin, false otherwise
     * @param index The index of admin address we want to remove in `admins` array
     */    
    function setAdmin(address adminAddress, bool addNew, uint256 index) external {
        require(msg.sender == owner, '!owner');
        if (addNew) {
            require(!isAdmin[adminAddress], "Admin already exisits");
            isAdmin[adminAddress] = true;
            admins.push(adminAddress);
            numberOfAdmins += 1;
        } else {
            require(admins[index] == adminAddress, 'Index does not match');
            isAdmin[adminAddress] = false;
            delete admins[index];
            numberOfAdmins -= 1;
        }
        
        emit SetAdmin(adminAddress, addNew, index);
    }
    
    /**
     * @notice Get users nonce
     * @param user Address of the user
     */  
    function getNonce(address user) external view returns (uint256) {
        return nonces[user];
    }
    
    /**
     * @notice Get total amount user claimed
     * @param user Address of the user
     */  
    function getClaimed(address user) external view returns (uint256) {
        return claimed[user];
    }
  
    /**
     * @notice Get hash of the input data
     * @param user Address of the user
     */  
    function getMessageHash(address user, uint256 amount, uint256 nonce) public pure returns(bytes32) {
        return keccak256(abi.encodePacked(user, amount, nonce));   
    }

    /**
     * @notice Get hash of the input hash and ethereum message prefix
     * @param hash Hash of some data
     */  
    function getEthereumMessageHash(bytes32 hash) public pure returns(bytes32) {
        return keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", hash));
    }
  
    /**
     * @notice Recover signer address from signature
     * @param hash Hash of some data
     * @param signature Signature of this hash
     */   
    function recoverSigner(bytes32 hash, bytes memory signature) internal pure returns (address) {
        bytes32 r;
        bytes32 s;
        uint8 v;
    
        if (signature.length != 65) {
            return (address(0));
        }
        
        assembly {
            r := mload(add(signature, 0x20))
            s := mload(add(signature, 0x40))
            v := byte(0, mload(add(signature, 0x60)))
        }
        
        if (v < 27) {
            v += 27;
        }
        
        if (v != 27 && v != 28) {
            return (address(0));
        } else {
            return ecrecover(hash, v, r, s);
        }
    }
}