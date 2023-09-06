// File: libraries/Signature.sol


pragma solidity ^0.8.7;

library Signature {
    function getEthSignedMessageHash(bytes32 _messageHash)
        internal
        pure
        returns (bytes32)
    {
        return
            keccak256(
                abi.encodePacked(
                    "\x19Ethereum Signed Message:\n32",
                    _messageHash
                )
            );
    }

    function verify(
        address _signer,
        bytes32 _messageHash,
        bytes memory signature
    ) internal pure returns (bool) {
        bytes32 ethSignedMessageHash = getEthSignedMessageHash(_messageHash);

        return recoverSigner(ethSignedMessageHash, signature) == _signer;
    }

    function recoverSigner(
        bytes32 _ethSignedMessageHash,
        bytes memory _signature
    ) internal pure returns (address) {
        (bytes32 r, bytes32 s, uint8 v) = splitSignature(_signature);

        return ecrecover(_ethSignedMessageHash, v, r, s);
    }

    function splitSignature(bytes memory sig)
        internal
        pure
        returns (
            bytes32 r,
            bytes32 s,
            uint8 v
        )
    {
        require(sig.length == 65, "invalid signature length");

        assembly {
            r := mload(add(sig, 32))
            s := mload(add(sig, 64))
            v := byte(0, mload(add(sig, 96)))
        }
    }
}

// File: libraries/Converter.sol


pragma solidity ^0.8.7;

library Converter {
    function toUint256(bytes memory _bytes)
        internal
        pure
        returns (uint256 value)
    {
        assembly {
            value := mload(add(_bytes, 0x20))
        }
    }
}

// File: NativeVRF.sol


// Native VRF Contracts (last updated v0.0.1) (NativeVRF.sol)
pragma solidity ^0.8.7;



interface IConsumer {
    function fulfillRandomWords(
        uint256 requestId,
        uint256[] memory randomWords
    ) external;
}

/**
 * @title NativeVRF
 * @dev This contract handles the random number generation in a native way. It offers simplified and secured process for on-chain
 * random number generation. People can use this contract as a center to generate random numbers. It does not require any hardware
 * specifics to setup a random request and fulfillments. This means that the contract can be freely deployed in any new EVM blockchains.
 * It allows DAO control, which provides high level of decentralization.
 */
contract NativeVRF {
    // Difficulty variables
    uint256 public constant MIN_DIFFICULTY = 1000;
    uint256 public expectedFulfillTime = 15; // seconds
    uint256 public estimatedHashPower = 100; // hash per second
    uint256 public difficulty = expectedFulfillTime * estimatedHashPower; // estimated attemps to generate a random feed input

    // Difficulty adjustment variables
    mapping(uint256 => uint256) public nBlockFulfillments; // mapping of block number and number of fulfillment in that block
    uint256 public latestFulfillmentBlock; // latest block number that fulfillments occur

    // Random control variables
    uint256 public currentRequestId = 1; // random id counter
    uint256 public latestFulfillId = 0;
    mapping(uint256 => uint256) public randomResults; // mapping of request id and generated random number
    mapping(uint256 => address) public requestInitializers; // mapping of request id and random requester address
    mapping(address => IConsumer) public consumers; 
    address owner;

    // Events
    event RandomRequested(uint256 indexed requestId);
    event RandomFullfilled(uint256 indexed requestId, uint256 random);

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }
    /**
     * @dev Initialize the first random result to generate later random numbers
     */
    constructor(uint256 seed) 
    {
        owner = msg.sender;

        requestInitializers[0] = msg.sender;
        randomResults[0] = uint256(
            keccak256(
                abi.encode(
                    seed,
                    0,
                    msg.sender,
                    tx.gasprice,
                    block.number,
                    block.timestamp,
                    block.difficulty,
                    blockhash(block.number - 1),
                    address(this)
                )
            )
        );

        latestFulfillmentBlock = block.number;
        nBlockFulfillments[block.number] = 1;
    }

    function addConsumer(address consumer) external onlyOwner
    {
        consumers[consumer] = IConsumer(consumer);
    }
    /**
     * @dev Request random numbers by putting rewards to the smart contract
     * Requirements:
     * - The reward per request must be grater than the `minReward` value
     */
    function requestRandom(uint256 numRequest) external returns (uint256) {
        
        require(address(consumers[msg.sender]) == msg.sender, "not consumer");
        require(numRequest >= 1, "At least one request");

        uint256[] memory requestIds = new uint256[](numRequest);
        uint256 requestId = 0;

        for (uint256 i = 0; i < numRequest; i++) {
            requestId = currentRequestId + i;
            requestInitializers[requestId] = msg.sender;
            requestIds[i] = requestId;
            emit RandomRequested(requestId);
        }
        
        currentRequestId = currentRequestId + numRequest;
        return requestId;
    }

    /**
     * @dev Fulfill random numbers by calculating signatures off-chain
     * Requirements:
     * - The length of all data must be equal
     * - The provided input must be complied with the corresponding signature value
     * - The signature values must be lower than the `threshold` value
     */
    function fullfillRandomness(uint256[] memory _requestIds, uint256[] memory _randInputs, bytes[] memory _signatures) external onlyOwner
    {
        require(tx.origin == msg.sender, "Require fulfill by EOA");
        require(_requestIds.length > 0, "Require at least one fulfillment");

        for (uint256 i = 0; i < _requestIds.length; i++) {
            fullfillRandomnessInternal(_requestIds[i], _randInputs[i], _signatures[i]);
            
        }

        nBlockFulfillments[block.number] += _requestIds.length;
        latestFulfillId += _requestIds.length;

        if (block.number > latestFulfillmentBlock) {
            recalculateThreshold();
            latestFulfillmentBlock = block.number;
        }
    }

    /**
     * @dev Validate random fulfill inputs and record random result if the inputs are valid
     * Requirements:
     * - The request must not yet be fulfilled
     * - The previous request must be already fulfilled
     * - The signature value must be less than the `threshold` value
     * 
     * Signature verification process:
     * - Compute message hash from the provided input
     * - Verify signature from the message hash and the sender address
     * - Convert the signature into a number value and compare to the `threshold` value
     */
    function fullfillRandomnessInternal(
        uint256 _requestId,
        uint256 _randInput,
        bytes memory _signature
    ) private {

        require(
            requestInitializers[_requestId] != address(0),
            "Random have not initialized"
        );
        require(randomResults[_requestId] == 0, "Already fullfilled");
        require(randomResults[_requestId-1] != 0, "No prior result");

        bytes32 messageHash = getMessageHash(_requestId, _randInput);

        require(
            Signature.verify(msg.sender, messageHash, _signature),
            "Invalid signature"
        );

        
        uint256 prevRand = randomResults[_requestId - 1];

        uint256 random = uint256(
            keccak256(
                abi.encode(
                    _randInput,
                    prevRand,
                    requestInitializers[_requestId],
                    tx.gasprice,
                    block.number,
                    block.timestamp,
                    block.difficulty,
                    blockhash(block.number - 1),
                    address(this)
                )
            )
        );

        uint256[] memory randomWords = new uint256[](1);
        randomWords[0] = random;

        randomResults[_requestId] = random;
        
        consumers[requestInitializers[_requestId]].fulfillRandomWords(_requestId, randomWords);

        emit RandomFullfilled(_requestId, random);
        
    }

    /**
     * @dev Adjust the threshold value to maintain system security and simplicity
     */
    function recalculateThreshold() private {
        uint256 prevFulFills = nBlockFulfillments[latestFulfillmentBlock];
        uint256 blockDiff = block.number - latestFulfillmentBlock;
        if (blockDiff > prevFulFills) {
            difficulty = MIN_DIFFICULTY;
        } else {
            difficulty = expectedFulfillTime * estimatedHashPower * (prevFulFills / blockDiff);
        }
    }

    /**
     * @dev Compute message hash from the provided input
     */
    function getMessageHash(uint256 _requestId, uint256 _randInput)
        public
        view
        returns (bytes32)
    {
        uint256 prevRand = _requestId > 0 ? randomResults[_requestId - 1] : 0; 
        return
            keccak256(
                abi.encodePacked(
                    prevRand,
                    _randInput
                )
            );
    }

    /**
     * @dev Convert signatures to number values
     */
    function convertSignatures(bytes[] memory _signatures)
        external
        pure
        returns (uint256[] memory)
    {
        uint256[] memory results = new uint256[](_signatures.length);
        for (uint256 i = 0; i < _signatures.length; i++) {
            results[i] = uint256(Converter.toUint256(_signatures[i]));
        }
        return results;
    }
}